+++
draft = false
date = 2019-07-10
title = "Android Development Notes"
description = "Some notes for Android Development"
tags = ["Mobile Development"]
+++

## Table of Contents

* [Table of Contents](#table-of-contents)
* [Basics](#basics)
* [App Resources](#app-resources)
* [Permissions](#permissions)
* [AndroidManifest.xml](#androidmanifestxml)
* [Layout](#layout)
* [Activities](#activities)
* [Fragments](#fragments)
* [Notifications](#notifications)
* [Android Studio Tips](#android-studio-tips)
* [Miscellaneous](#miscellaneous)
* [Resources](#resources)

## Basics

1. **Layout** is typically defined in XML, whereas **activities** are in Java class.

2. The Android operating system is a multi-user Linux system in which each app is a different user.

## App Resources

1. String array: `android:entries="@array/foo"`.

    ```xml
    <string-array name="foo">
        <item>bar1</item>
        <item>bar2</item>
    </string-array>
    ```

2. [Creating alias resources](https://developer.android.com/guide/topics/resources/providing-resources#AliasResources):

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <drawable name="icon">@drawable/icon_ca</drawable>
    </resources>
    ```

3. [Styles and Themes](https://developer.android.com/guide/topics/ui/look-and-feel/themes)

4. [Referencing style attributes](https://developer.android.com/guide/topics/resources/providing-resources#ReferencesToThemeAttributes): `android:textColor="?android:textColorSecondary`

5. [Support different pixel densities](https://developer.android.com/training/multiscreen/screendensities)

6. [Declare restricted screen support](https://developer.android.com/guide/practices/screens-distribution)

## Permissions

1. [Request App Permissions](https://developer.android.com/training/permissions/requesting#explain)

2. [Use an intent instead](https://developer.android.com/training/permissions/usage-notes#perms_vs_intents)

## AndroidManifest.xml

1. The `minSdkVersion` attribute declares the minimum version with which your app is compatible and the `targetSdkVersion` attribute declares the highest version on which you've optimized your app: `<uses-sdk android:minSdkVersion="4" android:targetSdkVersion="15" />`

## Layout

1. `<EditText>` is an editable text field:
    * `android:hint="@string/hint"`: normally "Enter a message", telling the user what to type;
    * `android:ems="10"`: 10-M space.

2. `android:layout_weight="number"` makes a view stretch.
    * If a has the weight of 1 and b has 2, a'll have 1/3 and b'll have 2/3 of the screen.
    * We usually have `android:layout_height="0dp"` (for vertical layout) above layout_weight.

3. `android:gravity="top"` moves the CONTENT of a view to the top of the view, whereas `android:layout_gravity="end"` moves the placement of the VIEW itself to the end.

4. `<FrameLayout></FrameLayout>` allows views to overlap. Ues it when we need to replace the fragments and add the changes to the back stack.

5. Surround LinearLayout/FrameLayout with `<ScrollView></ScrollView>` to get a vertical scrollbar. (`HorizontalScrollView` for the horizontal scrollbar.)

6. A CoordinatorLayout allows the behavior of one view to affect the behavior of another.

7. AdapterView:

    ```java
    ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
        android.R.layout.simple_list_item_1, myStringArray);
    ListView listView = (ListView) findViewById(R.id.listview);
    listView.setAdapter(adapter)
    // Create a message handling object as an anonymous class.
    private OnItemClickListener messageClickedHandler = new OnItemClickListener() {
        public void onItemClick(AdapterView parent, View v, int position, long id) {
        // Do something in response to the click
        }
    };

    listView.setOnItemClickListener(messageClickedHandler);
    ```

8. [Create a List with RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview)

9. [Re-using layouts with include](https://developer.android.com/training/improving-layouts/reusing-layouts): `<merge></merge>` `<include layout="@layout/foo"/>`

10. Spinners:

    ```java
    Spinner s1 = (Spinner) findViewById(R.id.spinner1);
    ArrayAdapter adapter = ArrayAdapter.createFromResource(
    this, R.array.colors, android.R.layout.simple_spinner_item);
    adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
    s1.setAdapter(adapter);
    s1.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
        @Override
        public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
            // TODO
        }

        @Override
        public void onNothingSelected(AdapterView<?> parent) {
           // sometimes you need nothing here
        }
    });
    ```

11. [Buttons](https://developer.android.com/guide/topics/ui/controls/button) Set onClick programmatically:

    ```java
    Button button = (Button) findViewById(R.id.button_send);
    button.setOnClickListener(new View.OnClickListener() {
        public void onClick(View v) {
            // Do something in response to button click
        }
    });
    ```

12. [Checkboxes](https://developer.android.com/guide/topics/ui/controls/checkbox)

13. [Radio Buttons](https://developer.android.com/guide/topics/ui/controls/radiobutton)

14. [Switch Buttons](https://www.tutlane.com/tutorial/android/android-switch-on-off-button-with-examples)

    ```xml
    android:checked="true"
    android:textOff="OFF"
    android:textOn="ON
    ```

    ```java
    Switch sw = (Switch) findViewById(R.id.switch1);
    sw.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
        public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
            if (isChecked) {
                // The toggle is enabled
            } else {
                // The toggle is disabled
            }
        }
    });
    ```

15. [Pickers](https://developer.android.com/guide/topics/ui/controls/pickers) (Time/Date Pickers)

16. Centralized onClick events:

    ```java
    public class ActivityA extends Activity implements View.OnClickListener {
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            findViewById(R.id.first).setOnClickListener(this);
            findViewById(R.id.second).setOnClickListener(this);
            findViewById(R.id.third).setOnClickListener(this);
        }

        @Override
        public void onClick(View v) {
            switch (v.getId()) {
                case R.id.first:
                    // Manage click.
                    break;

                case R.id.second:
                    // Manage click.
                    break;

                case R.id.third:
                    // Manage click.
                    break;
            }
        }
    }
    ```

17. App bar:

    * [Set up the app bar](https://developer.android.com/training/appbar/setting-up) (Use `Toolbar` instead of the native `ActionBar`) and [Add an up action](https://developer.android.com/training/appbar/up-action)

        ```xml
        android:theme="@style/Theme.AppCompat.Light.NoActionBar"
        ```

        ```xml
        <android.support.v7.widget.Toolbar
            android:id="@+id/my_toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="?attr/colorPrimary"
            android:elevation="4dp"
            android:theme="@style/ThemeOverlay.AppCompat.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/>
        ```

        ```xml
            <!-- A child of the main activity -->
       <activity
           android:name="com.example.myfirstapp.MyChildActivity"
           android:label="@string/title_activity_child"
           android:parentActivityName="com.example.myfirstapp.MainActivity" >

        <!-- Parent activity meta-data to support 4.0 and lower -->
            <meta-data
                android:name="android.support.PARENT_ACTIVITY"
                android:value="com.example.myfirstapp.MainActivity" />
        </activity>
        ```

        ```java
        // OnCreate
        Toolbar myToolbar = (Toolbar) findViewById(R.id.my_toolbar);
        setSupportActionBar(myToolbar);
            // Get a support ActionBar corresponding to this toolbar
        ActionBar ab = getSupportActionBar();

        // Enable the Up button
        ab.setDisplayHomeAsUpEnabled(true);
        ```

    * [Add and handle actions](https://developer.android.com/training/appbar/actions)

18. Toasts: `Toast toast = Toast.makeText(this, "hello world", Toast.LENGTH_SHORT); toast.show();`

## Activities

1. All activities have to extend the Activity class or its subclass.

2. R is a special Java class that enables you to retrieve references to resources. `TextView foo = findViewById(R.id.foo)`

3. Use Intent to call another activity (can be from other apps):
    * Explicit intent:

        ```java
        public void onSendMessage(View view) {
            Intent intent = new Intent(this, FooActivity.class); // explicit intent
            intent.putExtra("message", value); // message is the name of the extra
            startActivity(intent);
        }
        ```

        ```java
        ...
        Intent intent = getIntent();
        String string = intent.getStringExtra("message");
        ```

    * Implicit intent: (with actions to allow users to choose which app to run)

        ```java
        Intent intent = new Intent(Intent.ACTION_SEND); // ACTION_DIAL/ACTION_WEB_SEARCH
        intent.setType("text/plain");
        intent.putExtra(Intent.EXTRA_TEXT, messageText);
        String chooserTitle = getString(R.string.chooser);
        // Ensure that the user always get the chance to choose an activity
        Intent chosenIntent = Intent.createChooser(intent, chooserTitle);

        // Start an activity if it's safe
        if (intent.resolveActivity(getPackageManager()) != null) {
            startActivity(chosenIntent);
        }
        ```

    * [Common Intents](https://developer.android.com/guide/components/intents-common)

4. Save the instance state:

    ```java
    // Either in onCreate savedInstanceState != null or use the method below
    public void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);
        seconds = savedInstanceState.getInt("seconds");
        running = savedInstanceState.getBoolean("running");
    }

    @Override
    public void onSaveInstanceState(Bundle savedInstanceState) {
        super.onSaveInstanceState(savedInstanceState);
        savedInstanceState.putInt("seconds", seconds);
        savedInstanceState.putBoolean("running", running);
    }
    ```

5. Activity lifecycle:
    * When we implement them (`onCreate()`, `onStart()`, `onResume()`, `onPause()`, `onStop()`, `onRestart()`, `onDestroy()`), we must call the super class methods.
    * `onResume()` is called when the activity is started OR resumed; `onPause()` is called when the activity is paused OR stopped.
    * ![Activity lifecycle](/images/activity_lifecycle.png)

6. [Defining launch modes](https://developer.android.com/guide/components/activities/tasks-and-back-stack#TaskLaunchModes) (singleTop, singleTask, etc.)

## Fragments

1. Fragment lifecycle:

    ![Fragment lifecycle](/images/fragment_lifecycle.png)

2. Example:

    ```java
    public static class ExampleFragment extends Fragment {
        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
            // Inflate the layout for this fragment
            return inflater.inflate(R.layout.example_fragment, container, false);
            // In this case, this is false because the system is already inserting the inflated layout into the container
        }
    }
    ```

3. Add the fragment programmatically:

    ```java
    FragmentManager fragmentManager = getSupportFragmentManager();
    FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
    ExampleFragment fragment = new ExampleFragment();
    fragmentTransaction.add(R.id.fragment_container, fragment);
    fragmentTransaction.commit();
    ```

4. Fragment transactions:

    * Example:

        ```java
        // Create new fragment and transaction
        Fragment newFragment = new ExampleFragment();
        FragmentTransaction transaction = getSupportFragmentManager().beginTransaction();

        // Replace whatever is in the fragment_container view with this fragment,
        // and add the transaction to the back stack
        transaction.replace(R.id.fragment_container, newFragment);
        transaction.addToBackStack(null);

        // Commit the transaction
        transaction.commit();
        ```

5. For dynamic fragments, use FrameLayout instead of Fragment and use FragmentTransaction.

## Notifications

1. [Create a Notification](https://developer.android.com/training/notify-user/build-notification)

## Android Studio Tips

1. [Improve code inspection with annotations](https://developer.android.com/studio/write/annotations.html#java) (`@Nullable`, `@NonNull`...)

2. [Find sample code](https://developer.android.com/studio/write/sample-code)

3. [Tools attributes reference](https://developer.android.com/studio/write/tool-attributes) (`tools:text="foo"`, `tools:itemCount="3"...`)

4. [Debug your app](https://developer.android.com/studio/debug) (` private static final String TAG = "MyActivity"; Log.d(TAG, "foo");`)

5. [Write and View Logs with Logcat](https://developer.android.com/studio/debug/am-logcat) (e (error), w (warning), i (information), d (debug), v (verbose))

6. [Debug Your layout with Layout Inspector](https://developer.android.com/studio/debug/layout-inspector)

7. [Manage your app's UI resources with Resource Manager](https://developer.android.com/studio/write/resource-manager)

8. [Create app icons with Image Asset Studio](https://developer.android.com/studio/write/image-asset-studio#java)

9. [Create resizable bitmaps (9-Patch files)](https://developer.android.com/studio/write/draw9patch)

10. [Add Android App Links](https://developer.android.com/studio/write/app-link-indexing#java)

## Miscellaneous

1. Android builds the back stack to keep track of the activity/fragment transactions.

2. [Create Deep Links to App Content](https://developer.android.com/training/app-links/deep-linking):

    ```xml
    <intent-filter>
      ...
      <data android:scheme="https" android:host="www.example.com" />
      <data android:scheme="app" android:host="open.my.app" />
    </intent-filter>
    ```

3. [Verify Android App Links](https://developer.android.com/training/app-links/verify-site-associations)

4. [Adding Swipe-to-Refresh To Your App](https://developer.android.com/training/swipe/add-swipe-interface) (Add `SwipeRefreshLayout` as the parent of a single `ListView` or `GridView`.) (`android.support.v4.widget.SwipeRefreshLayout`)

5. [Responding to Refresh Request](https://developer.android.com/training/swipe/respond-refresh-request)

    ```java
    mySwipeRefreshLayout.setOnRefreshListener(
        new SwipeRefreshLayout.OnRefreshListener() {
            @Override
            public void onRefresh() {
                Log.i(LOG_TAG, "onRefresh called from SwipeRefreshLayout");

                // This method performs the actual data-refresh operation.
                // The method calls setRefreshing(false) when it's finished.
                myUpdateOperation();
            }
        }
    );
    ```

    ```java
    // For refreshing in the app bar
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {

            // Check if user triggered a refresh:
            case R.id.menu_refresh:
                Log.i(LOG_TAG, "Refresh menu item selected");

                // Signal SwipeRefreshLayout to start the progress indicator
                mySwipeRefreshLayout.setRefreshing(true);

                // Start the refresh background task.
                // This method calls setRefreshing(false) when it's finished.
                myUpdateOperation();

                return true;
        }

        // User didn't trigger a refresh, let the superclass handle this action
        return super.onOptionsItemSelected(item);
    }
    ```

## Resources

* [Head First Android Development: A Brain-Friendly Guide 2nd Edition](https://www.amazon.com/Head-First-Android-Development-Brain-Friendly/dp/1491974052/ref=sr_1_1?keywords=head+first+android&qid=1560338899&s=gateway&sr=8-1)

* <https://developer.android.com>
