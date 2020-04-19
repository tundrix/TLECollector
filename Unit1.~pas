unit Unit1;

interface

uses
  Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, StdCtrls, Buttons, ComCtrls, filectrl, ImgList, inifiles,
  ExtCtrls, IdBaseComponent, IdComponent, IdTCPConnection, IdTCPClient,
  IdHTTP;

type
  TForm1 = class(TForm)
    TreeView1: TTreeView;
    SpeedButton1: TSpeedButton;
    SpeedButton2: TSpeedButton;
    Memo1: TMemo;
    SpeedButton3: TSpeedButton;
    SpeedButton4: TSpeedButton;
    ImageList1: TImageList;
    SpeedButton5: TSpeedButton;
    SpeedButton6: TSpeedButton;
    Label1: TLabel;
    SpeedButton7: TSpeedButton;
    Panel1: TPanel;
    SpeedButton8: TSpeedButton;
    Memo2: TMemo;
    SpeedButton9: TSpeedButton;
    SpeedButton10: TSpeedButton;
    ProgressBar1: TProgressBar;
    procedure UpdateNodeFile(nodFile:TTreeNode);
    procedure MakeTLEMemo();
    procedure LoadTreeFromTLEDirectory(TLEDirectory:string; internetOrStaticDir:bool);
    procedure SpeedButton1Click(Sender: TObject);
    procedure TreeView1DblClick(Sender: TObject);
    procedure SpeedButton2Click(Sender: TObject);
    procedure SpeedButton4Click(Sender: TObject);
    procedure SpeedButton5Click(Sender: TObject);
    procedure FormCreate(Sender: TObject);
    procedure SpeedButton3Click(Sender: TObject);
    procedure SpeedButton6Click(Sender: TObject);
    procedure SpeedButton7Click(Sender: TObject);
    procedure SpeedButton9Click(Sender: TObject);
    procedure SpeedButton10Click(Sender: TObject);
    procedure SpeedButton8Click(Sender: TObject);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

var
  Form1: TForm1;

implementation

{$R *.dfm}


var
 inifPath:string;
 localInternetTLEDir:string;
 loadFromLocalInternetDirectory:bool; //local internet directory - local directory for store auto downloaded files
 defURLs:string;


//Select TLE folder
procedure TForm1.SpeedButton1Click(Sender: TObject);
var
 i:integer;
 dir:string;
 foundFile:TSearchRec;
 inif:TIniFile;
 defPath:string;
 executedDialog:bool;
