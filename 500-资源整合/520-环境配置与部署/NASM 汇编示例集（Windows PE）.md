---
tags:
  - NASM
  - x86汇编
  - Win32
  - PE文件
  - GoLink
  - Windows
  - 代码示例
date: 2026-09-10
---

# NASM 汇编示例集（Windows PE）

> **目标平台**：x86（32 位）、Windows 原生 PE 可执行文件
> **工具链**：NASM（汇编） + GoLink（链接）—— 安装参见 [[NASM 汇编工具链入门（Windows）]]
> **依赖 DLL**：`kernel32.dll`（基础 API）/ `user32.dll`（窗口 API）

本文收录两段最小可运行示例，覆盖 NASM 入门最常遇到的两种场景：**控制台输出** 与 **Windows GUI 窗口**。

---

## 一、示例一：Hello World（控制台输出）

通过调用 Windows 控制台 API（`GetStdHandle` + `WriteConsoleA`）输出字符串。

### 1.1 完整代码

```nasm
; hello.asm - NASM 32bit Windows PE
extern GetStdHandle
extern WriteConsoleA
extern ExitProcess

section .data
msg db 'Hello World', 0xA   ; 定义字符串（0xA = 换行符）
len equ $ - msg             ; 计算字符串长度

section .text
global _start
_start:
    push -11                 ; 参数：STD_OUTPUT_HANDLE (-11)
    call GetStdHandle        ; 调用API拿到控制台输出句柄

    push 0                   ; WriteConsole 第5个参数：lpReserved（保留）
    push esp                 ; WriteConsole 第4个参数：lpNumberOfCharsWritten（输出多少字节的返回值）
    push len                 ; WriteConsole 第3个参数：nNumberOfCharsToWrite（字符串长度）
    push msg                 ; WriteConsole 第2个参数：lpBuffer（字符串首地址）
    push eax                 ; WriteConsole 第1个参数：hConsoleOutput（控制台句柄）
    call WriteConsoleA       ; 调用API输出文字

    push 0
    call ExitProcess         ; 调用API，退出程序，返回码 0
```

### 1.2 编译链接

```powershell
nasm -f win32 hello.asm -o hello.obj
golink /entry:_start /console hello.obj kernel32.dll
```

| 命令 | 作用 |
| --- | --- |
| `nasm -f win32 hello.asm -o hello.obj` | 把 `hello.asm` 汇编成 32 位 Windows 目标文件 `hello.obj` |
| `golink /entry:_start /console hello.obj kernel32.dll` | 把 `hello.obj` 链接成 `.exe`，入口点为 `_start`，启用控制台子系统，并链接 `kernel32.dll` |

### 1.3 产物

执行后会生成两个文件：

- `hello.obj` —— 汇编阶段产物，可丢弃
- `hello.exe` —— 最终可执行文件，双击即可运行

---

## 二、示例二：创建 Windows 窗口

通过调用 Win32 GUI API 创建标准窗口。这是 NASM 入门的最难关卡——涉及结构体、消息循环、回调函数。

### 2.1 完整代码（含三处关键修复）

