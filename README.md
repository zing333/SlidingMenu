SlidingMenu ([Play Store Demo][7])
===========

SlidingMenu is an Open Source Android library that allows developers to easily create applications 
with sliding menus like those made popular in the Google+, YouTube, and Facebook apps. Feel free 
to use it all you want in your Android apps provided that you cite this project and include the license in your app.

SlidingMenu is currently used in some awesome Android apps. Here's a list of some of them: 
* [Foursquare][15]
* [LinkedIn][19]
* [Zappos][20]
* [Rdio][8]
* [Evernote Food][18]
* [Plume][4]
* [VLC for Android][5]
* [ESPN ScoreCenter][14]
* [MLS MatchDay][16]
* [9GAG][17]
* [Wunderlist 2][13]
* [The Verge][6]
* [MTG Familiar][9]
* [Mantano Reader][10]
* [Falcon Pro (BETA)][12]
* [MW3 Barracks][11]

If you are using SlidingMenu in your app and would like to be listed here, please let me know via [Twitter][1]!

Here's an older video of the example application in this repository : http://youtu.be/8vNaANLHw-c

Also, you can follow the project on Twitter : [@SlidingMenu][1]

Setup
-----
* In Eclipse, just import the library as an Android library project. Project > Clean to generate the binaries 
you need, like R.java, etc.
* Then, just add SlidingMenu as a dependency to your existing project and you're good to go!

Setup with ActionBarSherlock
----------------------------
* Setup as above.
* Checkout a clean copy of [ActionBarSherlock][2] and import into your Eclipse workspace.
* Add ActionBarSherlock as a dependency to SlidingMenu
* Go into the SlidingActivities that you plan on using make them extend Sherlock___Activity instead of ___Activity. 

How to Integrate this Library into Your Projects
------------------------------------------------
In order to integrate SlidingMenu into your own projects you can do one of two things.

__1.__      You can wrap your Activities in a SlidingMenu by constructing it programmatically (`new SlidingMenu(Context context)`)
and then calling `SlidingMenu.attachToActivity(Activity activity, SlidingMenu.SLIDING_WINDOW | SlidingMenu.SLIDING_CONTENT)`.
`SLIDING_WINDOW` will include the Title/ActionBar in the content section of the SlidingMenu, while `SLIDING_CONTENT`
does not. You can check it out in the example app AttachExample Activity.

__2.__      You can embed the SlidingMenu at the Activity level by making your Activity extend `SlidingActivity`.
* In your Activity's onCreate method, you will have to call `setContentView`, as usual, and also 
`setBehindContentView`, which has the same syntax as setContentView. `setBehindContentView` will place 
the view in the "behind" portion of the SlidingMenu. You will have access to the `getSlidingMenu` method so you can
customize the SlidingMenu to your liking.
* If you want to use another library such as ActionBarSherlock, you can just change the SlidingActivities to extend
the SherlockActivities instead of the regular Activities.

__3.__      You can use the SlidingMenu view directly in your xml layouts or programmatically in your Java code.
* This way, you can treat SlidingMenu as you would any other view type and put it in crazy awesome places like in the
rows of a ListView.
* So. Many. Possibilities.

Simple Example
-----
```java
public class SlidingExample extends Activity {

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setTitle(R.string.attach);
		// set the content view
		setContentView(R.layout.content);
		// configure the SlidingMenu
		SlidingMenu menu = new SlidingMenu(this);
        menu.setMode(SlidingMenu.LEFT);
		menu.setTouchModeAbove(SlidingMenu.TOUCHMODE_FULLSCREEN);
		menu.setShadowWidthRes(R.dimen.shadow_width);
		menu.setShadowDrawable(R.drawable.shadow);
		menu.setBehindOffsetRes(R.dimen.slidingmenu_offset);
		menu.setFadeDegree(0.35f);
		menu.attachToActivity(this, SlidingMenu.SLIDING_CONTENT);
		menu.setMenu(R.layout.menu);
	}
    
}
```

XML Usage
-----
If you decide to use SlidingMenu as a view, you can define it in your xml layouts like this:
```xml
<com.jeremyfeinstein.slidingmenu.lib.SlidingMenu
    xmlns:sliding="http://schemas.android.com/apk/res-auto"
    android:id="@+id/slidingmenulayout"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    sliding:viewAbove="@layout/YOUR_ABOVE_VIEW"
    sliding:viewBehind="@layout/YOUR_BEHIND_BEHIND"
    sliding:touchModeAbove="margin|fullscreen"
    sliding:behindOffset="@dimen/YOUR_OFFSET"
    sliding:behindWidth="@dimen/YOUR_WIDTH"
    sliding:behindScrollScale="@dimen/YOUR_SCALE"
    sliding:shadowDrawable="@drawable/YOUR_SHADOW"
    sliding:shadowWidth="@dimen/YOUR_SHADOW_WIDTH"
    sliding:fadeEnabled="true|false"
    sliding:fadeDegree="float"
    sliding:selectorEnabled="true|false"
    sliding:selectorDrawable="@drawable/YOUR_SELECTOR"/>
```
NOTE : you cannot use both behindOffset and behindWidth. You will get an exception if you try.
* `viewAbove` - a reference to the layout that you want to use as the above view of the SlidingMenu
* `viewBehind` - a reference to the layout that you want to use as the behind view of the SlidingMenu
* `touchModeAbove` - an enum that designates what part of the screen is touchable when the above view is 
showing. Margin means only the left margin. Fullscreen means the entire screen. Default is margin.
* `behindOffset` - a dimension representing the number of pixels that you want the above view to show when the
behind view is showing. Default is 0.
* `behindWidth` - a dimension representing the width of the behind view. Default is the width of the screen
(equivalent to behindOffset = 0).
* `behindScrollScale` - a float representing the relationship between the above view scrolling and the behind
behind view scrolling. If set to 0.5f, the behind view will scroll 1px for every 2px that the above view scrolls.
If set to 1.0f, the behind view will scroll 1px for every 1px that the above view scrolls. And if set to 0.0f, the
behind view will never scroll; it will be static. This one is fun to play around with. Default is 0.25f.
* `shadowDrawable` - a reference to a drawable to be used as a drop shadow from the above view onto the below view.
Default is no shadow for now.
* `shadowWidth` - a dimension representing the width of the shadow drawable. Default is 0.
* `fadeEnabled` - a boolean representing whether or not the behind view should fade when the SlidingMenu is closing
and "un-fade" when opening
* `fadeDegree` - a float representing the "amount" of fade. `1.0f` would mean fade all the way to black when the
SlidingMenu is closed. `0.0f` would mean do not fade at all.
* `selectorEnabled` - a boolean representing whether or not a selector should be drawn on the left side of the above
view showing a selected view on the behind view.
* `selectorDrawable` - a reference to a drawable to be used as the selector
NOTE : in order to have the selector drawn, you must call SlidingMenu.setSelectedView(View v) with the selected view.
Note that this will most likely not work with items in a ListView because of the way that Android recycles item views.

