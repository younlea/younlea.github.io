---
title: "MFC Progressbar 만들기 [퍼옴]"
excerpt_separator: "<!--more-->"
date: 2009-04-04 22:28:17 +0900
categories:
  - develop_env
tags:
  - dataScience
  - datascience
  - 데이터

toc : true
toc_sticky : true
---

훔.. 오늘은 progressbar를 만들어야 하는데.. ㅡ.ㅡ; 그냥.. 찾다보니.. 괜찮은 분이 올린글과.. 예제 코드가 있길래 올린다 ㅡ.ㅡ;

어쩌다 보니 직접 짜는건 없고 퍼오기만 하는것 같군 ㅜㅜ 

하이튼 올려볼까??

[출처 : [http://legendfinger.com](http://legendfinger.com/)]\
**1. 먼저 CProgressCtrl m\_wndProg; 를 선언한다.

2. 상태바에 Progress 콘트롤을 올리게 하는 함수를 만듭니다.**\
멤버함수를 추가한다는 것이지요. \
이름은 InitProgress()로 합시다.

1. **void** CMainFrame::InitProgress()
2. {
4. **if** (!m\_wndProg.Create(WS\_CHILD|WS\_VISIBLE, CRect(0,0,0,0), &m\_wndStatusBar, ID\_PROGRESS))
5. {
6. AfxMessageBox("ProGress콘트롤 생성 실패!!");
7. }
9. m\_wndProg.SetRange(0,100);
10. m\_wndProg.SetPos(50);
11. }

void CMainFrame::InitProgress(){if (!m\_wndProg.Create(WS\_CHILD|WS\_VISIBLE, CRect(0,0,0,0), &m\_wndStatusBar, ID\_PROGRESS)){AfxMessageBox("ProGress콘트롤 생성 실패!!");}m\_wndProg.SetRange(0,100);m\_wndProg.SetPos(50);}

설명 : 위에서 ID\_PROGRESS는 Resource Symbols에서 정의해주어야 한다.\
progress 콘트롤의 범위와 위치는 일단 가정해준다. 눈으로 봐야하니까...

　

**3. 자!! 그다음엔 메인프레임의 OnCreate()에서 위의 함수를 호출하면 됩니다.\**그렇게 했는데 눈에 안보인다구요?? 당연하지요... \
왜냐하면 윈도우의 사이즈가 변할떄마다 다시 그려줘야 하잖겠어요???\
해서 메인프레임의 OnSize(UINT nType, int cx, int cy)에 맵핑해줍니다.

1. **void** CMainFrame::OnSize(UINT nType, int cx, int cy)
2. {
3. **if** (m\_wndProg.m\_hWnd)
4. {
5. CRect rc;
6. m\_wndStatusBar.GetItemRect(1,rc);
7. m\_wndProg.SetWindowPos(NULL, rc.left, rc.top, rc.Width(), rc.Height(), SWP\_NOZORDER);
8. }
9. }

void CMainFrame::OnSize(UINT nType, int cx, int cy){if (m\_wndProg.m\_hWnd){CRect rc;m\_wndStatusBar.GetItemRect(1,rc);m\_wndProg.SetWindowPos(NULL, rc.left, rc.top, rc.Width(), rc.Height(), SWP\_NOZORDER);}}

상태바의 rc를 얻어서 그 위에 Progress를 올립니다. \
메인프레임의 사이즈가 변경될 때마다 말이지요... 

이번것은 Progress 콘트롤이 작동할때 생기고 끝나면 사라지게 합니다.\
약간은 복잡할수도 있습니다만.. 일단 따라해 보시구 궁금하면 물어주세요...앙

1. 우선 기본적인 MFC의 SDI골격을 갖춘 윈도우를 Wizard를 통해 만들고 나서...

2. 위저드에서 New Class를 통해 베이스클래스가 CStatusBarCtrl 인 \
우리가 만들 클래스 CMyStatusBar를 만듭니다. \
이렇게하면 편하기 땜에 만드는 거지요..\
잘 만들어졌지요? \
근데 우리가 만들 클래스의 부모는 CStatusBarCtrl 잖아요..\
이걸 CStatusBar로 다 바꿔주세요.. \
왜냐구요? 헤헤.. 지두 잘 몰라요. \
어쩄거나 바꿔 주셔야합니다. \
메뉴에서 EDIT인가에 Replace인가가 있죠? \
우리가 만든 [CMY](http://webdizen.new21.net/blog/entry/%EC%83%81%ED%83%9C%EB%B0%94%EC%9C%84%EC%97%90-ProgressBar-%EC%98%AC%EB%A6%AC%EA%B8%B0-2?TSSESSIONwwwwebdizennet=6f0555d734393a771b00f43d36c50358#)StatusBar.h 와 CMyStatusBar.cpp 내에 있는 \
CStatusBarCtrl라는건 죄다 CStatusBar로 바꿔줍니다. \
일일이 찾아서 해 줘도 되지만..그래도 Replace.

3. 그리고 난뒤 메인프레임의 헤더에 가 보면 CStatusBar m\_wndStatusBar; 라는 부분이 있죠? \
그걸 바꿔줍니다. \
CMyStatusBar m\_wndStatusBar; 로 이제 내가 만든클래스를 이용할수 있어요.. \
현재까지는 한게 없지요? \
그냥 놔둬도 되는걸 왜 건드리는지??\
하지만 이제부터입니다.\
이제 상태바를 내 맘대로 조종할수 있대요.. \
내 클래스에 코딩함으로서 말이지요.

4. 내가 만든 클래스 안에 CProgressCtrl m\_Prog;를 선언해줍니다.\
왜냐구요? \
상태바애 Progress콘트롤 올려야 하잖아요..

5. 자..이제 코딩. 역시 멤버함수를 하나 만듭니다.\
이름은 DisplayProgress()으로..

[view plain](http://webdizen.new21.net/blog/entry/%EC%83%81%ED%83%9C%EB%B0%94%EC%9C%84%EC%97%90-ProgressBar-%EC%98%AC%EB%A6%AC%EA%B8%B0-2?TSSESSIONwwwwebdizennet=6f0555d734393a771b00f43d36c50358#)[print](http://webdizen.new21.net/blog/entry/%EC%83%81%ED%83%9C%EB%B0%94%EC%9C%84%EC%97%90-ProgressBar-%EC%98%AC%EB%A6%AC%EA%B8%B0-2?TSSESSIONwwwwebdizennet=6f0555d734393a771b00f43d36c50358#)[?](http://webdizen.new21.net/blog/entry/%EC%83%81%ED%83%9C%EB%B0%94%EC%9C%84%EC%97%90-ProgressBar-%EC%98%AC%EB%A6%AC%EA%B8%B0-2?TSSESSIONwwwwebdizennet=6f0555d734393a771b00f43d36c50358#)

1. **void** CMyStatusBar::DisplayProgress()
2. {
3. CRect rc;
4. GetItemRect(0,rc);
5. rc.left = rc.Width()/3;
7. **if**(!m\_Prog.Create(WS\_CHILD|WS\_VISIBLE,rc,**this**,IDW\_PROGRESS))
8. {
9. AfxMessageBox("실패");
10. **return**;
11. }
13. SetPaneText(0,"파일로딩중");
14. UpdateWindow();
15. m\_Prog.SetRange(0,100);
16. m\_Prog.SetPos(20);
18. **for**(int i=0;i<25;i++)
19. {
20. ::Sleep(500);
21. m\_Prog.SetPos(i\*4);
22. }
24. m\_Prog.DestroyWindow();
25. SetPaneText(0,"파일로드 완료");
26. }

void CMyStatusBar::DisplayProgress(){CRect rc;GetItemRect(0,rc);rc.left = rc.Width()/3;if(!m\_Prog.Create(WS\_CHILD|WS\_VISIBLE,rc,this,IDW\_PROGRESS)){AfxMessageBox("실패");return;}SetPaneText(0,"파일로딩중");UpdateWindow();m\_Prog.SetRange(0,100);m\_Prog.SetPos(20);for(int i=0;i<25;i++){::Sleep(500);m\_Prog.SetPos(i\*4);}m\_Prog.DestroyWindow();SetPaneText(0,"파일로드 완료");}

설명 : IDW\_PROGRESS는 역시 Resource Symbols에서 등록해주고..\
어디서든 이 함수를 호출하면 상태바에 프로그레스가 생기고 동작하고 사라집니다.\
메인프레임에서 호출하는게 좋겠죠? 상태바객체가 메인프레임에 있으니까...\
m\_wndStatusBar.DisplayProgress() 라고 하면 되겠죠 

이상이고.. 

또 다른분이 올려주신 소스코드... 깨끗하네요.. ㅋㅋ [출처]http://blog.naver.com/firstzim/120042464346\
~~`testprogressbar-firstzim.zip`~~ *(이글루스 파일 첨부, 서버 종료로 접근 불가)*\
코드 분석해 보니까.. 요지는....아래 연결시켜주는 부분과[class의 변수를 선언할때 연결해 주면 된다.] \
void CTestProgressbarDlg::DoDataExchange(CDataExchange\* pDX)\
{\
 // IDC 와 변수의 상관 관계 시켜준다. \
 DDX\_Control(pDX, IDC\_PROGRESS1, m\_progressCtrl);\
 }

 m\_progressCtrl.SetRange(0,30);  // Progress bar 초기화 하는 부분.. 0부터 30까지 그리겠다라는거더군요. ^^;\
 // progress 의 초기치를 정해 준다. \
 m\_progressCtrl.SetPos(0);  // 초기에 얼마로 하겠느냐는건데.. 여기서는 0으로 했네요

그래고 m\_progressCtrl.SetPos(num); num값을 넣으면 값이 커지거나 작아지네요.. 우와 같단하다 ㅡ.ㅡ\

\