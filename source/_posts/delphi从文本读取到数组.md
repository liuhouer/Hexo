title: "delphi从文本读取到数组"
date: 2013/1/18
tags: [Delphi]
categories: delphi
---




//读文件 c:\1.txt 内容，以回车为分割存入数组
    procedure TForm1.Button4Click(Sender: TObject);
    var
      slist:TStringList; //存储文本文件内容
      pstrarray:array of string;   //数组
      i,icount:integer;       //文本文件行数
    begin
      slist:=TStringList.Create;
      slist.LoadFromFile('c:\1.txt');  //把文件内容载入到字符串数组列表
      icount:=slist.Count;
      setlength(pstrarray,icount); //设置动态数组长度
      for i:=0 to icount-1 do    //遍历文本，把每行数据存入数组
      begin
        pstrarray[i]:=slist.Strings[i];
      end;
      slist.free;
    end;