Caveats
-------
* Your layouts have to be based on a viewgroup, unfortunatly this negates the `<merge>` optimisations.
            

Developed By
------------
* Jeremy Feinstein

License
-------

    Copyright 2012-2014 Jeremy Feinstein
    
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
    
    http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
    
[1]: http://twitter.com/slidingmenu
[2]: http://actionbarsherlock.com/
[3]: https://play.google.com/store/apps/details?id=com.zappos.android&hl=en
[4]: https://play.google.com/store/apps/details?id=com.levelup.touiteur&hl=en
[5]: https://play.google.com/store/apps/details?id=org.videolan.vlc.betav7neon
[6]: https://play.google.com/store/apps/details?id=com.verge.android
[7]: http://bit.ly/TWejze
[8]: https://play.google.com/store/apps/details?id=com.rdio.android.ui
[9]: https://play.google.com/store/apps/details?id=com.gelakinetic.mtgfam
[10]: https://play.google.com/store/apps/details?id=com.mantano.reader.android
[11]: https://play.google.com/store/apps/details?id=com.phonegap.MW3BarracksFree
[12]: http://forum.xda-developers.com/showthread.php?p=34361296
[13]: http://bit.ly/xs1sMN
[14]: https://play.google.com/store/apps/details?id=com.espn.score_center
[15]: https://play.google.com/store/apps/details?id=com.joelapenna.foursquared
[16]: https://play.google.com/store/apps/details?id=com.mlssoccer
[17]: https://play.google.com/store/apps/details?id=com.ninegag.android.app
[18]: https://play.google.com/store/apps/details?id=com.evernote.food
[19]: https://play.google.com/store/apps/details?id=com.linkedin.android
[20]: https://play.google.com/store/apps/details?id=com.zappos.android
[21]: https://www.cnblogs.com/samchen2009/p/3294713.html
<!DOCTYPE NETSCAPE-Bookmark-file-1>
<!-- This is an automatically generated file.
     It will be read and overwritten.
     DO NOT EDIT! -->
