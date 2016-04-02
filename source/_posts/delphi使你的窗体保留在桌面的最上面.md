title: "delphi使你的窗体保留在桌面的最上面"
date: 2012/10/1  10:52 
tags: [Delphi]
categories: delphi
---
使你的窗体保留在桌面的最上面 
当我们想让一个窗体保留在桌面的最上面时，可以定义窗体的FormStyle属性，使 

窗体保持在最上面。但是，使用这种方法后，在切换窗体的模式时，窗体将闪烁。 

为了避免切换窗体模式时的闪烁，可以使用Windows   API函数SetWindowPos来解决 

这一问题，使用方法如下： 
SetWindowPos(Form1.handle,   HWND_TOPMOST,   Form1.Left,   Form1.Top,   

Form1.Width,   Form1.Height,0); 
用实际窗体名称代替 "Form1 "，调用这个命令就可以将窗体设置为保留在桌面的最 

上面。如要将窗体切换回正常的窗体，调用下面的命令： 
		SetWindowPos(Form1.handle,   HWND_NOTOPMOST,   Form1.Left,   Form1.Top,   

		Form1.Width,   Form1.Height,0);