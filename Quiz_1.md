![image](https://github.com/ksh19/Cpp/assets/102785836/734b51ca-381f-45fb-8bf6-2e46822cd5ee)


```ruby
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
	ON_COMMAND(ID_SIZE_9, &CPenView::OnSize8)
	ON_COMMAND(ID_LINE_4, &CPenView::OnLine4)
	ON_COMMAND(ID_LINE_1, &CPenView::OnLine1)
	ON_COMMAND(ID_SIZE_17, &CPenView::OnSize16)
	ON_COMMAND(ID_SIZE_33, &CPenView::OnSize33)
	ON_WM_CONTEXTMENU()
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
	int n = pDoc->m_oa.GetSize();
	for (int i = 0; i < n; i++) {
		CLine* pLine = (CLine*)pDoc->m_oa[i];
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

COLORREF Col;
int Size;
// CPenView 메시지 처리기
#include "CLine.h"
CPoint opnt;
void CPenView::OnMouseMove(UINT nFlags, CPoint point)
{
	if (nFlags == MK_LBUTTON) {
		CPenDoc* pDoc = GetDocument();
		CLine* pLine = new CLine(opnt, point, Size, Col);
		pDoc->m_oa.Add(pLine);
		Invalidate(FALSE);
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
}


void CPenView::OnSize8()
{
	Size = 8;
}


void CPenView::OnLine4()
{
	Size = 4;
}


void CPenView::OnLine1()
{
	Size = 1;
}


void CPenView::OnSize16()
{
	Size = 16;
}


void CPenView::OnSize33()
{
	Size = 32;
}



BOOL CPenView::PreTranslateMessage(MSG* pMsg)
{
	if (pMsg->message == WM_KEYDOWN) {
		if (pMsg->wParam == '1') { Size = 1; }
		if (pMsg->wParam == '2') { Size = 2; }
		if (pMsg->wParam == '3') { Size = 3; }
		if (pMsg->wParam == '4') { Size = 4; }
		if (pMsg->wParam == '5') { Size = 5; }
		if (pMsg->wParam == '6') { Size = 6; }
		if (pMsg->wParam == '7') { Size = 7; }
		if (pMsg->wParam == '8') { Size = 8; }
		if (pMsg->wParam == '9') { Size = 9; }
	}
	return CView::PreTranslateMessage(pMsg);
}

typedef int UNIT;
void CPenView::OnContextMenu(CWnd* /*pWnd*/, CPoint point)
{
	CMenu menu;
	menu.CreatePopupMenu();

	// 서브 메뉴 추가: 선 굵기 설정 (기존 메뉴 항목 사용)
	CMenu sizeMenu;
	sizeMenu.CreatePopupMenu();
	menu.AppendMenu(MF_STRING, ID_LINE_1, _T("굵기 1"));
	menu.AppendMenu(MF_STRING, ID_LINE_4, _T("굵기 4"));
	menu.AppendMenu(MF_STRING, ID_SIZE_8, _T("굵기 8"));
	// ... 기존의 선 굵기 항목들을 추가하세요
	

	// 서브 메뉴 추가: 색상 바꾸기 (기존 메뉴 항목 사용)
	menu.AppendMenu(MF_STRING, ID_SELECT_COLOR, _T("Color"));

	// 화면 좌표로 변환
	ClientToScreen(&point);

	// 메뉴 실행
	menu.TrackPopupMenu(TPM_LEFTALIGN | TPM_RIGHTBUTTON, point.x, point.y, this);

}
```