<META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">
<TITLE>Bookmarks</TITLE>
<H1>Bookmarks</H1>
<DL><p>
    <DT><H3 ADD_DATE="1541377221" LAST_MODIFIED="1542785224" PERSONAL_TOOLBAR_FOLDER="true">书签栏</H3>
    <DL><p>
        <DT><A HREF="https://blog.csdn.net/jxf_access/article/details/79564669" ADD_DATE="1541379143" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">CoordinatorLayout使用详解: 打造折叠悬浮效果 - jxf_access的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/briblue/article/details/77075198" ADD_DATE="1541379532" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">细说 AppbarLayout,如何理解可折叠 Toolbar 的定制 - frank 的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/andoop/article/details/52795255?locationNum=4&fps=1" ADD_DATE="1541385995" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">RxJava和RxAndroid使用详解 - andoop的博客 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/idaretobe/article/details/53786310" ADD_DATE="1541385999" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">RxJava简介及在androidstudio中引入RxAndroid - idaretobe的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/xu_song/article/details/78686439" ADD_DATE="1541386613" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">RxJava(RxAndroid)基本使用入门 - xu_song的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/dengpeng_/article/details/54910840" ADD_DATE="1541571070" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">android之ExoPlayer探索 - dengpeng_的博客 - CSDN博客</A>
        <DT><A HREF="http://google.github.io/ExoPlayer/doc/reference/" ADD_DATE="1541571651">com.google.android.exoplayer2.extractor.mp4 (ExoPlayer library)</A>
        <DT><A HREF="https://www.jianshu.com/p/cb54990219d9" ADD_DATE="1541755133" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACz0lEQVQ4jV2ST2hcZRTFf+f7vvfezOuk0cTWaYtVUEGyEhV0oaBQ0uJCpV1lUwuCBSMoLQiiIoJ/KNiCi1hdxC4UBP/gUtCNiOBG3XRRin9qaJPUUs1Mp0ln3nvfd11MI8WzvNx77uGco5VDe/bnLj9Rp3RbAoiNzHlJYhOWEmDIeQPMSWTS+SpVR4JZWADrRkjCpHZH1COsqsbX3qMsB+ewaxsSRjQsK/LbjbDgJLpDw9Kg7/KZ+7Tj5JcW7rgbTWwldHdhdc3Ui28wffQtlOfmpm+V39Z1w2Qm6Gr50L5ksZGfmLTpl9/FYiM/vZ1meYnqz1+xqwO27N1Pfe4s2Z330Px2xuqV81z5/CNZXZsjRchypl46RnNpVZdeOUz1x1mGp39i8NUntB+ZZe3k2/x9/DVsY52r33+j3uJxLCXkHbpwcNbULnFlh9a9D9J+6FHWv/uaiX0HMAlCIP1zGdcuIcuRHP1PP2D4y4+4dkmQc9jGOnE0pL5wjrL1OMpy+p8t4soOFhsUAmk0RHJMvfA6bnIKixGTCCaRRtfo7HmSyYPPs/rcASbnDtOZfYo46CM53JYO1e9n6J16b5yOJSSQGUESMgMYxxUjYftOLEXW3n8HUsPN86+S3TWDihZyjv8qIhEwAwkzGxfGedJwg/yWGW565ghgZDt30/y1AhJcf7aJMB6CpOsLCdcqaZaX6H14DARTR99EeQEpYjdymBEwM4QAI0YhEQd9wq7dbJ17FuUFKtr0P14gXemPlbtgmAnJwpjIkA+4PEd5QW/xBK7dRnkLgDTo4bftoPPEHC5kpMEaOD9WIJCct2btstZ/+BaqkVHXSk0FqTf2p6kpH7uf8oGHrf/FKQ1P/ywVLZOZtPz03tUi+O6wiYm6ElkmyfF/WNOAgKoyipa1suBGTbropDiPseQlVLRMyOxGpzcjDgE5byo75r0HY0lq5v8FS/1LVvsjW/AAAAAASUVORK5CYII=">仿网易云音乐播放界面 - 简书</A>
        <DT><A HREF="https://blog.csdn.net/marshal_zsx/article/details/81012137" ADD_DATE="1541983352" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">AndroidO audio系统之框架简介（一） - marshal_zsx的博客 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/yus201120/article/details/81774356" ADD_DATE="1541985015" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">Android AudioFocus音频焦点机制学习和理解 - 天花板的随笔 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/yzying1980/article/details/83890424" ADD_DATE="1542069488" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">开源License及分类 - yzying1980的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/liucheng61214/article/details/80378705" ADD_DATE="1542251664" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">UniversalMusicPlayer 解析一看就懂 - liucheng61214的专栏 - CSDN博客</A>
        <DT><A HREF="https://www.jianshu.com/p/29e5e8c75450" ADD_DATE="1542272622" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACz0lEQVQ4jV2ST2hcZRTFf+f7vvfezOuk0cTWaYtVUEGyEhV0oaBQ0uJCpV1lUwuCBSMoLQiiIoJ/KNiCi1hdxC4UBP/gUtCNiOBG3XRRin9qaJPUUs1Mp0ln3nvfd11MI8WzvNx77uGco5VDe/bnLj9Rp3RbAoiNzHlJYhOWEmDIeQPMSWTS+SpVR4JZWADrRkjCpHZH1COsqsbX3qMsB+ewaxsSRjQsK/LbjbDgJLpDw9Kg7/KZ+7Tj5JcW7rgbTWwldHdhdc3Ui28wffQtlOfmpm+V39Z1w2Qm6Gr50L5ksZGfmLTpl9/FYiM/vZ1meYnqz1+xqwO27N1Pfe4s2Z330Px2xuqV81z5/CNZXZsjRchypl46RnNpVZdeOUz1x1mGp39i8NUntB+ZZe3k2/x9/DVsY52r33+j3uJxLCXkHbpwcNbULnFlh9a9D9J+6FHWv/uaiX0HMAlCIP1zGdcuIcuRHP1PP2D4y4+4dkmQc9jGOnE0pL5wjrL1OMpy+p8t4soOFhsUAmk0RHJMvfA6bnIKixGTCCaRRtfo7HmSyYPPs/rcASbnDtOZfYo46CM53JYO1e9n6J16b5yOJSSQGUESMgMYxxUjYftOLEXW3n8HUsPN86+S3TWDihZyjv8qIhEwAwkzGxfGedJwg/yWGW565ghgZDt30/y1AhJcf7aJMB6CpOsLCdcqaZaX6H14DARTR99EeQEpYjdymBEwM4QAI0YhEQd9wq7dbJ17FuUFKtr0P14gXemPlbtgmAnJwpjIkA+4PEd5QW/xBK7dRnkLgDTo4bftoPPEHC5kpMEaOD9WIJCct2btstZ/+BaqkVHXSk0FqTf2p6kpH7uf8oGHrf/FKQ1P/ywVLZOZtPz03tUi+O6wiYm6ElkmyfF/WNOAgKoyipa1suBGTbropDiPseQlVLRMyOxGpzcjDgE5byo75r0HY0lq5v8FS/1LVvsjW/AAAAAASUVORK5CYII=">Android Room Orm框架学习 - 简书</A>
        <DT><A HREF="https://blog.csdn.net/xinzhou201/article/details/80881604" ADD_DATE="1542348209" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">GMTC分享——当插件化遇到 Android P - xinzhou201的专栏 - CSDN博客</A>
        <DT><A HREF="https://zhidao.baidu.com/question/2208010473387676188.html" ADD_DATE="1542356720" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAACP0lEQVQ4jVWSS0jUYRTFf983M5qmMw4+UYsS0zAKKjdimkHgogcRNIGryijJRZs2UW1sWRIJQZBBbazcRGZCZA9FKoOCGlTMkbSHOj4QZ3TGceb/3RbzN+iuLpfDPffcc5QxIoLWGAFBa5JljN2LIIJS9lwrhVLIOvrhR5oe8XMRrQnM8uwrRlAKsfE4kwssg9KMTtP6mpklcjNo3s+5DoamuVzPxQM2iQj2BS4nTs1MCCMUeFiK0DvCr0WKvfSOEk+AILLOoBQPPhCNU19BVhr+KXYW4U5jeY3IGg2VpLhsVUrhVIr+MVp6iMQRaDvBQICz1awlOL+PNCfVJVzvoaaUujIsgxKRO320vSMjlfJ8Ok4DDE2RvZECD4Cvnb4xCj10NlKahxahLI9Ygt+LVBQgwoXH+O5z9C4v/FiGYJiSHKIJxudRCqcIB7dz4xjzKzTVcPstL4fJcxOJcaWLLTmU5tL1jUIPW7MRQf0zDmjt5epzWg4TXePee7zpZKbScoRgiOIsardhDMoyghBLcOkpgVlOVbG7mL7vjAQZnOTQDgYnuHmc8nwSFg6NNoLWdH6h20+bj1cjfJokz01dGaEoDZWEVvG1M7GAQyeNE0Tw/8G9gRQHc2EcihQH3nR2FWEZMlMJxxieRilM0mml2LOZqSWuddNYTU4GTz4zMM6ZKgLzzC2jFUVZICiFMkaA1Ti33tAfwOXAm84mLysxfiwQDJPi4ORemmvtFP73JcsAONYTblksRnFpPOl2kIC/BsEA9PPta4oAAAAASUVORK5CYII=">chrome怎么显示菜单栏？_百度知道</A>
        <DT><A HREF="https://blog.csdn.net/songjinghao/article/details/72676635" ADD_DATE="1542592174" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">SystemUI 7.0学习总结一-SystemUI的启动 - songjinghao的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/andyhuabing/article/details/12851825" ADD_DATE="1542594492" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">android SystemUI 流程分析 - andyhuabing的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/andyhuabing/article/details/7349986" ADD_DATE="1542596013" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">android启动--深入理解zygote - andyhuabing的专栏 - CSDN博客</A>
        <DT><A HREF="https://blog.csdn.net/Innost/article/details/47660591" ADD_DATE="1542597922" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAABWklEQVQ4jbVSwUoCURQ9V2fGmUwxagLNwAiDQaHc+AFu/Qt3FtRnuLf+oMCN4GL20c6FEhiU4UKDIFHLJipwGvW2GBUHqXDRhQvvXs55953zLlVBWCZcS6EBCM6S7SRADIWk8DYpCg+tUb9vtlo8MBcJUBKJzWzWn06LweB8f/Rm1NbW5wnsUpRwPr+RyYAWVbH11HY8iQRhV9d9qZRdGqVSv1AY3NXHH+8kK+JWyO3zOwjq4dEM/Xhy3D09ozn3zEZjdibbVu2mpsTjAMxm8zYaZWb6we6JrbKmzS5j5l++ZkIYGwYAEGRNI0HgPwmvxSIAAnkikZ2LcykcZjDADADskj0MdmhwBwJ7V5cr+wcAmBnMX60H67lHgiCqqtXp3CeTsFVVQVVQBbj2rrZzOav/Ml4IQ9crUyRNl286UZK8yaQciwmqSm73eGAOe93PcnlQr9sT6N+39RsgVo7oiKSelAAAAABJRU5ErkJggg==">《深入理解Android 卷III》第七章 深入理解SystemUI - Innost的专栏 - CSDN博客</A>
        <DT><A HREF="http://adbdriver.com/downloads/" ADD_DATE="1542614876" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACcklEQVQ4ja1TO09UQRg938x9sNdl2ReLEl6CvAoTURMTCkNPYzCg0cLCmBgSGyoLNbcxdlpZ+AdMXClQEwpNABOzhcFnQQEEFFeWDYvsLsvucvfe+1nIRQWCjV81MznnfGfOfAMcUHGGZB6UB2H2Jybgux8PhL39jXHo8URDeD+ssueEQUOE0ouZwOXp1fagLkJYyy+rn9MzD/8pwDypEPW5ieSJfk023w34OSaEBv9WDU7G6kLMEzenMCX60OcQEQMA7VY8ZcK4d6ljfiWJI7F6Z0vTJS3OFUVTS62yyR/6z3Vi/E88MTMREScSk12RUNM11paaXXX2vAPLUYRKtu1CSHakQtIpRz+p1uk3mbXkRG/v2TFmJmXbBcdijUfb2lpHKpVmpJePO4qmIpVKoUrTiISUuVwend2tPTVBo0f7KgSAMQBCAfDrLkSWY4Oz2XVrdX1RUVWV8oUMXH8153I5VCoVrKRl2e/v1oio4MW240CTmiaVAoXCh/SV5aArWKKxvo6y2SyC1dVQVc01DEOXSpkANrwIFC9NSDX5bPrC2yrZHmipO3PMdSrCFpINH7MUAo5tU6pYWHr3fjTToF39uC3g7n4FuvMYAy2NGHUd2FJCCgFyHFi6AW1hHo9uX8QwANcjCG8xGIcEGKHA4TI7kQKcCMONluFG864dJbscKfl9tSWA2WRzh7czSE+H4ACEXBGvNtc6uoSemrYqeF6xxMtAjfuEhDqQ+qK/BohM2seBV+YQrFtXZr/7DOi6ZtuCNjPBMMlwpJx+MJIsmebf+L1/wRswK3pdl/bCVuHbbOFHdHjD3ZgDijDN393/S/0El038AESU4fQAAAAASUVORK5CYII=">Downloads - ADB Driver Installer</A>
        <DT><A HREF="https://www.jianshu.com/p/05a553c1ba95" ADD_DATE="1542684672" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACz0lEQVQ4jV2ST2hcZRTFf+f7vvfezOuk0cTWaYtVUEGyEhV0oaBQ0uJCpV1lUwuCBSMoLQiiIoJ/KNiCi1hdxC4UBP/gUtCNiOBG3XRRin9qaJPUUs1Mp0ln3nvfd11MI8WzvNx77uGco5VDe/bnLj9Rp3RbAoiNzHlJYhOWEmDIeQPMSWTS+SpVR4JZWADrRkjCpHZH1COsqsbX3qMsB+ewaxsSRjQsK/LbjbDgJLpDw9Kg7/KZ+7Tj5JcW7rgbTWwldHdhdc3Ui28wffQtlOfmpm+V39Z1w2Qm6Gr50L5ksZGfmLTpl9/FYiM/vZ1meYnqz1+xqwO27N1Pfe4s2Z330Px2xuqV81z5/CNZXZsjRchypl46RnNpVZdeOUz1x1mGp39i8NUntB+ZZe3k2/x9/DVsY52r33+j3uJxLCXkHbpwcNbULnFlh9a9D9J+6FHWv/uaiX0HMAlCIP1zGdcuIcuRHP1PP2D4y4+4dkmQc9jGOnE0pL5wjrL1OMpy+p8t4soOFhsUAmk0RHJMvfA6bnIKixGTCCaRRtfo7HmSyYPPs/rcASbnDtOZfYo46CM53JYO1e9n6J16b5yOJSSQGUESMgMYxxUjYftOLEXW3n8HUsPN86+S3TWDihZyjv8qIhEwAwkzGxfGedJwg/yWGW565ghgZDt30/y1AhJcf7aJMB6CpOsLCdcqaZaX6H14DARTR99EeQEpYjdymBEwM4QAI0YhEQd9wq7dbJ17FuUFKtr0P14gXemPlbtgmAnJwpjIkA+4PEd5QW/xBK7dRnkLgDTo4bftoPPEHC5kpMEaOD9WIJCct2btstZ/+BaqkVHXSk0FqTf2p6kpH7uf8oGHrf/FKQ1P/ywVLZOZtPz03tUi+O6wiYm6ElkmyfF/WNOAgKoyipa1suBGTbropDiPseQlVLRMyOxGpzcjDgE5byo75r0HY0lq5v8FS/1LVvsjW/AAAAAASUVORK5CYII=">Android 7.1.2(Android N) SystemUI--Recents Task... - 简书</A>
        <DT><A HREF="http://www.apkbus.com/thread-603886-1-1.html" ADD_DATE="1542685288" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAACbklEQVQ4jQXBW24cRRQG4P+cOt3V03OzjSWIUECgIJ4SCeUJiRUg8coKWBJLYANsAHGTwBJynDgxIk5iMpk4c7GHuXRPd1VX1eH76Lsfvy7M0MaB96qkKdsnrWwKg2TzKitkFEy+9DsdMbhL6qSPsXH2/dFdU1gyTHnndVuYSNtuPDjgaF3i94rsv7hZNq8TnPR876Pjz7756tuhHEdVRbcJ87eLl3fHHxwWhzlKNn2ntKf9Dz9/P9lcMIeGfTPmvAwyDL0iyOrNzeOTs7Pzs+1uk0smREaVNDKImUR5p6kSeKOR1YK4rSofm/PL07LMev1emY/IWIIHAqASc6JcCBIDNCoX9pNPP79YPZtfz57O/+nfObpz8GG1adbtuva7pFEawl7VEQ+ynIg7ir3B+P7Dh1ft1evttD3/taQy52LvqnW9SpREegMXKZFJhpAQkAKiDKU4tLvudrZ9YzoyyjDaUQOK4qqUjXNAQ9cyTIf67+vTk39/u7mZGorGACGQcJd8IgVBxJmCLWtk0wVfv5g+ffL8j+vby6C7o/GYNbWhJUrGxAgoWEpzlJwo2Kt7eX158uTPef2usGXflF/cu5+xTN6+miyvEisIAEmeDZs2RMJyPX/86tG8fteZAOVR7+jBvS9LKVO0V/N5pwBHIInTTWHjKk6eLc+er0/DsHXaJg8XsuVqZqV3u11545ONSIlUpQqz44OPJ7uLy9WjOps3qeaMhO3WLX8//SnL8sV2obbz2hgYAQvnYbGe/vLXotpvEtQgowiAPbtZPSVwoABEScIggIRIW980rgYUYCYLhQIgDcYBAEAKAQMA4X8kDXTN6FB8uAAAAABJRU5ErkJggg==">TensorFlow android demo 车道线 车辆 人脸 动作 骨架 识别 检测 - 安卓源码 安卓巴士 - 安卓开发 - Android开发 - 安卓 - 移动互联网门户</A>
        <DT><A HREF="https://www.baidu.com/s?ie=utf-8&f=8&rsv_bp=1&rsv_idx=1&tn=baidu&wd=frameworks%2Fbase%2Fpolicy%2Fsrc%2Fcom%2Fandroid%2Finternal%2Fpolicy%2Fimpl%2FPhoneWindowManager.java&oq=oschina&rsv_pq=ddfcd92600002539&rsv_t=e2c1Yjlx%2FzT%2FjSM2Y0PaWH4xhVbAgDhnleBUMa3CMXIOQ14FDRK2Tth9PrY&rqlang=cn&rsv_enter=0&inputT=748&rsv_n=2&rsv_sug3=85&rsv_sug2=0&rsv_sug4=748" ADD_DATE="1542686194" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAIAAACQkWg2AAACP0lEQVQ4jVWSS0jUYRTFf983M5qmMw4+UYsS0zAKKjdimkHgogcRNIGryijJRZs2UW1sWRIJQZBBbazcRGZCZA9FKoOCGlTMkbSHOj4QZ3TGceb/3RbzN+iuLpfDPffcc5QxIoLWGAFBa5JljN2LIIJS9lwrhVLIOvrhR5oe8XMRrQnM8uwrRlAKsfE4kwssg9KMTtP6mpklcjNo3s+5DoamuVzPxQM2iQj2BS4nTs1MCCMUeFiK0DvCr0WKvfSOEk+AILLOoBQPPhCNU19BVhr+KXYW4U5jeY3IGg2VpLhsVUrhVIr+MVp6iMQRaDvBQICz1awlOL+PNCfVJVzvoaaUujIsgxKRO320vSMjlfJ8Ok4DDE2RvZECD4Cvnb4xCj10NlKahxahLI9Ygt+LVBQgwoXH+O5z9C4v/FiGYJiSHKIJxudRCqcIB7dz4xjzKzTVcPstL4fJcxOJcaWLLTmU5tL1jUIPW7MRQf0zDmjt5epzWg4TXePee7zpZKbScoRgiOIsardhDMoyghBLcOkpgVlOVbG7mL7vjAQZnOTQDgYnuHmc8nwSFg6NNoLWdH6h20+bj1cjfJokz01dGaEoDZWEVvG1M7GAQyeNE0Tw/8G9gRQHc2EcihQH3nR2FWEZMlMJxxieRilM0mml2LOZqSWuddNYTU4GTz4zMM6ZKgLzzC2jFUVZICiFMkaA1Ti33tAfwOXAm84mLysxfiwQDJPi4ORemmvtFP73JcsAONYTblksRnFpPOl2kIC/BsEA9PPta4oAAAAASUVORK5CYII=">frameworks/base/policy/src/com/android/internal/policy/impl/PhoneWindowManag_百度搜索</A>
        <DT><A HREF="https://www.jianshu.com/p/2e0f403e5299" ADD_DATE="1542694691" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACz0lEQVQ4jV2ST2hcZRTFf+f7vvfezOuk0cTWaYtVUEGyEhV0oaBQ0uJCpV1lUwuCBSMoLQiiIoJ/KNiCi1hdxC4UBP/gUtCNiOBG3XRRin9qaJPUUs1Mp0ln3nvfd11MI8WzvNx77uGco5VDe/bnLj9Rp3RbAoiNzHlJYhOWEmDIeQPMSWTS+SpVR4JZWADrRkjCpHZH1COsqsbX3qMsB+ewaxsSRjQsK/LbjbDgJLpDw9Kg7/KZ+7Tj5JcW7rgbTWwldHdhdc3Ui28wffQtlOfmpm+V39Z1w2Qm6Gr50L5ksZGfmLTpl9/FYiM/vZ1meYnqz1+xqwO27N1Pfe4s2Z330Px2xuqV81z5/CNZXZsjRchypl46RnNpVZdeOUz1x1mGp39i8NUntB+ZZe3k2/x9/DVsY52r33+j3uJxLCXkHbpwcNbULnFlh9a9D9J+6FHWv/uaiX0HMAlCIP1zGdcuIcuRHP1PP2D4y4+4dkmQc9jGOnE0pL5wjrL1OMpy+p8t4soOFhsUAmk0RHJMvfA6bnIKixGTCCaRRtfo7HmSyYPPs/rcASbnDtOZfYo46CM53JYO1e9n6J16b5yOJSSQGUESMgMYxxUjYftOLEXW3n8HUsPN86+S3TWDihZyjv8qIhEwAwkzGxfGedJwg/yWGW565ghgZDt30/y1AhJcf7aJMB6CpOsLCdcqaZaX6H14DARTR99EeQEpYjdymBEwM4QAI0YhEQd9wq7dbJ17FuUFKtr0P14gXemPlbtgmAnJwpjIkA+4PEd5QW/xBK7dRnkLgDTo4bftoPPEHC5kpMEaOD9WIJCct2btstZ/+BaqkVHXSk0FqTf2p6kpH7uf8oGHrf/FKQ1P/ywVLZOZtPz03tUi+O6wiYm6ElkmyfF/WNOAgKoyipa1suBGTbropDiPseQlVLRMyOxGpzcjDgE5byo75r0HY0lq5v8FS/1LVvsjW/AAAAAASUVORK5CYII=">史上最详细的Android系统SystemUI 启动过程详细解析 - 简书</A>
        <DT><A HREF="https://www.cnblogs.com/samchen2009/p/3294713.html" ADD_DATE="1542768420" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAChklEQVQ4jX2TT2hUVxTGf+fe95LJNPOKJtjGqIgJtZIukjwmeY1ORbpxpW4tiC22qHQdcdFd11Jw5UIFEVEsdd1/CmLoJEynBIIuZDbqC52ITGIn/zrOu8eFbyRq4re899zv/r5zOMJ7VIhGTonYrwGjqtfvTU7+9HaNXeedAbQQRWfEmrPq9DzwtzFmfHtvb/fjOL4DSKtY1jFgYGCgc3MQPKTZPHqvVLoLkM8PDnbYzB8v6vXdxfv3a2t/e0dBEHyIQG15uZxS2lJpekaFZYLg07dxbRiG/trDYrFYFdXqplzuByABki9GR8cBtc6dGO3vD1q1XhiGnRnf/xK4tSZW4px+Z635pTAW7RXEqdOtou6G8bzvpavrHJXKA0BMuVx+bmH/WD4/uJZiYmqqXKsvDiFcQd3Np/Pzw07MJ57ndSqErQQegIOnxvNOAydTAgVkZmZmHrgIUBgZOSIih51ziRgZA64CagBckvwu6KEwDLtf+UFq4gFE0fAe49kL1hijqlZV82n/nAH4q1T6B1HN+v6x9KItNWkWwmi4zbTf9nzvoxdJ87JzSclaO1AYGupqTcEACU5+BU6lBI18Pv/xvij60babOyJSW1ltHJ0oTp1I1F0yYjKura2PFmLah599z35T+HxkXMW0C3ymKksJfFtfWv5zenp6AZBkpfFbkrUJRncBkzbNyjaYlVzugIg5jrIXaBehjtPNGd/v692xVZ88mY3janVhx/begwapPopnJ14TFON4hTjeH4bhB7nV1aSeydiORsNoEDTn5uYalUrl/zSuqnIN2PlGhFbTyuXyczaWA1hZ+O9WRxB8Bch62yhssGQt/fvs2eKWnp5aNputvwS6tfe4Ri+BiwAAAABJRU5ErkJggg==">图解Android - Zygote, System Server 启动分析 - 漫天尘沙 - 博客园</A>
        <DT><A HREF="https://www.jianshu.com/p/845190676431" ADD_DATE="1542785224" ICON="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAACz0lEQVQ4jV2ST2hcZRTFf+f7vvfezOuk0cTWaYtVUEGyEhV0oaBQ0uJCpV1lUwuCBSMoLQiiIoJ/KNiCi1hdxC4UBP/gUtCNiOBG3XRRin9qaJPUUs1Mp0ln3nvfd11MI8WzvNx77uGco5VDe/bnLj9Rp3RbAoiNzHlJYhOWEmDIeQPMSWTS+SpVR4JZWADrRkjCpHZH1COsqsbX3qMsB+ewaxsSRjQsK/LbjbDgJLpDw9Kg7/KZ+7Tj5JcW7rgbTWwldHdhdc3Ui28wffQtlOfmpm+V39Z1w2Qm6Gr50L5ksZGfmLTpl9/FYiM/vZ1meYnqz1+xqwO27N1Pfe4s2Z330Px2xuqV81z5/CNZXZsjRchypl46RnNpVZdeOUz1x1mGp39i8NUntB+ZZe3k2/x9/DVsY52r33+j3uJxLCXkHbpwcNbULnFlh9a9D9J+6FHWv/uaiX0HMAlCIP1zGdcuIcuRHP1PP2D4y4+4dkmQc9jGOnE0pL5wjrL1OMpy+p8t4soOFhsUAmk0RHJMvfA6bnIKixGTCCaRRtfo7HmSyYPPs/rcASbnDtOZfYo46CM53JYO1e9n6J16b5yOJSSQGUESMgMYxxUjYftOLEXW3n8HUsPN86+S3TWDihZyjv8qIhEwAwkzGxfGedJwg/yWGW565ghgZDt30/y1AhJcf7aJMB6CpOsLCdcqaZaX6H14DARTR99EeQEpYjdymBEwM4QAI0YhEQd9wq7dbJ17FuUFKtr0P14gXemPlbtgmAnJwpjIkA+4PEd5QW/xBK7dRnkLgDTo4bftoPPEHC5kpMEaOD9WIJCct2btstZ/+BaqkVHXSk0FqTf2p6kpH7uf8oGHrf/FKQ1P/ywVLZOZtPz03tUi+O6wiYm6ElkmyfF/WNOAgKoyipa1suBGTbropDiPseQlVLRMyOxGpzcjDgE5byo75r0HY0lq5v8FS/1LVvsjW/AAAAAASUVORK5CYII=">Android O Car Launcher解析 - 简书</A>
    </DL><p>
