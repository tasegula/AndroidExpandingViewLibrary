## About the Library
This library is strongly inspired in this concept from Hila Peleg in dribble: https://dribbble.com/shots/2340386-Shopping-List

For more details on how to use this library please refer to the example in this repository. You can se a video of the example working here: https://youtu.be/moWaruuaEP0


## Adding the Library to gradle file
```gradle
dependencies {
    compile 'com.diegodobelo.expandingview:expanding-view:0.9.1'
}
```

## Using the Library
### Layouts
First of all include the ExpandingList in your Activity (or Fragment) layout. This will be the list of items (ExpandItem):

```xml
<com.diegodobelo.expandingview.ExpandingList
        android:id="@+id/expanding_list_main"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
```

Now create a new layout (xml) file to represent the item (such as `res/layout/expanding_item.xml`). This will be the Item (header) that can be expanded to show sub items:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="94dp">
    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerVertical="true"
        android:gravity="center_vertical|left"
        android:textSize="22sp"/>
</RelativeLayout>
```

Create another layout file to represent the sub items (such as `/res/expanding_sub_item.xml`). This will be the sub items that will be shown when the Item is expanded:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="48dp">
    <TextView
        android:id="@+id/sub_title"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_centerVertical="true"
        android:layout_marginLeft="8dp"
        android:textSize="18sp"
        android:gravity="center_vertical|left"/>
</RelativeLayout>
```

Now create a layout file (such as `/res/expanding_layout.xml`) to represent the whole item, including both item layout and sub item layout. We will explain each custom attribute later:

```xml
<com.diegodobelo.expandingview.ExpandingItem
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:item_layout="@layout/expanding_item"
    app:sub_item_layout="@layout/expanding_sub_item"
    app:indicator_size="42dp"
    app:indicator_margin_left="16dp"
    app:indicator_margin_right="16dp"
    app:show_indicator="true"
    app:show_animation="true"
    app:start_collapsed="true"
    app:animation_duration="250"/>
```

Note that we included `expanding_item` and `expanding_sub_item` layouts created before.

### Java code

Now that you have all the required layouts you are able to use them in Java code. Let's start inflating the ExpandingList:

```java
ExpandingList expandingList = (ExpandingList) findViewById(R.id.expanding_list_main);
```

Create a new ExpandingItem in the ExpandingList. This method receives the `expanding_layout` created before. Yes! We can have different layouts for different items in the same list. The items will be created based on `expanding_item` layout and the sub items will be created based on `expanding_sub_item` layout:

```java
ExpandingItem item = expandingList.createNewItem(R.layout.expanding_layout);

/*ExpandingItem extends from View, so you can call
findViewById to get any View inside the layout*/
(TextView) item.findViewById(R.id.title)).setText("It Works!!");
```

Let's create the sub items. There is a method to create items in batch:

```java
//This will create 5 items
item.createSubItems(5);

//get a sub item View
View subItemZero = item.getSubItemView(0);
((TextView) subItemZero.findViewById(R.id.sub_title)).setText("Cool");

View subItemOne = item.getSubItemView(1);
((TextView) subItemOne.findViewById(R.id.sub_title)).setText("Awesome");

...

```

For each item you can set the indicator color and the indicator icon:

```java
item.setIndicatorColorRes(R.color.blue);
item.setIndicatorIconRes(R.drawable.ic_icon);

```
### ExpandingItem layout attributes

Attribute Name         | Type         | Default Value | Meaning                                                             | Mandatory
---------------------- | ------------ | ------------- | --------------------------------------------------------------------| --------
item_layout            | reference    |               | The layout for the Item (header).                                   | Yes
sub_item_layout        | reference    |               | The layout for the sub items.                                       | Yes
separator_layout       | reference    |               | A layout to separate items.                                         | No
indicator_size         | dimension    |       0dp     | The indicator size in dp.                                           | No
indicator_margin_left  | dimension    |       0dp     | The margin between the indicator and its left.                      | No
indicator_margin_right | dimension    |       0dp     | The margin between the indicator and its right.                     | No
show_indicator         | boolean      |       true    | true if you want to show the indicator. false otherwise.            | No
show_animation         | boolean      |       true    | true if you want to show animations. false otherwise.               | No
start_collapsed        | boolean      |       true    | true if you want the sub views to start collapsed. false otherwise. | No
animation_duration     | integer      |       300ms   | The animations duration in milliseconds.                            | No

## ExpandingItem public methods

#### `public void setStateChangedListener(OnItemStateChanged listener)`

Set a listener to listen item stage changed.

 * **Parameters:** `listener` — The listener of type {@link OnItemStateChanged}

#### `public boolean isExpanded()`

Tells if the item is expanded.

 * **Returns:** true if expanded. false otherwise.

#### `public int getSubItemsCount()`

Returns the count of sub items.

 * **Returns:** The count of sub items.

#### `public void collapse()`

Collapses the sub items.

#### `public void toggleExpanded()`

Expand or collapse the sub items.

#### `public void setIndicatorColorRes(int colorRes)`

Set the indicator color by resource.

 * **Parameters:** `colorRes` — The color resource.

#### `public void setIndicatorColor(int color)`

Set the indicator color by color value.

 * **Parameters:** `color` — The color value.

## `public void setIndicatorIconRes(int iconRes)`

Set the indicator icon by resource.

 * **Parameters:** `iconRes` — The icon resource.

#### `public void setIndicatorIcon(Drawable icon)`

Set the indicator icon.

 * **Parameters:** `icon` — Drawable of the indicator icon.

#### `@Nullable public View createSubItem()`

Creates a sub item based on sub_item_layout Layout, set as ExpandingItem layout attribute.

 * **Returns:** The inflated sub item view.

#### `@Nullable public View createSubItem(int position)`

Creates a sub item based on sub_item_layout Layout, set as ExpandingItem layout attribute. If position is -1, the item will be added in the end of the list.

 * **Parameters:** `position` — The position to add the new Item. Position should not be greater than the list size.
 * **Returns:** The inflated sub item view.

#### `public void createSubItems(int count)`

Creates as many sub items as requested in {@param count}.

 * **Parameters:** `count` — The quantity of sub items.

#### `public View getSubItemView(int position)`

Get a sub item at the given position.

 * **Parameters:** `position` — The sub item position. Should be > 0.
 * **Returns:** The sub item inflated view at the given position.

#### `public void removeSubItemAt(int position)`

Remove sub item at the given position.

 * **Parameters:** `position` — The position of the item to be removed.

#### `public void removeSubItemFromList(View view)`

Remove the given view representing the sub item. Should be an existing sub item.

 * **Parameters:** `view` — The sub item to be removed.

#### `public void removeSubItem(View view)`

Remove the given view representing the sub item, with animation. Should be an existing sub item.

 * **Parameters:** `view` — The sub item to be removed.

#### `public void removeAllSubItems()`

Remove all sub items.
