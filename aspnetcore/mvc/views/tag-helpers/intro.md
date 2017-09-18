---
title: "在 ASP.NET Core 標記協助程式"
author: rick-anderson
description: "瞭解什麼標記協助程式，以及如何在 ASP.NET Core 中使用它們。"
keywords: "ASP.NET Core，標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53a31ed8ca6ff24a19a33a56c3a896aa58cbb62a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="92d1d-104">在 ASP.NET Core 標記協助程式簡介</span><span class="sxs-lookup"><span data-stu-id="92d1d-104">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="92d1d-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92d1d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="92d1d-106">標記協助程式有哪些？</span><span class="sxs-lookup"><span data-stu-id="92d1d-106">What are Tag Helpers?</span></span>

<span data-ttu-id="92d1d-107">標記協助程式可讓伺服器端程式碼參與建立和呈現 HTML Razor 檔案中的項目。</span><span class="sxs-lookup"><span data-stu-id="92d1d-107">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="92d1d-108">例如，內建`ImageTagHelper`可以將版本號碼附加至映像名稱。</span><span class="sxs-lookup"><span data-stu-id="92d1d-108">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="92d1d-109">每當映像變更時，伺服器會產生新的唯一版本映像，讓用戶端保證以取得目前的映像 （而不是過期的快取映像）。</span><span class="sxs-lookup"><span data-stu-id="92d1d-109">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="92d1d-110">有許多內建的標記協助程式的一般工作-例如建立表單、 連結、 載入資產和更多的和更多可用公用 GitHub 儲存機制中以及 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="92d1d-110">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="92d1d-111">標記協助程式以 C# 中，撰寫，與它們目標項目名稱、 屬性名稱，或父標記的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="92d1d-111">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="92d1d-112">例如，內建`LabelTagHelper`可鎖定目標 HTML`<label>`項目時`LabelTagHelper`屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="92d1d-112">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="92d1d-113">如果您已熟悉[HTML Helper](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)，標記協助減少 Razor 檢視的 HTML 和 C# 之間的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="92d1d-113">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="92d1d-114">在許多情況下，HTML Helper 提供的替代方式給特定的標記協助程式，但請務必識別標記協助程式不會取代 HTML Helper，並不是標記協助程式的每個 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="92d1d-114">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="92d1d-115">[標記協助程式相較於 HTML Helper](#tag-helpers-compared-to-html-helpers)說明詳細的差異。</span><span class="sxs-lookup"><span data-stu-id="92d1d-115">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="92d1d-116">標記協助程式所提供的內容</span><span class="sxs-lookup"><span data-stu-id="92d1d-116">What Tag Helpers provide</span></span>

