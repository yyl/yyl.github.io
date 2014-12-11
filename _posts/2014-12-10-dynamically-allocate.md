---
layout: post
title:  "C++ Memory allocation"
date:   2014-12-10 22:00:00
categories: cpp
---

All variables without explicitly statement like we did below are allocated on the stack, and are gone once they are out of scope. To allocate variable is to create it by dynamically from the heap. Such variable will stay alive until one explicitly use `delete` to trash it.
	
	// allocate an int variable on heap, and return its address
    int *pnValue = new int;
	// allocate an int array on heap, and return the address of the first value
	int *ar_heap = new int[5];
	*pnValue = 1; // assign value to the variable
    delete pnValue;
	pnValue = nullptr;
    delete[] ar_heap; // unallocate both piece of memory
	
Once we dynamically allocate the memory, we have to take care of them by ourselves. This means we have to call `delete` at the time when we no longer need them. `delete` deallocates the memory, but we also need to set the pointer to `nullptr` so that it is not pointing to a deallocated memory. (TODO: verify)

Without care it is easy to create memory leak like this one below.

	int *pnValue = new int;
	pnValue = new int; // old address lost, memory leak results
	
TBD: alloc, malloc