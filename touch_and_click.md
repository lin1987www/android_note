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