<span data-ttu-id="92d1d-117">**HTML 易用的開發經驗**在大部分的情況，Razor 標記使用標記協助程式看起來像標準 HTML。</span><span class="sxs-lookup"><span data-stu-id="92d1d-117">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="92d1d-118">熟悉 HTML/CSS/JavaScript 前端設計工具可以編輯 Razor，無需了解 C# Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="92d1d-118">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="92d1d-119">**豐富的 IntelliSense 環境，以建立 HTML 和 Razor 標記**這種清晰對比，以 HTML Helper，伺服器端建立 Razor 檢視中的標記之前的方法。</span><span class="sxs-lookup"><span data-stu-id="92d1d-119">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="92d1d-120">[標記協助程式相較於 HTML Helper](#tag-helpers-compared-to-html-helpers)說明詳細的差異。</span><span class="sxs-lookup"><span data-stu-id="92d1d-120">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="92d1d-121">[標記協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)說明 IntelliSense 環境。</span><span class="sxs-lookup"><span data-stu-id="92d1d-121">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="92d1d-122">甚至 Razor C# 語法有經驗的開發人員會更有效率使用比撰寫 C# Razor 標記的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="92d1d-122">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="92d1d-123">**讓您更具生產力且能夠產生更強固、 可靠的方式和維護的程式碼使用伺服器上的資訊僅適用於**比方說，在過去在更新映像宗旨已變更的映像名稱，當您變更映像。</span><span class="sxs-lookup"><span data-stu-id="92d1d-123">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="92d1d-124">影像應該主動快取基於效能考量，除非您變更影像的名稱，就可能會取得過時複本的用戶端。</span><span class="sxs-lookup"><span data-stu-id="92d1d-124">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="92d1d-125">在過去，編輯映像之後，名稱已變更，並需要更新的 web 應用程式中的映像的每個參考。</span><span class="sxs-lookup"><span data-stu-id="92d1d-125">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="92d1d-126">不只是這非常人工大量，也很容易發生 （您可能遺漏了參考、 不小心輸入錯誤的字串，依此類推） 錯誤內建`ImageTagHelper`可以這樣做為您自動。</span><span class="sxs-lookup"><span data-stu-id="92d1d-126">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="92d1d-127">`ImageTagHelper`可以將版本附加數字的映像名稱，所以每當映像變更時，伺服器會自動產生新的唯一版本映像。</span><span class="sxs-lookup"><span data-stu-id="92d1d-127">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="92d1d-128">用戶端保證取得目前的映像。</span><span class="sxs-lookup"><span data-stu-id="92d1d-128">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="92d1d-129">使用此健全性和人力節省來自基本上可用`ImageTagHelper`。</span><span class="sxs-lookup"><span data-stu-id="92d1d-129">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="92d1d-130">大部分的內建的標記協助程式和目標現有的 HTML 元素的元素提供伺服器端的屬性。</span><span class="sxs-lookup"><span data-stu-id="92d1d-130">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="92d1d-131">例如，`<input>`所使用的檢視中的許多項目的*檢視/帳戶*資料夾包含`asp-for`屬性，它會擷取到轉譯的 HTML 與指定的模型屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="92d1d-131">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="92d1d-132">下列的 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="92d1d-132">The following Razor markup:</span></span>

```html
<label asp-for="Email"></label>
```

<span data-ttu-id="92d1d-133">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="92d1d-133">Generates the following HTML:</span></span>

```html
<label for="Email">Email</label>
```

<span data-ttu-id="92d1d-134">`asp-for`屬性可由`For`屬性`LabelTagHelper`。</span><span class="sxs-lookup"><span data-stu-id="92d1d-134">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="92d1d-135">請參閱[撰寫標記協助程式](authoring.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="92d1d-135">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="92d1d-136">管理標記協助程式範圍</span><span class="sxs-lookup"><span data-stu-id="92d1d-136">Managing Tag Helper scope</span></span>

<span data-ttu-id="92d1d-137">標記協助程式範圍由多種`@addTagHelper`， `@removeTagHelper`，而"！"退出字元。</span><span class="sxs-lookup"><span data-stu-id="92d1d-137">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="92d1d-138">`@addTagHelper`提供標記協助程式</span><span class="sxs-lookup"><span data-stu-id="92d1d-138">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="92d1d-139">如果您建立新 ASP.NET Core web 應用程式名為*AuthoringTagHelpers* （使用無驗證），下列*Views/_ViewImports.cshtml*檔案會加入至您的專案：</span><span class="sxs-lookup"><span data-stu-id="92d1d-139">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

<span data-ttu-id="92d1d-140">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]</span><span class="sxs-lookup"><span data-stu-id="92d1d-140">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]</span></span>

<span data-ttu-id="92d1d-141">`@addTagHelper`指示詞使標記協助程式可供檢視。</span><span class="sxs-lookup"><span data-stu-id="92d1d-141">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="92d1d-142">在此案例中，檢視檔案是*Views/_ViewImports.cshtml*，預設會繼承中的所有檢視檔案*檢視*資料夾和子的目錄; 可用標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="92d1d-142">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="92d1d-143">上述程式碼使用的萬用字元語法 (「\*") 來指定在指定的組件中的所有標記協助程式 (*Microsoft.AspNetCore.Mvc.TagHelpers*) 將可在每個檢視檔案*檢視*目錄或子目錄。</span><span class="sxs-lookup"><span data-stu-id="92d1d-143">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="92d1d-144">之後的第一個參數`@addTagHelper`指定載入標記協助程式 (我們使用 「\*"的所有標記協助程式)，而第二個參數會指定包含標記協助程式的組件 」 Microsoft.AspNetCore.Mvc.TagHelpers"。</span><span class="sxs-lookup"><span data-stu-id="92d1d-144">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="92d1d-145">*Microsoft.AspNetCore.Mvc.TagHelpers*是內建 ASP.NET Core 標記協助程式組件。</span><span class="sxs-lookup"><span data-stu-id="92d1d-145">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="92d1d-146">若要公開所有將在此專案中的標記協助程式 (這會建立名為組件*AuthoringTagHelpers*)，您會使用下列：</span><span class="sxs-lookup"><span data-stu-id="92d1d-146">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

