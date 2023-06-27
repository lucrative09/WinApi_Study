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

## 브러시 생성 함수 및 사용방법
CreateSolidBrush()... CreatehatchBrush().. 등등
ex) HBRUSH CreateSolidBrush(COLORREF color)

1) 브러시 생성           : CreateSolidBrush()
2) 브러시 설정           : SelectObject()
3) 도형 출력
4) 이전 브러시 복구      : SelectObject()
5) 생성한 브러시 제거    : DeleteObject()

## 비트맵(Bitmap)

이미지 종류
bmp, jpg, gif, tga 등

## 비트맵을 다루는 두가지 방법

1) 비트맵을 리소스에 등록
2) LoadImage()함수를 이용하여 파일로부터 읽어내는 방법

## 비트맵을 읽어 출력하는 순서

1) 비트맵의 핸들을 얻는다   LoadBitmap(), LoadImage()
2) 메모리 DC 생성          CreateCompatibleDC()
3) 메모리 DC에 비트맵 적용  SelectObject()
4) 비트맵 출력             BitBlt()
5) 메모리 DC와 비트맵 제거  DeleteCD(), DeleteObject()

## 키보드
os의 메세지를 통해서 모든 window api관련 내용을 만든다.

문자 키에 발생하는 메시지
일반적으로 키보드에서 입력할때 발생하는 이벤트
WM_CHAR
대소문자 구분 방법
wParan
아스키 코드 값

# 키보드 메시지
모든 키에 대해 발생하는 메시지
WM_KEYDOWN
- 키 구분 방법
wParam : 가상 키 코드, 문자는 대문자
- 가상 키 코드
VK_LEFT, VK_HOME
'1','A'문자상수 사용
```c++
case WM_KEYDOWN:
        switch (wParam)
        {
        case VK_LEFT:
            MessageBox(hWnd, "LeftKey를 눌렀습니다.", NULL, NULL);
            break;
        case VK_RIGHT:
            MessageBox(hWnd, "RightKey를 눌렀습니다.", NULL, NULL);
            break;
        case VK_F1:
            MessageBox(hWnd, "f1Key를 눌렀습니다.", NULL, NULL);
            break;
        }
       break;
```
# GetAsyncKeyState()
- 실시간으로 키 입력을 체크
- 메시지 큐에 저장되는 키 메시지의 단점을 보안
- 키 눌림이 있으면 음수값 리턴
  SHORT GetAsyncKeyState(int vKey)

## 마우스
- 마우스 메시지 타입
# WM_MOUSEMOVE
- 마우스 이동시 발생
- 마우스 위치 정보
    LOWORD(IParam) -> x 좌표
    HIWORD(IParam) -> y 좌표
```c++
 case WM_MOUSEMOVE:
    nXPos = LOWORD(lParam);
    nYPos = HIWORD(lParam);
    break;
```
# 마우스 드레그 기능 (WM_MOUSEMOVE)
MK_LBUTTON, MK_MBUTTON, MK_RBUTTON, MK_CONTROL
WM_MOUSEMOVE + MK_LBUTTON

## 타이머

# 역할

일정한 시간 간격으로 함수 호출 또는 WM_TIMER 메시지 발생

# 사용용도

일정한 시간 간격으로 코드 실행

# 생성 함수는 한개만 존재 UINT_PTR SetTimer()
참고해서 만들어보기
https://chanos.tistory.com/entry/Windows-API-Win32-API%EC%9D%98-%ED%83%80%EC%9D%B4%EB%A8%B8%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%B4-%EC%8B%9C%EA%B3%84-%EB%A7%8C%EB%93%A4%EA%B8%B0-1























