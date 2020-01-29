출처 : https://blog.naver.com/thanksyo/140184822010


 
[오류] java.lang.RuntimeException: Can't create handler inside thread that has not called Looper.prepare()
java.lang.RuntimeException: Can't create handler inside thread that has not called Looper.prepar...

blog.naver.com

​

원인 : 쓰레드 안에 쓰레드를 사용하였기 때문에 오류가 발생하였다.

​

해결방법 : 쓰레드 안에는 Handler 를 사용해서 해결하면 된다.

​

﻿
Handler mHandler = new Handler(Looper.getMainLooper());

mHandler.postDelayed(new Runnable() {

@Override

public void run() {

// 내용


}

}, 0);

﻿
​
