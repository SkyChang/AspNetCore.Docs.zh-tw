---
title: "ASP.NET Core 中介軟體"
author: rick-anderson
description: "深入了解 ASP.NET Core 中介軟體和要求管線。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: af16046c97964e8e1c16a4f5989fcfa794741c4d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="37947-103">ASP.NET Core 中介軟體的基本概念</span><span class="sxs-lookup"><span data-stu-id="37947-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="37947-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="37947-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="37947-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37947-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="37947-106">什麼是中介軟體</span><span class="sxs-lookup"><span data-stu-id="37947-106">What is middleware</span></span>

<span data-ttu-id="37947-107">中介軟體是組裝至應用程式管線來處理要求和回應的軟體。</span><span class="sxs-lookup"><span data-stu-id="37947-107">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="37947-108">每個元件：</span><span class="sxs-lookup"><span data-stu-id="37947-108">Each component:</span></span>

* <span data-ttu-id="37947-109">您可以選擇是否要將要求傳遞至管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="37947-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="37947-110">之前和之後叫用管線中的下一個元件時，就可以執行工作。</span><span class="sxs-lookup"><span data-stu-id="37947-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="37947-111">要求的委派可以用來建立要求管線。</span><span class="sxs-lookup"><span data-stu-id="37947-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="37947-112">將要求委派會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="37947-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="37947-113">要求使用設定委派[執行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)，[對應](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)，和[使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)擴充方法。</span><span class="sxs-lookup"><span data-stu-id="37947-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="37947-114">個別要求委派可以指定內建為匿名方法 （稱為內建的中介軟體），或是它可以定義可重複使用的類別中。</span><span class="sxs-lookup"><span data-stu-id="37947-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="37947-115">這些可重複使用的類別和內嵌匿名方法*中介軟體*，或*中介軟體元件*。</span><span class="sxs-lookup"><span data-stu-id="37947-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="37947-116">要求管線中的每個中介軟體元件負責叫用下一個元件在管線中，或者如果適當的話，最少運算鏈結。</span><span class="sxs-lookup"><span data-stu-id="37947-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="37947-117">[中介軟體的移轉 HTTP 模組](../migration/http-modules.md)說明要求管線中 ASP.NET Core 版本和舊版之間的差異，並提供更多的中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="37947-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="37947-118">建立與 IApplicationBuilder 的中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="37947-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="37947-119">ASP.NET Core 要求管線包含一連串要求委派，呼叫其中一個之後，這個圖表可顯示 （執行緒的執行如下所示的黑色箭號）：</span><span class="sxs-lookup"><span data-stu-id="37947-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![顯示要求抵達，透過三個 middlewares，並讓應用程式保持回應處理的要求處理模式。](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="37947-123">每個委派之前和之後的下一個委派，可以執行作業。</span><span class="sxs-lookup"><span data-stu-id="37947-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="37947-124">委派，也可以決定要將要求傳遞至下一步的委派，會呼叫最少運算要求管線。</span><span class="sxs-lookup"><span data-stu-id="37947-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="37947-125">最少運算通常會因為它可避免不必要的工作。</span><span class="sxs-lookup"><span data-stu-id="37947-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="37947-126">比方說，靜態檔案中介軟體可以傳回靜態檔案的要求，並最少運算管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="37947-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="37947-127">例外狀況處理委派需要呼叫及早在管線中，讓它們可以攔截管線的後續階段中發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="37947-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="37947-128">最簡單的可能 ASP.NET Core 應用程式設定單一要求委派會處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="37947-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="37947-129">此案例不包含實際的要求管線。</span><span class="sxs-lookup"><span data-stu-id="37947-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="37947-130">相反地，單一的匿名函式會呼叫每個 HTTP 要求的回應。</span><span class="sxs-lookup"><span data-stu-id="37947-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="37947-131">第一個[應用程式。執行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)委派終止管線。</span><span class="sxs-lookup"><span data-stu-id="37947-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="37947-132">您可以鏈結在一起的多個要求委派[應用程式。使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)。</span><span class="sxs-lookup"><span data-stu-id="37947-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="37947-133">`next`參數代表管線中的下一個委派。</span><span class="sxs-lookup"><span data-stu-id="37947-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="37947-134">(請記住，您就可以縮短由管線*不*呼叫*下一步*參數。)您可以通常執行動作之前和之後的下一個委派，如這個範例所示：</span><span class="sxs-lookup"><span data-stu-id="37947-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="37947-135">請勿呼叫`next.Invoke`回應傳送至用戶端之後。</span><span class="sxs-lookup"><span data-stu-id="37947-135">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="37947-136">若要變更`HttpResponse`開始回應後將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="37947-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="37947-137">例如，變更，例如設定標頭、 狀態碼和其他內容，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="37947-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="37947-138">寫入回應主體之後呼叫`next`:</span><span class="sxs-lookup"><span data-stu-id="37947-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="37947-139">可能會造成通訊協定違規。</span><span class="sxs-lookup"><span data-stu-id="37947-139">May cause a protocol violation.</span></span> <span data-ttu-id="37947-140">例如，寫入多個述`content-length`。</span><span class="sxs-lookup"><span data-stu-id="37947-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="37947-141">可能會損毀的本文格式。</span><span class="sxs-lookup"><span data-stu-id="37947-141">May corrupt the body format.</span></span> <span data-ttu-id="37947-142">例如，HTML 頁尾寫入 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="37947-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="37947-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted)是有用的提示，指出如果在傳送標頭和/或本文寫入。</span><span class="sxs-lookup"><span data-stu-id="37947-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="37947-144">排序</span><span class="sxs-lookup"><span data-stu-id="37947-144">Ordering</span></span>

