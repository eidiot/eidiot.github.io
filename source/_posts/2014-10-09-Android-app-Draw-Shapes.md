title: Android app Draw Shapes
date: 2014-10-09 05:42:31
tags:
---
My first Android App [Draw Shapes](https://play.google.com/store/apps/details?id=com.evancoding.drawshapes), which is actually my first assignment of android programming class, is now on [Google Play](https://play.google.com/store/apps/details?id=com.evancoding.drawshapes). [The code](https://github.com/evan-liu/DrawShaps) is shared at [GitHub](https://github.com/evan-liu/DrawShaps).

## ActionBar

I want to show the user what they are drawing on the action bar. So I put a `DrawingShapeView` in [action_bar layout](https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/res/layout/action_bar.xml), and set it up in my [MainActivity class](https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/MainActivity.java#L69-L85).

```java MainActivity.setupActionBar() https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/MainActivity.java#L69-L85
View actionBarView = LayoutInflater.from(this).inflate(R.layout.action_bar, null);
((DrawingShapeView) actionBarView.findViewById(R.id.drawingShape)).setup(model, painter);
//...
ActionBar actionBar = getActionBar();
actionBar.setCustomView(actionBarView);
actionBar.setDisplayShowHomeEnabled(false);
actionBar.setDisplayShowTitleEnabled(false);
actionBar.setDisplayShowCustomEnabled(true);

```

<!-- more -->

In [setup() method](https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/view/DrawingShapeView.java#L34-L45) of [DrawingShapeView class](https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/view/DrawingShapeView.java#L34-L45) I listen to the change of color/shape from [model class](https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/model/DrawModel.java) and redraw it.

```java DrawingShapeView.setup() https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/view/DrawingShapeView.java#L34-L45
model.setOnColorChangeListener(new DrawModel.OnColorChangeListener() {
    @Override
    public void onColorChanged(int color) {
        invalidate();
    }
});
model.setOnShapeChangeListener(new DrawModel.OnShapeChangeListener() {
    @Override
    public void onShapeChanged(Shape shape) {
        invalidate();
    }
});
```

## DrawerLayout

To use `DrawerLayout` you need to add [Support Library](http://developer.android.com/tools/support-library/index.html) to `build.gradle` of the app (not the top level one).

```gradle  app/build.gradle https://github.com/evan-liu/DrawShaps/blob/master/app/build.gradle
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile "com.android.support:support-v4:20.0.0"
}
```

And make the root to be `DrawerLayout`.

```xml activity_main.xml https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/res/layout/activity_main.xml
<android.support.v4.widget.DrawerLayout
    android:id="@+id/drawer_layout"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</android.support.v4.widget.DrawerLayout>
```

The [first child](https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/res/layout/activity_main.xml#L7-L10) is the main view, others will be the drawers. I put 2 drawers, one on left and one on right.

``` xml activity_main.xml https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/res/layout/activity_main.xml
// Main view
<com.evancoding.drawshapes.view.DrawView />
// Left drawer
<LinearLayout
    android:id="@+id/toolsLayout"
    android:layout_gravity="start">
</LinearLayout>
// Right drawer
<LinearLayout
    android:id="@+id/buttonsLayout"
    android:layout_gravity="end">
</LinearLayout>
```

You can then open/close the drawers in the activity class.

``` java MainActivity.setupDrawer() https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/MainActivity.java#L115-L135
private void setupDrawer() {
    drawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
    toolsLayout = findViewById(R.id.toolsLayout);
    buttonsLayout = findViewById(R.id.buttonsLayout);
}
```

``` java MainActivity.toggleDrawer() https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/MainActivity.java#L137-L143
private void toggleDrawer(View drawer) {
    if (drawerLayout.isDrawerOpen(drawer)) {
        drawerLayout.closeDrawer(drawer);
    } else {
        drawerLayout.openDrawer(drawer);
    }
}
```

## StarDrawer

It took me a lot of time to figure out how to [draw a Star](https://github.com/evan-liu/DrawShaps/blob/master/app/src/main/java/com/evancoding/drawshapes/shapes/StarDrawer.java). You can find it and all the other code on [GitHub](https://github.com/evan-liu/DrawShaps/).
