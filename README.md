# 05_21_homework
devcpp로 도형 만들기 및 visual studio 이용하여 모양 만들기
20210815 백대현

# DEV CPP 과제
https://github.com/100DH/05_21_homework/assets/93199016/c09b06fa-6b94-40bb-a3d8-925a91a021e5

기존의 사각형 큐브 만들기에서 코드를 수정하여 피라미드 모양이 나오도록 했습니다.
---------------------------
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
