---
layout: post
title:  "Squeezing a lightweight server"
date:   2014-07-15 10:22:00
categories: Server
---

The problem is to run a basic server with scripts/apps to receive upload data and dump into MySQL through https connection, on a VPS link with 256m memory. The server uses OpenVZ, so no swap, 256m is all I have. Be unfamiliar with such settings, I have already ran out of memory twice, in which the bash could not even fork any new process to execute commands. Therefore, I need to squeeze memory usage of what I want to put on the server.

FYI, the common commands used to check memory usage is `free -m` and `top`. The former returns the memory of machine in total, used and free, while the latter returns current alive process and their memory usage.

### Reduce MySQL memory footprint

MySQL is required for the server. Since I cannot shut it down, I have to reduce its usage on memory. Currently, it eats 18m when it runs stably:

    PID  USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
    1439 mysql     18   0  152m  18m 5692 S    0  7.3   0:00.07 mysqld

I searched online, read a few articles, explaining how configuration works in MySQL. For my version, the configuration is `/etc/mysql/my.cnf`, and of course you need a root account to modify such file.

First of all, it seems a good idea to diable InnoDB table. InnoDB and MyISAM are two most common storage engine for MySQL database. InnoDB provides high performance but requires more resources. It seems a common approach for normal scale web server is to disable InnDB and use MyISAM as the engine, by adding two lines to `my.cnf`:

    innodb=OFF
    default_storage_engine=MyISAM

Two things to note here. First, when I searched online some articles used `skip-innodb` to achieve the same configuration, but that only works for older version. Second, the two lines should be put under `[mysqld]` line, indicating it affects `mysqld`.

To check if InnoDB has been disabled use `mysqladmin -u root -p var | grep have_innodb`. If it returns the same as the following, then it works:

    | have_innodb                             | DISABLED  

Note in the command you have to provide your own MySQL username and password. After the modification, you should restart your database server. 

By disabling InnoDB, MySQL memory usage has been reduced to:

    PID   USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
    10211 mysql     18   0 54556 8804 4896 S    0  3.4   0:00.04 mysqld                     

9m, half of what it was before.

Next, we tune down memory usage by changing parameters. There are several parameters in `my.cnf` file, each of which is actually a cap for something. For example, `key_buffer_size` is the maximum size of a buffer where MySQL put index and indexed tables in to boost performance. I read through some articles, [some of them](http://www.mysqlperformanceblog.com/2006/09/29/what-to-tune-in-mysql-server-after-installation/) explained those parameters in details, [some](https://panel.cinfu.com/knowledgebase/5/Low-RAM-configuration-for-MySQL-and-Apache.html) gave sample values. 

What I did is following (original value is in the parenthesis):

    key_buffer_size = 2M (16M)
    thread_stack = 64K (192K)
    query_cache_limit = 128K (1M)
    query_cache_size = 2M (16M)

The first one is said to be the most important one. However people set really different values for it. Some claims at least `4M`, while some set `16K` without explaining. I set it to `2M`, and will check if it is appropriate with a method I describe later.

`thread_stack` determines the space to store an incoming query. Therefore, the larger the value, more complex the query is allowed to be. Sicne we do not have such requirement (we need only to dump data, i.e. `insert into`), I cut it off. Similarly, `query_cache_limit` and `query_cache_size` cache result for executed queries to boost performance. Again, we do not need to issue any queries other than `insert` (which is un-cache-able) to database, therefore I cut them off too.

As you can see, I set all numbers magically, without much concrete evidence to support. Fortunately, there are some scripts to give recommendations for such parameters based on database usage. I use `tuning_primer.sh`; they should be similar though. When your database has been runing for a while stably, it will generate usage statiscs. By that time, running those scripts will measure if your configuration works well so far. Thus, I will use these parameters for a while, and then decide if I need to change them.

For now, the memory usage is:

    PID   USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
    12196 mysql     18   0 25756 7200 4940 S    0  2.8   0:00.04 mysqld

I think MySQL is good for now.

### Web server

Apache is up and running, and it is eating memory:

    PID  USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND      
    1309 www-data  15   0 39820  20m 4172 S    0  8.2   0:00.94 apache2  
    1439 mysql     18   0  152m  18m 5692 S    0  7.3   0:00.07 mysqld   
    1310 www-data  16   0 25752 7528 2832 S    0  2.9   0:00.01 apache2  
    1311 www-data  15   0 25752 7444 2748 S    0  2.9   0:00.00 apache2  
    1275 root      18   0 24316 7412 4000 S    0  2.9   0:00.05 apache2  
    1312 www-data  15   0 25416 7320 2780 S    0  2.9   0:00.01 apache2 
    1313 www-data  18   0 25284 6696 2208 S    0  2.6   0:00.00 apache2  
    2002 www-data  18   0 25284 6688 2208 S    0  2.6   0:00.00 apache2

Several things:

1. Apache is "pre-fork" model, in which it generates spare children at start up (ones in the bottom)
2. HTTPS connection consumes much more than HTTP: 20m per connection
3. Apache keeps connections alive for a while even if client closes
4. Every new connection spawns one more child

I could do the same for Apache what I did to MySQL. The problem is Apache seems a too big saw when I only need a knife. I switched to [Lighttpd](http://www.lighttpd.net) consequently.

Originally I tried [CherryPy](), a Python web framwork. The biggest advantage of it is 1).simple and minimal; 2). has built-in WSGI server. That means I could write code for my app and serve direclty without any server softwares.

However, it seems to have a hard time to server HTTPS connection. It does have the support, but also a [bug](https://bitbucket.org/cherrypy/cherrypy/issue/1298/ssl-not-working). In addition, it maybe that I did not fully understand how it works, but in CherryPy one needs to define ports (e.g. 433 for HTTPS), and it needs root permission, which is not easy in Python virtual environment. The bug seems not exist in previous version (I used version 3.3.0), but rolling back to older version might not be a good idea for any production dev.

Anyway, I gave up CherryPy for now. It looks still quite cool though, with dead-easy setup and full control over the server.

Eventually I chose Lighttpd because

1. It works similarly as Apache, so that less transition work
2. It supports CGI and FastCGI, so Python web app survives
3. It is said to be fast and lightweight

One downside is it does not support WSGI. But it might worth as with some initial configuration I got:

    PID   USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND      
    19658 www-data  18   0  7488 1028  504 S    0  0.4   0:00.00 lighted

upon start up, and following when requests comming:

    PID   USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND      
    19658 www-data  18   0  7900 2140 1320 S    0  0.8   0:00.01 lighttpd

Definitely need further monitor, but so far so good.

Now with such tweaking, the memory footprint finally look like something I could work with (without running requests of course):

                 total       used       free     shared    buffers     cached
    Mem:           250         62        187          0          0          0
    -/+ buffers/cache:         62        187
    Swap:            0          0          0
