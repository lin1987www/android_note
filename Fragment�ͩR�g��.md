#Fragment生命週期#

##一般Fragment##

onViewStateRestored  
onResume

##使用FragmentStatePagerAdapter的Fragment生命週期##

###建立→顯示###

1.setUserVisibleHint(false)  
2.setUserVisibleHint(true)  
3.onCreateView  
4.onViewStateRestored  
5.onResume

###建立→未顯示###

1.setUserVisibleHint(false)  
2.onCreateView  
3.onViewStateRestored  
4.onResume

###建立→顯示→未顯示(沒有超出顯示範圍)###
setUserVisibleHint(false)

###建立→未顯示→顯示(進入顯示範圍)###
setUserVisibleHint(true)  

> 這裡所指的範圍是ViewPager.setOffscreenPageLimit的數量，預設為1，也就是目前Page向左右兩邊預先載入的頁數。


###螢幕旋轉時未必會觸發事件###
只要在在Activity中設定configChanges被指定的事件，就不會造成Activity重新建立，連帶Fragment也重新建立的情況，取而代之的是呼叫
**onConfigurationChanged**(Configuration newConfig) Method。

<activity  
...  
android:configChanges="orientation|screenSize|keyboard|keyboardHidden"  
...  
/>

[完整Fragment生命週期圖表來源](https://github.com/xxv/android-lifecycle)

![](https://github.com/xxv/android-lifecycle/raw/master/complete_android_fragment_lifecycle.png)