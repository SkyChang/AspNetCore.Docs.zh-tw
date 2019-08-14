---
title: ASP.NET Core Blazor 裝載模型
author: guardrex
description: 瞭解 Blazor 用戶端和伺服器端裝載模型。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 64393e826cb17550085f468f5916fca55973908f
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993395"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="209bd-103">ASP.NET Core Blazor 裝載模型</span><span class="sxs-lookup"><span data-stu-id="209bd-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="209bd-104">依[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="209bd-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="209bd-105">Blazor 是一種 web 架構, 設計用來在瀏覽器中以[WebAssembly](https://webassembly.org/)為基礎的 .net 執行時間 (*Blazor 客戶*端) 或伺服器端在 ASP.NET Core (*Blazor 伺服器端*) 中執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="209bd-106">無論裝載模型為何, 應用程式和元件模型*都相同*。</span><span class="sxs-lookup"><span data-stu-id="209bd-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="209bd-107">若要為本文所述的主控模型建立專案, 請參閱<xref:blazor/get-started>。</span><span class="sxs-lookup"><span data-stu-id="209bd-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="209bd-108">用戶端</span><span class="sxs-lookup"><span data-stu-id="209bd-108">Client-side</span></span>

<span data-ttu-id="209bd-109">Blazor 的主要裝載模型是在 WebAssembly 的瀏覽器中執行用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="209bd-110">Blazor 應用程式、其相依性，和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="209bd-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="209bd-111">應用程式會直接在瀏覽器 UI 執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="209bd-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="209bd-112">UI 更新和事件處理會在同一個進程中進行。</span><span class="sxs-lookup"><span data-stu-id="209bd-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="209bd-113">應用程式的資產會以靜態檔案的形式部署至 web 伺服器或服務, 以提供靜態內容給用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor 用戶端:Blazor 應用程式會在瀏覽器內的 UI 執行緒上執行。](hosting-models/_static/client-side.png)

<span data-ttu-id="209bd-115">若要使用用戶端裝載模型來建立 Blazor 應用程式, 請使用**Blazor WebAssembly 應用程式**範本 ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new))。</span><span class="sxs-lookup"><span data-stu-id="209bd-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="209bd-116">選取 [ **Blazor WebAssembly 應用程式**] 範本之後, 您可以選擇將應用程式設定為使用 ASP.NET Core 後端, 方法是選取 [ **ASP.NET Core**裝載] 核取方塊 ([dotnet] [[新增 blazorwasm-](/dotnet/core/tools/dotnet-new)裝載)]。</span><span class="sxs-lookup"><span data-stu-id="209bd-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="209bd-117">ASP.NET Core 應用程式會將 Blazor 應用程式提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="209bd-118">Blazor 用戶端應用程式可以使用 Web API 呼叫或[SignalR](xref:signalr/introduction), 透過網路與伺服器互動。</span><span class="sxs-lookup"><span data-stu-id="209bd-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="209bd-119">這些範本包含處理下列內容的*blazor. webassembly*腳本:</span><span class="sxs-lookup"><span data-stu-id="209bd-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="209bd-120">下載 .NET 執行時間、應用程式和應用程式的相依性。</span><span class="sxs-lookup"><span data-stu-id="209bd-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="209bd-121">初始化執行時間以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="209bd-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="209bd-122">用戶端裝載模型提供數個優點:</span><span class="sxs-lookup"><span data-stu-id="209bd-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="209bd-123">沒有 .NET 伺服器端相依性。</span><span class="sxs-lookup"><span data-stu-id="209bd-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="209bd-124">應用程式會在下載至用戶端之後完全正常運作。</span><span class="sxs-lookup"><span data-stu-id="209bd-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="209bd-125">用戶端資源和功能都可以充分運用。</span><span class="sxs-lookup"><span data-stu-id="209bd-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="209bd-126">工作會從伺服器卸載至用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="209bd-127">不需要 ASP.NET Core web 伺服器來裝載應用程式。</span><span class="sxs-lookup"><span data-stu-id="209bd-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="209bd-128">無伺服器部署案例可行 (例如, 從 CDN 提供應用程式)。</span><span class="sxs-lookup"><span data-stu-id="209bd-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="209bd-129">用戶端裝載有缺點:</span><span class="sxs-lookup"><span data-stu-id="209bd-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="209bd-130">應用程式僅限於瀏覽器的功能。</span><span class="sxs-lookup"><span data-stu-id="209bd-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="209bd-131">需要支援的用戶端硬體和軟體 (例如 WebAssembly 支援)。</span><span class="sxs-lookup"><span data-stu-id="209bd-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="209bd-132">下載大小較大, 且應用程式需要較長的時間來載入。</span><span class="sxs-lookup"><span data-stu-id="209bd-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="209bd-133">.NET 執行時間和工具支援較不成熟。</span><span class="sxs-lookup"><span data-stu-id="209bd-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="209bd-134">例如, [.NET Standard](/dotnet/standard/net-standard)支援和偵錯工具中有限制。</span><span class="sxs-lookup"><span data-stu-id="209bd-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="209bd-135">伺服器端</span><span class="sxs-lookup"><span data-stu-id="209bd-135">Server-side</span></span>