</DL><p>
		
============================================================================
1.frameworks/base/core/java/com/android/internal/os/ZygoteInit.java

public static void main(String argv[]) {
        ……此处省略一万字，下同
//启动system_server进程
        if (startSystemServer) {
            startSystemServer(abiList, socketName);
        }
        ……此处省略一万字，下同
}
private static boolean startSystemServer(String abiList, String socketName)
        throws MethodAndArgsCaller, RuntimeException {
        ……    
        pid = Zygote.forkSystemServer(
                parsedArgs.uid, parsedArgs.gid,
                parsedArgs.gids,
                parsedArgs.debugFlags,
                null,
                parsedArgs.permittedCapabilities,
                parsedArgs.effectiveCapabilities);
……
}



2.frameworks/base/core/java/com/android/internal/os/Zygote.java
public static int forkSystemServer(int uid, int gid, int[] gids, int debugFlags,
        int[][] rlimits, long permittedCapabilities, long effectiveCapabilities) {
    VM_HOOKS.preFork();
//通过JNI正式fork出system_server进程，并返回system_server的进程ID
    int pid = nativeForkSystemServer(
            uid, gid, gids, debugFlags, rlimits, permittedCapabilities, effectiveCapabilities);
        ……
}

3. frameworks/base/services/java/com/android/server/SystemServer.java
public static void main(String[] args) {
    new SystemServer().run();
}
private void run() {
……
    //Create the system service manager.
mSystemServiceManager = new SystemServiceManager(mSystemContext);

startBootstrapServices();
startCoreServices();
startOtherServices();
……
}

