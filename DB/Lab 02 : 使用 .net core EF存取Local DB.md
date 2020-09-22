使用 .net core EF存取MS SQL Local DB
===

## Overview
這個Lab快速介紹如何透過 C# 使用 .net core EF 存取 MS SQL Local DB資料庫，關於Local DB相關資訊可參考底下:

>SQL Server Express LocalDB 是 簡易版的 SQL 資料庫，
適合在開發測試階段使用，相關的資訊可以參考: [here](https://docs.microsoft.com/zh-tw/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15) 

## Prerequisites
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 建立空的 .net core console 專案。
4. 透過 .net core CLI 指令(底下)安裝  Entity Framework Core 工具 (這個動作只需要執行一次)
```bash
dotnet  tool install --global dotnet-ef
```
5. 安裝Visual Studio 2019 [here](https://visualstudio.microsoft.com/zh-hant/vs/)

## Steps

1. 建立 .net core console專案
在命令列模式建立資料夾，接著透過 dotnet new 指令建立專案
```bash
PS D:\> md cqlExpressTest
PS D:\> cd cqlExpressTest
PS D:\coredb> dotnet new console
```
系統會出現類似底下畫面...
```bash
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\test\cqlExpressTest\cqlExpressTest.csproj...
  正在判斷要還原的專案...
  已還原 C:\test\cqlExpressTest\cqlExpressTest.csproj (345 ms 內)。

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
已成功安裝工具 'dotnet-ef' ('3.1.x' 版)。  
PS D:\coredb>
```
如果一切成功，接著為專案安裝套件
```
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

安裝完成後，請開啟Visual Studio 2019，我們使用VS2019內的SQL Server物件總管來建立LocalDB。
![enter image description here](https://i.imgur.com/uwZzAe9.png)

你可以在資料庫節點上，手動加入一個新的資料庫。例如底下我們建立了Test01:
![enter image description here](https://i.imgur.com/X6i492U.png)

接著，我們可以繼續為Test01這個DB建立Table(名稱為Table1，並建立Id, field1, field2這三個field:
![enter image description here](https://i.imgur.com/B7tExc6.png)

建立上述Table的SQL大致如下:
```sql
CREATE TABLE [dbo].[Table1	] (
    [Id]     INT           IDENTITY (1, 1) NOT NULL,
    [Field1] NVARCHAR (50) NULL,
    [Field2] NVARCHAR (50) NULL,
    PRIMARY KEY CLUSTERED ([Id] ASC)
);
```

完成後，可以透過『檢視資料』的功能檢視或新增、編輯修改資料：
![enter image description here](https://i.imgur.com/Y5jotCT.png)
 
在資料表中準備好資料後，就可以嘗試使用了。

MS SQL Local DB的連線字串大概長的像底下這樣:
```
Server=(localdb)\MSSQLLocalDB;Initial Catalog=資料庫名稱;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False
```
 我們可以用上面這樣的連線字串，搭配 scaffold 指令:
```
dotnet ef dbcontext scaffold "連線字串" Microsoft.EntityFrameworkCore.SqlServer
```
組出所需要的命令，例如:
```
dotnet ef dbcontext scaffold "Server=(localdb)\MSSQLLocalDB;Initial Catalog=資料庫名稱;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False" Microsoft.EntityFrameworkCore.SqlServer --output-dir Models
```
請在Terminal執行上述指令。

如果成功，你會發現dotnet ef已經為您在專案中的Models資料夾底下，建立的ORM的db context類別(下圖A)：
![enter image description here](https://i.imgur.com/EE3DFPm.png)

試著建置(dotnet build)一下，並且接著在main.cs中撰寫如上圖B的程式碼:
```csharp
using System;
using System.Linq;

namespace cqlExpressTest
{
    class Program
    {
        static void Main(string[] args)
        {
            var db=new Models.test01Context();
            var ret=from c in db.Table1
            select c;

            foreach (var item in ret)
            {
                Console.WriteLine ($"NO: {item.Id} field1: {item.Field1} field2: {item.Field2}");
            }
        }
    }
}

```
儲存後，試著運行:
```
dotnet run
```

 執行後，將會取得資料庫中的內容，並顯示出來：  
    ![enter image description here](https://i.imgur.com/RiZfDTz.png)

您已成功的使用MS SQL Local DB了。 

相關參考資料
---
[SQL Server Express LocalDB](https://docs.microsoft.com/zh-tw/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15)  
[開始使用 EF Core](https://docs.microsoft.com/zh-tw/ef/core/get-started/?tabs=netcore-cli)    
[反向工程](https://docs.microsoft.com/zh-tw/ef/core/managing-schemas/scaffolding?tabs=dotnet-core-cli)  

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。