<span data-ttu-id="209bd-136">使用伺服器端裝載模型, 應用程式會在伺服器上從 ASP.NET Core 應用程式中執行。</span><span class="sxs-lookup"><span data-stu-id="209bd-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="209bd-137">UI 更新、事件處理及 JavaScript 呼叫會透過 [SignalR](xref:signalr/introduction) 連線處理。</span><span class="sxs-lookup"><span data-stu-id="209bd-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![瀏覽器會透過 SignalR 連線, 與伺服器上的應用程式互動 (裝載于 ASP.NET Core 應用程式內)。](hosting-models/_static/server-side.png)

<span data-ttu-id="209bd-139">若要使用伺服器端裝載模型來建立 Blazor 應用程式, 請使用 ASP.NET Core **Blazor 伺服器應用程式**範本 ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new))。</span><span class="sxs-lookup"><span data-stu-id="209bd-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="209bd-140">ASP.NET Core 應用程式會裝載伺服器端應用程式, 並建立用戶端連接的 SignalR 端點。</span><span class="sxs-lookup"><span data-stu-id="209bd-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="209bd-141">ASP.NET Core 應用程式會參考要新增`Startup`的應用程式類別:</span><span class="sxs-lookup"><span data-stu-id="209bd-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="209bd-142">伺服器端服務。</span><span class="sxs-lookup"><span data-stu-id="209bd-142">Server-side services.</span></span>
* <span data-ttu-id="209bd-143">應用程式至要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="209bd-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="209bd-144">*Blazor*腳本&dagger;會建立用戶端連接。</span><span class="sxs-lookup"><span data-stu-id="209bd-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="209bd-145">應用程式會負責保存和還原所需的應用程式狀態 (例如, 萬一網路連線中斷時)。</span><span class="sxs-lookup"><span data-stu-id="209bd-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="209bd-146">伺服器端裝載模型提供數個優點:</span><span class="sxs-lookup"><span data-stu-id="209bd-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="209bd-147">下載大小明顯小於用戶端應用程式, 且應用程式載入速度會更快。</span><span class="sxs-lookup"><span data-stu-id="209bd-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="209bd-148">應用程式會充分利用伺服器功能, 包括使用任何 .NET Core 相容的 Api。</span><span class="sxs-lookup"><span data-stu-id="209bd-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="209bd-149">伺服器上的 .NET Core 是用來執行應用程式, 因此現有的 .NET 工具 (例如, 「偵測」) 會如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="209bd-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="209bd-150">支援瘦用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-150">Thin clients are supported.</span></span> <span data-ttu-id="209bd-151">例如, 伺服器端應用程式會使用不支援 WebAssembly 的瀏覽器, 以及在資源限制的裝置上。</span><span class="sxs-lookup"><span data-stu-id="209bd-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="209bd-152">應用程式的 .NET/C#程式碼基底 (包括應用程式的元件代碼) 不會提供給用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="209bd-153">伺服器端裝載有缺點:</span><span class="sxs-lookup"><span data-stu-id="209bd-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="209bd-154">通常會有較高的延遲。</span><span class="sxs-lookup"><span data-stu-id="209bd-154">Higher latency usually exists.</span></span> <span data-ttu-id="209bd-155">每個使用者互動都牽涉到網路躍點。</span><span class="sxs-lookup"><span data-stu-id="209bd-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="209bd-156">沒有離線支援。</span><span class="sxs-lookup"><span data-stu-id="209bd-156">There's no offline support.</span></span> <span data-ttu-id="209bd-157">如果用戶端連線失敗, 應用程式就會停止運作。</span><span class="sxs-lookup"><span data-stu-id="209bd-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="209bd-158">對於具有許多使用者的應用程式而言, 擴充性是極具挑戰性的。</span><span class="sxs-lookup"><span data-stu-id="209bd-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="209bd-159">伺服器必須管理多個用戶端連接並處理用戶端狀態。</span><span class="sxs-lookup"><span data-stu-id="209bd-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="209bd-160">需要 ASP.NET Core 伺服器才能服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="209bd-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="209bd-161">無伺服器部署案例不可行 (例如, 從 CDN 提供應用程式)。</span><span class="sxs-lookup"><span data-stu-id="209bd-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="209bd-162">&dagger;*Blazor*腳本是從 ASP.NET Core 共用架構中的內嵌資源提供。</span><span class="sxs-lookup"><span data-stu-id="209bd-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="209bd-163">重新連接到相同的伺服器</span><span class="sxs-lookup"><span data-stu-id="209bd-163">Reconnection to the same server</span></span>