<span data-ttu-id="37947-145">順序中新增中介軟體元件`Configure`方法定義在要求中，會呼叫的順序和回應的相反順序。</span><span class="sxs-lookup"><span data-stu-id="37947-145">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="37947-146">此順序相當重要的安全性、 效能和功能。</span><span class="sxs-lookup"><span data-stu-id="37947-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="37947-147">Configure 方法 （如下所示） 會加入下列的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="37947-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="37947-148">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="37947-148">Exception/error handling</span></span>
2. <span data-ttu-id="37947-149">靜態檔案伺服器</span><span class="sxs-lookup"><span data-stu-id="37947-149">Static file server</span></span>
3. <span data-ttu-id="37947-150">驗證</span><span class="sxs-lookup"><span data-stu-id="37947-150">Authentication</span></span>
4. <span data-ttu-id="37947-151">MVC</span><span class="sxs-lookup"><span data-stu-id="37947-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="37947-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="37947-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="37947-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="37947-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="37947-154">在上述程式碼中`UseExceptionHandler`是第一個新增至管線的中介軟體元件，因此，它會攔截稍後呼叫中發生任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="37947-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="37947-155">靜態檔案中介軟體稱為及早在管線中，讓它可以處理要求，並最少運算無須經過剩餘的元件。</span><span class="sxs-lookup"><span data-stu-id="37947-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="37947-156">靜態檔案中介軟體提供**沒有**授權檢查。</span><span class="sxs-lookup"><span data-stu-id="37947-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="37947-157">任何檔案由提供服務，包括下*wwwroot*，都可以公開使用。</span><span class="sxs-lookup"><span data-stu-id="37947-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="37947-158">請參閱[靜態檔案處理](xref:fundamentals/static-files)如需安全靜態檔案的方法。</span><span class="sxs-lookup"><span data-stu-id="37947-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="37947-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="37947-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="37947-160">如果要求不會處理靜態檔案中介軟體，它會傳遞給身分識別的中介軟體 (`app.UseAuthentication`)，它會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="37947-160">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="37947-161">身分識別不最少運算未經驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="37947-161">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="37947-162">雖然身分識別進行驗證的要求時，授權 （及拒絕） 後才會 MVC 選取特定的 Razor 頁面或控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="37947-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="37947-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="37947-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="37947-164">如果要求不會處理靜態檔案中介軟體，它會傳遞給身分識別的中介軟體 (`app.UseIdentity`)，它會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="37947-164">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="37947-165">身分識別不最少運算未經驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="37947-165">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="37947-166">雖然身分識別進行驗證的要求時，授權 （及拒絕） 後才會 MVC 選取特定的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="37947-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="37947-167">下列範例會示範中介軟體訂購靜態檔案的要求處理前回應壓縮中介軟體的靜態檔案中介軟體的位置。</span><span class="sxs-lookup"><span data-stu-id="37947-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="37947-168">具有此中介軟體的順序，不會壓縮靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="37947-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="37947-169">MVC 回應[UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_)可以壓縮。</span><span class="sxs-lookup"><span data-stu-id="37947-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="37947-170">使用、 執行和對應</span><span class="sxs-lookup"><span data-stu-id="37947-170">Use, Run, and Map</span></span>

