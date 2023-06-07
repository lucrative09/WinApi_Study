#  WIN API_Study 그래픽 오브젝트

## GDI 오브젝트

PEN BRUSH BITMAP ....

## GDI 오브젝트 사용 방법

- 운영체제에서 제공
- 사용자가 직접 설정

## 스톡 오브젝트

단일 함수를 통해서 GDI OBJECT 관련 객체를 호출하여 사용할 수 있다.

```c++
GetStockObject()
HGDIOBJ GetStockObject(int fnObject)
fnobject
```

매개변수 fnObject에 들어갈 형태

형변환 예시 -> ex) fnObject = (HBRUSH)GetSotckObject(BLACK_BLUSH)
형변환 예시 -> ex) fnObject = (HPEN)GetSotckObject(BLACK_PEN)

브러시 : BLACK_BLUSH ......
펜 : BLACK_PEN ......
폰트 : ANSI_FIXED_FONT ....
팔레트 : DEFAULT_PALLETTE

## GDI 오브젝트 설정과 해제
```c++
1)Selectobject()
    HGDIOBJ SelectObject(HDC hdc, HGDIOBJ hgdiobj)
2)DeleteObject()
    BOOL Deleteobject(HGDIOBJ hObject)
```
## 펜 사용방법
1) 펜 생성 : CreatePen(), GetStockOjbect()
2) 펜 선택 : SelectObject()
3) 선 그리기 : MoveToEx()
4) 펜 제거 및 이전 펜 설정
    DeleteObject(), SelectObject()
    => 스톡 오브젝트는 제외

## 펜 사용 소스
```c++
    // 실선
    hPen = CreatePen(PS_SOLID, 1, RGB(0, 0, 0));
    hOldPen = (HPEN)SelectObject(hdc, hPen);
    MoveToEx(hdc, 10, 30, NULL);
    LineTo(hdc, 100, 30);
    DeleteObject(hPen);

    // 긴 점선
    hPen = CreatePen(PS_DASH, 1, RGB(0, 0, 0));
    SelectObject(hdc, hPen);
    MoveToEx(hdc, 10, 50, NULL);
    LineTo(hdc, 100, 50);
    DeleteObject(hPen);

    // 짧은 점선
    hPen = CreatePen(PS_DOT, 1, RGB(0, 0, 0));
    SelectObject(hdc, hPen);
    MoveToEx(hdc, 10, 70, NULL);
    LineTo(hdc, 100, 70);
    DeleteObject(hPen);
```

## 펜을 이용한 도형 출력

좌측 상단 점과 우측 하단점을 기준으로 도형을 만들어준다.

- 사각형넣기 LineTo 제외하고 Rectangle()함수를 넣어준다.
BOOL Rectangle(HDC hdc, int nLeftRect, int nTopRect, int nRightRect, int nBottomRect)
```c++
    // 사각형 넣기 
    hPen = CreatePen(PS_SOLID, 1, RGB(0, 0, 0));
    hOldPen = (HPEN)SelectObject(hdc, hPen);
    Rectangle(hdc, 10, 20, 50, 70);
    MoveToEx(hdc, 10, 30, NULL);
    DeleteObject(hPen);

    hPen = CreatePen(PS_DASH, 1, RGB(0, 0, 0));
    SelectObject(hdc, hPen);
    MoveToEx(hdc, 10, 50, NULL);
    Rectangle(hdc, 70, 20, 110, 70);
    DeleteObject(hPen);

    hPen = CreatePen(PS_DOT, 1, RGB(0, 0, 0));
    SelectObject(hdc, hPen);
    MoveToEx(hdc, 10, 70, NULL);
    Rectangle(hdc, 130, 20, 170, 70);
    DeleteObject(hPen);
```

## 원 출력
```c++
BOOL Ellipse(HDD hdc, int nLeftRect, int nTopRect, int nRightRect, int nBottomRect);
(nleftRect, nTopRect)
```
Rectangle 대신 넣는다고 생각하면 편리하다. Ellipse(hdc, 130, 20, 170, 70);

## 브러시(brush)
- 도형의 내부를 색상과 패턴으로 채우는 역활
- 스톡오브젝트를 사용한 브러시