<span data-ttu-id="92d1d-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="92d1d-147">[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]</span></span>

<span data-ttu-id="92d1d-148">如果您的專案包含`EmailTagHelper`與預設的命名空間 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，您可以提供標記協助程式的完整限定的名稱 (FQN):</span><span class="sxs-lookup"><span data-stu-id="92d1d-148">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="92d1d-149">若要新增的檢視使用 FQN 標記協助程式，您先新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，並接著組件名稱 (*AuthoringTagHelpers*)。</span><span class="sxs-lookup"><span data-stu-id="92d1d-149">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="92d1d-150">大部分的開發人員想要使用 「\*"的萬用字元語法。</span><span class="sxs-lookup"><span data-stu-id="92d1d-150">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="92d1d-151">萬用字元語法可讓您插入萬用字元"\*"FQN 中的尾碼。</span><span class="sxs-lookup"><span data-stu-id="92d1d-151">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="92d1d-152">例如，任何下列指示詞會顯示`EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="92d1d-152">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="92d1d-153">如先前所述，新增`@addTagHelper`指示詞加入*Views/_ViewImports.cshtml*檔案讓標記協助程式中的所有檢視檔案*檢視*目錄和子目錄。</span><span class="sxs-lookup"><span data-stu-id="92d1d-153">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="92d1d-154">您可以使用`@addTagHelper`指示詞中特定的檢視檔案，如果您想要選擇加入以公開這些檢視表標記協助專家。</span><span class="sxs-lookup"><span data-stu-id="92d1d-154">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="92d1d-155">`@removeTagHelper`移除標記協助程式</span><span class="sxs-lookup"><span data-stu-id="92d1d-155">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="92d1d-156">`@removeTagHelper`具有相同的兩個參數，做為`@addTagHelper`，並且會移除先前新增標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="92d1d-156">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="92d1d-157">例如，`@removeTagHelper`從檢視套用至特定檢視移除指定的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="92d1d-157">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="92d1d-158">使用`@removeTagHelper`中*Views/Folder/_ViewImports.cshtml*檔案從所有檢視中都移除指定的標記協助程式*資料夾*。</span><span class="sxs-lookup"><span data-stu-id="92d1d-158">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="92d1d-159">控制標記協助程式範圍*_ViewImports.cshtml*檔案</span><span class="sxs-lookup"><span data-stu-id="92d1d-159">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="92d1d-160">您可以加入*_ViewImports.cshtml*任何檢視資料夾中，檢視引擎要套用至和指示詞從這兩個檔案和*Views/_ViewImports.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="92d1d-160">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="92d1d-161">如果您加入空白*Views/Home/_ViewImports.cshtml*檔，以取得*首頁*檢視，會有任何變更因為*_ViewImports.cshtml*檔案是加總。</span><span class="sxs-lookup"><span data-stu-id="92d1d-161">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="92d1d-162">任何`@addTagHelper`指示詞加入*Views/Home/_ViewImports.cshtml*檔案 (不在預設*Views/_ViewImports.cshtml*檔案) 會公開至檢視這些標記協助程式只能在*首頁*資料夾。</span><span class="sxs-lookup"><span data-stu-id="92d1d-162">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="92d1d-163">選擇退出個別項目</span><span class="sxs-lookup"><span data-stu-id="92d1d-163">Opting out of individual elements</span></span>

