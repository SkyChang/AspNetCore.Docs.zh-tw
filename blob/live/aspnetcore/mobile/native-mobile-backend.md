---
title: "建立原生行動應用程式的後端服務"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: 0737cb1c81b6a143090234f37e5567d5c605b99e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="e78d6-102">建立原生行動應用程式的後端服務</span><span class="sxs-lookup"><span data-stu-id="e78d6-102">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="e78d6-103">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e78d6-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e78d6-104">行動裝置應用程式可以輕鬆地進行通訊與 ASP.NET Core 後端服務。</span><span class="sxs-lookup"><span data-stu-id="e78d6-104">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="e78d6-105">檢視或下載後端服務程式碼範例</span><span class="sxs-lookup"><span data-stu-id="e78d6-105">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="e78d6-106">範例原生行動應用程式</span><span class="sxs-lookup"><span data-stu-id="e78d6-106">The Sample Native Mobile App</span></span>

<span data-ttu-id="e78d6-107">本教學課程會示範如何建立使用 ASP.NET Core MVC 支援原生行動應用程式的後端服務。</span><span class="sxs-lookup"><span data-stu-id="e78d6-107">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="e78d6-108">它會使用[Xamarin Forms ToDoRest 應用程式](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/)為其原生用戶端，包括 Android、 iOS、 Windows 通用和 Windows Phone 裝置的個別原生用戶端。</span><span class="sxs-lookup"><span data-stu-id="e78d6-108">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="e78d6-109">您可以依照連結的教學課程，若要建立原生應用程式 （並安裝所需的免費 Xamarin 工具），以及下載 Xamarin 範例方案。</span><span class="sxs-lookup"><span data-stu-id="e78d6-109">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="e78d6-110">Xamarin 範例包含 ASP.NET Web API 2 services 專案，這篇文章 ASP.NET Core 應用程式取代 （用戶端所需的任何變更）。</span><span class="sxs-lookup"><span data-stu-id="e78d6-110">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![若要執行的 Rest 應用程式在 Android 手機上執行](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="e78d6-112">功能</span><span class="sxs-lookup"><span data-stu-id="e78d6-112">Features</span></span>

<span data-ttu-id="e78d6-113">ToDoRest 應用程式支援清單、 加入、 刪除和更新待辦項目。</span><span class="sxs-lookup"><span data-stu-id="e78d6-113">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="e78d6-114">每個項目有識別碼、 名稱、 提示和屬性，指出是否已完成尚未。</span><span class="sxs-lookup"><span data-stu-id="e78d6-114">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="e78d6-115">主要檢視的項目，如上所示，列出每個項目名稱，並指出它透過核取記號。</span><span class="sxs-lookup"><span data-stu-id="e78d6-115">The main view of the items, as shown above, lists each item's name and indicates if it is done with a checkmark.</span></span>

<span data-ttu-id="e78d6-116">點選`+`圖示會開啟 [新增項目] 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="e78d6-116">Tapping the `+` icon opens an add item dialog:</span></span>

![新增項目 對話方塊](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="e78d6-118">點選 [主要清單] 畫面上的項目會開啟編輯對話方塊，其中的項目名稱、 提示和完成的設定，您可以修改，或者刪除項目：</span><span class="sxs-lookup"><span data-stu-id="e78d6-118">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![[編輯項目] 對話方塊](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="e78d6-120">這個範例會使用後端服務裝載於 developer.xamarin.com，允許唯讀作業的預設設定。</span><span class="sxs-lookup"><span data-stu-id="e78d6-120">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="e78d6-121">若要測試自己對您的電腦上執行的下一節中所建立的 ASP.NET Core 應用程式，您將需要更新的應用程式`RestUrl`常數。</span><span class="sxs-lookup"><span data-stu-id="e78d6-121">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="e78d6-122">瀏覽至`ToDoREST`專案，並開啟*Constants.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="e78d6-122">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="e78d6-123">取代`RestUrl`具有 URL，其中包含您的電腦 IP 位址 （不 localhost 或 127.0.0.1，因為此位址是使用從裝置模擬器，不是從您的電腦）。</span><span class="sxs-lookup"><span data-stu-id="e78d6-123">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="e78d6-124">包含連接埠號碼 (5000)。</span><span class="sxs-lookup"><span data-stu-id="e78d6-124">Include the port number as well (5000).</span></span> <span data-ttu-id="e78d6-125">若要測試您的服務使用的裝置，請確定您沒有使用中防火牆封鎖這個連接埠的存取。</span><span class="sxs-lookup"><span data-stu-id="e78d6-125">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="e78d6-126">建立 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="e78d6-126">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="e78d6-127">Visual Studio 中建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e78d6-127">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="e78d6-128">選擇 Web API 範本和非驗證。</span><span class="sxs-lookup"><span data-stu-id="e78d6-128">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="e78d6-129">將專案命名*ToDoApi*。</span><span class="sxs-lookup"><span data-stu-id="e78d6-129">Name the project *ToDoApi*.</span></span>

![新的 ASP.NET Web 應用程式 對話方塊，以選取的 Web API 專案範本](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="e78d6-131">應用程式應該回應建立到 5000 之間的所有要求。</span><span class="sxs-lookup"><span data-stu-id="e78d6-131">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="e78d6-132">更新*Program.cs*包含`.UseUrls("http://*:5000")`為了達成此目的：</span><span class="sxs-lookup"><span data-stu-id="e78d6-132">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="e78d6-133">請確定您執行應用程式直接管理，而不是後置 IIS Express，預設情況下忽略非本機要求。</span><span class="sxs-lookup"><span data-stu-id="e78d6-133">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="e78d6-134">執行`dotnet run`從命令提示字元，或從 Visual Studio 工具列中的偵錯目標下拉式清單中選擇應用程式名稱設定檔。</span><span class="sxs-lookup"><span data-stu-id="e78d6-134">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="e78d6-135">將模型類別來代表待辦項目。</span><span class="sxs-lookup"><span data-stu-id="e78d6-135">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="e78d6-136">標記所需的欄位使用`[Required]`屬性：</span><span class="sxs-lookup"><span data-stu-id="e78d6-136">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="e78d6-137">API 方法需要某種方式來處理資料。</span><span class="sxs-lookup"><span data-stu-id="e78d6-137">The API methods require some way to work with data.</span></span> <span data-ttu-id="e78d6-138">使用相同`IToDoRepository`介面原始 Xamarin 範例會使用：</span><span class="sxs-lookup"><span data-stu-id="e78d6-138">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="e78d6-139">此範例中，實作只會使用私用集合的項目：</span><span class="sxs-lookup"><span data-stu-id="e78d6-139">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="e78d6-140">設定中的實作*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e78d6-140">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="e78d6-141">此時，您可以建立*ToDoItemsController*。</span><span class="sxs-lookup"><span data-stu-id="e78d6-141">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="e78d6-142">了解有關建立多個 web 應用程式開發介面中的[建置您第一個 Web 應用程式開發介面使用 ASP.NET Core MVC 和 Visual Studio](../tutorials/first-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="e78d6-142">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="e78d6-143">建立控制器</span><span class="sxs-lookup"><span data-stu-id="e78d6-143">Creating the Controller</span></span>

<span data-ttu-id="e78d6-144">將新的控制站新增至專案， *ToDoItemsController*。</span><span class="sxs-lookup"><span data-stu-id="e78d6-144">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="e78d6-145">它應該是繼承自 Microsoft.AspNetCore.Mvc.Controller。</span><span class="sxs-lookup"><span data-stu-id="e78d6-145">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="e78d6-146">新增`Route`屬性來指出控制器將會處理做為開頭的路徑的要求`api/todoitems`。</span><span class="sxs-lookup"><span data-stu-id="e78d6-146">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="e78d6-147">`[controller]`路由中的權杖會由控制器的名稱取代 (省略`Controller`尾碼)，並對全域路由來說特別有用。</span><span class="sxs-lookup"><span data-stu-id="e78d6-147">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="e78d6-148">深入了解[路由](../fundamentals/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="e78d6-148">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="e78d6-149">控制器必須`IToDoRepository`至函式，請要求此類型透過控制站的建構函式的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e78d6-149">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="e78d6-150">在執行階段，這個執行個體將會提供使用 framework 支援的[相依性插入](../fundamentals/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="e78d6-150">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="e78d6-151">這個 API 支援四種不同的 HTTP 動詞命令，以資料來源上執行 CRUD （建立、 讀取、 更新、 刪除） 作業。</span><span class="sxs-lookup"><span data-stu-id="e78d6-151">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="e78d6-152">這些最簡單的是對應至 HTTP GET 要求的 「 讀取 」 操作。</span><span class="sxs-lookup"><span data-stu-id="e78d6-152">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="e78d6-153">讀取項目</span><span class="sxs-lookup"><span data-stu-id="e78d6-153">Reading Items</span></span>

<span data-ttu-id="e78d6-154">要求的項目清單會在 GET 要求來完成`List`方法。</span><span class="sxs-lookup"><span data-stu-id="e78d6-154">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="e78d6-155">`[HttpGet]`屬性`List`方法會指出此動作，才應該處理 GET 要求。</span><span class="sxs-lookup"><span data-stu-id="e78d6-155">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="e78d6-156">此動作的路由為控制器上指定的路由。</span><span class="sxs-lookup"><span data-stu-id="e78d6-156">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="e78d6-157">您不一定需要的動作名稱做為路由的一部分。</span><span class="sxs-lookup"><span data-stu-id="e78d6-157">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="e78d6-158">您只需要確保每個動作都有唯一且明確的路由。</span><span class="sxs-lookup"><span data-stu-id="e78d6-158">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="e78d6-159">路由的屬性可以套用在控制器和方法層級，來建立特定的路由。</span><span class="sxs-lookup"><span data-stu-id="e78d6-159">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="e78d6-160">`List`方法會傳回 200 OK 回應程式碼和所有 ToDo 項目，序列化為 JSON。</span><span class="sxs-lookup"><span data-stu-id="e78d6-160">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="e78d6-161">您可以測試您新的應用程式開發介面方法，使用各種工具，例如[郵差](https://www.getpostman.com/docs/)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e78d6-161">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![郵差主控台顯示 todoitems 和顯示三個項目，傳回的 JSON 回應主體的 GET 要求](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="e78d6-163">建立項目</span><span class="sxs-lookup"><span data-stu-id="e78d6-163">Creating Items</span></span>

<span data-ttu-id="e78d6-164">依照慣例，建立新資料的項目會對應至 HTTP POST 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="e78d6-164">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="e78d6-165">`Create`方法有`[HttpPost]`屬性套用至其中，並接受`ToDoItem`執行個體。</span><span class="sxs-lookup"><span data-stu-id="e78d6-165">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="e78d6-166">因為`item`引數會傳遞給在 post 要求主體中，這個參數以裝飾`[FromBody]`屬性。</span><span class="sxs-lookup"><span data-stu-id="e78d6-166">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="e78d6-167">在方法內，會檢查有效性和資料存放區中的預先存在的項目，如果不發生任何問題，就會加入使用儲存機制。</span><span class="sxs-lookup"><span data-stu-id="e78d6-167">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it is added using the repository.</span></span> <span data-ttu-id="e78d6-168">檢查`ModelState.IsValid`執行[模型驗證](../mvc/models/validation.md)，而且應該在每個應用程式開發介面方法可接受使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="e78d6-168">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="e78d6-169">此範例會使用列舉，包含錯誤代碼會傳遞給行動用戶端：</span><span class="sxs-lookup"><span data-stu-id="e78d6-169">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="e78d6-170">測試加入新項目使用郵差選擇 POST 動詞命令，提供要求主體中的 JSON 格式中的新物件。</span><span class="sxs-lookup"><span data-stu-id="e78d6-170">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="e78d6-171">您也應該將要求標頭，指定`Content-Type`的`application/json`。</span><span class="sxs-lookup"><span data-stu-id="e78d6-171">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![郵差主控台顯示 POST 和回應](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="e78d6-173">方法會在回應中傳回新建立的項目。</span><span class="sxs-lookup"><span data-stu-id="e78d6-173">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="e78d6-174">更新的項目</span><span class="sxs-lookup"><span data-stu-id="e78d6-174">Updating Items</span></span>

<span data-ttu-id="e78d6-175">修改記錄使用 HTTP PUT 要求。</span><span class="sxs-lookup"><span data-stu-id="e78d6-175">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="e78d6-176">這項變更，以外`Edit`方法是幾乎完全相同`Create`。</span><span class="sxs-lookup"><span data-stu-id="e78d6-176">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="e78d6-177">請注意，如果找不到記錄、`Edit`動作將會傳回`NotFound`(404) 回應。</span><span class="sxs-lookup"><span data-stu-id="e78d6-177">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="e78d6-178">若要測試郵差，變更 PUT 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="e78d6-178">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="e78d6-179">要求主體中指定更新的物件資料。</span><span class="sxs-lookup"><span data-stu-id="e78d6-179">Specify the updated object data in the Body of the request.</span></span>

![郵差主控台顯示 PUT 和回應](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="e78d6-181">這個方法會傳回`NoContent`(204) 回應成功時，與現有的應用程式開發介面的一致性。</span><span class="sxs-lookup"><span data-stu-id="e78d6-181">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="e78d6-182">刪除項目</span><span class="sxs-lookup"><span data-stu-id="e78d6-182">Deleting Items</span></span>

<span data-ttu-id="e78d6-183">刪除資料錄被透過對服務進行的 DELETE 要求，並傳遞要刪除的項目 ID。</span><span class="sxs-lookup"><span data-stu-id="e78d6-183">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="e78d6-184">使用更新時，會收到不存在之項目的要求時`NotFound`回應。</span><span class="sxs-lookup"><span data-stu-id="e78d6-184">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="e78d6-185">否則，成功的要求將會得到`NoContent`(204) 回應。</span><span class="sxs-lookup"><span data-stu-id="e78d6-185">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="e78d6-186">請注意，在測試時的刪除功能，執行任何動作都需要在要求主體。</span><span class="sxs-lookup"><span data-stu-id="e78d6-186">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![郵差主控台顯示的刪除和回應](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="e78d6-188">常見的 Web API 慣例</span><span class="sxs-lookup"><span data-stu-id="e78d6-188">Common Web API Conventions</span></span>

<span data-ttu-id="e78d6-189">當您開發應用程式後端服務時，您會想要的一組一致的慣例或處理跨碼橫切入顧慮的原則。</span><span class="sxs-lookup"><span data-stu-id="e78d6-189">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="e78d6-190">例如，如上所示，在服務中要求的特定記錄，找不到接收`NotFound`回應，而非`BadRequest`回應。</span><span class="sxs-lookup"><span data-stu-id="e78d6-190">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="e78d6-191">同樣地，對這項服務的繫結的模型類型一律檢查傳入的命令`ModelState.IsValid`並傳回`BadRequest`無效的模型型別。</span><span class="sxs-lookup"><span data-stu-id="e78d6-191">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="e78d6-192">一旦您已經為您的應用程式開發介面識別通用的原則，您可以通常封裝在[篩選](../mvc/controllers/filters.md)。</span><span class="sxs-lookup"><span data-stu-id="e78d6-192">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="e78d6-193">深入了解[如何封裝 ASP.NET Core MVC 應用程式中常見的 API 原則](https://msdn.microsoft.com/magazine/mt767699.aspx)。</span><span class="sxs-lookup"><span data-stu-id="e78d6-193">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>