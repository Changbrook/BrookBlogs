解密方式

情况一：已知密码

这种情况谈解密似乎有点多余，不过这里要给出的是如何用VBA代码实现，因为当表格数量达到一定程度，代码自动化解密就有了用武之地。

首先，需要新建一个表格来盛放自己的VBA解密代码，这里不妨取名为crack.xlsm。假设需要解密的Excel文件是protected.xlsm。如图：




打开crack.xlsm，新建crack模块，写入以下代码：

01
Public Sub doCrack()
02
        destPh$ = "D:\Documents\VBACrackExample\protected.xlsm"
03
        Dim wb As Workbook
04
        Set wb = Application.Workbooks.Open(destPh)
05
        pw$ = "ABCDE"
06
        If Application.Wait(Now + TimeValue("0:00:05")) Then
07
        End If
08
        Application.VBE.CommandBars(1).Controls.Item(8).Controls.Item(5).Execute
09
        Application.SendKeys pw & "{ENTER}{ENTER}"
10
End Sub


运行doCrack宏，你将看到protected.xlsm的VBA工程已经加载进来，并且处于解锁状态，如图：



下面对doCrack模块代码进行解读。其中的关键代码是

1
Application.SendKeys pw & "{ENTER}{ENTER}"

这段代码中sendkeys方法可将将一个或多个按键消息发送到活动窗口，就如同用键盘进行输入一样。而

1
Application.VBE.CommandBars(1).Controls.Item(8).Controls.Item(5).Execute

则是执行菜单的工程属性选项，一般查看加密工程属性前必须输入密码，这样就触发了密码输入框的弹出。这个方法适用于所有版本的Excel文件解密，其实只是将输入密码的用户行为自动化而已。但是有一个缺点就是：解密过程中必须保持crack.xlsm拥有输入焦点的状态，否则就会出现如下一幕：



密码被输入到抢夺焦点的应用程序上了，这样不仅解密失败，还有暴露密码的风险。

情况二：密码未知

这种情况下解密VBA工程有两种方式，其中第一种方式需要区分Excel 2003和2007以上格式，第二种方式则是通用破解，分别介绍如下：

方式一：

A．对于Excel 2003格式的加密表格，首先使用HexEdit打开并搜索字符“DPB=”，如图：



然后替换“DPB”为“DPx”，保存退出HexEdit。双击打开表格，会有如下提示：



点击是继续加载，进入VBA工程，发现工程已经展开，但是需要查看VBA代码时却弹出如下错误框：



忽略该错误，直接查看工程属性，设置新的工程密码并确定，如图：



再次查看代码时，已经不再弹出错误框，保存退出后，之后打开VBA工程都可以使用你刚设的新密码解锁。

B．对于Excel 2007以及以上格式的加密表格，当同样用HexEdit打开时，发现根本搜索不到“DBP”字符串！其实Excel 2007的xlsm格式是一个压缩包，当使用解包软件打开表格时，可以看到其中有一个vbaProject.bin文件：



将vbaProject.bin文件解压出来，再用HexEdit打开，就可以搜索到熟悉的“DPB”字符串了！将其修改为“DPx”再塞回压缩包里，接下来的操作相信大家都会了吧。

这种方式虽然在破解过程中错误弹框不断，但是破解完成后，用自己的新密码替换了老密码，再次打开表格就完全没有任何后遗症。不过方法的缺点也比较明显，就是想要将其代码自动化较为困难，会需要使用较多的sendkeys语句，抢夺焦点问题依然存在。

方式二：

这个方法是一个叫Siwtom的越南程序员提供的，是VBA代码化的破解方式。下面给出破解步骤，首先将之前的crack.xlsm中的crack模块代码修改为如下：

01
Attribute VB_Name = "crack"
02
Option Explicit
03
 
04
Private Const PAGE_EXECUTE_READWRITE = &H40
05
 
06
Private Declare Sub MoveMemory Lib "kernel32" Alias "RtlMoveMemory" _
07
        (Destination As Long, Source As Long, ByVal Length As Long)
08
 
09
Private Declare Function VirtualProtect Lib "kernel32" (lpAddress As Long, _
10
        ByVal dwSize As Long, ByVal flNewProtect As Long, lpflOldProtect As Long) As Long
11
 
12
Private Declare Function GetModuleHandleA Lib "kernel32" (ByVal lpModuleName As String) As Long
13
 
14
Private Declare Function GetProcAddress Lib "kernel32" (ByVal hModule As Long, _
15
        ByVal lpProcName As String) As Long
16
 
17
Private Declare Function DialogBoxParam Lib "user32" Alias "DialogBoxParamA" (ByVal hInstance As Long, _
18
        ByVal pTemplateName As Long, ByVal hwndparent As Long, _
19
        ByVal lpDialogFunc As Long, ByVal dwInitParam As Long) As Integer