<span data-ttu-id="92d1d-164">您可以停用在項目層級，以標記協助程式退出字元標記協助程式 ("！")。</span><span class="sxs-lookup"><span data-stu-id="92d1d-164">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="92d1d-165">例如，`Email`中已停用驗證`<span>`標記協助程式退出字元：</span><span class="sxs-lookup"><span data-stu-id="92d1d-165">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="92d1d-166">您必須套用標記協助程式退出字元的開頭和結尾標記。</span><span class="sxs-lookup"><span data-stu-id="92d1d-166">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="92d1d-167">（在 Visual Studio 編輯器中自動將退出字元加入結尾標記，當您加入一個開頭標記）。</span><span class="sxs-lookup"><span data-stu-id="92d1d-167">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="92d1d-168">加入選擇退出字元之後，此項目和標記協助程式屬性不再顯示在特殊的字型。</span><span class="sxs-lookup"><span data-stu-id="92d1d-168">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="92d1d-169">使用`@tagHelperPrefix`進行明確標記協助程式使用方式</span><span class="sxs-lookup"><span data-stu-id="92d1d-169">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="92d1d-170">`@tagHelperPrefix`指示詞可讓您指定要啟用標記協助程式支援，並讓標記協助程式使用明確標記前置詞字串。</span><span class="sxs-lookup"><span data-stu-id="92d1d-170">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="92d1d-171">例如，您可以加入下列標記以*Views/_ViewImports.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="92d1d-171">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```html
@tagHelperPrefix th:
```
<span data-ttu-id="92d1d-172">在下列程式碼影像，標記協助程式前置詞設定為`th:`，因此只有在使用前置詞的元素`th:`支援標記協助程式 （啟用標記協助程式的項目會有特殊的字型）。</span><span class="sxs-lookup"><span data-stu-id="92d1d-172">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="92d1d-173">`<label>`和`<input>`元素有標記協助程式前置詞且標記協助程式功能，同時`<span>`元素則否。</span><span class="sxs-lookup"><span data-stu-id="92d1d-173">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element does not.</span></span>

![影像](intro/_static/thp.png)

<span data-ttu-id="92d1d-175">相同的階層規則套用至`@addTagHelper`也適用於`@tagHelperPrefix`。</span><span class="sxs-lookup"><span data-stu-id="92d1d-175">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="92d1d-176">標記協助程式的 IntelliSense 支援</span><span class="sxs-lookup"><span data-stu-id="92d1d-176">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="92d1d-177">當您在 Visual Studio 中建立新的 ASP.NET web 應用程式時，它會加入 NuGet 套件 」 Microsoft.AspNetCore.Razor.Tools"。</span><span class="sxs-lookup"><span data-stu-id="92d1d-177">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="92d1d-178">這是封裝會新增標記協助程式工具。</span><span class="sxs-lookup"><span data-stu-id="92d1d-178">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="92d1d-179">請考慮將寫入 HTML`<label>`項目。</span><span class="sxs-lookup"><span data-stu-id="92d1d-179">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="92d1d-180">當您輸入`<l`在 Visual Studio 編輯器中，IntelliSense 會顯示相符項目：</span><span class="sxs-lookup"><span data-stu-id="92d1d-180">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![影像](intro/_static/label.png)

<span data-ttu-id="92d1d-182">不只取得 HTML 說明，但圖示 ("@"符號與在其下的"<>")。</span><span class="sxs-lookup"><span data-stu-id="92d1d-182">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![影像](intro/_static/tagSym.png)

<span data-ttu-id="92d1d-184">識別項目，為標記協助程式的目標。</span><span class="sxs-lookup"><span data-stu-id="92d1d-184">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="92d1d-185">純的 HTML 項目 (例如`fieldset`) 顯示 「 <> 」 圖示。</span><span class="sxs-lookup"><span data-stu-id="92d1d-185">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="92d1d-186">純 HTML`<label>`標記棕色字型的屬性，以紅色顯示 （使用預設 Visual Studio 色彩佈景主題） 的 HTML 標記和屬性值以藍色。</span><span class="sxs-lookup"><span data-stu-id="92d1d-186">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![影像](intro/_static/LableHtmlTag.png)

<span data-ttu-id="92d1d-188">您輸入之後`<label`，IntelliSense 就會列出可用的 HTML/CSS 屬性和標記協助程式為目標屬性：</span><span class="sxs-lookup"><span data-stu-id="92d1d-188">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![影像](intro/_static/labelattr.png)

<span data-ttu-id="92d1d-190">IntelliSense 陳述式完成可讓您輸入 tab 鍵來完成選取的值的陳述式：</span><span class="sxs-lookup"><span data-stu-id="92d1d-190">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![影像](intro/_static/stmtcomplete.png)

