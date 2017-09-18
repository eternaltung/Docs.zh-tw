---
title: "移轉設定"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 8468d859-ff32-4a92-9e62-08c4a9e36594
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: 62660f7e58467a69f540966df188747b6fde57fe
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-configuration"></a><span data-ttu-id="d3515-103">移轉設定</span><span class="sxs-lookup"><span data-stu-id="d3515-103">Migrating Configuration</span></span>

<span data-ttu-id="d3515-104">由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="d3515-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="d3515-105">前一個發行項，開始[將 ASP.NET MVC 專案移轉至 ASP.NET Core MVC](mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="d3515-105">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="d3515-106">在本文中，我們可以移轉設定。</span><span class="sxs-lookup"><span data-stu-id="d3515-106">In this article, we migrate configuration.</span></span>

[<span data-ttu-id="d3515-107">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="d3515-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)

## <a name="setup-configuration"></a><span data-ttu-id="d3515-108">安裝程式組態</span><span class="sxs-lookup"><span data-stu-id="d3515-108">Setup Configuration</span></span>

<span data-ttu-id="d3515-109">不會再使用 ASP.NET Core *Global.asax*和*web.config*舊版 ASP.NET 所使用的檔案。</span><span class="sxs-lookup"><span data-stu-id="d3515-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="d3515-110">在舊版 ASP.NET 中，應用程式啟動邏輯已放置在`Application_StartUp`方法內*Global.asax*。</span><span class="sxs-lookup"><span data-stu-id="d3515-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="d3515-111">接著，在 ASP.NET MVC *Startup.cs*檔案會包含在專案的根目錄，和應用程式啟動時呼叫。</span><span class="sxs-lookup"><span data-stu-id="d3515-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="d3515-112">ASP.NET Core 已完全採用這種方法，藉由放置在所有的啟動邏輯*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="d3515-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="d3515-113">*Web.config*也已取代檔案中 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="d3515-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="d3515-114">設定本身可以現在設定，做為應用程式的啟動程序中所述的一部分*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="d3515-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="d3515-115">組態仍然可以利用 XML 檔案，但通常 ASP.NET Core 專案將組態值的檔案中放置 JSON 格式，例如*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="d3515-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="d3515-116">ASP.NET Core 組態系統可以也很容易存取環境變數，可提供更安全完善的位置環境專屬的值。</span><span class="sxs-lookup"><span data-stu-id="d3515-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="d3515-117">這是特別有用，例如連接字串和不能簽入至原始檔控制的 API 金鑰的密碼。</span><span class="sxs-lookup"><span data-stu-id="d3515-117">This is especially true for secrets like connection strings and API keys that should not be checked into source control.</span></span> <span data-ttu-id="d3515-118">請參閱[組態](../fundamentals/configuration.md)若要深入了解 ASP.NET Core 中組態。</span><span class="sxs-lookup"><span data-stu-id="d3515-118">See [Configuration](../fundamentals/configuration.md) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="d3515-119">如本文中，我們開始使用中的部分移轉 ASP.NET Core 專案[前一篇文章](mvc.md)。</span><span class="sxs-lookup"><span data-stu-id="d3515-119">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="d3515-120">安裝程式組態，將下列建構函式和屬性加入至*Startup.cs*專案的根目錄中找到的檔案：</span><span class="sxs-lookup"><span data-stu-id="d3515-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

<span data-ttu-id="d3515-121">[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span><span class="sxs-lookup"><span data-stu-id="d3515-121">[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span></span>

<span data-ttu-id="d3515-122">請注意，此時， *Startup.cs*檔案將不會編譯，因為我們還是需要新增下列`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="d3515-122">Note that at this point, the *Startup.cs* file will not compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="d3515-123">新增*appsettings.json*使用適當的項目範本的專案根目錄的檔案：</span><span class="sxs-lookup"><span data-stu-id="d3515-123">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![新增 AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="d3515-125">從 web.config 移轉組態設定</span><span class="sxs-lookup"><span data-stu-id="d3515-125">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="d3515-126">我們的 ASP.NET MVC 專案包含必要的資料庫連接字串中*web.config*，請在`<connectionStrings>`項目。</span><span class="sxs-lookup"><span data-stu-id="d3515-126">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="d3515-127">在 ASP.NET Core 專案中，我們將此資訊儲存在*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="d3515-127">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="d3515-128">開啟*appsettings.json*，並注意它已經包含下列：</span><span class="sxs-lookup"><span data-stu-id="d3515-128">Open *appsettings.json*, and note that it already includes the following:</span></span>

<span data-ttu-id="d3515-129">[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="d3515-129">[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span></span>


<span data-ttu-id="d3515-130">在上面所述的反白顯示列，請變更資料庫的名稱**_CHANGE_ME**至您的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="d3515-130">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="d3515-131">總結</span><span class="sxs-lookup"><span data-stu-id="d3515-131">Summary</span></span>

<span data-ttu-id="d3515-132">ASP.NET Core 置於單一檔案，以必要的服務和相依性可以定義，以及設定的所有應用程式的啟動邏輯。</span><span class="sxs-lookup"><span data-stu-id="d3515-132">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="d3515-133">它會取代*web.config*彈性設定功能，可以運用各種不同的檔案格式，例如 JSON，如環境變數的檔案。</span><span class="sxs-lookup"><span data-stu-id="d3515-133">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>