<span data-ttu-id="37947-171">您可以設定 HTTP 管線使用`Use`， `Run`，和`Map`。</span><span class="sxs-lookup"><span data-stu-id="37947-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="37947-172">`Use`方法可以縮短管線 (亦即，如果它不會呼叫`next`要求委派)。</span><span class="sxs-lookup"><span data-stu-id="37947-172">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="37947-173">`Run`是一項慣例，以及一些中介軟體元件暴露`Run[Middleware]`在管線結尾處執行的方法。</span><span class="sxs-lookup"><span data-stu-id="37947-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="37947-174">`Map*`分支管線，依照慣例可用擴充功能。</span><span class="sxs-lookup"><span data-stu-id="37947-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="37947-175">[地圖](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)分支要求管線會根據給定的要求路徑的相符項目。</span><span class="sxs-lookup"><span data-stu-id="37947-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="37947-176">如果要求路徑的開頭指定的路徑，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="37947-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="37947-177">下表顯示的要求和回應從`http://localhost:1234`使用先前的程式碼：</span><span class="sxs-lookup"><span data-stu-id="37947-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="37947-178">要求</span><span class="sxs-lookup"><span data-stu-id="37947-178">Request</span></span> | <span data-ttu-id="37947-179">回應</span><span class="sxs-lookup"><span data-stu-id="37947-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="37947-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="37947-180">localhost:1234</span></span> | <span data-ttu-id="37947-181">從非對應委派的 hello。</span><span class="sxs-lookup"><span data-stu-id="37947-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="37947-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="37947-182">localhost:1234/map1</span></span> | <span data-ttu-id="37947-183">測試 1 對應</span><span class="sxs-lookup"><span data-stu-id="37947-183">Map Test 1</span></span> |
| <span data-ttu-id="37947-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="37947-184">localhost:1234/map2</span></span> | <span data-ttu-id="37947-185">測試 2 對應</span><span class="sxs-lookup"><span data-stu-id="37947-185">Map Test 2</span></span> |
| <span data-ttu-id="37947-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="37947-186">localhost:1234/map3</span></span> | <span data-ttu-id="37947-187">從非對應委派的 hello。</span><span class="sxs-lookup"><span data-stu-id="37947-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="37947-188">當`Map`是使用，相符的路徑區段已從`HttpRequest.Path`和附加至`HttpRequest.PathBase`針對每個要求。</span><span class="sxs-lookup"><span data-stu-id="37947-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="37947-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions)分支要求管線會根據給定的述詞的結果。</span><span class="sxs-lookup"><span data-stu-id="37947-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="37947-190">型別的任何述詞`Func<HttpContext, bool>`可用來將要求對應至新分支的管線。</span><span class="sxs-lookup"><span data-stu-id="37947-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="37947-191">在下列範例中，使用述詞來偵測是否存在的查詢字串變數`branch`:</span><span class="sxs-lookup"><span data-stu-id="37947-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="37947-192">下表顯示的要求和回應從`http://localhost:1234`使用先前的程式碼：</span><span class="sxs-lookup"><span data-stu-id="37947-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="37947-193">要求</span><span class="sxs-lookup"><span data-stu-id="37947-193">Request</span></span> | <span data-ttu-id="37947-194">回應</span><span class="sxs-lookup"><span data-stu-id="37947-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="37947-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="37947-195">localhost:1234</span></span> | <span data-ttu-id="37947-196">從非對應委派的 hello。</span><span class="sxs-lookup"><span data-stu-id="37947-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="37947-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="37947-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="37947-198">使用分支 = master</span><span class="sxs-lookup"><span data-stu-id="37947-198">Branch used = master</span></span>|

<span data-ttu-id="37947-199">`Map`支援巢狀結構，例如：</span><span class="sxs-lookup"><span data-stu-id="37947-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="37947-200">`Map`也可以符合多個區段同時發生，例如：</span><span class="sxs-lookup"><span data-stu-id="37947-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="37947-201">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="37947-201">Built-in middleware</span></span>

