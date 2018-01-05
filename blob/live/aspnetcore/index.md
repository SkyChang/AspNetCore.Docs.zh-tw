---
title: "ASP.NET Core 簡介"
author: rick-anderson
description: "提供 ASP.NET Core 簡介。"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 3a18ed30819a3d395e9bfb5dba0547667a4425e8
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-aspnet-core"></a><span data-ttu-id="6859d-104">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="6859d-104">Introduction to ASP.NET Core</span></span>

<span data-ttu-id="6859d-105">由 [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Shaun Luttin](https://twitter.com/dicshaunary) 提供</span><span class="sxs-lookup"><span data-stu-id="6859d-105">By [Daniel Roth](https://github.com/danroth27), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Shaun Luttin](https://twitter.com/dicshaunary)</span></span>

<span data-ttu-id="6859d-106">ASP.NET Core 是一種跨平台且高效能的[開放原始碼](https://github.com/aspnet/home)架構，用於建置現代化、雲端式、網際網路連線的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6859d-106">ASP.NET Core is a cross-platform, high-performance, [open-source](https://github.com/aspnet/home) framework for building modern, cloud-based, Internet-connected applications.</span></span> <span data-ttu-id="6859d-107">利用 ASP.NET Core，您可以：</span><span class="sxs-lookup"><span data-stu-id="6859d-107">With ASP.NET Core, you can:</span></span>

* <span data-ttu-id="6859d-108">建置 Web 應用程式和服務、[IoT](https://www.microsoft.com/internet-of-things/) 應用程式，以及行動後端。</span><span class="sxs-lookup"><span data-stu-id="6859d-108">Build web apps and services, [IoT](https://www.microsoft.com/internet-of-things/) apps, and mobile backends.</span></span>
* <span data-ttu-id="6859d-109">在 Windows、macOS 和 Linux 上使用您最愛的開發工具。</span><span class="sxs-lookup"><span data-stu-id="6859d-109">Use your favorite development tools on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="6859d-110">部署到雲端或在內部部署。</span><span class="sxs-lookup"><span data-stu-id="6859d-110">Deploy to the cloud or on-premises.</span></span>
* <span data-ttu-id="6859d-111">在 [.NET Core 或 .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上執行。</span><span class="sxs-lookup"><span data-stu-id="6859d-111">Run on [.NET Core or .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="why-use-aspnet-core"></a><span data-ttu-id="6859d-112">為何使用 ASP.NET Core？</span><span class="sxs-lookup"><span data-stu-id="6859d-112">Why use ASP.NET Core?</span></span>

<span data-ttu-id="6859d-113">數百萬的開發人員已使用 (並持續使用) [ASP.NET 4.x](https://docs.microsoft.com/en-us/aspnet/overview) 來建立 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6859d-113">Millions of developers have used (and continue to use) [ASP.NET 4.x](https://docs.microsoft.com/en-us/aspnet/overview) to create web apps.</span></span> <span data-ttu-id="6859d-114">ASP.NET Core 是 ASP.NET 4.x 的重新設計，其架構變更可產生更為精簡且更加模組化的架構。</span><span class="sxs-lookup"><span data-stu-id="6859d-114">ASP.NET Core is a redesign of ASP.NET 4.x, with architectural changes that result in a leaner, more modular framework.</span></span>

<span data-ttu-id="6859d-115">ASP.NET Core 提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="6859d-115">ASP.NET Core provides the following benefits:</span></span>

* <span data-ttu-id="6859d-116">用於建置 Web UI 和 Web API 的統一劇本。</span><span class="sxs-lookup"><span data-stu-id="6859d-116">A unified story for building web UI and web APIs.</span></span>
* <span data-ttu-id="6859d-117">整合[現代化用戶端架構](xref:client-side/index)和開發工作流程。</span><span class="sxs-lookup"><span data-stu-id="6859d-117">Integration of [modern, client-side frameworks](xref:client-side/index) and development workflows.</span></span>
* <span data-ttu-id="6859d-118">雲端就緒、以環境為基礎的[組態系統](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="6859d-118">A cloud-ready, environment-based [configuration system](xref:fundamentals/configuration/index).</span></span>
* <span data-ttu-id="6859d-119">內建的[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="6859d-119">Built-in [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="6859d-120">輕量型、[高效能](https://github.com/aspnet/benchmarks) \(英文\) 且模組化的 HTTP 要求管線。</span><span class="sxs-lookup"><span data-stu-id="6859d-120">A lightweight, [high-performance](https://github.com/aspnet/benchmarks), and modular HTTP request pipeline.</span></span>
* <span data-ttu-id="6859d-121">能夠在 [IIS](xref:publishing/iis)、[Nginx](xref:publishing/linuxproduction)、[Apache](xref:publishing/apache-proxy)、[Docker](xref:publishing/docker) 上裝載，或自我裝載於您自己的處理序中。</span><span class="sxs-lookup"><span data-stu-id="6859d-121">Ability to host on [IIS](xref:publishing/iis), [Nginx](xref:publishing/linuxproduction), [Apache](xref:publishing/apache-proxy), [Docker](xref:publishing/docker), or self-host in your own process.</span></span>
* <span data-ttu-id="6859d-122">以 [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 為目標時，會有並存應用程式版本設定。</span><span class="sxs-lookup"><span data-stu-id="6859d-122">Side-by-side app versioning when targeting [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
* <span data-ttu-id="6859d-123">可簡化現代網頁程式開發的工具。</span><span class="sxs-lookup"><span data-stu-id="6859d-123">Tooling that simplifies modern web development.</span></span>
* <span data-ttu-id="6859d-124">能夠在 Windows、macOS 和 Linux 上建置並執行。</span><span class="sxs-lookup"><span data-stu-id="6859d-124">Ability to build and run on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="6859d-125">開放原始碼和[社群導向](https://live.asp.net/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="6859d-125">Open-source and [community-focused](https://live.asp.net/).</span></span>

<span data-ttu-id="6859d-126">ASP.NET Core 完全以 [NuGet](https://www.nuget.org/) 套件的形式提供。</span><span class="sxs-lookup"><span data-stu-id="6859d-126">ASP.NET Core ships entirely as [NuGet](https://www.nuget.org/) packages.</span></span> <span data-ttu-id="6859d-127">如此可讓您將應用程式最佳化，使其只包含需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="6859d-127">This allows you to optimize your app to include only the necessary NuGet packages.</span></span> <span data-ttu-id="6859d-128">事實上，以 .NET Core 為目標的 ASP.NET Core 2.x 應用程式只需要[單一 NuGet 套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="6859d-128">In fact, ASP.NET Core 2.x apps targeting .NET Core only require a [single NuGet package](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="6859d-129">應用程式介面區較小的優點包括更嚴密的安全性、減少維護工作，以及提升效能。</span><span class="sxs-lookup"><span data-stu-id="6859d-129">The benefits of a smaller app surface area include tighter security, reduced servicing, and improved performance.</span></span>

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a><span data-ttu-id="6859d-130">使用 ASP.NET Core MVC 建置 Web API 和 Web UI</span><span class="sxs-lookup"><span data-stu-id="6859d-130">Build web APIs and web UI using ASP.NET Core MVC</span></span>

<span data-ttu-id="6859d-131">ASP.NET Core MVC 提供了建置 [Web API](xref:tutorials/index#building-web-apis) 和 [Web 應用程式](xref:tutorials/index#building-web-applications)的功能：</span><span class="sxs-lookup"><span data-stu-id="6859d-131">ASP.NET Core MVC provides features to build [web APIs](xref:tutorials/index#building-web-apis) and [web apps](xref:tutorials/index#building-web-applications):</span></span>

* <span data-ttu-id="6859d-132">[模型檢視控制器 (MVC) 模式](xref:mvc/overview)有助於讓您的 Web API 和 Web 應用程式[可測試](testing/index.md)。</span><span class="sxs-lookup"><span data-stu-id="6859d-132">The [Model-View-Controller (MVC) pattern](xref:mvc/overview) helps make your web APIs and web apps [testable](testing/index.md).</span></span>
* <span data-ttu-id="6859d-133">[Razor 頁面](xref:mvc/razor-pages/index) (ASP.NET Core 2.0 中的新功能) 是以頁面為基礎的程式設計模型，可讓建置 Web UI 更容易且更具工作效率。</span><span class="sxs-lookup"><span data-stu-id="6859d-133">[Razor Pages](xref:mvc/razor-pages/index) (new in ASP.NET Core 2.0) is a page-based programming model that makes building web UI easier and more productive.</span></span>
* <span data-ttu-id="6859d-134">[Razor 標記](xref:mvc/views/razor)提供了適用於 [Razor 頁面](xref:mvc/razor-pages/index)和 [MVC 檢視](xref:mvc/views/overview)的高效率語法。</span><span class="sxs-lookup"><span data-stu-id="6859d-134">[Razor markup](xref:mvc/views/razor) provides a productive syntax for [Razor Pages](xref:mvc/razor-pages/index) and [MVC views](xref:mvc/views/overview).</span></span>
* <span data-ttu-id="6859d-135">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="6859d-135">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>
* <span data-ttu-id="6859d-136">[多個資料格式和內容交涉](mvc/models/formatting.md)的內建支援可讓您的 Web API 連線到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="6859d-136">Built-in support for [multiple data formats and content negotiation](mvc/models/formatting.md) lets your web APIs reach a broad range of clients, including browsers and mobile devices.</span></span>
* <span data-ttu-id="6859d-137">[模型繫結](xref:mvc/models/model-binding)會自動將 HTTP 要求中的資料對應至動作方法參數。</span><span class="sxs-lookup"><span data-stu-id="6859d-137">[Model binding](xref:mvc/models/model-binding) automatically maps data from HTTP requests to action method parameters.</span></span>
* <span data-ttu-id="6859d-138">[模型驗證](xref:mvc/models/validation)會自動執行用戶端和伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="6859d-138">[Model validation](xref:mvc/models/validation) automatically performs client- and server-side validation.</span></span>

## <a name="client-side-development"></a><span data-ttu-id="6859d-139">用戶端開發</span><span class="sxs-lookup"><span data-stu-id="6859d-139">Client-side development</span></span>

<span data-ttu-id="6859d-140">ASP.NET Core 可完美整合常用的用戶端架構和程式庫，包括 [Angular](xref:spa/angular)、[React](xref:spa/react) 與 [Bootstrap](xref:client-side/bootstrap)。</span><span class="sxs-lookup"><span data-stu-id="6859d-140">ASP.NET Core integrates seamlessly with popular client-side frameworks and libraries, including [Angular](xref:spa/angular), [React](xref:spa/react), and [Bootstrap](xref:client-side/bootstrap).</span></span> <span data-ttu-id="6859d-141">如需詳細資料，請參閱[用戶端開發](xref:client-side/index)。</span><span class="sxs-lookup"><span data-stu-id="6859d-141">See [Client-side development](xref:client-side/index) for more details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6859d-142">後續步驟</span><span class="sxs-lookup"><span data-stu-id="6859d-142">Next steps</span></span>

<span data-ttu-id="6859d-143">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="6859d-143">For more information, see the following resources:</span></span>

* [<span data-ttu-id="6859d-144">ASP.NET Core 教學課程</span><span class="sxs-lookup"><span data-stu-id="6859d-144">ASP.NET Core tutorials</span></span>](xref:tutorials/index)
* [<span data-ttu-id="6859d-145">ASP.NET Core 基本概念</span><span class="sxs-lookup"><span data-stu-id="6859d-145">ASP.NET Core fundamentals</span></span>](xref:fundamentals/index)
* <span data-ttu-id="6859d-146">[每週的 ASP.NET 社群之聲](https://live.asp.net/) \(英文\) 涵蓋了小組的進度和計劃，</span><span class="sxs-lookup"><span data-stu-id="6859d-146">[The weekly ASP.NET community standup](https://live.asp.net/) covers the team's progress and plans.</span></span> <span data-ttu-id="6859d-147">並提供新的部落格和協力廠商軟體。</span><span class="sxs-lookup"><span data-stu-id="6859d-147">It features new blogs and third-party software.</span></span>