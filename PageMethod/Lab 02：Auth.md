Lab 02：PageMethod Auth
===

## Overview
這個 Lab介紹如何使用  PageMethod 框架中的身分驗證功能。


## Prerequisites
1. 下載安裝 .net core sdk 3.1+ 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 安裝 PageMethod框架範本
dotnet new --install isRock.Web.Core.Razor.Example 
4. 建立WebApp專案
5. 安裝 isRock.Web.Core.Razor 套件
6. 安裝 isPageMethod  與 isLogin 範本

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
你會發現其中應該會有 pagemethodexample 或 isPageMethodExample 範本。

3.接著，請建立資料夾，並在資料夾中建立 .net core web app專案
```bash
md testAuth
cd testAuth
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
接著請執行底下命令 dotnet new isPageMethodExample
```
PS C:\test\fx> dotnet new isPageMethodExample
The template "isRock.Web.Core.Razor.Example" was created successfully.
```

接著請執行底下命令 dotnet new isloginpage
```
PS C:\test\fx> dotnet new isloginpage
The template "isRock.Web.Core.Razor.Example" was created successfully.
```
接著請開啟VS Code，依照專物中 readme-isLoginPage.txt 文件做必要的程式碼修正。

接著，請執行  dotnet add package isRock.Web.Core.Razor  
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

成功運行後， 你可以點選 Login做登入，登入時可用任意帳號密碼：
![enter image description here](https://i.imgur.com/xdyhGMs.png)

登入後，你會發現用戶身分出現在畫面右上角：
![enter image description here](https://i.imgur.com/cEtrsi7.png)

您可以透過網址 https://localhost:5001/examples/sample 查看測試頁面，你會發現，當輸入身高體重，會算出BMI
![enter image description here](https://i.imgur.com/TkcnRIL.png)

請終止程式運行，進入 sample.cshtml.cs ，將程式碼改為：
![enter image description here](https://i.imgur.com/J21rS8X.png)
你會發現，當EnableUsers設為 eric時，若登入者不是Eric，則會收錯誤。當設定 Auth() 屬性，用戶卻沒有登入時，也會收到錯誤。 

相關參考資料
---
如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。
