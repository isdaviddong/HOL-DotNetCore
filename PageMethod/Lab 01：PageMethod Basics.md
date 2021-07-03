Lab 01：PageMethod Basics
===

## Overview
這個 Lab介紹如何使用  PageMethod 框架快速開發 .net core WebApp應用程式。
![enter image description here](https://i.imgur.com/5mqOYq0.png  )

這是一個以 PageMethod 執行非同步(AJAX)頁面的 BMI 範例。

## Prerequisites
1. 下載安裝 .net core sdk 3.1+ 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 安裝 PageMethod框架範本
dotnet new --install isRock.Web.Core.Razor.Example 

## Steps
1. 在command line輸入 dotnet --version，確定您是 .net core 3.x以上版本
```bash
PS C:\> dotnet --version
```
系統會出現類似底下畫面...
```bash
PS C:\> dotnet --version
5.0.202
```

2.接著執行底下指令， 安裝 PageMethod Templates，最低版本不得低於 3.5.16
```bash
PS C:\> dotnet new --install isRock.Web.Core.Razor.Example 
或
PS C:\> dotnet new --install isRock.Web.Core.Razor.Example::3.5.16
```
系統會出現類似底下畫面...
```
PS C:\test\fx> dotnet new --install isRock.Web.Core.Razor.Example::3.5.16
  正在判斷要還原的專案...
  已還原 C:\Users\xxxxx\.templateengine\dotnetcli\v5.0.202\scratch\restore.csproj (32.77 sec 內)。

Templates                                       Short Name           Language    Tags
----------------------------------------------  -------------------  ----------  ----------------------
Console Application                             console              [C#],F#,VB  Common/Console
Class library                                   classlib             [C#],F#,VB  Common/Library
WPF Application                                 wpf                  [C#],VB     Common/WPF
WPF Class library                               wpflib               [C#],VB     Common/WPF
WPF Custom Control Library                      wpfcustomcontrollib  [C#],VB     Common/WPF
WPF User Control Library                        wpfusercontrollib    [C#],VB     Common/WPF
Windows Forms App                               winforms             [C#],VB     Common/WinForms
Windows Forms Control Library                   winformscontrollib   [C#],VB     Common/WinForms
Windows Forms Class Library                     winformslib          [C#],VB     Common/WinForms
Worker Service                                  worker               [C#],F#     Common/Worker/Web
Unit Test Project                               mstest               [C#],F#,VB  Test/MSTest
NUnit 3 Test Project                            nunit                [C#],F#,VB  Test/NUnit
NUnit 3 Test Item                               nunit-test           [C#],F#,VB  Test/NUnit
xUnit Test Project                              xunit                [C#],F#,VB  Test/xUnit
Razor Component                                 razorcomponent       [C#]        Web/ASP.NET
Razor Page                                      page                 [C#]        Web/ASP.NET
MVC ViewImports                                 viewimports          [C#]        Web/ASP.NET
MVC ViewStart                                   viewstart            [C#]        Web/ASP.NET
isRock.Web.Core.Razor.Template.isGoogleSSOPage  isGoogleSSOPage      [C#]        Web/ASP.NET
isRock.Web.Core.Razor.Template.isLineLoginPage  isLineLoginPage      [C#]        Web/ASP.NET
isRock.Web.Core.Razor.Template.isLoginPage      isLoginPage          [C#]        Web/ASP.NET
isRock.Web.Core.Razor.Template.isMSSSOPage      isMSSSOPage          [C#]        Web/ASP.NET
isRock.Web.Core.Razor.Example                   pagemethodexample    [C#]        Web/ASP.NET
Blazor Server App                               blazorserver         [C#]        Web/Blazor
Blazor WebAssembly App                          blazorwasm           [C#]        Web/Blazor/WebAssembly
ASP.NET Core Empty                              web                  [C#],F#     Web/Empty
ASP.NET Core Web App (Model-View-Controller)    mvc                  [C#],F#     Web/MVC
ASP.NET Core Web App                            webapp               [C#]        Web/MVC/Razor Pages
ASP.NET Core with Angular                       angular              [C#]        Web/MVC/SPA
ASP.NET Core with React.js                      react                [C#]        Web/MVC/SPA
ASP.NET Core with React.js and Redux            reactredux           [C#]        Web/MVC/SPA
Razor Class Library                             razorclasslib        [C#]        Web/Razor/Library
ASP.NET Core Web API                            webapi               [C#],F#     Web/WebAPI
ASP.NET Core gRPC Service                       grpc                 [C#]        Web/gRPC
dotnet gitignore file                           gitignore                        Config
global.json file                                globaljson                       Config
NuGet Config                                    nugetconfig                      Config
Dotnet local tool manifest file                 tool-manifest                    Config
Web Config                                      webconfig                        Config
Solution File                                   sln                              Solution
Protocol Buffer File                            proto                            Web/gRPC
```
你會發現其中應該會有 pagemethodexample 或 pagemethodexample 範本。

3.接著，請建立資料夾，並在資料夾中建立 .net core web app專案
```bash
md fx
cd fx
dotnet new webapp
```
應該會看到類似底下畫面
```
PS C:\test\fx> dotnet new webapp
The template "ASP.NET Core Web App" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore/5.0-third-party-notices for details.

Processing post-creation actions...
Running 'dotnet restore' on C:\test\fx\fx.csproj...
  正在判斷要還原的專案...
  已還原 C:\test\fx\fx.csproj (121 ms 內)。
Restore succeeded.
```
接著請執行底下命令 dotnet new pagemethodexample
```
PS C:\test\fx> dotnet new pagemethodexample
The template "isRock.Web.Core.Razor.Example" was created successfully.
```
接著執行  dotnet add package isRock.Web.Core.Razor  
```
PS C:\test\fx> dotnet add package isRock.Web.Core.Razor
  正在判斷要還原的專案...
  Writing C:\Users\twdav\AppData\Local\Temp\tmpA1B6.tmp
info : 正在將套件 'isRock.Web.Core.Razor' 的 PackageReference 新增至專案 'C:\test\fx\fx.csproj'。
info :   GET https://api.nuget.org/v3/registration5-gz-semver2/isrock.web.core.razor/index.json
info :   OK https://api.nuget.org/v3/registration5-gz-semver2/isrock.web.core.razor/index.json 1102 毫秒
info : 正在還原 C:\test\fx\fx.csproj 的封裝...
info : 套件 'isRock.Web.Core.Razor' 與專案 'C:\test\fx\fx.csproj' 中的所有架構相容。
info : 已將套件 'isRock.Web.Core.Razor' 版本 '3.1.5' 的 PackageReference 新增至檔案 'C:\test\fx\fx.csproj'。
info : 正在認可還原...
info : 正在產生 MSBuild 檔案 C:\test\fx\obj\fx.csproj.nuget.g.props。
info : 正在將資產檔案寫入磁碟。路徑: C:\test\fx\obj\project.assets.json
log  : 已還原 C:\test\fx\fx.csproj (2.29 sec 內)。
```
最後建置該專案
```
PS C:\test\fx> dotnet build
Microsoft (R) Build Engine for .NET 16.9.0+57a23d249 版
Copyright (C) Microsoft Corporation. 著作權所有，並保留一切權利。

  正在判斷要還原的專案...
  所有專案都在最新狀態，可進行還原。
  fx -> C:\test\fx\bin\Debug\net5.0\fx.dll
  fx -> C:\test\fx\bin\Debug\net5.0\fx.Views.dll

建置成功。
    0 個警告
    0 個錯誤

經過時間 00:00:02.31
```
如果沒有問題，就可以執行 dotnet run 運行該網站：
```
PS C:\test\fx> dotnet run
正在建置...
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\test\fx
```
成功運行後，您即可輸入網址
```
https://localhost:5001/examples/sample
```
執行結果如下：
![enter image description here](https://i.imgur.com/5mqOYq0.png   )

這是一個以 PageMethod 執行非同步(AJAX)頁面的 BMI 範例。輸入身高體重後，點選計算，會出現BMI計算結果。
網址:  https://localhost:5001/examples/SampleWithLayout 則是有搭配 Layout的版本：
![enter image description here](https://i.imgur.com/oMqVsCM.png)

可檢視 examples資料夾下的 .cshtml頁面比較差異。

## 關於程式碼
關於程式碼的部分，請參考 /examples/SampleWithLayout.cshtml.cs
請特別留意，WebApp繼承的頁面是 isPageModel(而非PageModel)：
![enter image description here](https://i.imgur.com/dcuXEzM.png)
此外，CalBmi這個Method掛上了 [PageMethod] 這個 attribute。這也是該method之所以可以被前端Button呼叫的原因，我們看前端頁面 SampleWithLayout.cshtml。 前端頁面中，按鈕 ButtonCal Click時，會執行到 ExecutePageMethod這個方法：
![enter image description here](https://i.imgur.com/LZNtCeH.png)

該方法會執行後端的 PageMethod，並且將類似底下的JSON參數 { height:  170 , weight:  70 }，傳遞給後端的 PageMethod：
![enter image description here](https://i.imgur.com/wDJRDkb.png)
這也是後端para物件可以接收到前端傳來的 height與weight的原因。最後，再將計算結果result，回傳給前端。

前端收到結果後，再顯示出來：
![enter image description here](https://i.imgur.com/bK8LFAt.png)

這個行為，形成了PageMethod的主要運行結構。

相關參考資料
---
如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。
