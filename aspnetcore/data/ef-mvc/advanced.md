---
title: "使用 EF Core-ASP.NET Core MVC 進階-10-10"
author: tdykstra
description: "此教學課程介紹幾個有用，可注意當您超越開發使用 Entity Framework 核心的 ASP.NET web 應用程式的基本概念的主題。"
keywords: "ASP.NET Core、 Entity Framework Core，原始的 sql，檢查 sql、 儲存機制模式、 單位的工作模式中，自動變更偵測現有的資料庫"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 92a2986a-d005-4ff6-9559-6657fd466bb7
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: 210f8e8b91c2487e5c4b73fdeb6ff0d5aa35c0c5
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="e03e8-104">進階的主題-EF Core 與 ASP.NET Core MVC 教學課程 (10-10)</span><span class="sxs-lookup"><span data-stu-id="e03e8-104">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="e03e8-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e03e8-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e03e8-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="e03e8-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="e03e8-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="e03e8-108">在上一個教學課程中，您可以實作資料表每個階層繼承。</span><span class="sxs-lookup"><span data-stu-id="e03e8-108">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="e03e8-109">此教學課程介紹幾個有用，可注意當您超越開發使用 Entity Framework 核心的 ASP.NET Core web 應用程式的基本概念的主題。</span><span class="sxs-lookup"><span data-stu-id="e03e8-109">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="e03e8-110">原始的 SQL 查詢</span><span class="sxs-lookup"><span data-stu-id="e03e8-110">Raw SQL Queries</span></span>

<span data-ttu-id="e03e8-111">使用 Entity Framework 的優點之一是它可避免中斷您太接近儲存資料的特定方法的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e03e8-111">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="e03e8-112">它會產生 SQL 查詢和命令，這也讓您不必自行撰寫。</span><span class="sxs-lookup"><span data-stu-id="e03e8-112">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="e03e8-113">但有例外狀況，當您需要執行手動建立的特定 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="e03e8-113">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="e03e8-114">針對這些案例，Entity Framework 程式碼的第一個 API 會包含可讓您將直接對資料庫的 SQL 命令的方法。</span><span class="sxs-lookup"><span data-stu-id="e03e8-114">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="e03e8-115">您在 EF Core 1.0 中有下列選項：</span><span class="sxs-lookup"><span data-stu-id="e03e8-115">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="e03e8-116">使用`DbSet.FromSql`查詢來傳回實體類型的方法。</span><span class="sxs-lookup"><span data-stu-id="e03e8-116">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="e03e8-117">傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤對資料庫內容所除非您[關閉追蹤](crud.md#no-tracking-queries)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-117">The returned objects must be of the type expected by the `DbSet` object, and they are automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="e03e8-118">使用`Database.ExecuteSqlCommand`非查詢命令。</span><span class="sxs-lookup"><span data-stu-id="e03e8-118">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="e03e8-119">如果您需要執行可傳回類型不是實體的查詢，您可以使用 ADO.NET 與 EF 所提供的資料庫連接。</span><span class="sxs-lookup"><span data-stu-id="e03e8-119">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="e03e8-120">即使您使用這個方法來擷取實體類型不被追蹤的資料庫內容，傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="e03e8-120">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="e03e8-121">因為永遠是 true 時您 web 應用程式中執行 SQL 命令，您必須採取一些預防措施以保護您的網站，SQL 資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="e03e8-121">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="e03e8-122">方法之一是使用參數化的查詢，以確定網頁所提交的字串，無法解譯為 SQL 命令。</span><span class="sxs-lookup"><span data-stu-id="e03e8-122">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="e03e8-123">在本教學課程中，您將使用參數化的查詢時將使用者輸入整合到查詢。</span><span class="sxs-lookup"><span data-stu-id="e03e8-123">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="e03e8-124">呼叫傳回實體的查詢</span><span class="sxs-lookup"><span data-stu-id="e03e8-124">Call a query that returns entities</span></span>

