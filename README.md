# 05_21_homework
devcpp로 도형 만들기 및 visual studio 이용하여 모양 만들기
20210815 백대현

## 소감문
이번 과제는 visual studio와 dev cpp를 이용하여 진행하였는데, dev cpp는 이전에도 과제를 하면서 사용해본 경험이 있어서 과제를 하는데에 큰 어려움이 있진 않았고, visual studio는 평소에 잘 사용해보지 않아서 수업시간에 하는 실습을 따라가는것도 조금 힘들었지만, 교수님께서 실습하는법을 2번이나 자세히 알려주셔서 어렵지 않게 과제를 수행할 수 있었습니다. 감사합니다. 기존 p5.js나 three.js, openGL등 여러가지 컴퓨터 그래픽을 표현하는 방법들을 배웠는데, 이번 과제를 통해서 느낀점은 컴퓨터 그래픽을 표현하는데에는 정말 많은 언어로 표현이 가능하다는 것을 알게되었고, 정말 많은 연습을 해야 좋은 결과물을 얻을 수 있을 것 같다는 생각을 하게 되었습니다. 매일매일 복습을 통하여 수업시간에 배운 내용을 잊지 않도록 공부해서 좋은 성적을 거둘 수 있도록 노력하겠습니다.

## DEV CPP 과제
https://github.com/100DH/05_21_homework/assets/93199016/c09b06fa-6b94-40bb-a3d8-925a91a021e5

기존의 사각형 큐브 만들기에서 코드를 수정하여 피라미드 모양이 나오도록 했습니다.
---------------------------

## Visual Studio mfc 과제
https://github.com/100DH/05_21_homework/assets/93199016/6a2433ee-bfa4-4220-a50f-a01fe5bd472b

기존의 동그라미, 사각형, Hello ANU가 출력되던 것에서 삼각형 4개를 사각형의 각각의 변에 붙여서 표창처럼 만들어보았습니다.
---------------------------
## 실행 방법

- DEV CPP 실행방법
git허브에 올라와있는 코드를 DEV CPP 에 붙여넣어 실행합니다.
- Visual Studio mfc 실행방법
git허브에 올라와있는 코드를 복사한 후, visual studio mfc 앱에서 소스파일에 있는 view 파일에 붙여넣기 한 후, 실행합니다.

