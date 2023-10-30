```
#include <afxwin.h>
#include <afxmt.h>
#include <vector>

class DownloadThread : public CWinThread
{
public:
    CEvent m_Event;
    CString m_strUrl;
    CString m_strResult;

    virtual BOOL InitInstance()
    {
        return TRUE;
    }

    virtual int Run()
    {
        // 실제 다운로드 작업을 수행
        // 이 예제에서는 단순히 URL을 시뮬레이션하고 결과를 반환
        Sleep(3000); // 다운로드 시뮬레이션 (실제 작업은 여기에 구현)
        m_strResult = m_strUrl + _T(" 다운로드 완료");
        m_Event.SetEvent(); // 다운로드 완료 이벤트 설정
        return 0;
    }
};

class CDownloadManagerApp : public CWinApp
{
public:
    virtual BOOL InitInstance()
    {
        AfxEnableControlContainer();
        CFrameWnd* pFrame = new CFrameWnd;
        if (!pFrame)
            return FALSE;

        pFrame->Create(NULL, _T("다운로드 관리자"));
        pFrame->ShowWindow(SW_SHOW);
        pFrame->UpdateWindow();

        // 다운로드 스레드 생성
        std::vector<DownloadThread*> threads;
        for (int i = 1; i <= 5; i++)
        {
            DownloadThread* thread = (DownloadThread*)AfxBeginThread(RUNTIME_CLASS(DownloadThread));
            thread->m_strUrl.Format(_T("다운로드 %d"), i);
            threads.push_back(thread);
        }

        // 모든 다운로드 완료 대기
        for (DownloadThread* thread : threads)
        {
            WaitForSingleObject(thread->m_Event, INFINITE);
        }

        // 다운로드 결과 표시
        CString strResults;
        for (DownloadThread* thread : threads)
        {
            strResults += thread->m_strResult + _T("\n");
        }
        AfxMessageBox(strResults);

        return TRUE;
    }
};

CDownloadManagerApp theApp;
```
- MFC를 사용하여 다중 스레드를 활용하여 병렬 작업을 수행하고 그 결과를 메인 UI 스레드에서 표시하는 간단한 다운로드 관리자 예제이다.
- 이 예제는 5개의 다운로드 스레드를 생성하고 각 스레드가 다운로드 작업을 시뮬레이션하도록 하고 모든 다운로드가 완료되면 그 결과를 메시지 박스를 통해 표시한다.
- 실제 다운로드가 아니라 시간을 지연하는 작업을 수행하고 그 결과를 반환하는 것을 시뮬레이션하는 예제이다.

<br/>

### 소감
``
MFC를 배우면서 Windows 애플리케이션 개발에 대한 기초를 다졌습니다. MFC를 통해 GUI 프로그래밍과 스레드 관리 등을 이해했습니다. 
그러나 WinUI는 더 현대적이고 강력한 도구로, UI 디자인과 향상된 사용자 경험을 구현하는데 도움이 될 것입니다. 
후반기에는 WinUI를 배우며 모바일 앱 및 데스크톱 앱을 개발하고 더 많은 기술을 습득하겠습니다. 
더 효율적인 개발과 사용자 친화적인 디자인을 통해 애플리케이션을 더욱 발전시킬 것이며, 최선을 다해 노력하여 훌륭한 앱을 만들겠습니다.
``