```nasm
; main.asm - NASM + GoLink 窗口修复版
extern __imp__RegisterClassExA@48
extern __imp__CreateWindowExA@48
extern __imp__ShowWindow@8
extern __imp__UpdateWindow@4
extern __imp__GetMessageA@16
extern __imp__TranslateMessage@4
extern __imp__DispatchMessageA@4
extern __imp__PostQuitMessage@4
extern __imp__DefWindowProcA@16
extern __imp__ExitProcess@4
extern __imp__GetModuleHandleA@4

section .data
ClassName db "MyWinClass",0
WinTitle  db "NASM Win32 Window",0

; WNDCLASSEX 结构体
wndClass:
    dd 48               ; cbSize
    dd 0                ; style
    dd WndProc          ; lpfnWndProc
    dd 0                ; cbClsExtra
    dd 0                ; cbWndExtra
    dd 0                ; hInstance (稍后填充)
    dd 0                ; hIcon
    dd 0                ; hCursor
    dd 1                ; hbrBackground <---【修复1】改为1 (COLOR_WINDOW+1)，否则窗口透明
    dd 0                ; lpszMenuName
    dd ClassName        ; lpszClassName
    dd 0                ; hIconSm

section .bss
msg resb 28
hInstance resd 1

section .text
global _start
_start:
    ; 获取当前程序hInstance
    push 0
    call [__imp__GetModuleHandleA@4]
    mov [hInstance], eax

    ; ==========【修复3】修正结构体偏移 ==========
    ; WNDCLASSEX 中 hInstance 的偏移量是 20 (5 * 4 bytes)
    mov [wndClass + 20], eax

    ; 注册窗口类
    push wndClass
    call [__imp__RegisterClassExA@48]

    ; 创建窗口
    push 0                  ; lpParam
    push dword [hInstance]  ; hInstance
    push 0                  ; hMenu
    push 0                  ; hWndParent
    push 400                ; height
    push 600                ; width
    push 100                ; y
    push 100                ; x
    ; ==========【修复2】修正窗口样式 ==========
    ; 0x00CF0000 是 WS_OVERLAPPEDWINDOW (标准带边框窗口)
    ; 之前的 0x90000000 是 WS_POPUP | WS_VISIBLE (无边框，透明背景导致不可见)
    push 0x00CF0000
    push WinTitle
    push ClassName
    push 0                  ; dwExStyle
    call [__imp__CreateWindowExA@48]

    cmp eax,0
    jz exit_loop

    push 1                  ; nCmdShow = SW_SHOW
    push eax
    call [__imp__ShowWindow@8]

    push eax
    call [__imp__UpdateWindow@4]

msg_loop:
    push 0
    push 0
    push 0
    push msg
    call [__imp__GetMessageA@16]

    cmp eax, 0
    jle exit_loop

    push msg
    call [__imp__TranslateMessage@4]

    push msg
    call [__imp__DispatchMessageA@4]
    jmp msg_loop

exit_loop:
    push 0
    call [__imp__ExitProcess@4]

WndProc:
    push ebp
    mov ebp, esp
    mov eax, [ebp+12] ; uMsg

    cmp eax, 2 ; WM_DESTROY
    jne default_msg
    push 0
    call [__imp__PostQuitMessage@4]
    jmp wnd_proc_end

default_msg:
    push dword [ebp+20] ; lParam
    push dword [ebp+16] ; wParam
    push dword [ebp+12] ; Msg
    push dword [ebp+8]  ; hWnd
    call [__imp__DefWindowProcA@16]

wnd_proc_end:
    mov esp, ebp
    pop ebp
    ret 16
```

### 2.2 三处关键修复说明

代码中的 `【修复1】` `【修复2】` `【修复3】` 是新手最容易出错的三个点：

| 编号 | 位置 | 问题 | 修复 |
| --- | --- | --- | --- |
| **修复1** | `hbrBackground` | 默认值 `0` 会让窗口背景透明 | 改为 `1`（`COLOR_WINDOW+1`，系统默认窗口色） |
| **修复2** | `CreateWindowEx` 样式参数 | `0x90000000`（`WS_POPUP \| WS_VISIBLE`）会创建无边框透明窗口 | 改为 `0x00CF0000`（`WS_OVERLAPPEDWINDOW`，标准带边框窗口） |
| **修复3** | `WNDCLASSEX` 结构体 | `hInstance` 字段偏移量容易写错 | 偏移量 `20 = 5 * 4 bytes`（`cbSize`/`style`/`lpfnWndProc`/`cbClsExtra`/`cbWndExtra` 后） |

### 2.3 编译运行

```powershell
nasm -f win32 main.asm -o main.obj
golink /entry:_start main.obj user32.dll kernel32.dll
```

> 注意：`main.asm` 需要链接 **`user32.dll`**（窗口 API），不能像 Hello World 只链接 `kernel32.dll`。

---

## 三、两个示例对比

| 维度 | Hello World | Windows 窗口 |
| --- | --- | --- |
| **API 类别** | Kernel32 控制台 API | User32 GUI API |
| **必需 DLL** | `kernel32.dll` | `user32.dll` + `kernel32.dll` |
| **核心 API** | `GetStdHandle` / `WriteConsoleA` | `RegisterClassExA` / `CreateWindowExA` / `GetMessageA` |
| **消息循环** | 不需要 | 必须（`GetMessage` + `DispatchMessage`） |
| **回调函数** | 不需要 | `WndProc` 处理 `WM_DESTROY` 等消息 |
| **学习价值** | 入门，理解参数压栈与调用约定 | 进阶，理解 Win32 GUI 编程模型 |

---

## 四、参考

- **工具链配置**：[NASM 汇编工具链入门（Windows）](/500-资源整合/520-环境配置与部署/NASM%20汇编工具链入门（Windows）.md)
- **官方 NASM 文档**：<https://www.nasm.us/doc/>
- **Microsoft Win32 API 参考**：<https://learn.microsoft.com/en-us/windows/win32/api/>
- **GoLink 下载**：<http://www.godevtool.com/>