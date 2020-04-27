//Fragment를 보여줄 때
~~~~~~
FragmentTransaction ft = getSupportFragmentManager().beginTransaction();

ft.show(fragment);

ft.commit();
~~~~~~
 

//Fragment를 숨길 때
~~~~~~
FragmentTransaction ft = getSupportFragmentManager().beginTransaction();

ft.hide(fragment);

ft.commit();
~~~~~~