<span data-ttu-id="e03e8-125">`DbSet<TEntity>`類別會提供方法可讓您執行查詢，以傳回型別的實體`TEntity`。</span><span class="sxs-lookup"><span data-stu-id="e03e8-125">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="e03e8-126">若要查看這對您所做的運作將會在變更程式碼`Details`部門控制站的方法。</span><span class="sxs-lookup"><span data-stu-id="e03e8-126">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="e03e8-127">在*DepartmentsController.cs*，請在`Details`方法，擷取的部門使用的程式碼取代`FromSql`方法呼叫，如下列反白顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="e03e8-127">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

<span data-ttu-id="e03e8-128">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]</span><span class="sxs-lookup"><span data-stu-id="e03e8-128">[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]</span></span>

<span data-ttu-id="e03e8-129">若要確認新的程式碼正常運作，請選取**部門** 索引標籤，然後**詳細資料**其中一個部門。</span><span class="sxs-lookup"><span data-stu-id="e03e8-129">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![部門詳細資料](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="e03e8-131">呼叫會傳回其他類型的查詢</span><span class="sxs-lookup"><span data-stu-id="e03e8-131">Call a query that returns other types</span></span>

<span data-ttu-id="e03e8-132">先前您建立學生統計資料方格中，以顯示每個註冊日期的學生總數一樣的 「 關於 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="e03e8-132">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="e03e8-133">從學生實體集取得資料 (`_context.Students`) 並將結果規劃一份使用 LINQ`EnrollmentDateGroup`檢視模型物件。</span><span class="sxs-lookup"><span data-stu-id="e03e8-133">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="e03e8-134">假設您想要撰寫 SQL 本身而不是使用 LINQ。</span><span class="sxs-lookup"><span data-stu-id="e03e8-134">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="e03e8-135">若要執行此操作，您必須執行 SQL 查詢會傳回實體物件以外的項目。</span><span class="sxs-lookup"><span data-stu-id="e03e8-135">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="e03e8-136">在 EF Core 1.0，方法之一是撰寫 ADO.NET 程式碼，並從 EF 取得資料庫連接。</span><span class="sxs-lookup"><span data-stu-id="e03e8-136">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="e03e8-137">在*HomeController.cs*，取代`About`方法取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e03e8-137">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

<span data-ttu-id="e03e8-138">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]</span><span class="sxs-lookup"><span data-stu-id="e03e8-138">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]</span></span>

<span data-ttu-id="e03e8-139">加入 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="e03e8-139">Add a using statement:</span></span>

<span data-ttu-id="e03e8-140">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]</span><span class="sxs-lookup"><span data-stu-id="e03e8-140">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]</span></span>

<span data-ttu-id="e03e8-141">執行 「 關於 」 頁面。</span><span class="sxs-lookup"><span data-stu-id="e03e8-141">Run the About page.</span></span> <span data-ttu-id="e03e8-142">它會顯示相同的資料以前一樣。</span><span class="sxs-lookup"><span data-stu-id="e03e8-142">It displays the same data it did before.</span></span>

![有關頁面](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="e03e8-144">呼叫更新查詢</span><span class="sxs-lookup"><span data-stu-id="e03e8-144">Call an update query</span></span>

<span data-ttu-id="e03e8-145">假設 Contoso 大學系統管理員想要在資料庫中，例如變更的每個課程信用額度數量執行全域的變更。</span><span class="sxs-lookup"><span data-stu-id="e03e8-145">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="e03e8-146">如果該大學有大量的課程，很效率不佳，擷取這些全部都做為實體，並將它們個別變更。</span><span class="sxs-lookup"><span data-stu-id="e03e8-146">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="e03e8-147">在本節中，您將實作網頁，可讓使用者能夠指定所要依據變更的所有課程，信用額度數目的因素，您將執行 SQL UPDATE 陳述式來進行變更。</span><span class="sxs-lookup"><span data-stu-id="e03e8-147">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="e03e8-148">網頁看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="e03e8-148">The web page will look like the following illustration:</span></span>

![更新課程信用額度頁面](advanced/_static/update-credits.png)

<span data-ttu-id="e03e8-150">在*CoursesContoller.cs*，HttpGet 和 HttpPost 加入 UpdateCourseCredits 方法：</span><span class="sxs-lookup"><span data-stu-id="e03e8-150">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

<span data-ttu-id="e03e8-151">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]</span><span class="sxs-lookup"><span data-stu-id="e03e8-151">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]</span></span>

