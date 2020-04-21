ConsecutiveScrollerLayout是Android下支持多个滑动布局(RecyclerView、WebView、ScrollView等)和lView等)和普通控件(TextView、ImageView、LinearLayou、自定义View等)持续连贯滑动的容器,它使所有的子View像一个整体一样连续顺畅滑动。并且支持布局吸顶功能。

#### 效果图

![sample](https://upload-images.jianshu.io/upload_images/2365010-1d0ebf289428ce8c.gif?imageMogr2/auto-orient/strip) 
![sticky](https://upload-images.jianshu.io/upload_images/2365010-f2b64d20022d2566.gif?imageMogr2/auto-orient/strip)

### 引入依赖

在Project的build.gradle在添加以下代码

```groovy
allprojects {
      repositories {
         ...
         maven { url 'https://jitpack.io' }
      }
   }
```
在Module的build.gradle在添加以下代码
```groovy
// 使用了Androidx
implementation 'com.github.donkingliang:ConsecutiveScroller:2.4.0'

// 或者

// 使用Android support包
implementation 'com.github.donkingliang:ConsecutiveScroller:1.4.0'
```
由于Androidx和Android support包不兼容，所以ConsecutiveScroller使用两个版本分别支持使用Androidx和使用Android support包的项目。
大版本号1使用Android support包，大版本号2使用Androidx。

### 基本使用

ConsecutiveScrollerLayout的使用非常简单，把需要滑动的布局作为ConsecutiveScrollerLayout的直接子View即可。

```xml
<?xml version="1.0" encoding="utf-8"?>
<com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/scrollerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:scrollbars="vertical">

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:background="@android:color/holo_red_dark"
        android:gravity="center"
        android:orientation="vertical">

    </LinearLayout>

    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </androidx.core.widget.NestedScrollView>

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:scaleType="fitXY"
        android:src="@drawable/temp" />

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </ScrollView>

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

</com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout>
```

### 布局吸顶

要实现布局的吸顶效果，在以前，我们可能会写两个一摸一样的布局，一个隐藏在顶部，一个嵌套在ScrollView下，通过监听ScrollView的滑动来显示和隐藏顶部的布局。这种方式实现起来既麻烦也不优雅。ConsecutiveScrollerLayout内部实现了子View吸附顶部的功能，只要设置一个属性，就可以实现吸顶功能。而且支持设置多个子View吸顶，后面的View要吸顶时会把前面的吸顶View推出屏幕。

```xml
<?xml version="1.0" encoding="utf-8"?>
<com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/scrollerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:scrollbars="vertical">

  <!-- 设置app:layout_isSticky="true"就可以使View吸顶 -->
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:color/white"
        android:padding="10dp"
        android:text="吸顶View - 1"
        android:textColor="@android:color/black"
        android:textSize="18sp"
        app:layout_isSticky="true" />

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:color/white"
        android:orientation="vertical"
        app:layout_isSticky="true">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="10dp"
            android:text="吸顶View - 2 我是个LinearLayout"
            android:textColor="@android:color/black"
            android:textSize="18sp" />

    </LinearLayout>

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:color/white"
        android:padding="10dp"
        android:text="吸顶View - 3"
        android:textColor="@android:color/black"
        android:textSize="18sp"
        app:layout_isSticky="true" />

    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </androidx.core.widget.NestedScrollView>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:color/white"
        android:padding="10dp"
        android:text="吸顶View - 4"
        android:textColor="@android:color/black"
        android:textSize="18sp"
        app:layout_isSticky="true" />

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

</com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout>
```

**注意：**

1、吸顶功能使用了Android 5.0之后才有的API:setTranslationZ()，所有吸顶功能不支持5.0以前的手机。

2、由于吸顶功能需要通过设置View的z来时吸顶View显示在所有View的上面，所以使用者不要给View设置z或者elevation。

3、对于一些View,如果它显示在吸顶View的前面，把吸顶View重叠覆盖了，是因为它的z比吸顶View的z大，你可以把它的elevation设置为0，或者给吸顶的View设置一个大点的elevation值。

### 局部滑动

ConsecutiveScrollerLayout将所有的子View视作一个整体，由它统一处理页面的滑动事件，所以它默认会拦截可滑动的子View的滑动事件，由自己来分发处理。并且会追踪用户的手指滑动事件，计算调整ConsecutiveScrollerLayout滑动偏移。如果你希望某个子View自己处理自己的滑动事件，可以通过设置layout_isConsecutive属性来告诉父View不要拦截它的滑动事件，这样就可以实现在这个View自己的高度内滑动自己的内容，而不会作为ConsecutiveScrollerLayout的一部分来处理。

```xml
<?xml version="1.0" encoding="utf-8"?>
<com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/scrollerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:scrollbars="vertical">
  
<!--设置app:layout_isConsecutive="false"使父布局不拦截滑动事件-->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_isConsecutive="false">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="10dp"
            android:text="下面的红色区域是个RecyclerView，它在自己的范围内单独滑动。"
            android:textColor="@android:color/black"
            android:textSize="18sp"
            app:layout_isSticky="true" />

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView1"
            android:layout_width="match_parent"
            android:layout_height="300dp"
            android:layout_marginLeft="50dp"
            android:layout_marginRight="50dp"
            android:layout_marginBottom="30dp"
            android:background="@android:color/holo_red_dark"/>

    </LinearLayout>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:text="下面的是个NestedScrollView，它在自己的范围内单独滑动。"
        android:textColor="@android:color/black"
        android:textSize="18sp" />

    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="250dp"
        app:layout_isConsecutive="false">

    </androidx.core.widget.NestedScrollView>

</com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout>
```

在这个例子中NestedScrollView希望在自己的高度里滑动自己的内容，而不是跟随ConsecutiveScrollerLayout滑动，只要给它设置layout_isConsecutive="false"就可以了。而LinearLayout虽然不是滑动布局，但是在下面嵌套了个滑动布局RecyclerView，所以它也需要设置layout_isConsecutive="false"。

ConsecutiveScrollerLayout支持NestedScrolling机制，如果你的局部滑动的view实现了NestedScrollingChild接口(如：RecyclerView、NestedScrollView等)，它滑动完成后会把滑动事件交给父布局。如果你不想你的子view或它的下级view与父布局嵌套滑动，可以给子view设置app:layout_isNestedScroll="false"。它可以禁止子view与ConsecutiveScrollerLayout的嵌套滑动

### 滑动子view的下级view

ConsecutiveScrollerLayout默认情况下只会处理它的直接子view的滑动，但有时候需要滑动的布局可能不是ConsecutiveScrollerLayout的直接子view，而是子view所嵌套的下级view。比如ConsecutiveScrollerLayout嵌套FrameLayout,FrameLayout嵌套ScrollView，我们希望ConsecutiveScrollerLayout也能正常处理ScrollView的滑动。为了支持这种需求，ConsecutiveScroller提供了一个接口：IConsecutiveScroller。子view实现IConsecutiveScroller接口，并通过实现接口方法告诉ConsecutiveScrollerLayout需要滑动的下级view,ConsecutiveScrollerLayout就能正确地处理它的滑动事件。IConsecutiveScroller需要实现两个方法：
```java
    /**
     * 返回当前需要滑动的下级view。在一个时间点里只能有一个view可以滑动。
     */
    View getCurrentScrollerView();

    /**
     * 返回所有可以滑动的子view。由于ConsecutiveScrollerLayout允许它的子view包含多个可滑动的子view，所以返回一个view列表。
     */
    List<View> getScrolledViews();
```
在前面提到的例子中，我们可以这样实现：
```java
public class MyFrameLayout extends FrameLayout implements IConsecutiveScroller {

    @Override
    public View getCurrentScrollerView() {
        // 返回需要滑动的ScrollView
        return getChildAt(0);
    }

    @Override
    public List<View> getScrolledViews() {
        // 返回需要滑动的ScrollView
        List<View> views = new ArrayList<>();
        views.add(getChildAt(0));
        return views;
    }
}
```
```xml
<?xml version="1.0" encoding="utf-8"?>
<com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/scrollerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:scrollbars="vertical">

    <com.donkingliang.consecutivescrollerdemo.widget.MyFrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <ScrollView
            android:layout_width="match_parent"
            android:layout_height="match_parent">
            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="match_parent">

            </LinearLayout>
        </ScrollView>
    </com.donkingliang.consecutivescrollerdemo.widget.MyFrameLayout>
</com.donkingliang.consecutivescroller.ConsecutiveScrollerLayout>
```
这样ConsecutiveScrollerLayout就能正确地处理ScrollView的滑动。这是一个简单的例子，在实际的需求中，我们一般不需要这样做。

**注意：** getCurrentScrollerView()和getScrolledViews()必须正确地返回需要滑动的view，这些view可以是经过多层嵌套的，不一定是直接子view。所以使用者应该按照自己的实际场景去实现者两个方法。

#### 对ViewPager的支持
IConsecutiveScroller的一个常用的场景是对ViewPager的支持。ViewPager是左右滑动的控件，但是我们一般会在ViewPager下嵌套RecyclerView等列表布局。为了能让ConsecutiveScrollerLayout正确地滑动ViewPager下的RecyclerView，使RecyclerView与ConsecutiveScrollerLayout形成一个滑动整体。需要让ViewPager实现IConsecutiveScroller接口，并返回需要滑动的RecyclerView。
```java
	public class MyViewPager extends ViewPager implements IConsecutiveScroller {

    /**
     * 返回当前需要滑动的view。
     */
    @Override
    public View getCurrentScrollerView() {
        int count = getChildCount();
        for (int i = 0; i < count; i++) {
            View view = getChildAt(i);
            if (view.getX() == getScrollX()) {
                return view;
            }
        }
        return this;
    }

    /**
     * 返回全部需要滑动的下级view
     */
    @Override
    public List<View> getScrolledViews() {
        List<View> views = new ArrayList<>();
        int count = getChildCount();
        if (count > 0) {
            for (int i = 0; i < count; i++) {
                views.add(getChildAt(i));
            }
        } else {
            views.add(this);
        }
        return views;
    }
}
```
我在demo中提供了ViewPager实现IConsecutiveScroller的例子和效果，有兴趣的朋友可以去体验一下。

**重要：** 再提醒一次，我在这里提供的例子并不是通用的，使用者应该按照自己的实际场景去实现者两个方法。

### 使用腾讯x5的WebView
由于腾讯x5的VebView是一个FrameLayout嵌套WebView的布局，而不是一个WebView的子类，所以要在ConsecutiveScrollerLayout里使用它，需要把它的滑动交给它里面的WebView。自定义MyWebView继承腾讯的WebView,重写它的scrollBy()方法即可。
```java
public class MyWebView extends com.tencent.smtt.sdk.WebView {

    @Override
    public void scrollBy(int x, int y) {
       // 把滑动交给它的子view
        getView().scrollBy(x, y);
    }
}
```
通过实现IConsecutiveScroller接口同样可以实现对x5的WebView支持。
```java
public class MyWebView extends com.tencent.smtt.sdk.WebView {

    @Override
    public View getCurrentScrollerView() {
        return getView();
    }

    @Override
    public List<View> getScrolledViews() {
        List<View> views = new ArrayList<>();
        views.add(getView());
        return views;
    }
   
}
```
另外需要隐藏它的子view的滚动条
```java
View view = webView.getView();
view.setVerticalScrollBarEnabled(false);
view.setHorizontalScrollBarEnabled(false);
view.setOverScrollMode(OVER_SCROLL_NEVER);
```

### 其他注意事项

1、WebView在加载的过程中如果滑动的布局，可能会导致WebView与其他View在显示上断层，使用下面的方法一定程度上可以避免这个问题。

```java
webView.setWebChromeClient(new WebChromeClient() {
            @Override
            public void onProgressChanged(WebView view, int newProgress) {
                super.onProgressChanged(view, newProgress);
                scrollerLayout.checkLayoutChange();
            }
        });
```

2、继承AbsListView的布局(ListView、GridView等)，在滑动上可能会与用户的手指滑动不同步，推荐使用RecyclerView代替。

3、ConsecutiveScrollerLayout的子View不支持margin，子View间的间距可以通过Space设置。

4、使用ConsecutiveScrollerLayout提供的setOnVerticalScrollChangeListener()方法监听布局的滑动事件。View所提供的setOnScrollChangeListener()方法已无效。

5、通过getOwnScrollY()方法获取ConsecutiveScrollerLayout的垂直滑动距离，View的getScrollY()方法获取的不是ConsecutiveScrollerLayout的整体滑动距离。