begin
  //inifPath:=extractfiledir(paramstr(0))+'\tlecollector.ini';
  inif:=TIniFile.Create(inifPath);
  defPath:=inif.ReadString('Options','StaticTLEDirectory','c:\');
  executedDialog:=SelectDirectory('Select TLE Directory','',dir);
  //executedDialog:=SelectDirectory(dir,[],0);
  if executedDialog then
    begin
      TreeView1.Items.Clear();
      if FindFirst(dir+'\*.txt', 0, foundFile)=0 then
        begin
          inif.WriteString('Options','StaticTLEDirectory',dir);
          findclose(foundFile);

          SpeedButton5Click(sender);
        end;

      //SpeedButton4Click(Sender);
      //messagebox(0,'Готово','Готово',0);
    end;
                 {
  for i:= 0 to 10 do
    begin
      TreeView1.Items.Add(nil,'hello'+inttostr(i));
    end;                 }
  //TreeView1.Items.
end;

procedure TForm1.UpdateNodeFile(nodFile:TTreeNode);
var
 nodSat:TTreeNode;
 foundFile:TSearchRec;
begin
  nodFile.StateIndex:=1;
  nodSat:=nodFile.getFirstChild();
  repeat
    //if nodSat.StateIndex=2 then
      begin
        if nodSat.StateIndex>1 then
          begin
            nodFile.Expand(False);
            nodFile.StateIndex:=2;
          end;
      end;
    nodSat:=nodSat.getNextSibling();
  until (not assigned(nodSat));
end;

function ExtractSatLines(filePath: string; satName: string):string;
var
 tf:textfile;
 textLine:string;
 res:string;
begin
  AssignFile(tf, filePath);
  Reset(tf);
  res:='';
  ReadLn(tf, textLine);
  while not Eof(tf) do
    begin
      if (textLine[1]<>'1')and(textLine[1]<>'2') then
        begin
          if satName=textLine then
            begin
              res:=res+textline;
              res:=res+#$0D+#$0A;
              ReadLn(tf, textLine);
              res:=res+textline;
              res:=res+#$0D+#$0A;
              ReadLn(tf, textLine);
              res:=res+textline;
              res:=res+#$0D+#$0A;
              break;
            end;
        end;
      ReadLn(tf, textLine);
    end;
  CloseFile(tf);
  ExtractSatLines:=res;
end;

procedure TForm1.MakeTLEMemo();
var
 inif:TIniFile;
 //inifPath:string;
 filePath:string;
 nodFile:TTreeNode;
 nodSat:TTreeNode;
 i:integer;
 readVal:string;
 satLines:string;
 allSatLines:string;
begin
  //inifPath:=extractfiledir(paramstr(0))+'\tlecollector.ini';
  inif:=TIniFile.Create(inifPath);
  nodFile:=TreeView1.Items.GetFirstNode;
  if assigned(nodFile) then
    begin
      allSatLines:='';
      repeat
        nodSat:=nodFile.getFirstChild();
        if assigned(nodSat) then
          begin
            repeat
              if nodSat.StateIndex=2 then
                begin
                  readVal:=inif.ReadString('Options', 'StaticTLEDirectory', '');
                  filepath:=readVal+'\'+nodFile.Text;
                  satLines:=ExtractSatLines(filePath, nodSat.Text);
                  allSatLines:=allSatLines+satLines;
                end;
              nodSat:=nodSat.getNextSibling();
            until (not assigned(nodSat));
          end;
        //i:=i+1;
        nodFile:=nodFile.getNextSibling();
      until (not assigned(nodFile));
      memo1.Text:=allSatLines;
    end;
end;

procedure TForm1.TreeView1DblClick(Sender: TObject);
var
  N: TTreeNode;
begin
  N:=TreeView1.Selected;
  if N.Level=1 then
    begin
      N.StateIndex:=3-N.StateIndex;
      UpdateNodeFile(N.Parent);
      MakeTLEMemo();
    end;
end;

//Save checkboxes to INI file
procedure TForm1.SpeedButton2Click(Sender: TObject);
var
 inif:TIniFile;
 //inifPath:string;
 str:string;
 nodFile:TTreeNode;
 nodSat:TTreeNode;
 i:integer;
begin
  //inifPath:=extractfiledir(paramstr(0))+'\tlecollector.ini';
  inif:=TIniFile.Create(inifPath);
  inif.EraseSection('SatellitesUsed');

  str:=inttostr (TreeView1.Items.Count);
  //messagebox(0,pchar(str),'',0);
  nodFile:=TreeView1.Items.GetFirstNode;

  if assigned(nodFile) then
    begin
      i:=0;
      repeat
        nodSat:=nodFile.getFirstChild();
        if assigned(nodSat) then
          begin
            //i:=0;
            repeat
              if nodSat.StateIndex=2 then
                begin
                  inif.WriteInteger('SatellitesUsed', nodFile.Text+'+'+nodSat.Text, nodSat.StateIndex);
                end;
              nodSat:=nodSat.getNextSibling();
            until (not assigned(nodSat));
          end;
        //i:=i+1;
        nodFile:=nodFile.getNextSibling();
      until (not assigned(nodFile));
    end;
end;

//Load checkboxes from INI file
procedure TForm1.SpeedButton4Click(Sender: TObject);
var
 inif:TIniFile;
 //inifPath:string;
 //str:string;
 nodFile:TTreeNode;
 nodSat:TTreeNode;
 //i:integer;
 readVal:integer;
begin
  //inifPath:=extractfiledir(paramstr(0))+'\tlecollector.ini';
  inif:=TIniFile.Create(inifPath);


  //str:=inttostr (TreeView1.Items.Count);
  //messagebox(0,pchar(str),'',0);
  nodFile:=TreeView1.Items.GetFirstNode;

  if assigned(nodFile) then
    begin
      //i:=0;
      repeat
        nodSat:=nodFile.getFirstChild();
        if assigned(nodSat) then
          begin
            //i:=0;
            repeat
              //if nodSat.StateIndex=2 then
                begin
                  readVal:=inif.ReadInteger('SatellitesUsed', nodFile.Text+'+'+nodSat.Text, 1);
                  nodSat.StateIndex:=readVal;
                  if readVal>1 then
                    begin
                      nodFile.Expand(False);
                      nodFile.StateIndex:=2;
                    end;
                end;
              nodSat:=nodSat.getNextSibling();
            until (not assigned(nodSat));
          end;
        //i:=i+1;
        nodFile:=nodFile.getNextSibling();
      until (not assigned(nodFile));
    end;
end;

procedure TForm1.LoadTreeFromTLEDirectory(TLEDirectory:string; internetOrStaticDir:bool);
var
 inif:TIniFile;
 PathExists:bool;
 fileName:string;
 foundFile:TSearchRec;
 tf:textfile;
 textLine:string;
 curNode,curNode2:TTreeNode;
 defPath:string;
begin
  inif:=TIniFile.Create(inifPath);
  PathExists:=DirectoryExists(TLEDirectory);
  if PathExists then
    begin
      TreeView1.Items.Clear();
      if FindFirst(TLEDirectory+'\*.txt', 0, foundFile)=0 then
        begin
          repeat
            fileName:=foundFile.Name;
            curNode:=TreeView1.Items.Add(nil, fileName);
            curNode.StateIndex:=1;

            AssignFile(tf, TLEDirectory+'\'+fileName);
            Reset(tf);
            //if filesize(tf)<100 then
            ReadLn(tf, textLine);
            while not Eof(tf) do
              begin
                if (textLine[1]<>'1')and(textLine[1]<>'2') then
                  begin
                    curNode2:=TreeView1.Items.AddChild(curNode, textLine);
                    curNode2.StateIndex:=1;
                  end;
                ReadLn(tf, textLine);
              end;
            CloseFile(tf);

          until FindNext(foundFile)<>0;
          findclose(foundFile);

          SpeedButton4Click(nil);
          MakeTLEMemo();
          messagebox(0,'Готово','Готово',0);
          SpeedButton5.Enabled:=true;//это здесь, потому что текущая процедура запускается не только пользователем
          SpeedButton4.Enabled:=true;
          SpeedButton2.Enabled:=true;
          loadFromLocalInternetDirectory:=internetOrStaticDir;
          inif.WriteBool('Options','LastLoadFromLocalInternetDirectory',loadFromLocalInternetDirectory);
          if loadFromLocalInternetDirectory then
            begin
              label1.Caption:='From Internet';
            end
           else
            begin
              label1.Caption:=TLEDirectory;
            end;
        end;
    end;
end;

//Refresh from folder
procedure TForm1.SpeedButton5Click(Sender: TObject);
var
 inif:TIniFile;
 dir:string;
begin
  inif:=TIniFile.Create(inifPath);
  dir:=inif.readString('Options','StaticTLEDirectory','');
  LoadTreeFromTLEDirectory(dir,false);
end;

procedure SaveUrls(inif:TIniFile; urls:TStrings);
var
 i:integer;
 realCount:integer;
begin
  realCount:=0;
  for i:=0 to urls.Count do
    begin
      if length(urls[i])>0 then realCount:=realCount+1;
    end;
  if realCount>0 then
    begin
      inif.EraseSection('TLEURLs');
      inif.WriteInteger('TLEURLs','Count',realCount);
      realCount:=0;
      for i:=0 to urls.Count do
        begin
          if length(urls[i])>0 then
            begin
              inif.WriteString('TLEURLs','url'+IntToStr(realCount),urls[i]);
              realCount:=realCount+1;
            end;
        end;
    end;
end;

function LoadURLs(inif:TIniFile):TStrings;
var
 i:integer;
 realCount:integer;
 curURL:string;
 res:TStrings;
begin
  res:=TStringList.Create();
  realCount:=inif.ReadInteger('TLEURLs','Count',0);
  if realCount>0 then
    begin
      for i:=0 to realCount-1 do
        begin
          curURL:=inif.ReadString('TLEURLs','url'+IntToStr(i),'');
          res.Add(curURL);
        end;
    end;
  LoadURLs:=res;
end;

procedure TForm1.FormCreate(Sender: TObject);
var
 inif:TIniFile;
 dir:string;
begin
  localInternetTLEDir:=extractfiledir(paramstr(0))+'\internettle';
  inifPath:=extractfiledir(paramstr(0))+'\tlecollector.ini';
  inif:=TIniFile.Create(inifPath);
  loadFromLocalInternetDirectory:=inif.ReadBool('Options','LastLoadFromLocalInternetDirectory',True);

  defURLs:=memo2.Text;
  memo2.Lines:=LoadURLs(inif);

  if not DirectoryExists(localInternetTLEDir) then
    begin
      CreateDir(localInternetTLEDir);
    end;
  if loadFromLocalInternetDirectory then
    begin
      LoadTreeFromTLEDirectory(localInternetTLEDir,true);
    end
   else
    begin
      dir:=inif.readString('Options','StaticTLEDirectory','');
      LoadTreeFromTLEDirectory(dir,false);
    end;
end;

procedure TForm1.SpeedButton3Click(Sender: TObject);
var
 sd:TSaveDialog;
 inif:TIniFile;
 defPath:string;
begin
  //inifPath:=extractfiledir(paramstr(0))+'\tlecollector.ini';
  inif:=TIniFile.Create(inifPath);
  defPath:=inif.readString('Options','SaveTLEFile','');
  sd:=TSaveDialog.Create(self);
  sd.InitialDir:=ExtractFileDir(defPath);
  sd.FileName:=defPath;
  sd.Filter:='*.txt|*.txt';
  sd.Options:=sd.Options + [ofOverwritePrompt];
  if sd.Execute then
    begin
      Memo1.Lines.SaveToFile(sd.FileName);
      inif.WriteString('Options','SaveTLEFile',sd.FileName);
    end;
end;

function Download(url:string; idHttp:TIdHTTP):string;
var
 resStr:string;
begin
  resStr:='not downloaded';
  try
      resstr:=idHttp.Get(url);
  except
  end;

  Download:=resStr;
end;

procedure WriteFile(text:string; filepath:string);
var
 fout:TFileStream;
begin
  fout:=TFileStream.Create(filepath,fmCreate);
  fout.Write(text[1],length(text));
  fout.Free();
end;

//Refresh from internet
procedure TForm1.SpeedButton6Click(Sender: TObject);
var
 inif:TIniFile;
 dir:string;

 i,filenamepos:integer;
 dirpath,filepath,url:string;
 tledata:string;
 idHttp:TIdHTTP;
begin
  inif:=TIniFile.Create(inifPath);
  idHttp:=TIdHTTP.Create(self);

  ProgressBar1.Position:=0;
  ProgressBar1.Visible:=true;
  ProgressBar1.Max:=memo2.Lines.Count -1 ;
  for i:=0 to memo2.Lines.Count -1 do
    begin
      url:=memo2.Lines[i];
      filenamepos:=pos('elements/',url)+9;
      filepath:=localInternetTLEDir+'\'+copy(url,filenamepos,length(url)-filenamepos+1);

      tledata:=Download(url,idHttp);
      WriteFile(tledata, filepath);

      ProgressBar1.Position:=i;
      Application.ProcessMessages();
    end;

  ProgressBar1.Visible:=false;
  //MessageBox(0,'Загрузка окончена','Готово',0);






  LoadTreeFromTLEDirectory(localInternetTLEDir,true);
end;

procedure TForm1.SpeedButton7Click(Sender: TObject);
begin
  panel1.Left:=memo1.Left;
  panel1.Top:=memo1.Top;
  panel1.Width:=memo1.Width;
  panel1.Height:=memo1.Height;
  panel1.Visible:=SpeedButton7.Down;
end;

procedure TForm1.SpeedButton9Click(Sender: TObject);
var
 inif:TIniFile;
begin
  inif:=TIniFile.Create(inifPath);
  SaveUrls(inif,memo2.Lines);
end;

procedure TForm1.SpeedButton10Click(Sender: TObject);
var
 urls:string;
 streamURLs:TStringStream;
 inif:TIniFile;
begin
  inif:=TIniFile.Create(inifPath);
  memo2.Lines:=LoadURLs(inif);
end;

procedure TForm1.SpeedButton8Click(Sender: TObject);
begin
  memo2.Text:=defURLs;
end;

end.