private void startBootstrapServices() {
    ……
    // Activity manager runs the show.
    mActivityManagerService = mSystemServiceManager.startService(
            ActivityManagerService.Lifecycle.class).getService();
    ……
}
private void startOtherServices() {
……
statusBar = new StatusBarManagerService(context, wm);
    ServiceManager.addService(Context.STATUS_BAR_SERVICE, statusBar);
……
startSystemUi(context);
……
}
static final void startSystemUi(Context context) {
    Intent intent = new Intent();
    intent.setComponent(new ComponentName("com.android.systemui",
                "com.android.systemui.SystemUIService"));
    intent.addFlags(Intent.FLAG_DEBUG_TRIAGED_MISSING);
    //Slog.d(TAG, "Starting service: " + intent);
    context.startServiceAsUser(intent, UserHandle.SYSTEM);
}

4.frameworks/base/packages/SystemUI/src/com/android/systemui/SystemUIService.java
public void onCreate() {
    super.onCreate();
    ((SystemUIApplication) getApplication()).startServicesIfNeeded();
}
5. frameworks/base/packages/SystemUI/src/com/android/systemui/SystemUIApplication.java
private final Class<?>[] SERVICES = new Class[] {
        com.android.systemui.tuner.TunerService.class,
        com.android.systemui.keyguard.KeyguardViewMediator.class,
        com.android.systemui.recents.Recents.class,
        com.android.systemui.volume.VolumeUI.class,
        Divider.class,
        com.android.systemui.statusbar.SystemBars.class,
        com.android.systemui.usb.StorageNotification.class,
        com.android.systemui.power.PowerUI.class,
        com.android.systemui.media.RingtonePlayer.class,
        com.android.systemui.keyboard.KeyboardUI.class,
        com.android.systemui.tv.pip.PipUI.class,
        com.android.systemui.shortcut.ShortcutKeyDispatcher.class,
        com.android.systemui.VendorServices.class
};
public void startServicesIfNeeded() {
    startServicesIfNeeded(SERVICES);
}
private void startServicesIfNeeded(Class<?>[] services) {
    ……
//创建相关Service实例，比如SystemBars(PhoneStatusBar)（最近任务栏）
//frameworks/base/packages/SystemUI/src/com/android/
//systemui/statusbar/SystemBars.java(PhoneStatusBar.java)
Object newService = SystemUIFactory.getInstance().createInstance(cl);
    mServices[i] = (SystemUI) ((newService == null) ? cl.newInstance() : newService);
    ……
mServices[i].start();
……
    if (mBootCompleted) {
        mServices[i].onBootCompleted();
    }
}
frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/SystemBars.java
public void start() {
    ……
    mServiceMonitor = new ServiceMonitor(TAG, DEBUG,
            mContext, Settings.Secure.BAR_SERVICE_COMPONENT, this);
    mServiceMonitor.start();  // will call onNoService if no remote service is found
}

