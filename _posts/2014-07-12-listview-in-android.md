---
layout: post
title:  "ListView and Dialog in Android"
date:   2014-07-12 10:00:00
categories: Android
---

**Somehow I really lost the appetite of finishing this. So I will just leave it here as it is. No warrant on the code here.**

In one of my study app I need to create a screen for NASA TLX form. I decided to make it a list. The list will have six items, each of which represents one item in the form. When participants click one of them, it leads them into a new screen, where they could input the score for the item. Then they could press back to go back to the list view. That is all.

Clearly my simple listview does not need any dynamic loading of data, as all items are predefiend and fixed. Surprisingly, Android doc does a bad job explaining basic usage of fixed list view, so does everyone else on the internet. Most of them are talking about dynamic list, how to load data from database to adapt the list, how to auto refresh, etc. Therefore, I decided to talk a few about a plain old fixed list view.

### simplest list in 4 steps

In Android, there is actually a dedicated activity for list view: `ListActivity`. Basic logic of how it works is:

1. create a `Listactivity`
2. create your data to be the list
3. make an adapter to take in the data, and a list view xml template
4. set the adapter of the activity to be the adapter you created

Thats it. Code is below:

    public class TLXActivity extends ListActivity {

      SimpleCursorAdapter mAdapter;

	    @Override
    	protected void onCreate(Bundle savedInstanceState) {
    		super.onCreate(savedInstanceState);
    		String[] values = new String[] {
            "New item 1",
    				"New item 2",
    				"New item3" };

    		ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                    android.R.layout.simple_list_item_1, values);

            // Assign adapter to List
            setListAdapter(adapter);

    	}

    	@Override
    	public void onListItemClick(ListView l, View v, int position, long id) {
    		// Do something when a list item is clicked
    	}
    }

Things to note here: `android.R.layout.simple_list_item_1` is a default list item layout provided by Android. It takes care of how each single item in your list should be displayed so that you do not have to write ti yourself, if all you need to do is just displaying a basic list. You need to implement `onListItemClick`if you want to do anything to the click action of each item on your list.

### make your own list layout

In 2 steps:

1. create a layout xml file containing a `ListView` object
2. add it to the activity in `onCreate()` using `setContentView()`

Things to note: the `ListView` object has to have an id of `@android:id/list` so that activity will identify it.

### make your own row layout

Like I said, `android.R.layout.simple_list_item_1` is the default layout predefined by Android for basic display. If you want customized one like I do, you could create your own.

1. create customized layout xml file in `res/layout`
2. add it to your `SimpleArrayAdapter` constructor

I need each item to display both a text and an image, as the image will be a check once pariticpants complete the correspnding task. Heres my row layout:

    <?xml version="1.0" encoding="utf-8"?>
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" >

        <TextView
            android:id="@+id/row_item"
            android:layout_width="wrap_content"
            android:layout_height="32dp"
            android:layout_gravity="center"
            android:layout_alignParentLeft="true"
            android:text="@+id/row_item" >
        </TextView>

        <ImageView
            android:id="@+id/icon"
            android:layout_width="wrap_content"
            android:layout_height="32dp"
            android:layout_alignParentRight="true"
            android:src="@drawable/ic_row_check" >

        </ImageView>

    </RelativeLayout>

And the constructor in `ListActivity` now:

    ArrayAdapter<String> adapter = new ArrayAdapter<String>(
				this,
                R.layout.row_layout,
                R.id.row_item,
                values);

A few notes:

1. If you want the text align to left and image align to the right for each row item in the list, like I do, you need a `RelativeLayout` instead of `LinearLayout`
2. My layout has been tweaked yet, so text and image are mismatched right now
3. In the constructor, you put a `Context` object, the customized row layout id, the id of textview which will display items in your list, and your list object, in the same order as I do.

### Dialog

Now when each of the item got clicked, I want the app to display a dialog in which people could drag a SeekBar to set a certain score for each item. The list view would also be able to display the score after it is set.

The first thing is to add a class extended from `DialogFragment` nested under the list activity. [Official doc](http://developer.android.com/guide/topics/ui/dialogs.html) has it pretty much covered.

### Customize dialog UI

Since I want a [`SeekBar`](http://developer.android.com/design/building-blocks/seek-bars.html) in the dialog, I have to define it layout by myself.

Defining a xml layout with `SeekBar` item is pretty easy. Once I have the xml file, I have to inflate the dialog object with the xml file:

			LayoutInflater inflater = getActivity().getLayoutInflater();
			final View dialogView = inflater.inflate(
					R.layout.layout_item_dialog, null);
			builder.setView(dialogView);

This is done in `onCreateDialog()` method of the dialog class.

### Customize SeekBar

Here I mean the range, step size and some other aspects of a SeekBar:

	final View dialogView = inflater.inflate(
						R.layout.layout_item_dialog, null);
	item_score_seekbar = (SeekBar) dialogView
						.findViewById(R.id.tlx_item_score);
    item_score_seekbar.setMax(20);
    item_score_seekbar.setProgress(0);
	item_score_seekbar.setOnSeekBarChangeListener(new OnSeekBarChangeListener() {
							@Override
							public void onProgressChanged(SeekBar seekBar,
									int progress, boolean fromUser) {
								TextView cur_text = (TextView) dialogView
										.findViewById(R.id.cur_score);
								cur_text.setText(Integer.toString(progress * 5));
							}
							...
						});

Note here, instead of calling `findByViewId()` directly as usual, I first get the current dialog view explicitly, then call `findViewById` function of the view. The reason is because in `DialogFragment` class somehow the view is not activated automatically like activity classes do. Therefore you have to create the view manually before calling the function. It seems implementing `OnCreateView` could also do the trick but I have not tried.

The two lines after that sets the maximum value and initial starting value. Nothing new here. However, what I actually want is to set the maximum value to be 100, and _step size_ to be 5. To achieve this, I first set the maximum value to be 20, and then in `OnSeekBarChangeListener.onProgressChanged` I set the displayed score (`cur_text`) to be `progress * 5`, in which `progress` is the actual value of the seekbar. Doing so, the displayed value increases 5 every time people drag the bar by one. 

One function I want for the SeekBar dialog is to save the score every time people close the dialog, and display the saved score if people open the dialog of the same item again. To do so I need to put some save code in a right place:

	builder.setPositiveButton("Submit",
			new DialogInterface.OnClickListener() {
				public void onClick(DialogInterface dialog,
						int id) {
					...
					TextView cur_text = (TextView) dialogView
							.findViewById(R.id.cur_score);
					String cur_score = cur_text.getText().toString();
					scores.put(MyApp.item_names[item_index],cur_score);
				}
			});
			
This is a part of the dialog builder code. For the button on the dialog I added the `OnClickListener`, where I add few lines of codes to capture the current displayed score and store in a `ContentValue` object with appropriate key. This object works similarly to a hash table.

