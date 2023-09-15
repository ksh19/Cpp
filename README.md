# Cpp

## SDI 기반 펜 만들기
<PenView.cpp>
```
// PenView.cpp: CPenView 클래스의 구현
//

#include "pch.h"
#include "framework.h"
// SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
// 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
#ifndef SHARED_HANDLERS
#include "Pen.h"
#endif

#include "PenDoc.h"
#include "PenView.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// CPenView

IMPLEMENT_DYNCREATE(CPenView, CView)

BEGIN_MESSAGE_MAP(CPenView, CView)
	// 표준 인쇄 명령입니다.
	ON_COMMAND(ID_FILE_PRINT, &CView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_DIRECT, &CView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_PREVIEW, &CView::OnFilePrintPreview)
	ON_WM_MOUSEMOVE()
	ON_COMMAND(ID_SELECT_COLOR, &CPenView::OnSelectColor)
	ON_COMMAND(ID_SIZE_1, &CPenView::OnSize1)
	ON_COMMAND(ID_SIZE_4, &CPenView::OnSize4)
	ON_COMMAND(ID_SIZE_8, &CPenView::OnSize8)
	ON_COMMAND(ID_SIZE_16, &CPenView::OnSize16)
	ON_COMMAND(ID_SIZE_32, &CPenView::OnSize32)
END_MESSAGE_MAP()

// CPenView 생성/소멸

CPenView::CPenView() noexcept
{
	// TODO: 여기에 생성 코드를 추가합니다.

}

CPenView::~CPenView()
{
}

BOOL CPenView::PreCreateWindow(CREATESTRUCT& cs)
{
	// TODO: CREATESTRUCT cs를 수정하여 여기에서
	//  Window 클래스 또는 스타일을 수정합니다.

	return CView::PreCreateWindow(cs);
}

// CPenView 그리기

#include "CLine.h"
void CPenView::OnDraw(CDC* pDC)
{
	CPenDoc* pDoc = GetDocument();
	ASSERT_VALID(pDoc);
	if (!pDoc)
		return;
	int n = pDoc->m_oa.GetSize(); // 크기를 나타냄
	for (int i = 0; i < n; i++) {
		CLine* pLine = (CLine *)pDoc->m_oa[i];
		pLine->Draw(pDC);
	}
	// TODO: 여기에 원시 데이터에 대한 그리기 코드를 추가합니다.
}


// CPenView 인쇄

BOOL CPenView::OnPreparePrinting(CPrintInfo* pInfo)
{
	// 기본적인 준비
	return DoPreparePrinting(pInfo);
}

void CPenView::OnBeginPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄하기 전에 추가 초기화 작업을 추가합니다.
}

void CPenView::OnEndPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄 후 정리 작업을 추가합니다.
}


// CPenView 진단

#ifdef _DEBUG
void CPenView::AssertValid() const
{
	CView::AssertValid();
}

void CPenView::Dump(CDumpContext& dc) const
{
	CView::Dump(dc);
}

CPenDoc* CPenView::GetDocument() const // 디버그되지 않은 버전은 인라인으로 지정됩니다.
{
	ASSERT(m_pDocument->IsKindOf(RUNTIME_CLASS(CPenDoc)));
	return (CPenDoc*)m_pDocument;
}
#endif //_DEBUG


// CPenView 메시지 처리기

COLORREF Col;
int Size;
void CPenView::OnMouseMove(UINT nFlags, CPoint point)
{
	if (nFlags == MK_LBUTTON) {
		CPenDoc* pDoc = GetDocument();
		CLine* pLine = new CLine(opnt, point, Size, Col);
		pDoc->m_oa.Add(pLine);
		Invalidate(FALSE); //계속되는 화면 초기화
	}
	opnt = point;
	CView::OnMouseMove(nFlags, point);
}


void CPenView::OnSelectColor()
{
	CColorDialog dlg;
	if (dlg.DoModal() == IDOK) {
		Col = dlg.GetColor();
	}
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize1()
{
	Size = 1;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize4()
{
	Size = 4;// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize8()
{
	Size = 8;// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize16()
{
	Size = 16;// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CPenView::OnSize32()
{
	Size = 32;// TODO: 여기에 명령 처리기 코드를 추가합니다.
}
```
<CLine.cpp>
```
#include "pch.h"
#include "CLine.h"

IMPLEMENT_SERIAL(CLine, CObject, 1)

void CLine::Serialize(CArchive& ar)
{
	if (ar.IsStoring())
	{	// storing code
		ar << m_From << m_To << m_Size << m_Col;
	}
	else
	{   // loading code
		ar >> m_From >> m_To >> m_Size >> m_Col;
	}
}
```
<PenDoc.cpp>
```

// PenDoc.cpp: CPenDoc 클래스의 구현
//

#include "pch.h"
#include "framework.h"
// SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
// 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
#ifndef SHARED_HANDLERS
#include "Pen.h"
#endif

#include "PenDoc.h"

#include <propkey.h>

#ifdef _DEBUG
#define new DEBUG_NEW
#endif

// CPenDoc

IMPLEMENT_DYNCREATE(CPenDoc, CDocument)

BEGIN_MESSAGE_MAP(CPenDoc, CDocument)
END_MESSAGE_MAP()


// CPenDoc 생성/소멸

CPenDoc::CPenDoc() noexcept
{
	// TODO: 여기에 일회성 생성 코드를 추가합니다.

}

CPenDoc::~CPenDoc()
{
}

BOOL CPenDoc::OnNewDocument()
{
	if (!CDocument::OnNewDocument())
		return FALSE;

	// TODO: 여기에 재초기화 코드를 추가합니다.
	// SDI 문서는 이 문서를 다시 사용합니다.
	INT_PTR n = m_oa.GetCount();
	for (INT_PTR i = 0; i < n; i++) {
		delete m_oa[i];
	}
	m_oa.RemoveAll();
	return TRUE;
}




// CPenDoc serialization

void CPenDoc::Serialize(CArchive& ar)
{
	m_oa.Serialize(ar); // CObArray m_oa
}

#ifdef SHARED_HANDLERS

// 축소판 그림을 지원합니다.
void CPenDoc::OnDrawThumbnail(CDC& dc, LPRECT lprcBounds)
{
	// 문서의 데이터를 그리려면 이 코드를 수정하십시오.
	dc.FillSolidRect(lprcBounds, RGB(255, 255, 255));

	CString strText = _T("TODO: implement thumbnail drawing here");
	LOGFONT lf;

	CFont* pDefaultGUIFont = CFont::FromHandle((HFONT) GetStockObject(DEFAULT_GUI_FONT));
	pDefaultGUIFont->GetLogFont(&lf);
	lf.lfHeight = 36;

	CFont fontDraw;
	fontDraw.CreateFontIndirect(&lf);

	CFont* pOldFont = dc.SelectObject(&fontDraw);
	dc.DrawText(strText, lprcBounds, DT_CENTER | DT_WORDBREAK);
	dc.SelectObject(pOldFont);
}

// 검색 처리기를 지원합니다.
void CPenDoc::InitializeSearchContent()
{
	CString strSearchContent;
	// 문서의 데이터에서 검색 콘텐츠를 설정합니다.
	// 콘텐츠 부분은 ";"로 구분되어야 합니다.

	// 예: strSearchContent = _T("point;rectangle;circle;ole object;");
	SetSearchContent(strSearchContent);
}

void CPenDoc::SetSearchContent(const CString& value)
{
	if (value.IsEmpty())
	{
		RemoveChunk(PKEY_Search_Contents.fmtid, PKEY_Search_Contents.pid);
	}
	else
	{
		CMFCFilterChunkValueImpl *pChunk = nullptr;
		ATLTRY(pChunk = new CMFCFilterChunkValueImpl);
		if (pChunk != nullptr)
		{
			pChunk->SetTextValue(PKEY_Search_Contents, value, CHUNK_TEXT);
			SetChunkValue(pChunk);
		}
	}
}

#endif // SHARED_HANDLERS

// CPenDoc 진단

#ifdef _DEBUG
void CPenDoc::AssertValid() const
{
	CDocument::AssertValid();
}

void CPenDoc::Dump(CDumpContext& dc) const
{
	CDocument::Dump(dc);
}
#endif //_DEBUG


// CPenDoc 명령
```
<CLine.h>
```
#pragma once
#include <afx.h>
class CLine :
    public CObject
{
    DECLARE_SERIAL(CLine)
    CPoint m_From; //멤버 변수
    CPoint m_To;
    int m_Size;
    COLORREF m_Col;
public:
    CLine(){}
    CLine(CPoint From, CPoint To, int Size, COLORREF Col) { //복사 기능
        m_From = From;
        m_To = To;
        m_Size = Size;
        m_Col = Col;
    }
    void Draw(CDC* pDC) { //이동 후 선 그어줌
        CPen pen(PS_SOLID, m_Size, m_Col);
        pDC->SelectObject(&pen);
        pDC->MoveTo(m_From);
        pDC->LineTo(m_To);
    }
    virtual void Serialize(CArchive& ar);
};
```
<PenDoc.h>
```

// PenDoc.h: CPenDoc 클래스의 인터페이스
//


#pragma once


class CPenDoc : public CDocument
{
protected: // serialization에서만 만들어집니다.
	CPenDoc() noexcept;
	DECLARE_DYNCREATE(CPenDoc)

// 특성입니다.
public:
	CObArray m_oa;
// 작업입니다.
public:

// 재정의입니다.
public:
	virtual BOOL OnNewDocument();
	virtual void Serialize(CArchive& ar);
#ifdef SHARED_HANDLERS
	virtual void InitializeSearchContent();
	virtual void OnDrawThumbnail(CDC& dc, LPRECT lprcBounds);
#endif // SHARED_HANDLERS

// 구현입니다.
public:
	virtual ~CPenDoc();
#ifdef _DEBUG
	virtual void AssertValid() const;
	virtual void Dump(CDumpContext& dc) const;
#endif

protected:

// 생성된 메시지 맵 함수
protected:
	DECLARE_MESSAGE_MAP()

#ifdef SHARED_HANDLERS
	// 검색 처리기에 대한 검색 콘텐츠를 설정하는 도우미 함수
	void SetSearchContent(const CString& value);
#endif // SHARED_HANDLERS
};
```
<PenView.h>
```

// PenView.h: CPenView 클래스의 인터페이스
//

#pragma once


class CPenView : public CView
{
protected: // serialization에서만 만들어집니다.
	CPenView() noexcept;
	DECLARE_DYNCREATE(CPenView)

// 특성입니다.
public:
	CPenDoc* GetDocument() const;

// 작업입니다.
public:
	CPoint opnt;
// 재정의입니다.
public:
	virtual void OnDraw(CDC* pDC);  // 이 뷰를 그리기 위해 재정의되었습니다.
	virtual BOOL PreCreateWindow(CREATESTRUCT& cs);
protected:
	virtual BOOL OnPreparePrinting(CPrintInfo* pInfo);
	virtual void OnBeginPrinting(CDC* pDC, CPrintInfo* pInfo);
	virtual void OnEndPrinting(CDC* pDC, CPrintInfo* pInfo);

// 구현입니다.
public:
	virtual ~CPenView();
#ifdef _DEBUG
	virtual void AssertValid() const;
	virtual void Dump(CDumpContext& dc) const;
#endif

protected:

// 생성된 메시지 맵 함수
protected:
	DECLARE_MESSAGE_MAP()
public:
	afx_msg void OnMouseMove(UINT nFlags, CPoint point);
	afx_msg void OnSelectColor();
	afx_msg void OnSize1();
	afx_msg void OnSize4();
	afx_msg void OnSize8();
	afx_msg void OnSize16();
	afx_msg void OnSize32();
};

#ifndef _DEBUG  // PenView.cpp의 디버그 버전
inline CPenDoc* CPenView::GetDocument() const
   { return reinterpret_cast<CPenDoc*>(m_pDocument); }
#endif
```
![image](https://github.com/ksh19/Cpp/assets/102785836/f170b845-f5df-4c7e-871f-bc97fc0f7b27)


## MFC 기반 펜 만들기
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

