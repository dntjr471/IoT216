# IoT216

#210311
# 사각형 넓이 , 피타고라스
```
#include <stdio.h>
#include <iostream>

class Point
{
public:
    Point(int a, int b);
    {
        x = a; , y = b;
    }
    ~Point() {}

private:
public:     //외부 클래스에도 사용 가능
    int x;
    int y;
    int* Pointfunc()
    {
        return (int*)malloc(1000);
    }
};

class Rect
{
private:
public:
    Point p1;
    Point p2;
    Point p13;

    Rect(int x1, int y1, int x2, int y2)
    {
        p1.x = pp1.x;
        p1.y = pp1.y;
        p2.x = pp2.x;
        p2.y = pp2.y;
        p13.x = pp1.x;
        p13.y = pp2.y;
    }

    int area()  //x*y : 두 점으로 이루어진 사각형의 넓이
    {
        int x = p2.x - p1.x;
        int y = p2.y - p1.y;
        return x * y;
    }

    double distance();  // 피타고라스 정리
    int tLength();
    //{
    //    int x = p2.x - p1.x;
    //    int y = p2.y - p1.y;
    //    return sqrt((x*x)+(y*y));
    //}
};

class RectEx : Rect
{
    Point p3 = { p1.x, p2.y };   //(p1.x, p2.y)
    Point p4 = { p2.x, p1.y };   //(p2.x, p1.y)  p1,p2의 값을 이용
public:
    /*
    Class Rectangle의 모든 내용
    */
    int tLength()
    {
        int x = p1.x - p2.x;
        int y = p1.y - p2.y;
        return x * 2 + y * 2;
    }
};

double Rect::distance()
    {
        int x = p2.x - p1.x;
        int y = p2.y - p1.y;
        return sqrt(x * x + y * y);
    }

    // distance

int main()
{
    Point p1(10,10), p2(20, 20);

    Rect r1 = { p1,p2 };

    //RectEx r2 = { { 10,10 }, { 20,20 } };
    //r1.p1.x = 15;
    //r1.p1.y = 15;
    printf("두 점 p1(%d,%d), p2(%d,%d)로 구성되는 사각형의 면적은 %d 입니다.", r1.p1.x
    , r1.p1.y, r1.p2.x, r1.p2.y, r1.area());

    printf("두 점 사이의 길이는 %.2f 입니다.", r1.distance());
    printf("사각형의 전체 둘레길이는 %d 입니다.", r2.tLength();
}


```
#210311

#include <stdio.h>
#include <conio.h>


int main()
{
	int i, j, k;
	char* str = ".........";
	char ch = '|';

	while (1)
	{
		k = getch()-0x30; // '0'~'9' : 30h~39h
		if(k<0 || k>9) break;
		for (i = 0; i < 10; i++)
		{
			if (i == k) printf("%c", ch);
			else        printf("%c", *(str + i));
		}
		printf("\r");
	}
}



## 210315
# MFC 수업 자료