20
 
21
Dim HookBytes(0 To 5) As Byte
22
Dim OriginBytes(0 To 5) As Byte
23
Dim pFunc As Long
24
Dim flag As Boolean
25
Public sourceID As Long
26
 
27
Private Function GetPtr(ByVal Value As Long) As Long
28
    GetPtr = Value
29
End Function
30
'恢复替换的内存数据
31
Private Sub RecoverBytes()
32
    If flag Then MoveMemory ByVal pFunc, ByVal VarPtr(OriginBytes(0)), 6
33
End Sub
34
'勾住工程解密对话框
35
Private Function Hook() As Boolean
36
    Dim TmpBytes(0 To 5) As Byte
37
    Dim p As Long
38
    Dim OriginProtect As Long
39
 
40
    Hook = False
41
 
42
    pFunc = GetProcAddress(GetModuleHandleA("user32.dll"), "DialogBoxParamA")
43
 
44
 
45
    If VirtualProtect(ByVal pFunc, 6, PAGE_EXECUTE_READWRITE, OriginProtect) <> 0 Then
46
 
47
        MoveMemory ByVal VarPtr(TmpBytes(0)), ByVal pFunc, 6
48
        If TmpBytes(0) <> &H68 Then
49
 
50
            MoveMemory ByVal VarPtr(OriginBytes(0)), ByVal pFunc, 6
51
 
52
            p = GetPtr(AddressOf MyDialogBoxParam)
53
 
54
            HookBytes(0) = &H68
55
            MoveMemory ByVal VarPtr(HookBytes(1)), ByVal VarPtr(p), 4
56
            HookBytes(5) = &HC3
57
 
58
            MoveMemory ByVal pFunc, ByVal VarPtr(HookBytes(0)), 6
59
            flag = True
60
            Hook = True
61
        End If
62
    End If
63
End Function
64
'自己定义的对话框，当资源号为4070时，直接返回1
65
Private Function MyDialogBoxParam(ByVal hInstance As Long, _
66
        ByVal pTemplateName As Long, ByVal hwndparent As Long, _
67
        ByVal lpDialogFunc As Long, ByVal dwInitParam As Long) As Integer
68
    If pTemplateName = 4070 Then
69
        MyDialogBoxParam = 1
70
    Else
71
        RecoverBytes
72
        MyDialogBoxParam = DialogBoxParam(hInstance, pTemplateName, _
73
                           hwndparent, lpDialogFunc, dwInitParam)
74
        Hook
75
    End If
76
End Function
77
 
78
Public Sub doCrack()
79
    If Hook Then
80
        Dim destPh As String
81
        destPh = "D:\Documents\VBACrackExample\protected.xlsm"
82
        Dim wb As Workbook
83
        Set wb = Application.Workbooks.Open(destPh)
84
    End If
85
End Sub
86
 
87
Public Sub endCrack()
88
    RecoverBytes
89
End Sub


运行doCrack宏，你将看到protected.xlsm的VBA工程已经加载进来，但是需要注意的时，它仍然处于加密状态，当用户展开该工程时，却可以毫无障碍地查看VBA代码，就好像一直是处于未加密状态一样。为什么会这样呢，下面分析一下代码的原理。

这段代码破解的核心函数是Hook函数，首先作者借助其他手段获悉到输入密码的对话框是VBE6.dll调用DialogBoxParamA显示VB6INTL.dll资源中的第4070号对话框。Hook函数中先取得DialogBoxParamA的函数指针，将其API入口修改为自己定义的一个对话框函数MyDialogBoxParam，该函数的返回值总是1。这样当Excel想要调用密码输入框来判断用户是否正确输入密码时，总是能得到密码正确的答复。这个函数的原理就像在内存某处挂了一个钩子，当密码输入框要弹出时就将其钩住，直接扔出一个返回值给调用者。

因此，这就能解释为什么工程明明处于加密状态，但运行了Hook函数后，工程却可以毫无障碍地浏览VBA代码。

这种方式用于自动化解密非常合适，它没有焦点抢夺的问题，Hook函数一旦打开，操作加密表格就跟操作未加密表格一样。只是在实际运用到Excel宏代码注入工具时，还有一些细节需要处理，这里就不赘述了。它缺点是，由于替换了内存数据，很容易造成Excel程序崩溃，因此，在每次doCrack后，都要切记执行一次endCrack以恢复内存数据。如果有什么不可控因素导致endCrack没有执行，那么可能迎来一些致命问题。


三，总结

以上就是目前现有的Excel解密方式及其优缺点详解，附录给出了用到的解密文件供参考，希望对大家有所帮助，同时欢迎各种讨论指正。