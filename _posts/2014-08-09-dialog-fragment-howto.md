---
layout: post
title:  "Android Dialogs HOWTO"
date:   2014-08-09 11:20:00
categories: Android
---

I read almost everything from Android dev site [here](http://developer.android.com/guide/topics/ui/dialogs.html), and you should too. Here I summarize some practices that I find reasonable for my project.

### Make it a separate class

I need multiple dialogs in the application, most of them contain some text description, and either one "OK" button, or two buttons ("YES" or "NO"). If you have multiple similar dialogs like I do, you should make a generic class extend `DialogFragment`, and make different dialogs based on it. Otherwise, you have to define a lot of dialogs and lots of duplicate code.

A class for dialog would look like this:

    public class TwoButtonDialogFragment extends DialogFragment {
        ....
        @Override
	    public Dialog onCreateDialog(Bundle savedInstanceState) {
            AlertDialog.Builder builder = new AlertDialog.Builder(getActivity());
		    builder.setMessage(...)
				.setTitle(...)
				.setIcon(...)
				.setPositiveButton(...)
            return builder.create();
	    }
    }

The `onCreateDialog` is responsible for making the dialog.

### A constructor of the class

To customize the dialog (e.g. different messages, title, etc) one needs to pass arguments into the constructor of the class. `DialogFragment` class supports static constructor:

    public static TwoButtonDialogFragment newInstance(Account account, String message) {
		TwoButtonDialogFragment f = new TwoButtonDialogFragment();

		// Supply account_type argument
		Bundle args = new Bundle();
		args.putString("message", message);
		f.setArguments(args);
		return f;
	}

And in `onCreateDialog` call `getArguments().getString("message");` to retreive the argument you put in.

Note now you have create the dialog like this:

	TwoButtonDialogFragment accountinfodialog = TwoButtonDialogFragment.newInstance(account.getDescription());

And show it:
    
    String dialog_tag = "accountinfodialog";
    FragmentManager fm = getSupportFragmentManager();
	accountinfodialog.show(fm, dialog_tag);

Now you can create different dialogs at different places as you like. The `dialog_tag` is very important as it helps you identify different dialogs if for example you have created several in one activity.

### Pass onClick event back to activity

I need the activity which starts the dialog to do stuff after people click buttons on the dialog, therefore it would be convenient if we could directly implement the `onClick` function in the activity, to let activity do their stuff. 

To have such functon, the `DialogFragment` should have a public interface for the onCLickListener it has. Then other acitivies just have to implement the interface and its functions.

A public interface for two button dialog could look like this:

    public interface TwoButtonDialogListener {
        public void onDialogPositiveClick(DialogFragment dialog);
        public void onDialogNegativeClick(DialogFragment dialog);
    }

Also add `onAttach` to the dialog class:

    // Use this instance of the interface to deliver action events
    TwoButtonDialogListener mListener;
    
    // Override the Fragment.onAttach() method to instantiate the TwoButtonDialogListener
    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
        // Verify that the host activity implements the callback interface
        try {
            // Instantiate the TwoButtonDialogListener so we can send events to the host
            mListener = (TwoButtonDialogListener) activity;
        } catch (ClassCastException e) {
            // The activity doesn't implement the interface, throw exception
            throw new ClassCastException(activity.toString()
                    + " must implement TwoButtonDialogListener");
        }
    }
   
In the `onClick` function of `onCreateDialog`, then you just add `mListener.onDialogPositiveClick(TwoButtonDialogFragment.this);`, so that the class know the `onClick` function will be implemented by whoever implements the interface.

Now in other activities, they need to add `implements TwoButtonDialogListener` at the class definition, and implements the two functions, if it is for the `TwoButtonDialogListener` interface.