# DEV CPP 코드
        /**************************
         * Includes
         *
         **************************/
        
        #include <windows.h>
        #include <gl/gl.h>
        
        
        /**************************
         * Function Declarations
         *
         **************************/
        
        LRESULT CALLBACK WndProc (HWND hWnd, UINT message,
        WPARAM wParam, LPARAM lParam);
        void EnableOpenGL (HWND hWnd, HDC *hDC, HGLRC *hRC);
        void DisableOpenGL (HWND hWnd, HDC hDC, HGLRC hRC);
        
        
        /**************************
         * WinMain
         *
         **************************/
        
        int WINAPI WinMain (HINSTANCE hInstance,
                            HINSTANCE hPrevInstance,
                            LPSTR lpCmdLine,
                            int iCmdShow)
        {
            WNDCLASS wc;
            HWND hWnd;
            HDC hDC;
            HGLRC hRC;        
            MSG msg;
            BOOL bQuit = FALSE;
            float theta = 0.0f;
        
            /* register window class */
            wc.style = CS_OWNDC;
            wc.lpfnWndProc = WndProc;
            wc.cbClsExtra = 0;
            wc.cbWndExtra = 0;
            wc.hInstance = hInstance;
            wc.hIcon = LoadIcon (NULL, IDI_APPLICATION);
            wc.hCursor = LoadCursor (NULL, IDC_ARROW);
            wc.hbrBackground = (HBRUSH) GetStockObject (BLACK_BRUSH);
            wc.lpszMenuName = NULL;
            wc.lpszClassName = "GLSample";
            RegisterClass (&wc);
        
            /* create main window */
            hWnd = CreateWindow (
              "GLSample", "OpenGL Sample", 
              WS_CAPTION | WS_POPUPWINDOW | WS_VISIBLE,
              0, 0, 512, 512,
              NULL, NULL, hInstance, NULL);
        
            /* enable OpenGL for the window */
            EnableOpenGL (hWnd, &hDC, &hRC);
        
            /* program main loop */
            while (!bQuit)
            {
                /* check for messages */
                if (PeekMessage (&msg, NULL, 0, 0, PM_REMOVE))
                {
                    /* handle or dispatch messages */
                    if (msg.message == WM_QUIT)
                    {
                        bQuit = TRUE;
                    }
                    else
                    {
                        TranslateMessage (&msg);
                        DispatchMessage (&msg);
                    }
                }
                else
                {
                    /* OpenGL animation code goes here */
        
                    // Setup depth testing
        			glEnable(GL_DEPTH_TEST);
        			glDepthFunc(GL_LEQUAL);
        			
        			glClearColor(0.0f, 0.0f, 0.0f, 0.0f);
        			glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
        			
        			glPushMatrix();
        			glRotatef(theta, 1.0f, 1.0f, 0.0f);
        			
        			glBegin(GL_TRIANGLES);
        			
        			// Front face
        			glColor4f(0.3f, 0.5f, 1.0f, 1.0f); glVertex3f(0.0f, 0.5f, 0.0f); // Top vertex
        			glColor4f(1.0f, 1.0f, 1.0f, 1.0f); glVertex3f(-0.5f, -0.5f, 0.5f); // Bottom left vertex
        			glColor4f(1.0f, 1.0f, 0.0f, 1.0f); glVertex3f(0.5f, -0.5f, 0.5f); // Bottom right vertex
        			
        			// Right face
        			glColor4f(0.3f, 0.5f, 1.0f, 1.0f); glVertex3f(0.0f, 0.5f, 0.0f); // Top vertex
        			glColor4f(1.0f, 1.0f, 1.0f, 1.0f); glVertex3f(0.5f, -0.5f, 0.5f); // Bottom left vertex
        			glColor4f(1.0f, 1.0f, 0.0f, 1.0f); glVertex3f(0.5f, -0.5f, -0.5f); // Bottom right vertex
        			
        			// Back face
        			glColor4f(0.3f, 0.5f, 1.0f, 1.0f); glVertex3f(0.0f, 0.5f, 0.0f); // Top vertex
        			glColor4f(1.0f, 1.0f, 1.0f, 1.0f); glVertex3f(0.5f, -0.5f, -0.5f); // Bottom left vertex
        			glColor4f(1.0f, 1.0f, 0.0f, 1.0f); glVertex3f(-0.5f, -0.5f, -0.5f); // Bottom right vertex
        			
        			// Left face
        			glColor4f(0.3f, 0.5f, 1.0f, 1.0f); glVertex3f(0.0f, 0.5f, 0.0f); // Top vertex
        			glColor4f(1.0f, 1.0f, 1.0f, 1.0f); glVertex3f(-0.5f, -0.5f, -0.5f); // Bottom left vertex
        			glColor4f(1.0f, 1.0f, 0.0f, 1.0f); glVertex3f(-0.5f, -0.5f, 0.5f); // Bottom right vertex
        			
        			glEnd();
        			
        			// Bottom face
        			glBegin(GL_QUADS);
        			glColor4f(1.0f, 0.5f, 0.0f, 1.0f); glVertex3f(-0.5f, -0.5f, 0.5f); // Bottom left vertex
        			glColor4f(1.0f, 0.5f, 0.0f, 1.0f); glVertex3f(0.5f, -0.5f, 0.5f); // Bottom right vertex
        			glColor4f(1.0f, 0.5f, 0.0f, 1.0f); glVertex3f(0.5f, -0.5f, -0.5f); // Top right vertex
        			glColor4f(1.0f, 0.5f, 0.0f, 1.0f); glVertex3f(-0.5f, -0.5f, -0.5f); // Top left vertex
        			glEnd();
        			
        			glPopMatrix();
        			
        			SwapBuffers(hDC);
        			
        			theta += 1.0f;
        			Sleep(1);
                }
            }
        
            /* shutdown OpenGL */
            DisableOpenGL (hWnd, hDC, hRC);
        
            /* destroy the window explicitly */
            DestroyWindow (hWnd);
        
            return msg.wParam;
        }
        
        
        /********************
         * Window Procedure
         *
         ********************/
        
        LRESULT CALLBACK WndProc (HWND hWnd, UINT message,
                                  WPARAM wParam, LPARAM lParam)
        {
        
            switch (message)
            {
            case WM_CREATE:
                return 0;
            case WM_CLOSE:
                PostQuitMessage (0);
                return 0;
        
            case WM_DESTROY:
                return 0;
        
            case WM_KEYDOWN:
                switch (wParam)
                {
                case VK_ESCAPE:
                    PostQuitMessage(0);
                    return 0;
                }
                return 0;
        
            default:
                return DefWindowProc (hWnd, message, wParam, lParam);
            }
        }
        
        
        /*******************
         * Enable OpenGL
         *
         *******************/
        
        void EnableOpenGL (HWND hWnd, HDC *hDC, HGLRC *hRC)
        {
            PIXELFORMATDESCRIPTOR pfd;
            int iFormat;
        
            /* get the device context (DC) */
            *hDC = GetDC (hWnd);
        
            /* set the pixel format for the DC */
            ZeroMemory (&pfd, sizeof (pfd));
            pfd.nSize = sizeof (pfd);
            pfd.nVersion = 1;
            pfd.dwFlags = PFD_DRAW_TO_WINDOW | 
              PFD_SUPPORT_OPENGL | PFD_DOUBLEBUFFER;
            pfd.iPixelType = PFD_TYPE_RGBA;
            pfd.cColorBits = 24;
            pfd.cDepthBits = 16;
            pfd.iLayerType = PFD_MAIN_PLANE;
            iFormat = ChoosePixelFormat (*hDC, &pfd);
            SetPixelFormat (*hDC, iFormat, &pfd);
        
            /* create and enable the render context (RC) */
            *hRC = wglCreateContext( *hDC );
            wglMakeCurrent( *hDC, *hRC );
        
        }
        
        
        /******************
         * Disable OpenGL
         *
         ******************/
        
        void DisableOpenGL (HWND hWnd, HDC hDC, HGLRC hRC)
        {
            wglMakeCurrent (NULL, NULL);
            wglDeleteContext (hRC);
            ReleaseDC (hWnd, hDC);
        }

