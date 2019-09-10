# SQLite  
#### 1. SQLiteDatabase  
* 안드로이드에서 데이터베이스 프로그램의 핵심 클래스는 SQLiteDatabase이다.  
* 데이터베이스에 데이터를 저장하고 가져오고 수정, 삭제하는 모든 SQL 질의문은 SQLiteDatabase 클래스의 함수를 이용한다.  
  
  #### 1.1 SQLiteDatabase 객체 얻기  
  * SQLiteDatabase 객체를 얻으려면 openOrCreateDatabase()함수 사용.  
  * 이 함수에 첫번째 매개변수는 개발자가 지정하는 데이터베이스 파일명이다.  
  ~~~~~
  SQLiteDatabase db=openOrCreateDatabase("memodb", MODE_PRIVATE, null);  
  ~~~~~  
  * 이렇게 얻은 객체를 함수를 이용해 SQL 문을 수행한다.  
  EX)  execSQL(String sql) : insert, update 등 select 문이 아닌 나머지 SQL 수행.  
       rawQuery(String sql, String[] selectionArgs, Object[] bindArgs): select SQL 수행.  
       
       #### 1.2 데이터저장.  
       * 데이터베이스에 데이터를 저장하려면 insert 문을 사용.  
       ~~~~
       db.execSQL("insert into tb_memo (title, content) values (?,?)", new String[]{title, content});
       ~~~~
       
       * 위 함수에서 큰따옴표로 묶인 첫 번째 매개변수가 SQL 문이다.  
       * 만약 이곳에서 데이터 부분(values)을 ?로 작성했따면, 두 번째 매개변수에서 각각의 ?에 대응하는 데이터를 지정.  
       * 위의 예에서는 ?가 두 개이므로 문자열 두 개를 가지는 배열을 적용했습니다. 첫 번째 데이터가 첫 번째 ? 부분에 적용.  
       
       #### 1.3 데이터 찾아서 가져오기.   
       * 저장된 데이터를 찾아서 가져오려면 select 문 사용. 이때 rawQuery()함수 사용.  
       ~~~~  
       Cursor cursor = db.rawQuery("select title, content from tb_memo order by _id desc limit 1", null);  
       ~~~~  
       SQL문에 ? 표현이 없어 두 번째 매개변수 부분이 NULL이다.  
       rawQuery()함수의 결괏값은 Cursor 객체이다.    
       * Cursor는 선택된 행(row)의 집합 객체 정도로 생각하면 된다.  
         
       #### 1.4 Cursor행을 선택하는 함수.  
       1. moveToNext(): 순서상으로 다음 행 선택.  
       2. moveToFirst(): 가장 첫 번째 행 선택.  
       3. moveToLast(): 가장 마지막 행 선택.  
       4. moveToPrevious(): 순서상으로 이전 행 선택.  
       * moveToNext() 함수를 이용하여 행을 선택하고, 선택된 행의 getString() 함수를 이용하여 열 데이터를 가져와 보기.  
       ~~~~
       while (cursor.moveToNext()){
            titleView.setText(cursor.getString(0));
            contentview.setText(cursor.getString(1));
       }
       ~~~~  
       
#### 2. SQLiteOpenHelper 클래스  
간단하게 SQLiteDatabase 와 Cursor 클래스만 사용해도 모든 SQL 문을 수행할 수 있지만, SQLiteOpenHelper 클래스를 이용할 수도 있다.  
이 클래스를 사용하면 select,insert 작업을 수행할 때 테이블 생성 여부를 판단하지 않아도 되는 이점이 있다.  
* SQLiteOpenHelper클래스는 추상 클래스이므로 서브 클래스를 만들어 사용해야 한다.  
~~~~  
public class DBhelper extends SQLiteOpenHelper { 
 public static final int DATABASE_VERSION = 1;  
 
 public DBHelper(Context context) {
   super(context, "memodb", null, DATABASE_VERSION);
   }
 @Override
 public void onCreate(SQLiteDatabase db) {
  //...
 }  
 @Override  
 public void onUpgrade(SQLiteDatabase db, int oldVersion , int newVersion) {
 //...
 }
}
~~~~  
  
  
  
  위의 코드는 SQLiteOpenHelper를 상속받아 작성한 하위 클래스의 구조입니다.  
  생성자를 보면 super(context, "memodb", null, DATABASE_VERSION); 구문이 있는데 "memodb"가 데이터베이스 파일명이다.  
  그리고, DATABASE_VERSION은 데이터베이스 버전 정보이다.  
  * SQLiteOpenHelper를 작성하려면 onCreate()와 onUpgrade() 하마수를 재정의해야 한다.  
    * oncreate():앱이 설치된 후 SQLiteOpenHelper가 최초로 이용되는 순간 한 번 호출.  
    * onUpgrade(): 데이터베이스 버전이 변경될 때마다 호출.  
    
     
       
       이처럼 SQLiteOpenHelper를 정의했다면 실제 SQL 문 수행을 위해 SQLiteDatabase 객체를 획득.  
       이때는 SQLiteOpenHelper 클래스의 getReaderableDatabase() 함수와 getWritableDatabase() 함수를 이용합니다.  
       
#### 3.insert,query,update,delete 함수 이용.
* insert(String table, String nullColumnHack, ContentValues values)  
* update(String table, ContentValues values, String whereClause, String[] whereArgs)  
* delete(String table, String whereClause, String[] WhereArgs)  
* query(String table, String[] columns, String selection, String[] selectionArgs, String groupBy, String having, String orderBy, String limit)    

~~~~
Cursor c=db.query("USER_TB", new String[]{"name", "phone"},
      "ID=?", new String[]{"kkang"},  
      null, null,null);  
~~~~  
위는 query()함수를 이용한 예이다. USER_TB 테이블에서 ID가 "kkabg"인 사용자를 선택하여 해당 사용자의 name과 phone 값을 불러온다.   
group by, having, order by 조건은 null을 대입하여 생락하였다.  

SQLite실습은 SQLite사용2.md에서 

        