<span data-ttu-id="e03e8-152">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]</span><span class="sxs-lookup"><span data-stu-id="e03e8-152">[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]</span></span>

<span data-ttu-id="e03e8-153">當控制器處理 HttpGet 要求時，不會傳回在`ViewData["RowsAffected"]`，並檢視顯示空白的文字方塊和送出按鈕，在上圖所示。</span><span class="sxs-lookup"><span data-stu-id="e03e8-153">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="e03e8-154">當**更新**按鈕、 HttpPost 方法呼叫時，和倍增器已在文字方塊中輸入的值。</span><span class="sxs-lookup"><span data-stu-id="e03e8-154">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="e03e8-155">然後執行的程式碼會更新課程並傳回受影響的資料列數目至檢視中的 SQL `ViewData`。</span><span class="sxs-lookup"><span data-stu-id="e03e8-155">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="e03e8-156">當檢視表取得`RowsAffected`值，它會顯示更新的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="e03e8-156">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="e03e8-157">在**方案總管 中**，以滑鼠右鍵按一下*檢視/課程*資料夾，然後再按一下**新增 > 新的項目**。</span><span class="sxs-lookup"><span data-stu-id="e03e8-157">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="e03e8-158">在**加入新項目**] 對話方塊中，按一下**ASP.NET**下**已安裝**在左窗格中，按一下 [ **MVC 檢視頁面**，並將新的檢視*UpdateCourseCredits.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e03e8-158">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="e03e8-159">在*Views/Courses/UpdateCourseCredits.cshtml*，範本程式碼取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e03e8-159">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

<span data-ttu-id="e03e8-160">[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="e03e8-160">[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]</span></span>

<span data-ttu-id="e03e8-161">執行`UpdateCourseCredits`方法藉由選取**課程** 索引標籤，然後再新增 「 / UpdateCourseCredits"瀏覽器的網址列中的 URL 的結尾 (例如： `http://localhost:5813/Courses/UpdateCourseCredits`)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-161">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="e03e8-162">在文字方塊中輸入的數字：</span><span class="sxs-lookup"><span data-stu-id="e03e8-162">Enter a number in the text box:</span></span>

![更新課程信用額度頁面](advanced/_static/update-credits.png)

<span data-ttu-id="e03e8-164">按一下 [更新] 。</span><span class="sxs-lookup"><span data-stu-id="e03e8-164">Click **Update**.</span></span> <span data-ttu-id="e03e8-165">您會看到受影響的資料列數目：</span><span class="sxs-lookup"><span data-stu-id="e03e8-165">You see the number of rows affected:</span></span>

![更新課程信用額度頁面上受影響的資料列](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="e03e8-167">按一下**返回清單**若要查看課程信用額度的修訂編號取代清單。</span><span class="sxs-lookup"><span data-stu-id="e03e8-167">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="e03e8-168">請注意，實際執行程式碼會確定一律更新中有效的資料結果。</span><span class="sxs-lookup"><span data-stu-id="e03e8-168">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="e03e8-169">簡化如下所示的程式碼無法將信用額度足以導致大於 5 的數字的數目。</span><span class="sxs-lookup"><span data-stu-id="e03e8-169">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="e03e8-170">(`Credits`屬性具有`[Range(0, 5)]`屬性。)更新查詢可行，但是無效的資料可能會導致系統其他部分假設信用額度的數目是 5 或更少的非預期的結果。</span><span class="sxs-lookup"><span data-stu-id="e03e8-170">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="e03e8-171">如需原始的 SQL 查詢的詳細資訊，請參閱[原始的 SQL 查詢](https://docs.microsoft.com/ef/core/querying/raw-sql)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-171">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="e03e8-172">檢查傳送至資料庫的 SQL</span><span class="sxs-lookup"><span data-stu-id="e03e8-172">Examine SQL sent to the database</span></span>

<span data-ttu-id="e03e8-173">有時候很有幫助能夠看到實際傳送至資料庫的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="e03e8-173">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="e03e8-174">內建的記錄功能，適用於 ASP.NET Core 自動使用 EF 核心撰寫包含 SQL 查詢和更新的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="e03e8-174">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="e03e8-175">在本節中，您會看到 SQL 記錄的一些範例。</span><span class="sxs-lookup"><span data-stu-id="e03e8-175">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="e03e8-176">開啟*StudentsController.cs*和`Details`方法上設定中斷點`if (student == null)`陳述式。</span><span class="sxs-lookup"><span data-stu-id="e03e8-176">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="e03e8-177">在偵錯模式中執行應用程式，並移至詳細資料頁面的學生。</span><span class="sxs-lookup"><span data-stu-id="e03e8-177">Run the application in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="e03e8-178">移至**輸出**視窗顯示偵錯輸出，而且您會看到查詢：</span><span class="sxs-lookup"><span data-stu-id="e03e8-178">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="e03e8-179">您會發現這裡喜歡的項目可能會令您吃驚： SQL 選取最多 2 個資料列 (`TOP(2)`) 從 Person 資料表。</span><span class="sxs-lookup"><span data-stu-id="e03e8-179">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="e03e8-180">`SingleOrDefaultAsync`方法未解析為 1 個資料列的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e03e8-180">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="e03e8-181">原因如下：</span><span class="sxs-lookup"><span data-stu-id="e03e8-181">Here's why:</span></span>

* <span data-ttu-id="e03e8-182">如果查詢會傳回多個資料列，則方法會傳回 null。</span><span class="sxs-lookup"><span data-stu-id="e03e8-182">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="e03e8-183">若要判斷查詢是否會傳回多個資料列，必須檢查是否它會傳回至少 2 EF。</span><span class="sxs-lookup"><span data-stu-id="e03e8-183">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="e03e8-184">請注意，您不必使用偵錯模式，並取得記錄輸出中的中斷點處停止**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="e03e8-184">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="e03e8-185">它只是便利方式停止您想要查看輸出的點記錄。</span><span class="sxs-lookup"><span data-stu-id="e03e8-185">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="e03e8-186">如果不這樣做，請繼續記錄，您必須捲動至尋找您感興趣的部分。</span><span class="sxs-lookup"><span data-stu-id="e03e8-186">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="e03e8-187">儲存機制和單位的工作模式</span><span class="sxs-lookup"><span data-stu-id="e03e8-187">Repository and unit of work patterns</span></span>

<span data-ttu-id="e03e8-188">許多開發人員撰寫程式碼以搭配 Entity Framework 的程式碼周圍的包裝函式實作的儲存機制和工作模式的單位。</span><span class="sxs-lookup"><span data-stu-id="e03e8-188">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="e03e8-189">這些模式被用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="e03e8-189">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="e03e8-190">實作這些模式可以協助您隔離您的應用程式資料存放區中的變更，以及有助於自動化的單元測試為導向的開發 (TDD)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-190">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="e03e8-191">不過，撰寫額外的程式碼來實作這些模式做不一定有數種原因使用 EF，應用程式的最佳選擇：</span><span class="sxs-lookup"><span data-stu-id="e03e8-191">However, writing additional code to implement these patterns is not always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="e03e8-192">EF 內容類別本身，以隔離您的程式碼，從資料存放區特有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e03e8-192">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="e03e8-193">EF 內容類別可做為資料庫的工作單位的類別可讓您更新您不要使用 EF。</span><span class="sxs-lookup"><span data-stu-id="e03e8-193">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="e03e8-194">EF 包含而不需要撰寫的程式碼儲存機制實作 TDD 的功能。</span><span class="sxs-lookup"><span data-stu-id="e03e8-194">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="e03e8-195">如需如何實作儲存機制和工作模式單位資訊，請參閱[此教學課程系列的 Entity Framework 5 新版](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-195">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="e03e8-196">Entity Framework Core 實作可用於測試的記憶體中資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="e03e8-196">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="e03e8-197">如需詳細資訊，請參閱[測試 InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-197">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="e03e8-198">自動變更偵測</span><span class="sxs-lookup"><span data-stu-id="e03e8-198">Automatic change detection</span></span>

<span data-ttu-id="e03e8-199">Entity Framework 藉由比較原始值與實體的目前值，決定如何變更實體 （並因此需要哪些更新傳送至資料庫）。</span><span class="sxs-lookup"><span data-stu-id="e03e8-199">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="e03e8-200">查詢或附加的實體時，會儲存原始值。</span><span class="sxs-lookup"><span data-stu-id="e03e8-200">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="e03e8-201">會導致變更自動偵測的方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="e03e8-201">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="e03e8-202">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="e03e8-202">DbContext.SaveChanges</span></span>

* <span data-ttu-id="e03e8-203">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="e03e8-203">DbContext.Entry</span></span>

* <span data-ttu-id="e03e8-204">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="e03e8-204">ChangeTracker.Entries</span></span>

<span data-ttu-id="e03e8-205">如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，可能會暫時停用自動變更偵測使用收到的顯著效能改善`ChangeTracker.AutoDetectChangesEnabled`屬性。</span><span class="sxs-lookup"><span data-stu-id="e03e8-205">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="e03e8-206">例如: </span><span class="sxs-lookup"><span data-stu-id="e03e8-206">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="e03e8-207">Entity Framework Core 來源的程式碼及開發計劃</span><span class="sxs-lookup"><span data-stu-id="e03e8-207">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="e03e8-208">Entity Framework Core 的原始程式碼位於[https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-208">The source code for Entity Framework Core is available at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="e03e8-209">除了原始程式碼，您可以取得夜間組建，發出追蹤，功能規格、 設計會議記錄[未來的開發藍圖](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)，以及其他更多。</span><span class="sxs-lookup"><span data-stu-id="e03e8-209">Besides source code, you can get nightly builds, issue tracking, feature specs, design meeting notes, [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap), and more.</span></span> <span data-ttu-id="e03e8-210">您可以檔 bug，而且您可能會造成您自己的 EF 原始碼的增強功能。</span><span class="sxs-lookup"><span data-stu-id="e03e8-210">You can file bugs, and you can contribute your own enhancements to the EF source code.</span></span>

<span data-ttu-id="e03e8-211">雖然開啟原始碼時，Entity Framework Core 則完全支援以 Microsoft 產品。</span><span class="sxs-lookup"><span data-stu-id="e03e8-211">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="e03e8-212">Microsoft Entity Framework 小組將控制接受哪些比重保留，並測試所有的程式碼變更，以確保每次發行的品質。</span><span class="sxs-lookup"><span data-stu-id="e03e8-212">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="e03e8-213">從現有資料庫的反向工程</span><span class="sxs-lookup"><span data-stu-id="e03e8-213">Reverse engineer from existing database</span></span>

<span data-ttu-id="e03e8-214">若要反向工程資料模型，包括實體類別，從現有的資料庫，使用[scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext)命令。</span><span class="sxs-lookup"><span data-stu-id="e03e8-214">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="e03e8-215">請參閱[-快速入門教學課程](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-215">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="e03e8-216">使用動態的 LINQ 來簡化排序選取範圍的程式碼</span><span class="sxs-lookup"><span data-stu-id="e03e8-216">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="e03e8-217">[本系列的第三個教學課程](sort-filter-page.md)示範以硬式編碼的資料行名稱來撰寫 LINQ 程式碼`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="e03e8-217">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="e03e8-218">可從中選擇的兩個資料行，這可正常運作，但如果您有許多資料行的程式碼可以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e03e8-218">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="e03e8-219">若要解決這個問題，您可以使用`EF.Property`方法，以指定的屬性名稱做為字串。</span><span class="sxs-lookup"><span data-stu-id="e03e8-219">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="e03e8-220">若要試用這種方法，取代`Index`方法中的`StudentsController`為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="e03e8-220">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

<span data-ttu-id="e03e8-221">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]</span><span class="sxs-lookup"><span data-stu-id="e03e8-221">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]</span></span>

## <a name="next-steps"></a><span data-ttu-id="e03e8-222">後續步驟</span><span class="sxs-lookup"><span data-stu-id="e03e8-222">Next steps</span></span>

<span data-ttu-id="e03e8-223">如此即完成這一系列的教學課程，在 ASP.NET MVC 應用程式中使用 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="e03e8-223">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="e03e8-224">如需 EF Core 的詳細資訊，請參閱[Entity Framework 的核心文件集](https://docs.microsoft.com/ef/core)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-224">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="e03e8-225">也會提供一本書：[動作中的 Entity Framework Core](https://www.manning.com/books/entity-framework-core-in-action)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-225">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="e03e8-226">如需如何部署 web 應用程式建置它之後，請參閱[發行和部署](../../publishing/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-226">For information about how to deploy your web application after you've built it, see [Publishing and deployment](../../publishing/index.md).</span></span>

<span data-ttu-id="e03e8-227">如需 ASP.NET Core MVC、 驗證和授權，例如相關的其他主題，請參閱[ASP.NET Core 文件](https://docs.microsoft.com/aspnet/core/)。</span><span class="sxs-lookup"><span data-stu-id="e03e8-227">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="e03e8-228">通知</span><span class="sxs-lookup"><span data-stu-id="e03e8-228">Acknowledgments</span></span>

<span data-ttu-id="e03e8-229">Tom Dykstra 和 Rick Anderson (twitter @RickAndMSFT) 撰寫本教學課程。</span><span class="sxs-lookup"><span data-stu-id="e03e8-229">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="e03e8-230">Rowan Miller、 狄 Vega 和 Entity Framework 小組的其他成員協助程式碼檢閱，並幫助我們已針對教學課程中撰寫程式碼時，會發生的問題進行偵錯。</span><span class="sxs-lookup"><span data-stu-id="e03e8-230">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="e03e8-231">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="e03e8-231">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="e03e8-232">另一個處理序所使用的 ContosoUniversity.dll</span><span class="sxs-lookup"><span data-stu-id="e03e8-232">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="e03e8-233">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="e03e8-233">Error message:</span></span>

> <span data-ttu-id="e03e8-234">無法開啟 '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 進行寫入-' 處理序無法存取檔案 '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 因為另一個處理序正在使用它。</span><span class="sxs-lookup"><span data-stu-id="e03e8-234">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="e03e8-235">解決方案:</span><span class="sxs-lookup"><span data-stu-id="e03e8-235">Solution:</span></span>

<span data-ttu-id="e03e8-236">停止 IIS Express 中的站台。</span><span class="sxs-lookup"><span data-stu-id="e03e8-236">Stop the site in IIS Express.</span></span> <span data-ttu-id="e03e8-237">請移至 Windows 系統匣中，尋找 IIS Express 和它的圖示上按一下滑鼠右鍵，選取 Contoso 大學站台，然後按一下**停止站台**。</span><span class="sxs-lookup"><span data-stu-id="e03e8-237">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="e03e8-238">移轉建立結構中向上和向下方法沒有程式碼</span><span class="sxs-lookup"><span data-stu-id="e03e8-238">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="e03e8-239">可能的原因：</span><span class="sxs-lookup"><span data-stu-id="e03e8-239">Possible cause:</span></span>

<span data-ttu-id="e03e8-240">EF CLI 命令不會自動關閉並儲存程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="e03e8-240">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="e03e8-241">如果您未儲存變更，當您執行`migrations add`命令、 EF 找不到您的變更。</span><span class="sxs-lookup"><span data-stu-id="e03e8-241">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="e03e8-242">解決方案:</span><span class="sxs-lookup"><span data-stu-id="e03e8-242">Solution:</span></span>

<span data-ttu-id="e03e8-243">執行`migrations remove`命令，儲存您的程式碼變更，並重新執行`migrations add`命令。</span><span class="sxs-lookup"><span data-stu-id="e03e8-243">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="e03e8-244">執行資料庫更新時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="e03e8-244">Errors while running database update</span></span>

<span data-ttu-id="e03e8-245">很可能在具有現有資料的資料庫進行結構描述變更時，取得其他錯誤。</span><span class="sxs-lookup"><span data-stu-id="e03e8-245">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="e03e8-246">如果您移轉時發生錯誤，您無法解決，您可以變更連接字串中的資料庫名稱，或刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="e03e8-246">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="e03e8-247">使用新資料庫時，若要移轉，沒有資料並更新資料庫指令更有可能能順利完成。</span><span class="sxs-lookup"><span data-stu-id="e03e8-247">With a new database, there is no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="e03e8-248">最簡單的方法是在資料庫重新命名*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="e03e8-248">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="e03e8-249">下次您執行`database update`，就會建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="e03e8-249">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="e03e8-250">若要刪除 SSOX 中的資料庫，以滑鼠右鍵按一下資料庫，請按一下**刪除**，然後在**資料庫刪除**對話方塊中，選取**關閉現有連接**按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="e03e8-250">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="e03e8-251">若要使用 CLI 來刪除資料庫，執行`database drop`CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="e03e8-251">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="e03e8-252">尋找 SQL Server 執行個體時發生錯誤</span><span class="sxs-lookup"><span data-stu-id="e03e8-252">Error locating SQL Server instance</span></span>

<span data-ttu-id="e03e8-253">錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="e03e8-253">Error Message:</span></span>

> <span data-ttu-id="e03e8-254">建立 SQL Server 的連接時發生網路相關或執行個體特定錯誤。</span><span class="sxs-lookup"><span data-stu-id="e03e8-254">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="e03e8-255">找不到或無法存取伺服器。</span><span class="sxs-lookup"><span data-stu-id="e03e8-255">The server was not found or was not accessible.</span></span> <span data-ttu-id="e03e8-256">確認執行個名稱是否正確，以及 SQL Server 是否設定為允許遠端連線</span><span class="sxs-lookup"><span data-stu-id="e03e8-256">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="e03e8-257">(提供者： SQL 網路介面，錯誤： 26-尋找指定時發生錯誤伺服器/執行個體)</span><span class="sxs-lookup"><span data-stu-id="e03e8-257">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="e03e8-258">解決方案:</span><span class="sxs-lookup"><span data-stu-id="e03e8-258">Solution:</span></span>

<span data-ttu-id="e03e8-259">請檢查連接字串。</span><span class="sxs-lookup"><span data-stu-id="e03e8-259">Check the connection string.</span></span> <span data-ttu-id="e03e8-260">如果您已經手動刪除資料庫檔案，變更建構字串以使用新的資料庫中資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="e03e8-260">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e03e8-261">上一步</span><span class="sxs-lookup"><span data-stu-id="e03e8-261">Previous</span></span>](inheritance.md)