------------------------------
# Visual Studio mfc 코드

        // mfchomeworkView.cpp: CmfchomeworkView 클래스의 구현
        //
        
        #include "pch.h"
        #include "framework.h"
        // SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
        // 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
        #ifndef SHARED_HANDLERS
        #include "mfchomework.h"
        #endif
        
        #include "mfchomeworkDoc.h"
        #include "mfchomeworkView.h"
        
        #ifdef _DEBUG
        #define new DEBUG_NEW
        #endif
        
        
        // CmfchomeworkView
        
        IMPLEMENT_DYNCREATE(CmfchomeworkView, CView)
        
        BEGIN_MESSAGE_MAP(CmfchomeworkView, CView)
        	// 표준 인쇄 명령입니다.
        	ON_COMMAND(ID_FILE_PRINT, &CView::OnFilePrint)
        	ON_COMMAND(ID_FILE_PRINT_DIRECT, &CView::OnFilePrint)
        	ON_COMMAND(ID_FILE_PRINT_PREVIEW, &CView::OnFilePrintPreview)
        	ON_WM_MOUSEMOVE()
        END_MESSAGE_MAP()
        
        // CmfchomeworkView 생성/소멸
        
        CmfchomeworkView::CmfchomeworkView() noexcept
        {
        	// TODO: 여기에 생성 코드를 추가합니다.
        
        }
        
        CmfchomeworkView::~CmfchomeworkView()
        {
        }
        
        BOOL CmfchomeworkView::PreCreateWindow(CREATESTRUCT& cs)
        {
        	// TODO: CREATESTRUCT cs를 수정하여 여기에서
        	//  Window 클래스 또는 스타일을 수정합니다.
        
        	return CView::PreCreateWindow(cs);
        }
        
        // CmfchomeworkView 그리기
        CPoint pnt;
        void CmfchomeworkView::OnDraw(CDC* pDC)
        {
        	CmfchomeworkDoc* pDoc = GetDocument();
        	ASSERT_VALID(pDoc);
        	if (!pDoc)
        		return;
        
        	// TODO: 여기에 원시 데이터에 대한 그리기 코드를 추가합니다.
        	pDC->Rectangle(pnt.x - 150, pnt.y - 150, pnt.x + 150, pnt.y + 150);
        	pDC->Ellipse(pnt.x - 150, pnt.y - 150, pnt.x + 150, pnt.y + 150);
        	pDC->TextOutW(pnt.x - 30, pnt.y, L"Hello ANU");
        
        	POINT trianglePoints1[3];
        	trianglePoints1[0] = { pnt.x, pnt.y + 450 };  
        	trianglePoints1[1] = { pnt.x - 150, pnt.y + 150 };  
        	trianglePoints1[2] = { pnt.x + 150, pnt.y + 150 };  
        
        	POINT trianglePoints2[3];
        	trianglePoints2[0] = { pnt.x, pnt.y - 450 };  
        	trianglePoints2[1] = { pnt.x - 150, pnt.y - 150 };  
        	trianglePoints2[2] = { pnt.x + 150, pnt.y - 150 };  
        
        	POINT trianglePoints3[3];
        	trianglePoints3[0] = { pnt.x - 450, pnt.y  };  
        	trianglePoints3[1] = { pnt.x - 150, pnt.y - 150 };  
        	trianglePoints3[2] = { pnt.x - 150, pnt.y + 150 };  
        
        	POINT trianglePoints4[3];
        	trianglePoints4[0] = { pnt.x + 450, pnt.y  };  
        	trianglePoints4[1] = { pnt.x + 150, pnt.y - 150 }; 
        	trianglePoints4[2] = { pnt.x + 150, pnt.y + 150 }; 
        
        	// 삼각형 그리기
        	pDC->Polygon(trianglePoints1, 3);
        	pDC->Polygon(trianglePoints2, 3);
        	pDC->Polygon(trianglePoints3, 3);
        	pDC->Polygon(trianglePoints4, 3);
        }
        
        
        // CmfchomeworkView 인쇄
        
        BOOL CmfchomeworkView::OnPreparePrinting(CPrintInfo* pInfo)
        {
        	// 기본적인 준비
        	return DoPreparePrinting(pInfo);
        }
        
        void CmfchomeworkView::OnBeginPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
        {
        	// TODO: 인쇄하기 전에 추가 초기화 작업을 추가합니다.
        }
        
        void CmfchomeworkView::OnEndPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
        {
        	// TODO: 인쇄 후 정리 작업을 추가합니다.
        }
        
        
        // CmfchomeworkView 진단
        
        #ifdef _DEBUG
        void CmfchomeworkView::AssertValid() const
        {
        	CView::AssertValid();
        }
        
        void CmfchomeworkView::Dump(CDumpContext& dc) const
        {
        	CView::Dump(dc);
        }
        
        CmfchomeworkDoc* CmfchomeworkView::GetDocument() const // 디버그되지 않은 버전은 인라인으로 지정됩니다.
        {
        	ASSERT(m_pDocument->IsKindOf(RUNTIME_CLASS(CmfchomeworkDoc)));
        	return (CmfchomeworkDoc*)m_pDocument;
        }
        #endif //_DEBUG
        
        
        // CmfchomeworkView 메시지 처리기
        
        
        void CmfchomeworkView::OnMouseMove(UINT nFlags, CPoint point)
        {
        	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.
        	pnt = point; 
        	Invalidate(true); 
        	CView::OnMouseMove(nFlags, point);
        }

