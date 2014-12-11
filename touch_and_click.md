##Android處理點擊

點擊都包裝成MotionEvent，而MotionEvent的動作基本如下

- ACTION_DOWN
- ACTION_UP
- ACTION_MOVE
- ACTION_CANCEL

而MotionEvent通常還附帶以下資訊

- 點擊位置
- 點擊手指數
- 點擊時間

而手勢判定是從**ACTION_DOWN**開始，到**ACTION_UP**結束

###點擊事件的生命週期

Activity接收點擊事件，然後派送點擊事件dispatchTouchEvent()。

派送順序由最外層(也就是最頂層)開始往下送，ViewGroup會先收到，將點擊事件派送給所屬的Child View(可能是ViewGroup或者View)，而點擊事件可以在ViewGroup中的onInterceptTouchEvent回傳true進行攔截。

如果沒有任何View處理這個點擊事件的話，將由Activity的onTouchEvent()進行處理。

如果有透過setOnTouchListener設定OnTouchListener，也可以在OnTouchListener.onTouch回傳true進行攔截。

###dispatchTouchEvent() Activity和View處理上的差異

Activity先執行dispatchTouchEvent()，若沒有任何View或ViewGroup處理點擊事件的話，最後才會執行onTouchEvent()

View先執行透過setOnTouchListener設定OnTouchListener.onTouch()，如果沒有處理的話，最後才執行View.onTouchEvent()

ViewGroup會先執行onInterceptTouchEvent()判斷是否攔截，如果ACTION_DOWN之後被攔截的話，會派送ACTION_CANCEL給被攔截的View，之後再根據重疊的逆順序執行child.dispatchTouchEvent()，若還是沒有處理，則執行OnTouchListener.onTouch()，若還是沒有處理，最後才執行onTouchEvent()

只要Child View接受了ACTION_DOWN,之後ViewGroup會將後續的事件ACTION_MOVE或ACTION_UP直接派送給他，如果沒有被攔截的話。


##事件 

onInterceptTouchEvent


##focusable vs focusableInTouchMode

###以ListView的Item為範例

clickable, focusable皆為**true**時，且Touch在View上，才會觸發**state_pressed**狀態

clickable, focusable, focusableInTouchMode皆為**true**，且Touch在View上，才會觸發**state_focused**狀態

關於Widget預設狀態，參考 [http://www.phonesdevelopers.com/1698395/](http://www.phonesdevelopers.com/1698395/ "android touch mode default value")

| Widget                    	| Clickable 	| LongClickable 	| Focusable 	| FocusableInTouchMode 	|
|---------------------------	|-----------	|---------------	|-----------	|----------------------	|
| TextView                  	|           	|               	|           	|                      	|
| ImageView                 	|           	|               	|           	|                      	|
| Button                    	|     v     	|               	|     v     	|                      	|
| ImageButton               	|     v     	|               	|     v     	|                      	|
| RadioButton               	|     v     	|               	|     v     	|                      	|
| CheckBox                  	|     v     	|               	|     v     	|                      	|
| ToggleButton              	|     v     	|               	|     v     	|                      	|
| ZoomButton                	|           	|       v       	|     v     	|                      	|
| EditText                  	|     v     	|       v       	|     v     	|           v          	|
| ExtractEditText           	|     v     	|       v       	|     v     	|           v          	|
| AutoCompleteTextView      	|     v     	|       v       	|     v     	|           v          	|
| MultiAutoCompleteTextView 	|     v     	|       v       	|     v     	|           v          	|
| SeekBar                   	|           	|               	|     v     	|                      	|
| RatingBar                 	|           	|               	|     v     	|                      	|
| LinearLayout              	|           	|               	|           	|                      	|
| Chronometer               	|           	|               	|           	|                      	|
| DigitalClock              	|           	|               	|           	|                      	|
| AnalogClock               	|           	|               	|           	|                      	|
| GLSurfaceView             	|           	|               	|           	|                      	|
| SurfaceView               	|           	|               	|           	|                      	|
| VideoView                 	|           	|               	|           	|                      	|
| ProgressBar               	|           	|               	|           	|                      	|

focusable是系統賦予給的View，而View會嘗試給予Child View focus的狀態!常見的例子就是如果畫面中有EditText會自動出現鍵盤!

focusableInTouchMode是一定要使用者點擊才會觸發state_pressed的狀態!

常見問題是不會觸發OnItemClickListenerm可能是因為ListView裡面的Item包含focusable為true的Widget,像以上所提的Wdiget都是

如下範例就會不會觸發OnItemClickListenerm，但是CheckBox然可以正常check

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:layout_gravity="center"
	    android:padding="4dp"
	    android:orientation="vertical">
    
    <TextView
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:text="Medium Text"
	    android:id="@+id/textView" />
    
    <CheckBox
	    android:layout_width="wrap_content"
	    android:layout_height="wrap_content"
	    android:text="New CheckBox"
	    android:id="@+id/checkBox" />
    </LinearLayout>

不會觸發原因是有clickable,focusable,focusableInTouchMode為**true**View處理了Touch的事件了，上面的範例是CheckBox處理掉Touch的事件。

要正常觸發OnClick事件，必須讓這Touch事件沒人處理才行!


#####解法有兩個
1.是在Parent的Layout(LinearLayout)設定

    android:descendantFocusability="blocksDescendants"

2.被設定成focusable為true的Widget改成false

    android:focusable="false"

###必須點選2次才會觸發click問題
只要clickable, focusable, focusableInTouchMode皆為**true**就必須點2次才會觸發onClick事件



##隱藏鍵盤



##EditText Enter


參考資料

http://cyrilmottier.com/2012/02/16/listview-tips-tricks-5-enlarged-touchable-areas/

http://files.cnblogs.com/sunzn/PRE_andevcon_mastering-the-android-touch-system.pdf