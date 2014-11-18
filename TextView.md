##TextView

注意若要採用
Ref[2] 若存在     android:gravity="center"  顯示會異常

  畫面不和諧的code 
    android:gravity="center"
    android:lines="1"
    android:maxLines="1"
  改用下列方式測試 , but  android:singleLine="true" 不被建議使用，參考Ref[2]
    android:singleLine="true"
    android:gravity="center"
			
			
###Ref

[1]http://blog.csdn.net/ameyume/article/details/6094287
[2]http://stackoverflow.com/questions/15698864/edittext-single-line-in-android
