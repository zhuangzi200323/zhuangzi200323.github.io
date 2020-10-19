---
title: CreateProcess使用
date: 2009-09-02 15:06:15
img: '/medias/15.jpg'
categories:
- MFC
---

## CreateProcess使用

STARTUPINFO si;
ZeroMemory(&si, sizeof(si));
si.cb = sizeof(STARTUPINFO);
si.dwFlags = STARTF_USESHOWWINDOW;
si.wShowWindow = SW_SHOW;

PROCESS_INFORMATION pi;
ZeroMemory(&pi, sizeof(pi));


LPTSTR szCmdline = TEXT("testApp.exe");

// Start the child process.
if(!CreateProcess(szCmdline, NULL, NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi))
{
   CString szTmp;
   szTmp.Format(L"CreateProcess failed!!!Error code:%d\n“, GetLastError());
  OutputDebugString(szTmp);
   return;
}

// Wait until child process exits.
WaitForSingleObject(pi.hProcess, INFINITE);

// Close process and thread handles.
CloseHandle( pi.hProcess );
CloseHandle( pi.hThread );