title: "delphi把多个时间标签解析成多行的一个方法 "
date:  2014-07-08 16:33:10 
tags: [Delphi]
categories: delphi
---
先读入一个文件，解析前：
[00:01.10][00:01.72]阳光下的泡沫　是彩色的
[00:08.45]就像被骗的我　是幸福的
[00:15.58]追究什么对错　你的谎言　基于你还爱我
[00:27.21][00:28.74]美丽的泡沫　虽然一刹花火
[00:35.89]你所有承诺　虽然都太脆弱
[00:42.84]但爱像泡沫　如果能够看破　有什么难过
[00:50.10][00:57.95]早该知道泡沫　一触就破

//----------------------------------------------------------------------------------------
解析后：
[00:01.10]阳光下的泡沫　是彩色的
[00:01.72]阳光下的泡沫　是彩色的
[00:08.45]就像被骗的我　是幸福的
[00:15.58]追究什么对错　你的谎言　基于你还爱我
[00:27.21]美丽的泡沫　虽然一刹花火
[00:28.74]美丽的泡沫　虽然一刹花火
[00:35.89]你所有承诺　虽然都太脆弱
[00:42.84]但爱像泡沫　如果能够看破　有什么难过
[00:50.10]早该知道泡沫　一触就破
[00:57.95]早该知道泡沫　一触就破

<!--more-->




			procedure TForm1.Button1Click(Sender: TObject);
			var  str: WideString;
			FTextList  :TStrings;
			i:integer;
			k:integer;

			timestr:String;
			content:WideString;
			lastIndex:integer;
			timeNum:integer;
			n:integer;


			begin
			FTextList:=TStringList.create;
			if(op1.Execute) then
			   begin
			     FTextList.loadFromfile(op1.FileName);
			     for i:=0 to FTextList.count -1 do
			     begin
			     str := FTextList[i];
			     lastIndex:=LastDelimiter(']',str);
			     //showmessage(inttostr(lastIndex));
			     //计算时间标签的个数
			     timeNum:=lastIndex div 10;
			     showmessage(inttostr(timeNum));

			     content :=copy(str,lastIndex+1,length(str)-lastIndex) ;
			     //m1.Lines.Add(content);
			         if timeNum=0 then    m1.Lines.Add(str)
			         else
			         for n:=0 to timeNum-1 do
			         begin
			           timestr:=copy(str,n*10+1,10) ;
			           m1.Lines.Add(timestr+content);
			         end;


			      end;
			    end;
			end;