### [세계 시계 앱 만들어보기] 국가를 선택했을 때 TableView Cell



아이폰의 세계 시계 앱을 만들어보다가 에러가 나는 건 아닌데 해결되지 않는 부분이 있어 정리해보려고한다.

먼저 앱을 빌드 시키면



![스크린샷 2020-05-29 오후 4.56.37](/Users/igayeong/Library/Application Support/typora-user-images/스크린샷 2020-05-29 오후 4.56.37.png)

이런 화면이 뜨는데 현재 시간을 볼 국가들을 지정해주지 않았기 때문에 TableView에 아무것도 뜨지 않는다.

뜨게 하기 위해서 BarButtonItem 'Add'를 눌러보면

![스크린샷 2020-05-29 오후 4.58.11](/Users/igayeong/Library/Application Support/typora-user-images/스크린샷 2020-05-29 오후 4.58.11.png)

이렇게 <code>TimeZone.knownTimeZoneIdentifiers</code> 에서 불러온 국가들의 리스트들을 볼수있다 여기에서 tableview의 delegate 중 하나인 <code>didselected</code> 에서 셀을 선택했을 때 그 셀이 indexPath.row를 받아와 초기 화면에서 뷰를 reloadData()를 하면 되는데

현재 나는 셀을 추가하면 할 수록 



![스크린샷 2020-05-29 오후 5.02.53](/Users/igayeong/Library/Application Support/typora-user-images/스크린샷 2020-05-29 오후 5.02.53.png)

셀만 추가될 뿐 셀안의 내용을 달라지지 않는다..

이것의 문제를 찾아보려고한다.