@Override
public void onNoService() {
    ……
    createStatusBarFromConfig();  // fallback to using an in-process implementation
}
private void createStatusBarFromConfig() {
    ……
//frameworks/base/packages/SystemUI/res/values/config.xml
    final String clsName = mContext.getString(R.string.config_statusBarComponent);
    ……
cls = mContext.getClassLoader().loadClass(clsName);
    ……
mStatusBar = (BaseStatusBar) cls.newInstance();
    ……
    mStatusBar.start();
    ……
}


frameworks/base/packages/SystemUI/res/values/config.xml
<!-- Component to be used as the status bar service.  Must implement the IStatusBar
 interface.  This name is in the ComponentName flattened format (package/class)  -->
<string name="config_statusBarComponent"       
translatable="false">com.android.systemui.statusbar.phone.StatusBar</string>
frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBar.java
@Override
public void start() {
    ……
    super.start();
    ……
}
private void addStatusBarWindow() {
    ……
makeStatusBarView();
    mStatusBarWindowManager = new StatusBarWindowManager(mContext);
……
}
protected PhoneStatusBarView makeStatusBarView() {
    ……
    inflateStatusBarWindow(context);
……
mNotificationPanel = (NotificationPanelView) mStatusBarWindow.findViewById(
        R.id.notification_panel);
    mNotificationPanel.setStatusBar(this);
……
}
protected void inflateStatusBarWindow(Context context) {
    mStatusBarWindow = (StatusBarWindowView) View.inflate(context,
            R.layout.super_status_bar, null);
}

frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/BaseStatusBar.java
public void start() {
    ……
createAndAddWindows();
……
}
frameworks/base/packages/SystemUI/res/layout/super_status_bar.xml
<com.android.systemui.statusbar.phone.StatusBarWindowView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:sysui="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">

    <com.android.systemui.statusbar.BackDropView
            android:id="@+id/backdrop"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:visibility="gone"
            sysui:ignoreRightInset="true"
            >
        <ImageView android:id="@+id/backdrop_back"
                   android:layout_width="match_parent"
                   android:scaleType="centerCrop"
                   android:layout_height="match_parent" />
        <ImageView android:id="@+id/backdrop_front"
                   android:layout_width="match_parent"
                   android:layout_height="match_parent"
                   android:scaleType="centerCrop"
                   android:visibility="invisible" />
    </com.android.systemui.statusbar.BackDropView>

    <com.android.systemui.statusbar.ScrimView android:id="@+id/scrim_behind"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:importantForAccessibility="no"
        sysui:ignoreRightInset="true"
        />

    <com.android.systemui.statusbar.AlphaOptimizedView
        android:id="@+id/heads_up_scrim"
        android:layout_width="match_parent"
        android:layout_height="@dimen/heads_up_scrim_height"
        android:background="@drawable/heads_up_scrim"
        sysui:ignoreRightInset="true"
        android:importantForAccessibility="no"/>

    <include layout="@layout/status_bar"
        android:layout_width="match_parent"
        android:layout_height="@dimen/status_bar_height" />

    <include layout="@layout/brightness_mirror" />

    <ViewStub android:id="@+id/fullscreen_user_switcher_stub"
              android:layout="@layout/car_fullscreen_user_switcher"
              android:layout_width="match_parent"
              android:layout_height="match_parent"/>

    <include layout="@layout/status_bar_expanded"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:visibility="gone" />

    <com.android.systemui.statusbar.ScrimView android:id="@+id/scrim_in_front"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:importantForAccessibility="no"
        sysui:ignoreRightInset="true"
        />

</com.android.systemui.statusbar.phone.StatusBarWindowView>

 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
