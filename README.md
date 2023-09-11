# Cpp

```ruby

// MFCApplication5Dlg.cpp: 구현 파일
//

#include "pch.h"
#include "framework.h"
#include "MFCApplication5.h"
#include "MFCApplication5Dlg.h"
#include "afxdialogex.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// 응용 프로그램 정보에 사용되는 CAboutDlg 대화 상자입니다.

class CAboutDlg : public CDialogEx
{
public:
   CAboutDlg();

// 대화 상자 데이터입니다.
#ifdef AFX_DESIGN_TIME
   enum { IDD = IDD_ABOUTBOX };
#endif

   protected:
   virtual void DoDataExchange(CDataExchange* pDX);    // DDX/DDV 지원입니다.

// 구현입니다.
protected:
   DECLARE_MESSAGE_MAP()
};

CAboutDlg::CAboutDlg() : CDialogEx(IDD_ABOUTBOX)
{
}

void CAboutDlg::DoDataExchange(CDataExchange* pDX)
{
   CDialogEx::DoDataExchange(pDX);
}

BEGIN_MESSAGE_MAP(CAboutDlg, CDialogEx)
END_MESSAGE_MAP()


// CMFCApplication5Dlg 대화 상자



CMFCApplication5Dlg::CMFCApplication5Dlg(CWnd* pParent /*=nullptr*/)
   : CDialogEx(IDD_MFCAPPLICATION5_DIALOG, pParent)
{
   m_hIcon = AfxGetApp()->LoadIcon(IDR_MAINFRAME);
}

void CMFCApplication5Dlg::DoDataExchange(CDataExchange* pDX)
{
   CDialogEx::DoDataExchange(pDX);
   DDX_Control(pDX, IDC_SLIDER1, m_Slider);
}

BEGIN_MESSAGE_MAP(CMFCApplication5Dlg, CDialogEx)
   ON_WM_SYSCOMMAND()
   ON_WM_PAINT()
   ON_WM_QUERYDRAGICON()
   ON_WM_MOUSEMOVE()
   ON_BN_CLICKED(IDC_BUTTON1, &CMFCApplication5Dlg::OnBnClickedButton1)
   ON_NOTIFY(NM_CUSTOMDRAW, IDC_SLIDER1, &CMFCApplication5Dlg::OnNMCustomdrawSlider1)
END_MESSAGE_MAP()


// CMFCApplication5Dlg 메시지 처리기

BOOL CMFCApplication5Dlg::OnInitDialog()
{
   CDialogEx::OnInitDialog();

   // 시스템 메뉴에 "정보..." 메뉴 항목을 추가합니다.

   // IDM_ABOUTBOX는 시스템 명령 범위에 있어야 합니다.
   ASSERT((IDM_ABOUTBOX & 0xFFF0) == IDM_ABOUTBOX);
   ASSERT(IDM_ABOUTBOX < 0xF000);

   CMenu* pSysMenu = GetSystemMenu(FALSE);
   if (pSysMenu != nullptr)
   {
      BOOL bNameValid;
      CString strAboutMenu;
      bNameValid = strAboutMenu.LoadString(IDS_ABOUTBOX);
      ASSERT(bNameValid);
      if (!strAboutMenu.IsEmpty())
      {
         pSysMenu->AppendMenu(MF_SEPARATOR);
         pSysMenu->AppendMenu(MF_STRING, IDM_ABOUTBOX, strAboutMenu);
      }
   }

   // 이 대화 상자의 아이콘을 설정합니다.  응용 프로그램의 주 창이 대화 상자가 아닐 경우에는
   //  프레임워크가 이 작업을 자동으로 수행합니다.
   SetIcon(m_hIcon, TRUE);         // 큰 아이콘을 설정합니다.
   SetIcon(m_hIcon, FALSE);      // 작은 아이콘을 설정합니다.

   // TODO: 여기에 추가 초기화 작업을 추가합니다.

   return TRUE;  // 포커스를 컨트롤에 설정하지 않으면 TRUE를 반환합니다.
}

void CMFCApplication5Dlg::OnSysCommand(UINT nID, LPARAM lParam)
{
   if ((nID & 0xFFF0) == IDM_ABOUTBOX)
   {
      CAboutDlg dlgAbout;
      dlgAbout.DoModal();
   }
   else
   {
      CDialogEx::OnSysCommand(nID, lParam);
   }
}

// 대화 상자에 최소화 단추를 추가할 경우 아이콘을 그리려면
//  아래 코드가 필요합니다.  문서/뷰 모델을 사용하는 MFC 애플리케이션의 경우에는
//  프레임워크에서 이 작업을 자동으로 수행합니다.

void CMFCApplication5Dlg::OnPaint()
{
   if (IsIconic())
   {
      CPaintDC dc(this); // 그리기를 위한 디바이스 컨텍스트입니다.

      SendMessage(WM_ICONERASEBKGND, reinterpret_cast<WPARAM>(dc.GetSafeHdc()), 0);

      // 클라이언트 사각형에서 아이콘을 가운데에 맞춥니다.
      int cxIcon = GetSystemMetrics(SM_CXICON);
      int cyIcon = GetSystemMetrics(SM_CYICON);
      CRect rect;
      GetClientRect(&rect);
      int x = (rect.Width() - cxIcon + 1) / 2;
      int y = (rect.Height() - cyIcon + 1) / 2;

      // 아이콘을 그립니다.
      dc.DrawIcon(x, y, m_hIcon);
   }
   else
   {
      CDialogEx::OnPaint();
   }
}

// 사용자가 최소화된 창을 끄는 동안에 커서가 표시되도록 시스템에서
//  이 함수를 호출합니다.
HCURSOR CMFCApplication5Dlg::OnQueryDragIcon()
{
   return static_cast<HCURSOR>(m_hIcon);
}


CPoint opnt;
COLORREF m_Col; // 색상 변경
int n;

void CMFCApplication5Dlg::OnNMCustomdrawSlider1(NMHDR* pNMHDR, LRESULT* pResult) // 슬라이더로 펜 굵기 설정
{
   LPNMCUSTOMDRAW pNMCD = reinterpret_cast<LPNMCUSTOMDRAW>(pNMHDR);
   *pResult = 0;
   n = m_Slider.GetPos();
}

void CMFCApplication5Dlg::OnMouseMove(UINT nFlags, CPoint point) // 마우스 펜 기능
{
   if (nFlags & MK_LBUTTON) {
      CClientDC dc(this);
      CPen pen(PS_SOLID, n, m_Col);
      dc.SelectObject(&pen);
      dc.MoveTo(opnt);
      dc.LineTo(point);
   }
   opnt = point; // 마우스 포인터 업데이트
   CDialogEx::OnMouseMove(nFlags, point);
}

void CMFCApplication5Dlg::OnBnClickedButton1() // 색상 선택 버튼 
{
   CColorDialog dlg;
   if (dlg.DoModal() == IDOK) {
      m_Col = dlg.GetColor();
   }
   
}
```

![image](https://github.com/ksh19/Cpp/assets/102785836/e7e0703e-33f4-48da-8eb7-6d43e9eacf0a)