```
// MFCApp1.cpp : 애플리케이션에 대한 진입점을 정의합니다.
//

#include "framework.h"
#include "MFCApp1.h"

#define MAX_LOADSTRING 100

// 전역 변수:
HINSTANCE hInst;                                // 현재 인스턴스입니다.
WCHAR szTitle[MAX_LOADSTRING];                  // 제목 표시줄 텍스트입니다.
WCHAR szWindowClass[MAX_LOADSTRING];            // 기본 창 클래스 이름입니다.

// 이 코드 모듈에 포함된 함수의 선언을 전달합니다:
ATOM                MyRegisterClass(HINSTANCE hInstance);
BOOL                InitInstance(HINSTANCE, int);
LRESULT CALLBACK    WndProc(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK    About(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK    OpenTest(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK    NewTest(HWND, UINT, WPARAM, LPARAM);

int APIENTRY wWinMain(_In_ HINSTANCE hInstance,
                     _In_opt_ HINSTANCE hPrevInstance,
                     _In_ LPWSTR    lpCmdLine,
                     _In_ int       nCmdShow)
{
    UNREFERENCED_PARAMETER(hPrevInstance);
    UNREFERENCED_PARAMETER(lpCmdLine);

    // TODO: 여기에 코드를 입력합니다.

    // 전역 문자열을 초기화합니다.
    LoadStringW(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
    LoadStringW(hInstance, IDC_MFCAPP1, szWindowClass, MAX_LOADSTRING);
    MyRegisterClass(hInstance);

    // 애플리케이션 초기화를 수행합니다:
    if (!InitInstance (hInstance, nCmdShow))
    {
        return FALSE;
    }

    HACCEL hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_MFCAPP1));

    MSG msg;

    // 기본 메시지 루프입니다:
    while (GetMessage(&msg, nullptr, 0, 0))
    {
        if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }
    }

    return (int) msg.wParam;
}



//
//  함수: MyRegisterClass()
//
//  용도: 창 클래스를 등록합니다.
//
ATOM MyRegisterClass(HINSTANCE hInstance)
{
    WNDCLASSEXW wcex;

    wcex.cbSize = sizeof(WNDCLASSEX);

    wcex.style          = CS_HREDRAW | CS_VREDRAW;
    wcex.lpfnWndProc    = WndProc;
    wcex.cbClsExtra     = 0;
    wcex.cbWndExtra     = 0;
    wcex.hInstance      = hInstance;
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_MFCAPP1));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
    wcex.lpszMenuName   = MAKEINTRESOURCEW(IDC_MFCAPP1);
    wcex.lpszClassName  = szWindowClass;
    wcex.hIconSm        = LoadIcon(wcex.hInstance, MAKEINTRESOURCE(IDI_SMALL));

    return RegisterClassExW(&wcex);
}

//
//   함수: InitInstance(HINSTANCE, int)
//
//   용도: 인스턴스 핸들을 저장하고 주 창을 만듭니다.
//
//   주석:
//
//        이 함수를 통해 인스턴스 핸들을 전역 변수에 저장하고
//        주 프로그램 창을 만든 다음 표시합니다.
//
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // 인스턴스 핸들을 전역 변수에 저장합니다.

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}

//
//  함수: WndProc(HWND, UINT, WPARAM, LPARAM)
//
//  용도: 주 창의 메시지를 처리합니다.
//
//  WM_COMMAND  - 애플리케이션 메뉴를 처리합니다.
//  WM_PAINT    - 주 창을 그립니다.
//  WM_DESTROY  - 종료 메시지를 게시하고 반환합니다.
//
//
LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
    case WM_COMMAND:
        {
            int wmId = LOWORD(wParam);
            // 메뉴 선택을 구문 분석합니다:
            switch (wmId)
            {
            case ID_FILEOPEN:
                DialogBox(hInst, MAKEINTRESOURCE(IDD_DLG_Test1), hWnd, OpenTest);
                break;
            case ID_NEWFILE:
                DialogBox(hInst, MAKEINTRESOURCE(IDD_DLG_Test1), hWnd, NewTest);
                break;
            case IDM_ABOUT:
                DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
                break;
            case IDM_EXIT:
                DestroyWindow(hWnd);
                break;
            default:
                return DefWindowProc(hWnd, message, wParam, lParam);
            }
        }
        break;
    case WM_PAINT:
        {
            PAINTSTRUCT ps;
            HDC hdc = BeginPaint(hWnd, &ps);
            // TODO: 여기에 hdc를 사용하는 그리기 코드를 추가합니다...
            EndPaint(hWnd, &ps);
        }
        break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// 정보 대화 상자의 메시지 처리기입니다.
INT_PTR CALLBACK About(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{
    UNREFERENCED_PARAMETER(lParam);
    switch (message)
    {
    case WM_INITDIALOG:
        return (INT_PTR)TRUE;

    case WM_COMMAND:
        if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
        {
            EndDialog(hDlg, LOWORD(wParam));
            return (INT_PTR)TRUE;
        }
        break;
    }
    return (INT_PTR)FALSE;
}
INT_PTR CALLBACK OpenTest(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{
    UNREFERENCED_PARAMETER(lParam);
    switch (message)
    {
    case WM_INITDIALOG:
        return (INT_PTR)TRUE;

    case WM_COMMAND:
        if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
        {
            EndDialog(hDlg, LOWORD(wParam));
            return (INT_PTR)TRUE;
        }
        break;
    }
    return (INT_PTR)FALSE;
}

INT_PTR CALLBACK NewTest(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{
    UNREFERENCED_PARAMETER(lParam);
    switch (message)
    {
    case WM_INITDIALOG:
        return (INT_PTR)TRUE;

    case WM_COMMAND:
        if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
        {
            EndDialog(hDlg, LOWORD(wParam));
            return (INT_PTR)TRUE;
        }
        break;
    }
    return (INT_PTR)FALSE;
}
```
