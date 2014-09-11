PullToRefresh-PinnedSection-ListView
====================================

A merge of PullToRefreshListView and PinnedSectionListView for Android.

## Library
Android-PullToRefresh: https://github.com/chrisbanes/Android-PullToRefresh

pinned-section-listview: https://github.com/beworker/pinned-section-listview

## Environment
Eclipse Luna, Android 4.2.2

## Screenshot

<img src="https://github.com/tongcpp/PullToRefresh-PinnedSection-ListView/blob/master/screen0.png"  height="540" width="360" />
<img src="https://github.com/tongcpp/PullToRefresh-PinnedSection-ListView/blob/master/screen1.png"  height="540" width="360" />

<img src="https://github.com/tongcpp/PullToRefresh-PinnedSection-ListView/blob/master/screen2.png"  height="540" width="360" />

## 为什么做PullToRefresh-PinnedSection-ListView
前段时间因为项目需求，需要在Android中对ListView同时增加下拉刷新和分段头悬停的效果，受到[dkmeteor](https://github.com/dkmeteor)的启发，Merge了两个Github上的开源项目：

 * [Android-PullToRefresh](https://github.com/chrisbanes/Android-PullToRefresh)(handmark版,目前已不再更新)

 * [StickyListHeaders](https://github.com/emilsjolander/StickyListHeaders)(目前版本为2.x)

 由于既有项目里的StickyListHeaders代码为1.x版本，StickyListHeadersListView继承自ListView，故与handmark版的PullToRefreshListView做merge时很顺畅；

 但2.x版的StickyListHeadersListView继承自FrameLayout，与PullToRefresh的融合并不顺利，若要整理拆分出一个独立的lib时遇到很多的问题，
 故在分断头悬停需求上采用了另一个类似的开源项目：

 * [pinned-section-listview](https://github.com/beworker/pinned-section-listview)

## 我是如何做的
前面已经介绍过这个过程是“很顺畅”的：

1.Library方面，基于PullToRefresh的Library修改，首先使其依赖StickyListHeaders的Library，通过拷贝src/com/handmark/pulltorefresh/library/PullToRefreshListView.java类，
新建PullToRefreshPinnedSectionListView.java类;

2.修改PullToRefreshPinnedSectionListView类中createListView()方法，注释以下代码

    //		if (VERSION.SDK_INT >= VERSION_CODES.GINGERBREAD) {
    //			lv = new InternalListViewSDK9(context, attrs);
    //		} else {
    //			lv = new InternalListView(context, attrs);
    //		}
添加

    lv = new PinnedSectionListView(context, attrs);

3.Example方面，基于pinned-section-listview的example修改，令其依赖PullToRefresh的Library;

4.修改主类资源文件activity_main.xml,设置List组件为新类：


    <com.handmark.pulltorefresh.library.PullToRefreshPinnedSectionListView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/list"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:headerDividersEnabled="false"
    android:footerDividersEnabled="false"
    android:divider="@null"
    />

5.改写SampleActivity.java类中getListView()方法：

    mpPullToRefreshPinnedSectionListView = (PullToRefreshPinnedSectionListView) findViewById(R.id.list);
    return mpPullToRefreshPinnedSectionListView.getRefreshableView();

即可通过

    ListView list = getListView();

继续进行原example其他操作，详情可阅读[项目代码](https://github.com/tongcpp/PullToRefresh-PinnedSection-ListView/blob/master/example/src/com/hb/examples/SampleActivity.java)

## 另一种实现方式
本例的实现方式依赖于handmark版下拉刷新组件的灵活性，更重要的一点，要求分段头悬停组件是继承自ListView实现；
故同理也可用handmark版下拉刷新组件和1.x版的StickyListHeaders组件实现；

另一种实现方式为[pull-to-refresh-sticky-list](https://github.com/dkmeteor/pull-to-refresh-sticky-list)，
其采取合并的是2.x版的StickyListHeaders和johannilsson的android-pulltorefresh，实现形式不同，但效果类似，看过代码后实现起来也“相当顺畅”，有兴趣的同学可以参照此项目；

## 建议
建议在熟悉或使用过原有组件类库的前提下使用本类库。
