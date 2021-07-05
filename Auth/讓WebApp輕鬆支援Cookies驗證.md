
讓WebApp輕鬆支援Cookies驗證
===

## Overview
這個 Lab介紹如何透過 C# 使用 .net core WebApp(Razor Page Model) 以cookie authentication方式建立登入功能。
<img src=https://arock.blob.core.windows.net/blogdata202107/EC092504-FB5B-4059-ADAD-A58C03D15309.GIF>

## Prerequisites
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 安裝 nuget 範本套件
```
dotnet new --install isRock.Web.Core.Razor.Example
```
4. 建立空的 .net core WebApp專案。 

## Steps
1. command line輸入 dotnet --version，確定您是 .net core 3.x以上版本
```bash
PS C:\> dotnet --version
```
系統會出現類似底下畫面...
```bash
PS :\> dotnet --version
5.0.202
```

2. 確定有安裝 .net core之後，在命令列模式透過底下指令安裝 nuget 範本
```
dotnet new --install isRock.Web.Core.Razor.Example
```
若是第一次安裝(或成功更新)，系統應當會出現類似底下畫面
>
> Determining projects to restore...
  Restored C:\Users\vmadmin\.templateengine\dotnetcli\v3.1.402\scratch\restore.csproj (in 2.66 sec).

3. 建立 .net core WebApp專案
在命令列模式建立資料夾，接著透過 dotnet new 指令建立專案
```bash
PS DC:\testcode\> md test
PS CD:\testcode\> cd testlogin
PS CD:\testcode\testlogincoredb> dotnet new WebApp
```
系統會出現類似底下畫面...
```bash
PS C:\testcode\testlogin> dotnet new webapp
The template "ASP.NET Core Web App" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore/-third-party-notices for details.

Processing post-creation actions...
Running 'dotnet restore' on C:\testcode\testlogin\testlogin.csproj...
  正在判斷要還原的專案...
  已還原 C:\testcode\testlogin\testlogin.csproj (157 ms 內)。
Restore succeeded.

```
2. 在專案資料夾中，執行
```
dotnet new islogin
```
系統會出現類似底下畫面...
```bash
PS C:\testcode\testlogin> dotnet new isloin
The template "isRock.Web.Core.Razor.Template.isLoginPage" was created successfully.
```
3.  使用 鍵入 ode . 指令以VS Code開啟專案
```
code .
```
 執行後會開啟VS Code:![enter image description here](https://i.imgur.com/oYzyBMM.png) 

4. 為了讓專案支援 cookie authentication, 我們需要找到 startup.cs，將底下程式碼做些調整：
```js
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddRazorPages();
        }
```
修改為
```js
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddRazorPages();
            #region ForSignIn
            services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme).AddCookie();
            #endregion
        }
```
並且將
```js
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseStaticFiles();

            app.UseRouting();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapRazorPages();
            });
        }
```
修改為:
```js
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts();
            }

            app.UseHttpsRedirection();
            app.UseStaticFiles();

            app.UseRouting();

            app.UseAuthorization();
            #region ForSignIn
            app.UseCookiePolicy();
            app.UseAuthentication();
            #endregion
            app.UseEndpoints(endpoints =>
            {
                endpoints.MapRazorPages();
            });
        }
```
另外必須引用
```c#
#region ForSignIn
using Microsoft.AspNetCore.Authentication.Cookies;
#endregion
```
8. 最後，修改 /Shared/_Layout.cshtml 以配合登入UI，將底下程式碼...
```html
    <header>
        <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
            <div class="container">
                <a class="navbar-brand" asp-area="" asp-page="/Index">testwebapp</a>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
                        aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                    <ul class="navbar-nav flex-grow-1">
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-page="/Index">Home</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-page="/Privacy">Privacy</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </header>
```
修改為...
```html
    <header>
        <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
            <div class="container">
                <a class="navbar-brand" asp-area="" asp-page="/Index">testwebapp</a>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse"
                    aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                    <ul class="navbar-nav flex-grow-1">
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-page="/Index">Home</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-page="/Privacy">Privacy</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-page="/Login">Login/Logout</a>
                        </li>
                    </ul>
                </div>
                <div>
                    current user : @User.Identity.Name
                </div>
            </div>
        </nav>
    </header>
```
完成後(記得儲存所有修改)，接著使用 dotnet run 執行該程式，運行起來後，你可以在底下畫面點選 Login：
![enter image description here](https://i.imgur.com/NLga3Hu.png)

系統並沒有限制帳號密碼，你可以輸入任何帳號，都會成功登入。
您可以觀察Login.cshtml.cs以了解相關程式碼。

當您登入之後，不管如何切換頁面，都會發現該用戶都處於登入狀態：
 ![enter image description here](http://arock.blob.core.windows.net/blogdata202107/EC092504-FB5B-4059-ADAD-A58C03D15309.GIF)

用戶如需登出，點選『Logout』即可。

相關參考資料
---
https://docs.microsoft.com/zh-tw/aspnet/core/security/authentication/?view=aspnetcore-5.0

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。

Author Info.
===
https://www.studyhost.tw/NewCourses/%E8%AC%9B%E5%B8%AB%E7%B0%A1%E4%BB%8B-David
