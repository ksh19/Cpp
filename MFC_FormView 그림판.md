- My11View.cpp

```ruby
// My11View.cpp: CMy11View 클래스의 구현
//

#include "pch.h"
#include "framework.h"
// SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
// 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
#ifndef SHARED_HANDLERS
#include "My11.h"
#endif

#include "My11Doc.h"
#include "My11View.h"

#ifdef _DEBUG
#define new DEBUG_NEW
#endif


// CMy11View

IMPLEMENT_DYNCREATE(CMy11View, CFormView)

BEGIN_MESSAGE_MAP(CMy11View, CFormView)
	// 표준 인쇄 명령입니다.
	ON_COMMAND(ID_FILE_PRINT, &CFormView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_DIRECT, &CFormView::OnFilePrint)
	ON_COMMAND(ID_FILE_PRINT_PREVIEW, &CFormView::OnFilePrintPreview)
	ON_EN_CHANGE(IDC_EDIT1, &CMy11View::OnEnChangeEdit1)
	ON_WM_LBUTTONDOWN()
	ON_WM_MOUSEMOVE()
	ON_COMMAND(ID_SELECT_COLOR, &CMy11View::OnSelectColor)
	ON_NOTIFY(NM_CUSTOMDRAW, IDC_SLIDER1, &CMy11View::OnNMCustomdrawSlider1)
	ON_NOTIFY(UDN_DELTAPOS, IDC_SPIN1, &CMy11View::OnDeltaposSpin1)
	ON_COMMAND(ID_SIZE_1, &CMy11View::OnSize1)
	ON_COMMAND(ID_SIZE_4, &CMy11View::OnSize4)
	ON_COMMAND(ID_SIZE_16, &CMy11View::OnSize16)
	
	ON_BN_CLICKED(IDC_BUTTON1, &CMy11View::OnBnClickedButton1)
END_MESSAGE_MAP()

// CMy11View 생성/소멸

CMy11View::CMy11View() noexcept
	: CFormView(IDD_MY11_FORM)
{
	// TODO: 여기에 생성 코드를 추가합니다.

}

CMy11View::~CMy11View()
{
}
CMFCColorButton m_ColorButton;
void CMy11View::DoDataExchange(CDataExchange* pDX)
{
	CFormView::DoDataExchange(pDX);
	DDX_Control(pDX, IDC_EDIT1, m_Edit);
	DDX_Control(pDX, IDC_SPIN1, m_Spin);
	DDX_Control(pDX, IDC_SLIDER1, m_Slider);
	DDX_Control(pDX, IDC_PROGRESS3, m_Progress);
}

BOOL CMy11View::PreCreateWindow(CREATESTRUCT& cs)
{
	// TODO: CREATESTRUCT cs를 수정하여 여기에서
	//  Window 클래스 또는 스타일을 수정합니다.

	return CFormView::PreCreateWindow(cs);
}

void CMy11View::OnInitialUpdate()
{
	CFormView::OnInitialUpdate();
	GetParentFrame()->RecalcLayout();
	ResizeParentToFit();

}


// CMy11View 인쇄

BOOL CMy11View::OnPreparePrinting(CPrintInfo* pInfo)
{
	// 기본적인 준비
	return DoPreparePrinting(pInfo);
}

void CMy11View::OnBeginPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄하기 전에 추가 초기화 작업을 추가합니다.
}

void CMy11View::OnEndPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
{
	// TODO: 인쇄 후 정리 작업을 추가합니다.
}

void CMy11View::OnPrint(CDC* pDC, CPrintInfo* /*pInfo*/)
{
	// TODO: 여기에 사용자 지정 인쇄 코드를 추가합니다.
}


// CMy11View 진단

#ifdef _DEBUG
void CMy11View::AssertValid() const
{
	CFormView::AssertValid();
}

void CMy11View::Dump(CDumpContext& dc) const
{
	CFormView::Dump(dc);
}

CMy11Doc* CMy11View::GetDocument() const // 디버그되지 않은 버전은 인라인으로 지정됩니다.
{
	ASSERT(m_pDocument->IsKindOf(RUNTIME_CLASS(CMy11Doc)));
	return (CMy11Doc*)m_pDocument;
}
#endif //_DEBUG


// CMy11View 메시지 처리기


void CMy11View::OnEnChangeEdit1()
{
	// TODO:  RICHEDIT 컨트롤인 경우, 이 컨트롤은
	// CFormView::OnInitDialog() 함수를 재지정 
	//하고 마스크에 OR 연산하여 설정된 ENM_CHANGE 플래그를 지정하여 CRichEditCtrl().SetEventMask()를 호출하지 않으면
	// 이 알림 메시지를 보내지 않습니다.

	// TODO:  여기에 컨트롤 알림 처리기 코드를 추가합니다.
}

CPoint opnt;
void CMy11View::OnLButtonDown(UINT nFlags, CPoint point){


	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.

	CFormView::OnLButtonDown(nFlags, point);
}

COLORREF col;
int Size;
void CMy11View::OnMouseMove(UINT nFlags, CPoint point)
{
	
	CClientDC dc(this);
	int n = GetDlgItemInt(IDC_EDIT1);
	CPen pen(PS_SOLID, Size, col );
	dc.SelectObject(&pen);
	if (nFlags & MK_LBUTTON) {
		dc.MoveTo(opnt);
		dc.LineTo(point);
	}
		opnt = point;
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.

	CFormView::OnMouseMove(nFlags, point);
}


void CMy11View::OnSelectColor()
{
	CColorDialog dlg;
	if (dlg.DoModal() == IDOK) {
		col = dlg.GetColor();
	}
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CMy11View::OnNMCustomdrawSlider1(NMHDR* pNMHDR, LRESULT* pResult)
{
	LPNMCUSTOMDRAW pNMCD = reinterpret_cast<LPNMCUSTOMDRAW>(pNMHDR);
	*pResult = 0;
	Size = m_Slider.GetPos();

	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	*pResult = 0;
}


void CMy11View::OnDeltaposSpin1(NMHDR* pNMHDR, LRESULT* pResult)
{
	LPNMUPDOWN pNMUpDown = reinterpret_cast<LPNMUPDOWN>(pNMHDR);
	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
	*pResult = 0;
	Size = m_Spin.GetPos();
}


void CMy11View::OnSize1()
{
	Size = 1;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CMy11View::OnSize4()
{
	Size = 4;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}


void CMy11View::OnSize16()
{
	Size = 16;
	// TODO: 여기에 명령 처리기 코드를 추가합니다.
}





void CMy11View::OnBnClickedButton1()
{
	m_Progress.SetPos(50);

	// TODO: 여기에 컨트롤 알림 처리기 코드를 추가합니다.
}
```
