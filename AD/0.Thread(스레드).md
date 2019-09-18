안드로이드 스튜디오
=======================     
## 스레드-핸들러 구조 테스트  
시간이 줄어드는 부분을 스레드로 처리하며 스레드에 의한 시간 값을 핸들러를 이용해 UI 스레드에 의뢰해서 화면에 출력합니다.
``` 
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    ImageView startView;
    ImageView pauseView;
    TextView textView;

    Thread thread;

    boolean loopFlag = true;
    boolean isFirst = true;
    boolean isRun;



    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        startView = (ImageView) findViewById(R.id.main_startBtn);
        pauseView = (ImageView) findViewById(R.id.main_pauseBtn);
        textView = (TextView) findViewById(R.id.main_textView);

        startView.setOnClickListener(this);
        pauseView.setOnClickListener(this);

        thread = new MyThread();
    }

    @Override
    public void onClick(View v) {
        if (v == startView) {
            if (isFirst) {
                isFirst = false;
                isRun = true;
                thread.start();
            } else {
                isRun = true;
            }
        } else if (v == pauseView) {
            isRun = false;
        }
    }

    Handler handler = new Handler(){
        @Override
        public void handleMessage(Message msg){
            if(msg.what == 1){
                textView.setText(String.valueOf(msg.arg1));
            }else if (msg.what ==2){
                textView.setText(String.valueOf(msg.obj));
            }
        }
    };

    class MyThread extends Thread {
        public void run() {
            try{
                int count=10;
                while (loopFlag){
                    Thread.sleep(1000);
                    if (isRun){
                        count--;
                        Message message = new Message();
                        message.what = 1;
                        message.arg1 = count;
                        handler.sendMessage(message);
                     if(count == 0){
                        message = new Message();
                        message.what = 2;
                        message.obj = "Finish!!";
                        handler.sendMessage(message);

                        //theread 종료되게 설정
                        loopFlag = false;
                    }
                }
            }
        }catch (Exception e){
            }
        }
    }

}

```