<span data-ttu-id="92d1d-192">輸入標記協助程式屬性，因為變更標記和屬性的字型。</span><span class="sxs-lookup"><span data-stu-id="92d1d-192">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="92d1d-193">使用預設 Visual Studio"Blue"或"Light"色彩佈景主題，字型為粗體紫色。</span><span class="sxs-lookup"><span data-stu-id="92d1d-193">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="92d1d-194">如果您使用 「 暗色調 」 佈景主題的字型是粗體藍綠色。</span><span class="sxs-lookup"><span data-stu-id="92d1d-194">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="92d1d-195">使用預設佈景主題取得這份文件中的影像。</span><span class="sxs-lookup"><span data-stu-id="92d1d-195">The images in this document were taken using the default theme.</span></span>

![影像](intro/_static/labelaspfor2.png)

<span data-ttu-id="92d1d-197">您可以輸入 Visual Studio *CompleteWord*快顯 (Ctrl + 空格鍵是[預設](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)置於雙引號內 ("")，而您現在在 C# 中，就像您會在 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="92d1d-197">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="92d1d-198">IntelliSense 會顯示頁面模型上所有的方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="92d1d-198">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="92d1d-199">方法和屬性可用的屬性類型，所以`ModelExpression`。</span><span class="sxs-lookup"><span data-stu-id="92d1d-199">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="92d1d-200">在下列影像中，我編輯`Register` 檢視中，所以`RegisterViewModel`可用。</span><span class="sxs-lookup"><span data-stu-id="92d1d-200">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![影像](intro/_static/intellemail.png)

<span data-ttu-id="92d1d-202">IntelliSense 會列出的屬性和方法在頁面上的模型。</span><span class="sxs-lookup"><span data-stu-id="92d1d-202">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="92d1d-203">豐富的 IntelliSense 環境，可協助您選取的 CSS 類別：</span><span class="sxs-lookup"><span data-stu-id="92d1d-203">The rich IntelliSense environment helps you select the CSS class:</span></span>

![影像](intro/_static/iclass.png)

![影像](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="92d1d-206">相較於 HTML Helper 的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="92d1d-206">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="92d1d-207">標記協助程式附加至 HTML 元素在 Razor 檢視中，雖然[HTML Helper](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)會叫用的方法與 HTML 顛倒 Razor 檢視中。</span><span class="sxs-lookup"><span data-stu-id="92d1d-207">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="92d1d-208">請考慮下列的 Razor 標記 CSS 類別 「 標題 」 建立的 HTML 標籤：</span><span class="sxs-lookup"><span data-stu-id="92d1d-208">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="92d1d-209">在 (`@`) 符號會告知 Razor 這是程式碼啟動。</span><span class="sxs-lookup"><span data-stu-id="92d1d-209">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="92d1d-210">下面兩個參數 ("FirstName"和"名字:") 是字串，因此[IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense)無法幫助。</span><span class="sxs-lookup"><span data-stu-id="92d1d-210">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="92d1d-211">最後的引數：</span><span class="sxs-lookup"><span data-stu-id="92d1d-211">The last argument:</span></span>

```html
new {@class="caption"}
```

<span data-ttu-id="92d1d-212">用來代表屬性的匿名物件。</span><span class="sxs-lookup"><span data-stu-id="92d1d-212">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="92d1d-213">因為**類別**是保留的關鍵字在 C# 中，使用`@`符號，以強制執行 C# 來解譯"@class="做為符號 （屬性名稱）。</span><span data-stu-id="92d1d-213">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="92d1d-214">前端的設計工具 (人熟悉 HTML/CSS/JavaScript 和其他用戶端技術，但不是熟悉 C# 和 Razor)，大部分的行是外部角色。 <span data-ttu-id="92d1d-215">有沒有來自 IntelliSense 的說明，必須撰寫的整行。</span><span class="sxs-lookup"><span data-stu-id="92d1d-215">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="92d1d-216">使用`LabelTagHelper`，相同的標記可以寫成：</span><span class="sxs-lookup"><span data-stu-id="92d1d-216">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![影像](intro/_static/label2.png)

<span data-ttu-id="92d1d-218">標記協助程式版本，當您輸入`<l`在 Visual Studio 編輯器中，IntelliSense 會顯示相符項目：</span><span class="sxs-lookup"><span data-stu-id="92d1d-218">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![影像](intro/_static/label.png)

<span data-ttu-id="92d1d-220">IntelliSense 可協助您撰寫的整行。</span><span class="sxs-lookup"><span data-stu-id="92d1d-220">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="92d1d-221">`LabelTagHelper`也預設為設定的內容`asp-for`到 「 名字 」; 屬性值 ("FirstName")依照 camel 命名法的大小寫慣例的內容轉換為句子空間，其中每個新的大小寫字母，就會發生在屬性名稱所組成。</span><span class="sxs-lookup"><span data-stu-id="92d1d-221">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="92d1d-222">在下列標記：</span><span class="sxs-lookup"><span data-stu-id="92d1d-222">In the following markup:</span></span>

![影像](intro/_static/label2.png)

<span data-ttu-id="92d1d-224">會產生：</span><span class="sxs-lookup"><span data-stu-id="92d1d-224">generates:</span></span>

```html
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="92d1d-225">如果您將內容加入到依照 camel 命名法的大小寫慣例使用句子大小寫的內容不使用`<label>`。</span><span class="sxs-lookup"><span data-stu-id="92d1d-225">The camel-cased to sentence-cased content is not used if you add content to the `<label>`.</span></span> <span data-ttu-id="92d1d-226">例如: </span><span class="sxs-lookup"><span data-stu-id="92d1d-226">For example:</span></span>

![影像](intro/_static/1stName.png)

<span data-ttu-id="92d1d-228">會產生：</span><span class="sxs-lookup"><span data-stu-id="92d1d-228">generates:</span></span>

```html
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="92d1d-229">下圖的程式碼顯示的表單一部分*Views/Account/Register.cshtml* Razor 檢視舊版 ASP.NET 4.5.x MVC 範本產生隨附於 Visual Studio 2015。</span><span class="sxs-lookup"><span data-stu-id="92d1d-229">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![影像](intro/_static/regCS.png)

<span data-ttu-id="92d1d-231">Visual Studio 編輯器顯示 C# 程式碼以灰色背景。</span><span class="sxs-lookup"><span data-stu-id="92d1d-231">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="92d1d-232">例如， `AntiForgeryToken` HTML Helper:</span><span class="sxs-lookup"><span data-stu-id="92d1d-232">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```html
@Html.AntiForgeryToken()
```

<span data-ttu-id="92d1d-233">即會顯示灰色背景。</span><span class="sxs-lookup"><span data-stu-id="92d1d-233">is displayed with a grey background.</span></span> <span data-ttu-id="92d1d-234">大部分的登錄檢視中標記為 C#。</span><span class="sxs-lookup"><span data-stu-id="92d1d-234">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="92d1d-235">使用標記協助程式的對等方法來比較：</span><span class="sxs-lookup"><span data-stu-id="92d1d-235">Compare that to the equivalent approach using Tag Helpers:</span></span>

![影像](intro/_static/regTH.png)

<span data-ttu-id="92d1d-237">標記是更簡潔且更容易讀取、 編輯和維護的 HTML Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="92d1d-237">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="92d1d-238">C# 程式碼會降低該伺服器需要了解最小值。</span><span class="sxs-lookup"><span data-stu-id="92d1d-238">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="92d1d-239">Visual Studio 編輯器顯示標記以特殊的字型標記協助程式的目標。</span><span class="sxs-lookup"><span data-stu-id="92d1d-239">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="92d1d-240">請考慮*電子郵件*群組：</span><span class="sxs-lookup"><span data-stu-id="92d1d-240">Consider the *Email* group:</span></span>

<span data-ttu-id="92d1d-241">[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]</span><span class="sxs-lookup"><span data-stu-id="92d1d-241">[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]</span></span>

<span data-ttu-id="92d1d-242">每個 「 asp-」 屬性的值為"Email"，但是"Email"不是字串。</span><span class="sxs-lookup"><span data-stu-id="92d1d-242">Each of the "asp-" attributes has a value of "Email", but "Email" is not a string.</span></span> <span data-ttu-id="92d1d-243">在此內容中，"Email"是 C# 模型運算式屬性`RegisterViewModel`。</span><span class="sxs-lookup"><span data-stu-id="92d1d-243">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="92d1d-244">在 Visual Studio 編輯器可協助您撰寫**所有**中的標記的註冊表單中，而 Visual Studio 不提供任何說明中的 HTML Helper 方法的程式碼的大部分標記協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="92d1d-244">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="92d1d-245">[標記協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)會深入探討在使用 Visual Studio 編輯器中的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="92d1d-245">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="92d1d-246">相較於 Web 伺服器控制項的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="92d1d-246">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="92d1d-247">標記協助程式沒有; 與其相關的項目它們只參與呈現的項目和內容。</span><span class="sxs-lookup"><span data-stu-id="92d1d-247">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="92d1d-248">ASP.NET [Web 伺服器控制項](https://msdn.microsoft.com/library/7698y1f0.aspx)宣告和頁面叫用。</span><span class="sxs-lookup"><span data-stu-id="92d1d-248">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="92d1d-249">[Web 伺服器控制項](https://msdn.microsoft.com/library/zsyt68f1.aspx)具有非一般的生命週期可以讓開發及偵錯更加困難。</span><span class="sxs-lookup"><span data-stu-id="92d1d-249">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="92d1d-250">Web 伺服器控制項可讓您使用的用戶端控制項，將功能加入至用戶端文件物件模型 (DOM) 項目。</span><span class="sxs-lookup"><span data-stu-id="92d1d-250">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="92d1d-251">標記協助程式有沒有 DOM</span><span class="sxs-lookup"><span data-stu-id="92d1d-251">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="92d1d-252">Web 伺服器控制項包括瀏覽器自動偵測。</span><span class="sxs-lookup"><span data-stu-id="92d1d-252">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="92d1d-253">標記協助程式不知道的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="92d1d-253">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="92d1d-254">多個標記協助程式就可以處理相同的項目 (請參閱[避免標記協助程式衝突](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts)) 時通常無法撰寫的 Web 伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="92d1d-254">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="92d1d-255">標記協助程式的標籤和 HTML 項目，它們範圍，內容可以修改，但不直接修改頁面上的其他任何項目。</span><span class="sxs-lookup"><span data-stu-id="92d1d-255">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="92d1d-256">Web 伺服器控制項具有更廣泛的範圍，而且可以執行的動作會影響您的網頁; 的其他部分啟用非預期的副作用。</span><span class="sxs-lookup"><span data-stu-id="92d1d-256">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="92d1d-257">Web 伺服器控制項使用類型轉換器，將字串轉換成物件。</span><span class="sxs-lookup"><span data-stu-id="92d1d-257">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="92d1d-258">標記協助程式，您可以使用原生方式在 C# 中，因此您不需要進行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="92d1d-258">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="92d1d-259">Web 伺服器控制項使用[System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel)來實作元件和控制項的 run-time 和設計階段行為。</span><span class="sxs-lookup"><span data-stu-id="92d1d-259">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="92d1d-260">`System.ComponentModel`包含基底類別和介面的實作屬性和型別轉換子、 繫結至資料來源，以及授權元件。</span><span class="sxs-lookup"><span data-stu-id="92d1d-260">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="92d1d-261">對比，以標記協助程式，通常衍生自`TagHelper`，而`TagHelper`基底類別會公開只有兩個方法，`Process`和`ProcessAsync`。</span><span class="sxs-lookup"><span data-stu-id="92d1d-261">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="92d1d-262">自訂標記協助程式項目字型</span><span class="sxs-lookup"><span data-stu-id="92d1d-262">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="92d1d-263">您可以自訂字型和顏色標示從**工具** > **選項** > **環境** > **字型和色彩**:</span><span class="sxs-lookup"><span data-stu-id="92d1d-263">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![影像](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="92d1d-265">其他資源</span><span class="sxs-lookup"><span data-stu-id="92d1d-265">Additional Resources</span></span>

* [<span data-ttu-id="92d1d-266">撰寫標記協助程式</span><span class="sxs-lookup"><span data-stu-id="92d1d-266">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="92d1d-267">使用表單</span><span class="sxs-lookup"><span data-stu-id="92d1d-267">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="92d1d-268">[GitHub 上的 TagHelperSamples](https://github.com/dpaquette/TagHelperSamples)含有標記協助程式的範例使用[Bootstrap](http://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="92d1d-268">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->