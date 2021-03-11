# IoT216


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
