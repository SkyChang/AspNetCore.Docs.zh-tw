---
uid: webhooks/index
title: "ASP.NET Webhook 概觀 |Microsoft 文件"
author: rick-anderson
description: "ASP.NET Webhook 簡介。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="70f7d-103">ASP.NET Webhook 概觀</span><span class="sxs-lookup"><span data-stu-id="70f7d-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="70f7d-104">Webhook 為輕量型的 HTTP 模式，提供簡單的 pub/sub 模型一起接線 Web Api 和 SaaS 服務。</span><span class="sxs-lookup"><span data-stu-id="70f7d-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="70f7d-105">當事件發生在服務中時，會傳送通知形式的 HTTP POST 要求到已註冊的訂閱者。</span><span class="sxs-lookup"><span data-stu-id="70f7d-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="70f7d-106">POST 要求包含可讓收件者要據此採取行動的事件的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="70f7d-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="70f7d-107">其簡化，因為 Webhook 已公開的服務，包括大量[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)， [Slack](http://www.slack.com)，[等量磁碟區](http://www.stripe.com)， [Trello](http://www.trello.com/)，等等。</span><span class="sxs-lookup"><span data-stu-id="70f7d-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="70f7d-108">比方說，WebHook 可能表示檔案已在變更[Dropbox](http://dropbox.com/)，或在 GitHub 中的程式碼變更已認可或已起始付款中[PayPal](http://www.paypal.com/)，或已在中建立卡片[Trello](http://www.trello.com/)。</span><span class="sxs-lookup"><span data-stu-id="70f7d-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="70f7d-109">有無限的可能性 ！</span><span class="sxs-lookup"><span data-stu-id="70f7d-109">The possibilities are endless!</span></span>

<span data-ttu-id="70f7d-110">Microsoft ASP.NET Webhook 可以更輕鬆地傳送和接收 Webhook 做為您的 ASP.NET 應用程式的一部分：</span><span class="sxs-lookup"><span data-stu-id="70f7d-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="70f7d-111">在接收端，它會提供通用模型用於接收和處理從任意數目的 WebHook 提供者的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="70f7d-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="70f7d-112">這樣做，可以支援現成[Dropbox](http://dropbox.com/)， [GitHub](http://www.github.com/)， [Bitbucket](https://bitbucket.org/)， [MailChimp](http://www.mailchimp.com/)， [PayPal](http://www.paypal.com/)，[推送](http://www.pusher.com)， [Salesforce](http://www.salesforce.com)， [Slack](http://www.slack.com)，[等量磁碟區](http://www.stripe.com)， [Trello](http://www.trello.com/)，[WordPress](http://www.wordpress.com)和[Zendesk](https://www.zendesk.com/)但所以可以輕鬆地加入更多的支援。</span><span class="sxs-lookup"><span data-stu-id="70f7d-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="70f7d-113">在傳送端中，它會提供支援供管理和儲存訂閱也將事件通知傳送至正確的訂閱者集合。</span><span class="sxs-lookup"><span data-stu-id="70f7d-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="70f7d-114">這可讓您定義您自己的 「 訂閱者 」 可以訂閱並事情發生時予以通知的事件集。</span><span class="sxs-lookup"><span data-stu-id="70f7d-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="70f7d-115">兩個部分可以一起或分開使用根據您的案例。</span><span class="sxs-lookup"><span data-stu-id="70f7d-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="70f7d-116">如果您只需要 Webhook 接收來自其他服務，則您可以使用只有的接收者部分;如果您只想要公開 Webhook 供其他人使用，然後您可以這麼做。</span><span class="sxs-lookup"><span data-stu-id="70f7d-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="70f7d-117">ASP.NET Web API 2 和 ASP.NET MVC 5 為目標的程式碼，可做為[GitHub 上的 OSS](https://github.com/aspnet/WebHooks)。</span><span class="sxs-lookup"><span data-stu-id="70f7d-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="70f7d-118">Webhook 概觀</span><span class="sxs-lookup"><span data-stu-id="70f7d-118">WebHooks Overview</span></span>

<span data-ttu-id="70f7d-119">Webhook 為模式，這表示不同如何使用從服務到服務，但基本的概念是一樣。</span><span class="sxs-lookup"><span data-stu-id="70f7d-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="70f7d-120">您可以將 Webhook 視為簡單 pub/sub 模型其中使用者可以在其他地方發生的事件訂閱。</span><span class="sxs-lookup"><span data-stu-id="70f7d-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="70f7d-121">事件通知會傳送為 HTTP POST 要求，其中包含事件本身的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="70f7d-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="70f7d-122">通常的 HTTP POST 要求包含 JSON 物件或觸發程序判斷 WebHook 寄件者包括造成 WebHook 事件的相關資訊的 HTML 表單資料。</span><span class="sxs-lookup"><span data-stu-id="70f7d-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="70f7d-123">例如，從的 WebHook POST 要求主體範例[GitHub](http://www.github.com/)看起來像這樣的結果的特定儲存機制中正在開啟新議題：</span><span class="sxs-lookup"><span data-stu-id="70f7d-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

<span data-ttu-id="70f7d-124">若要確保此 WebHook 確實是來自預定的傳送者，會在 POST 要求以某種方式安全，並驗證收件者。</span><span class="sxs-lookup"><span data-stu-id="70f7d-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="70f7d-125">例如， [GitHub Webhook](https://developer.github.com/webhooks/)包含*X 中樞簽章型*具有雜湊的要求主體，所以您不需擔心接收者實作所檢查的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="70f7d-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="70f7d-126">WebHook 流程通常會像下面這樣：</span><span class="sxs-lookup"><span data-stu-id="70f7d-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="70f7d-127">WebHook 寄件者會公開用戶端可以訂閱的事件。</span><span class="sxs-lookup"><span data-stu-id="70f7d-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="70f7d-128">事件描述可觀察變更到系統，例如新的資料項目已插入，已完成處理程序，或其他項目。</span><span class="sxs-lookup"><span data-stu-id="70f7d-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="70f7d-129">WebHook 接收者可訂閱註冊 WebHook 四個項目所組成：</span><span class="sxs-lookup"><span data-stu-id="70f7d-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="70f7d-130">URI 形式的 HTTP POST 要求; 其中應該公佈事件通知</span><span class="sxs-lookup"><span data-stu-id="70f7d-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="70f7d-131">一組篩選描述特定的事件，應該引發此 WebHook。</span><span class="sxs-lookup"><span data-stu-id="70f7d-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="70f7d-132">秘密金鑰用來簽署的 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="70f7d-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="70f7d-133">這是要包含在 HTTP POST 要求中的其他資料。</span><span class="sxs-lookup"><span data-stu-id="70f7d-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="70f7d-134">這可以例如額外的 HTTP 標頭欄位或 HTTP POST 要求主體中包含的屬性。</span><span class="sxs-lookup"><span data-stu-id="70f7d-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="70f7d-135">事件發生時，一旦找到相符的 WebHook 註冊，並提交 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="70f7d-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="70f7d-136">通常，產生的 HTTP POST 要求的是如果因某種原因收件者沒有回應的 HTTP POST 要求會導致錯誤回應重試數次。</span><span class="sxs-lookup"><span data-stu-id="70f7d-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="70f7d-137">Webhook 處理管線</span><span class="sxs-lookup"><span data-stu-id="70f7d-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="70f7d-138">Microsoft ASP.NET Webhook 處理管線的內送的 Webhook 看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="70f7d-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET Webhook 處理管線](_static/WebHookReceivers.png)

<span data-ttu-id="70f7d-140">兩個索引鍵這裡的概念*接收者*和*處理常式*:</span><span class="sxs-lookup"><span data-stu-id="70f7d-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="70f7d-141">*接收者*必須負責處理來自特定寄件者的 WebHook 特定類別，以及強制執行安全性檢查，以確保 WebHook 要求確實是來自預定的傳送者。</span><span class="sxs-lookup"><span data-stu-id="70f7d-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="70f7d-142">*處理常式*通常是使用者程式碼執行處理特定的 WebHook。</span><span class="sxs-lookup"><span data-stu-id="70f7d-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="70f7d-143">下列節點中的詳細說明這些概念。</span><span class="sxs-lookup"><span data-stu-id="70f7d-143">In the following nodes these concepts are described in more details.</span></span>