## listView 에서 Click LongClick 중복 사용법

~~~~~~
// Click,LongClick 중복 사용 방법. return ture 설정하면 Long클릭 후 클릭은 처리 안됨
item.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Log.d("CLICK", "OnClickListener");
    }
});
         
item.setOnLongClickListener(new View.OnLongClickListener() {
    @Override
    public boolean onLongClick(View v) {
        Log.d("CLICK", "OnLongClickListener");
        
        // 다음 이벤트 계속 진행 false, 이벤트 완료 true
        return true;
    }
});
~~~~~~
