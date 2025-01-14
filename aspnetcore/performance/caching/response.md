---
title: ASP.NET Core 中的回應快取
author: rick-anderson
description: 了解如何使用回應快取來降低頻寬需求，並提升 ASP.NET Core 應用程式的效能。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/15/2019
uid: performance/caching/response
ms.openlocfilehash: 4ebac97689347245d25e0954b33729d78dd1b516
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378825"
---
# <a name="response-caching-in-aspnet-core"></a>ASP.NET Core 中的回應快取

作者： [John 羅文](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)和[Luke Latham](https://github.com/guardrex)

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

回應快取可減少用戶端或 proxy 對 web 伺服器提出的要求數目。 回應快取也會減少 web 伺服器執行以產生回應的工作量。 回應快取是由標頭控制，可指定您希望用戶端、proxy 和中介軟體快取回應的方式。

[ResponseCache 屬性](#responsecache-attribute)會參與設定回應快取標頭，用戶端可能會在快取回應時接受。 [回應快取中介軟體](xref:performance/caching/middleware)可以用來快取伺服器上的回應。 中介軟體可以使用 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 屬性來影響伺服器端快取行為。

## <a name="http-based-response-caching"></a>以 HTTP 為基礎的回應快取

[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格描述網際網路快取的行為。 用於快取的主要 HTTP 標頭是快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)，用來指定快取指示*詞。* 指示詞會控制快取行為，因為要求會從用戶端傳送到伺服器，而回應會從伺服器傳回用戶端。 要求和回應會在 proxy 伺服器上移動，而 proxy 伺服器也必須符合 HTTP 1.1 快取規格。

一般 `Cache-Control` 指示詞如下表所示。

| 指示詞                                                       | 動作 |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | 快取可能會儲存回應。 |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | 回應不得由共用快取儲存。 私用快取可能會儲存並重複使用回應。 |
| [最大壽命](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | 用戶端不接受其年齡大於指定秒數的回應。 範例： `max-age=60` （60秒），`max-age=2592000` （1個月） |
| [無-快取](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **在要求上**：快取不得使用預存回應來滿足要求。 源伺服器會重新產生用戶端的回應，中介軟體會在其快取中更新儲存的回應。<br><br>**回應時**：回應不得用於後續要求，而不需在源伺服器上進行驗證。 |
| [否-存放區](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **在要求上**：快取不得儲存要求。<br><br>**回應時**：快取不得儲存回應的任何部分。 |

下表顯示在快取中扮演角色的其他快取標頭。

| 頁首                                                     | 功能 |
| ---------------------------------------------------------- | -------- |
| [存在](https://tools.ietf.org/html/rfc7234#section-5.1)     | 在源伺服器上產生或成功驗證回應後的時間量估計（以秒為單位）。 |
| [失效](https://tools.ietf.org/html/rfc7234#section-5.3) | 回應被視為過時的時間。 |
| [雜](https://tools.ietf.org/html/rfc7234#section-5.4)  | 存在，以提供與 HTTP/1.0 快取的回溯相容性，以設定 `no-cache` 的行為。 如果 `Cache-Control` 標頭存在，則會忽略 @no__t 1 標頭。 |
| [相同](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | 指定在快取回應的原始要求和新要求中都符合所有的 @no__t 0 標頭欄位時，不能傳送快取的回應。 |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>以 HTTP 為基礎的快取會遵循要求快取控制指示詞

快取[控制標頭的 HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格需要快取，以接受用戶端所傳送的有效 @no__t 1 標頭。 用戶端可以使用 @no__t 0 的標頭值提出要求，並強制服務器為每個要求產生新的回應。

如果您考慮 HTTP 快取的目標，一律會遵循用戶端 `Cache-Control` 要求標頭是合理的。 在官方規格中，快取的目的是要降低跨用戶端、proxy 和伺服器網路上滿足要求的延遲和網路負擔。 這不一定是控制源伺服器負載的方式。

使用回應快取[中介軟體](xref:performance/caching/middleware)時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。 [規劃中介軟體的增強功能](https://github.com/aspnet/AspNetCore/issues/2612)，是在決定提供快取回應時，設定中介軟體以忽略要求的 @no__t 1 標頭的機會。 規劃的增強功能可讓您有機會更妥善控制伺服器負載。

## <a name="other-caching-technology-in-aspnet-core"></a>ASP.NET Core 中的其他快取技術

### <a name="in-memory-caching"></a>記憶體內部快取

記憶體內部快取會使用伺服器記憶體來儲存快取的資料。 這種類型的快取適用于單一伺服器，或使用*粘滯會話*的多部伺服器。 「粘滯話」表示用戶端的要求一律會路由傳送至相同的伺服器進行處理。

如需詳細資訊，請參閱<xref:performance/caching/memory>。

### <a name="distributed-cache"></a>分散式快取

當應用程式裝載于雲端或伺服器陣列時，使用分散式快取將資料儲存在記憶體中。 快取會在處理要求的伺服器之間共用。 如果用戶端的快取資料可供使用，則用戶端可以提交由群組中的任何伺服器所處理的要求。 ASP.NET Core 提供 SQL Server 和 Redis 分散式快取。

如需詳細資訊，請參閱<xref:performance/caching/distributed>。

### <a name="cache-tag-helper"></a>快取標記協助程式

使用快取標記協助程式，從 MVC 視圖或 Razor 頁面快取內容。 快取標記協助程式會使用記憶體內部快取來儲存資料。

如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>。

### <a name="distributed-cache-tag-helper"></a>分散式快取標籤協助程式

使用分散式快取標記協助程式，從分散式雲端或 web 伺服陣列案例中的 MVC 視圖或 Razor 頁面快取內容。 分散式快取標記協助程式會使用 SQL Server 或 Redis 來儲存資料。

如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>。

## <a name="responsecache-attribute"></a>ResponseCache 屬性

@No__t-0 指定在回應快取中設定適當標頭所需的參數。

> [!WARNING]
> 針對包含已驗證用戶端資訊的內容停用快取。 應該只針對不會根據使用者身分識別或使用者是否已登入而變更的內容來啟用快取。

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 會根據指定的查詢索引鍵清單值，來改變預存的回應。 當提供了 `*` 的單一值時，中介軟體會依所有要求查詢字串參數來改變回應。

必須啟用回應快取[中介軟體](xref:performance/caching/middleware)，才能設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 屬性。 否則，就會擲回執行時間例外狀況。 @No__t-0 屬性沒有對應的 HTTP 標頭。 屬性是由回應快取中介軟體所處理的 HTTP 功能。 若要讓中介軟體提供快取的回應，查詢字串和查詢字串值必須符合先前的要求。 例如，請考慮下表所示的要求和結果順序。

| 要求                          | 結果                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | 從伺服器傳回。 |
| `http://example.com?key1=value1` | 從中介軟體傳回。 |
| `http://example.com?key1=value2` | 從伺服器傳回。 |

第一個要求是由伺服器傳回，並在中介軟體中快取。 中介軟體會傳回第二個要求，因為查詢字串符合先前的要求。 第三個要求不在中介軟體快取中，因為查詢字串值不符合先前的要求。

@No__t-0 是用來設定和建立（透過 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>） `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter`。 @No__t-0 會執行更新適當 HTTP 標頭和回應功能的工作。 篩選準則：

* 移除 `Vary`、`Cache-Control` 和 `Pragma` 的任何現有標頭。
* 根據 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 中設定的屬性寫出適當的標頭。
* 如果已設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>，則更新回應快取 HTTP 功能。

### <a name="vary"></a>相同

只有在設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 屬性時，才會寫入這個標頭。 屬性設定為 `Vary` 屬性的值。 下列範例會使用 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 屬性：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

使用範例應用程式，使用瀏覽器的網路工具來查看回應標頭。 下列回應標頭會與 Cache1 頁面回應一起傳送：

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a>NoStore 和 Location。無

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 會覆寫大部分的其他屬性。 當這個屬性設定為 `true` 時，@no__t 1 標頭會設定為 `no-store`。 如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 設定為 `None`：

* `Cache-Control` 設定為 `no-store,no-cache`。
* `Pragma` 設定為 `no-cache`。

如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 是 `false`，而 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 是 `None`，`Cache-Control`，而 `Pragma` 設定為 `no-cache`。

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 通常會設定為錯誤網頁的 `true`。 範例應用程式中的 [Cache2] 頁面會產生回應標頭，以指示用戶端不要儲存回應。

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

範例應用程式會傳回具有下列標頭的 Cache2 頁面：

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>位置和持續時間

若要啟用快取，<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 必須設定為正數值，<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 必須是 `Any` （預設值）或 `Client`。 在此情況下，`Cache-Control` 標頭會設定為位置值，後面接著回應的 `max-age`。

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 的 `Any` 和 @no__t 2 的選項會分別轉譯為 `public` 和 `private` 的 @no__t 3 標頭值。 如先前所述，將 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 設定為 `None` 會將 `Cache-Control` 和 @no__t 3 標頭設定為 `no-cache`。

下列範例顯示範例應用程式中的 Cache3 頁面模型，以及設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 並保留預設 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 值所產生的標頭：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

範例應用程式會傳回具有下列標頭的 Cache3 頁面：

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a>快取設定檔

快取設定檔在 `Startup.ConfigureServices` 中設定 MVC/Razor Pages 時，可以設定為選項，而不是複製許多控制器動作屬性的回應快取設定。 在參考的快取設定檔中找到的值，會用來做為 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 的預設值，並由屬性上指定的任何屬性加以覆寫。

設定快取設定檔。 下列範例顯示範例應用程式 `Startup.ConfigureServices` 中的30秒快取設定檔：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

範例應用程式的 Cache4 頁面模型會參照 `Default30` 快取設定檔：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

@No__t-0 可套用至：

* Razor Page 處理常式（類別） &ndash; 屬性無法套用至處理常式方法。
* MVC 控制器（類別）。
* MVC 動作（方法） &ndash; 方法層級屬性會覆寫在類別層級屬性中指定的設定。

由 @no__t 0 快取設定檔套用至 Cache4 頁面回應的產生標頭：

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a>其他資源

* [將回應儲存在快取中](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-控制項](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
