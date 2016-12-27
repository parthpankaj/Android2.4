# Android2.4
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="4dip" >

    <TextView
        android:id="@+id/msg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="0"
        android:paddingBottom="4dip" />

    <EditText
        android:id="@+id/saved"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:background="@android:color/holo_blue_light"
        android:freezesText="true"
        android:text="Hellow" >

        <requestFocus />
    </EditText>

</LinearLayout>
--------------------
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:gravity="center_horizontal"
    android:layout_width="match_parent" android:layout_height="match_parent">

    <TextView android:layout_width="match_parent" android:layout_height="wrap_content"
        android:gravity="center_vertical|center_horizontal"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:text="Demonstration of hiding and showing fragments." />

    <LinearLayout android:orientation="horizontal" android:padding="4dip"
        android:gravity="center_vertical" android:layout_weight="1"
        android:layout_width="match_parent" android:layout_height="wrap_content">

        <Button android:id="@+id/frag1hide"
            android:layout_width="wrap_content" android:layout_height="wrap_content"
            android:text="Hide" />

        <fragment android:name="com.example.pankaj.assignment24"
            android:id="@+id/fragment1" android:layout_weight="1"
            android:layout_width="0px" android:layout_height="wrap_content" />

    </LinearLayout>

    <LinearLayout android:orientation="horizontal" android:padding="4dip"
        android:gravity="center_vertical" android:layout_weight="1"
        android:layout_width="match_parent" android:layout_height="wrap_content">

        <Button android:id="@+id/frag2hide"
            android:layout_width="wrap_content" android:layout_height="wrap_content"
            android:text="Hide" />

        <fragment android:name="com.example.pankaj.assignment24"
            android:id="@+id/fragment2" android:layout_weight="1"
            android:layout_width="0px" android:layout_height="wrap_content" />

    </LinearLayout>

</LinearLayout>
---------------------------------
package com.example.pankaj.assignment24;

        import android.app.Activity;
        import android.app.Fragment;
        import android.app.FragmentManager;
        import android.app.FragmentTransaction;
        import android.os.Bundle;
        import android.view.LayoutInflater;
        import android.view.View;
        import android.view.View.OnClickListener;
        import android.view.ViewGroup;
        import android.widget.Button;
        import android.widget.TextView;

/**
 * Demonstration of hiding and showing fragments.
 */
public class FragmentHideShow extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        // The content view embeds two fragments; now retrieve them and attach
        // their "hide" button.
        FragmentManager fm = getFragmentManager();
        addShowHideListener(R.id.frag1hide, fm.findFragmentById(R.id.fragment1));
        addShowHideListener(R.id.frag2hide, fm.findFragmentById(R.id.fragment2));
    }

    void addShowHideListener(int buttonId, final Fragment fragment) {
        final Button button = (Button)findViewById(buttonId);
        button.setOnClickListener(new OnClickListener() {
            public void onClick(View v) {
                FragmentTransaction ft = getFragmentManager().beginTransaction();
                ft.setCustomAnimations(android.R.animator.fade_in,
                        android.R.animator.fade_out);
                if (fragment.isHidden()) {
                    ft.show(fragment);
                    button.setText("Hide");
                } else {
                    ft.hide(fragment);
                    button.setText("Show");
                }
                ft.commit();
            }
        });
    }

    public static class FirstFragment extends Fragment {
        TextView mTextView;

        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {
            View v = inflater.inflate(R.layout.activity_main, container, false);
            View tv = v.findViewById(R.id.msg);
            ((TextView)tv).setText("The fragment saves and restores this text.");

            // Retrieve the text editor, and restore the last saved state if needed.
            mTextView = (TextView)v.findViewById(R.id.saved);
            if (savedInstanceState != null) {
                mTextView.setText(savedInstanceState.getCharSequence("text"));
            }
            return v;
        }

        @Override
        public void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);

            // Remember the current text, to restore if we later restart.
            outState.putCharSequence("text", mTextView.getText());
        }
    }

    public static class SecondFragment extends Fragment {

        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {
            View v = inflater.inflate(R.layout.activity_main, container, false);
            View tv = v.findViewById(R.id.msg);
            ((TextView)tv).setText("The TextView saves and restores this text.");

            // Retrieve the text editor and tell it to save and restore its state.
            // Note that you will often set this in the layout XML, but since
            // we are sharing our layout with the other fragment we will customize
            // it here.
            ((TextView)v.findViewById(R.id.saved)).setSaveEnabled(true);
            return v;
        }
    }
}
