使用 .net core ef 存取既有的 Azure Database(DB First)
===

## Overview
這個lab介紹如何透過 C# 使用 .net core ef 存取既有的SQL Azure資料庫

## Prerequisites
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 建立空的 .net core console 專案。
4. 透過.net core CLI 指令(底下)安裝  Entity Framework Core 工具
```dos
dotnet  tool install --global dotnet-ef
```
5. 開發環境安裝好 SQL Server Management Studio (建議) [https://docs.microsoft.com/zh-tw/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15](https://docs.microsoft.com/zh-tw/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)

## Steps

1. 建立 .net core console專案
在命令列模式建立資料夾，接著透過 dotnet new 指令建立專案
```bash
PS D:\> md coredb
PS D:\> cd coredb
PS D:\coredb> dotnet new console
```
系統會出現類似底下畫面...
```bash
The template "Console Application" was created successfully.  
  
Processing post-creation actions...  
Running 'dotnet restore' on D:\coredb\coredb.csproj...  
D:\coredb\coredb.csproj 的還原於 385.19 ms 完成。  
  
Restore succeeded.
```

2.接著執行底下指令， 安裝  Entity Framework Core CLI工具 套件...
(這個動作只需要執行一次)
```bash
dotnet  tool install --global dotnet-ef
```
系統會出現類似底下畫面...
```bash
PS D:\coredb> dotnet tool install --global dotnet-ef  
您可以使用下列命令來叫用工具: dotnet-ef  
已成功安裝工具 'dotnet-ef' ('3.1.7' 版)。  
PS D:\coredb>
```
如果一切成功，接著為專案安裝套件
```
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

安裝完成後，請準備好SQL Azure資料庫相關連線資料。  
例如，底下是可試用的Azure DB:
```
azure 資料庫位置url: exampledb2020.database.windows.net
DB Name: cNorthWind
帳號: acc01
密碼: vdsk@31dxbZf
```
有了上述相關資料，你可以組出可用的連線字串，例如:
```
連線字串: 
Server=tcp:exampledb2020.database.windows.net,1433;Initial Catalog=cNorthWind;Persist Security Info=False;User ID=acc01;Password=vdsk@31dxbZf;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```
接著，我們在專案資料夾中，鍵入底下指令:
```
dotnet ef dbcontext scaffold "連線字串" Microsoft.EntityFrameworkCore.SqlServer
```
例如:
```
dotnet ef dbcontext scaffold "Server=tcp:exampledb2020.database.windows.net,1433;Initial Catalog=cNorthWind;Persist Security Info=False;User ID=acc01;Password=vdsk@31dxbZf;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;" Microsoft.EntityFrameworkCore.SqlServer
```
如果成功，你會發現dotnet ef已經為您在專案中建立的ORM的db context類別：

![enter image description here](https://i.imgur.com/ZsU8TzN.png)

試著建置一下:
```
dotnet build
```
成功後，你可以試著在program.cs中撰寫底下程式：
```csharp
using System;
using System.Linq;

namespace t1
{
    class Program
    {
        static void Main(string[] args)
        {
            //Console.WriteLine("Hello World!");
            var db = new cNorthWindContext();
            var ret = from c in db.PhoneBook
                      select c;

            foreach (var item in ret)
            {
                Console.WriteLine($"{item.Name} - {item.Tel} - {item.Address} ");
            }
        }
    }
}

```
 執行後，將會取得資料庫中的內容，並顯示出來：
 
 ![enter image description here](https://i.imgur.com/mySdpnl.png)
 
相關參考資料
---
[https://docs.microsoft.com/zh-tw/ef/core/get-started/?tabs=netcore-cli](https://docs.microsoft.com/zh-tw/ef/core/get-started/?tabs=netcore-cli)
[https://docs.microsoft.com/zh-tw/ef/core/managing-schemas/scaffolding?tabs=dotnet-core-cli](https://docs.microsoft.com/zh-tw/ef/core/managing-schemas/scaffolding?tabs=dotnet-core-cli)

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。
