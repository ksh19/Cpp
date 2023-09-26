- PenView.cpp
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

- CLine.cpp
```ruby
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

- PenDoc.cpp
```ruby

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

#include propkey.h>

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

- CLine.h
```ruby
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

- PenDoc.h
```ruby
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
```ruby
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