<span data-ttu-id="209bd-164">Blazor 伺服器端應用程式需要伺服器的作用中 SignalR 連接。</span><span class="sxs-lookup"><span data-stu-id="209bd-164">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="209bd-165">如果連接中斷, 應用程式會嘗試重新連線到伺服器。</span><span class="sxs-lookup"><span data-stu-id="209bd-165">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="209bd-166">只要用戶端的狀態仍在記憶體中, 用戶端會話就會繼續, 而不會失去狀態。</span><span class="sxs-lookup"><span data-stu-id="209bd-166">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="209bd-167">當用戶端偵測到連線已遺失時, 會在用戶端嘗試重新連線時, 向使用者顯示預設的 UI。</span><span class="sxs-lookup"><span data-stu-id="209bd-167">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="209bd-168">如果重新連線失敗, 則會提供使用者重試的選項。</span><span class="sxs-lookup"><span data-stu-id="209bd-168">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="209bd-169">若要自訂 UI, 請在 *_Host*的 [ `id` Razor 頁面] 中, 以`components-reconnect-modal`做為它的來定義元素。</span><span class="sxs-lookup"><span data-stu-id="209bd-169">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="209bd-170">用戶端會根據連接的狀態, 使用下列其中一個 CSS 類別來更新這個元素:</span><span class="sxs-lookup"><span data-stu-id="209bd-170">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="209bd-171">`components-reconnect-show`&ndash;顯示 UI 以指出連線已中斷, 而且用戶端正在嘗試重新連接。</span><span class="sxs-lookup"><span data-stu-id="209bd-171">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="209bd-172">`components-reconnect-hide`&ndash;用戶端具有使用中的連線, 並隱藏 UI。</span><span class="sxs-lookup"><span data-stu-id="209bd-172">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="209bd-173">`components-reconnect-failed`&ndash;重新連接失敗。</span><span class="sxs-lookup"><span data-stu-id="209bd-173">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="209bd-174">若要再次嘗試重新連接`window.Blazor.reconnect()`, 請呼叫。</span><span class="sxs-lookup"><span data-stu-id="209bd-174">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="209bd-175">預呈現後的具狀態重新連接</span><span class="sxs-lookup"><span data-stu-id="209bd-175">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="209bd-176">在建立伺服器的用戶端連接之前, 預設會設定 Blazor 伺服器端應用程式, 以預先呈現伺服器上的 UI。</span><span class="sxs-lookup"><span data-stu-id="209bd-176">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="209bd-177">這是在 *_Host* Razor 頁面中設定:</span><span class="sxs-lookup"><span data-stu-id="209bd-177">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="209bd-178">用戶端會重新連線至伺服器, 其狀態與用來呈現應用程式的狀態相同。</span><span class="sxs-lookup"><span data-stu-id="209bd-178">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="209bd-179">如果應用程式的狀態仍在記憶體中, 則在建立 SignalR 連接之後, 元件狀態不會重新顯示。</span><span class="sxs-lookup"><span data-stu-id="209bd-179">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="209bd-180">從 Razor 頁面和 views 轉譯具狀態的互動式元件</span><span class="sxs-lookup"><span data-stu-id="209bd-180">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="209bd-181">可設定狀態的互動式元件可以新增至 Razor 頁面或視圖。</span><span class="sxs-lookup"><span data-stu-id="209bd-181">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="209bd-182">當頁面或視圖呈現時, 會使用它來資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="209bd-182">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="209bd-183">一旦用戶端連線已建立, 只要狀態仍在記憶體中, 應用程式就會重新連接到元件狀態。</span><span class="sxs-lookup"><span data-stu-id="209bd-183">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="209bd-184">例如, 下列 Razor 頁面會呈現`Counter`具有使用表單指定之初始計數的元件:</span><span class="sxs-lookup"><span data-stu-id="209bd-184">For example, the following Razor page renders a `Counter` component with an initial count that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="209bd-185">偵測應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="209bd-185">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="209bd-186">設定適用于 Blazor 伺服器端應用程式的 SignalR 用戶端</span><span class="sxs-lookup"><span data-stu-id="209bd-186">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="209bd-187">有時候, 您需要設定 Blazor 伺服器端應用程式所使用的 SignalR 用戶端。</span><span class="sxs-lookup"><span data-stu-id="209bd-187">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="209bd-188">例如, 您可能會想要在 SignalR 用戶端上設定記錄, 以診斷連線問題。</span><span class="sxs-lookup"><span data-stu-id="209bd-188">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="209bd-189">在*Pages/_Host. cshtml*檔案中設定 SignalR 用戶端:</span><span class="sxs-lookup"><span data-stu-id="209bd-189">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="209bd-190">將屬性新增`<script>`至 blazor 的標記。 `autostart="false"`</span><span class="sxs-lookup"><span data-stu-id="209bd-190">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="209bd-191">呼叫`Blazor.start`並傳入指定 SignalR 產生器的設定物件。</span><span class="sxs-lookup"><span data-stu-id="209bd-191">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="209bd-192">其他資源</span><span class="sxs-lookup"><span data-stu-id="209bd-192">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>