<span data-ttu-id="37947-202">ASP.NET Core 隨附下列的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="37947-202">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="37947-203">中介軟體</span><span class="sxs-lookup"><span data-stu-id="37947-203">Middleware</span></span> | <span data-ttu-id="37947-204">描述</span><span class="sxs-lookup"><span data-stu-id="37947-204">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="37947-205">驗證</span><span class="sxs-lookup"><span data-stu-id="37947-205">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="37947-206">提供的驗證支援。</span><span class="sxs-lookup"><span data-stu-id="37947-206">Provides authentication support.</span></span> |
| [<span data-ttu-id="37947-207">CORS</span><span class="sxs-lookup"><span data-stu-id="37947-207">CORS</span></span>](xref:security/cors) | <span data-ttu-id="37947-208">設定跨原始資源共用。</span><span class="sxs-lookup"><span data-stu-id="37947-208">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="37947-209">回應快取</span><span class="sxs-lookup"><span data-stu-id="37947-209">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="37947-210">提供支援快取回應。</span><span class="sxs-lookup"><span data-stu-id="37947-210">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="37947-211">回應的壓縮</span><span class="sxs-lookup"><span data-stu-id="37947-211">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="37947-212">提供支援壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="37947-212">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="37947-213">路由傳送</span><span class="sxs-lookup"><span data-stu-id="37947-213">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="37947-214">定義，並限制要求的路由。</span><span class="sxs-lookup"><span data-stu-id="37947-214">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="37947-215">工作階段</span><span class="sxs-lookup"><span data-stu-id="37947-215">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="37947-216">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="37947-216">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="37947-217">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="37947-217">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="37947-218">提供靜態檔案和目錄瀏覽提供服務的支援。</span><span class="sxs-lookup"><span data-stu-id="37947-218">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="37947-219">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="37947-219">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="37947-220">提供重寫 Url，並將要求重新導向的支援。</span><span class="sxs-lookup"><span data-stu-id="37947-220">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="37947-221">寫入中介軟體</span><span class="sxs-lookup"><span data-stu-id="37947-221">Writing middleware</span></span>

<span data-ttu-id="37947-222">中介軟體通常是封裝在類別和使用擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="37947-222">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="37947-223">請考慮下列的中介軟體會從查詢字串設定為目前要求的文化特性：</span><span class="sxs-lookup"><span data-stu-id="37947-223">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="37947-224">注意： 上述範例程式碼會示範建立中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="37947-224">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="37947-225">請參閱[全球化和當地語系化](xref:fundamentals/localization)ASP.NET Core 內建的當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="37947-225">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="37947-226">您可以藉由傳遞文化特性，例如測試中介軟體`http://localhost:7997/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="37947-226">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="37947-227">下列程式碼會將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="37947-227">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="37947-228">下列擴充方法會公開透過中的介軟體[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="37947-228">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="37947-229">下列程式碼呼叫的中介軟體從`Configure`:</span><span class="sxs-lookup"><span data-stu-id="37947-229">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="37947-230">中介軟體應該遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)藉由公開在其建構函式及其相依性。</span><span class="sxs-lookup"><span data-stu-id="37947-230">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="37947-231">中介軟體建構一次，每個*應用程式存留期*。</span><span class="sxs-lookup"><span data-stu-id="37947-231">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="37947-232">請參閱*每個要求的相依性*下方如果您需要與中介軟體要求內的共用服務。</span><span class="sxs-lookup"><span data-stu-id="37947-232">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="37947-233">中介軟體元件可以解決其相依性的相依性插入透過建構函式的參數。</span><span class="sxs-lookup"><span data-stu-id="37947-233">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="37947-234">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)也可以接受其他參數直接。</span><span class="sxs-lookup"><span data-stu-id="37947-234">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="37947-235">每個要求的相依性</span><span class="sxs-lookup"><span data-stu-id="37947-235">Per-request dependencies</span></span>

<span data-ttu-id="37947-236">中介軟體建構應用程式啟動時，無法個別要求，因為*範圍*中介軟體建構函式所使用的存留時間服務不會與其他相依性插入類型共用期間每個要求。</span><span class="sxs-lookup"><span data-stu-id="37947-236">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="37947-237">如果您必須共用*範圍*服務中介軟體和其他類型之間，加入這些服務可`Invoke`方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="37947-237">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="37947-238">`Invoke`方法可以接受其他參數會填入相依性插入。</span><span class="sxs-lookup"><span data-stu-id="37947-238">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="37947-239">例如: </span><span class="sxs-lookup"><span data-stu-id="37947-239">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="37947-240">資源</span><span class="sxs-lookup"><span data-stu-id="37947-240">Resources</span></span>

* [<span data-ttu-id="37947-241">此文件中使用的範例程式碼</span><span class="sxs-lookup"><span data-stu-id="37947-241">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="37947-242">將 HTTP 模組移轉至中介軟體</span><span class="sxs-lookup"><span data-stu-id="37947-242">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="37947-243">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="37947-243">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="37947-244">要求的功能</span><span class="sxs-lookup"><span data-stu-id="37947-244">Request Features</span></span>](request-features.md)