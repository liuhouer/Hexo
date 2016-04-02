title: "delphi怎样使一个子窗体在主窗体最小化后而不最小化"
date: 2013/1/17
tags: [Delphi]
categories: delphi
---
delphi怎样使一个子窗体在主窗体最小化后而不最小化
	unit Unit2; 

	interface 

	uses 
	Windows, Messages, SysUtils, Classes, Graphics, Controls, Forms, Dialogs; 

	type 
	TForm2 = class(TForm) 
	procedure CreateParams(var Params: TCreateParams);override; 
	private 
	{ Private declarations } 
	public 
	{ Public declarations } 
	end; 


<!--more-->

	var 
	Form2: TForm2; 

	implementation 

	{$R *.DFM} 

	procedure TForm2.CreateParams(var Params:TcreateParams); 
	begin 
	inherited; 
	params.WndParent:=getdesktopwindow; 
	end; 

	end.   