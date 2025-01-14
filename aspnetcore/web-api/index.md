---
title: 使用 ASP.NET Core 建立 Web API
author: scottaddie
description: 了解使用 ASP.NET Core 建立 Web API 的基本概念。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2019
uid: web-api/index
ms.openlocfilehash: aab9b848eb6e69055b019c9253c716898e9847e2
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149339"
---
# <a name="create-web-apis-with-aspnet-core"></a>使用 ASP.NET Core 建立 Web API

作者：[Scott Addie](https://github.com/scottaddie) 與 [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core 支援使用 C# 建立 RESTful 服務，也稱為 Web API。 若要處理要求，Web API 會使用控制器。 Web API 中的「控制器」都衍生自類別 `ControllerBase`。 本文說明如何使用控制器來處理 Web API 要求。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples)。 ([如何下載](xref:index#how-to-download-a-sample))。

## <a name="controllerbase-class"></a>ControllerBase 類別

Web API 由一個或多個衍生自<xref:Microsoft.AspNetCore.Mvc.ControllerBase>的控制器類別所組成。 Web API 專案範本提供入門控制器：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

請不要從 <xref:Microsoft.AspNetCore.Mvc.Controller> 類別衍生以建立 Web API 控制器。 `Controller` 衍生自 `ControllerBase` 並會新增檢視支援，以供處理網頁，而不是 Web API 要求。 此規則有例外狀況：如果您打算針對 views 和 web Api 使用相同的控制器，請從`Controller`衍生。

`ControllerBase` 類別提供許多處理 HTTP 要求的實用屬性和方法。 例如，`ControllerBase.CreatedAtAction` 會傳回 201 狀態碼：

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

以下是 `ControllerBase` 提供的一些其他方法範例。

|方法   |注意    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| 傳回 400 狀態碼。|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>|傳回 404 狀態碼。|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|傳回檔案。|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|叫用[模型繫結](xref:mvc/models/model-binding)。|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|叫用[模型驗證](xref:mvc/models/validation)。|

如需所有可用方法和屬性的清單，請參閱 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。

## <a name="attributes"></a>屬性

<xref:Microsoft.AspNetCore.Mvc> 命名空間提供的屬性，可用來設定 Web API 控制器和動作方法的行為。 下列範例會使用屬性來指定支援的 HTTP 動作動詞，以及任何可能傳回的已知 HTTP 狀態碼：

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

以下是一些其他可用的屬性範例。

|屬性|注意|
|---------|-----|
|[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |指定控制器或動作的 URL 模式。|
|[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |指定模型繫結要包含的前置詞和屬性。|
|[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |識別支援 HTTP GET 動作動詞的動作。|
|[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|指定動作所接受的資料類型。|
|[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|指定動作所傳回的資料類型。|

如需包含可用屬性的清單，請參閱 <xref:Microsoft.AspNetCore.Mvc> 命名空間。

## <a name="apicontroller-attribute"></a>ApiController 屬性

您可以將[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性套用至控制器類別，以啟用下列固定、API 特定的行為：

* [屬性路由需求](#attribute-routing-requirement)
* [HTTP 400 自動回應](#automatic-http-400-responses)
* [繫結來源參數推斷](#binding-source-parameter-inference)
* [多部分/表單資料要求推斷](#multipartform-data-request-inference)
* [錯誤狀態碼的問題詳細資料](#problem-details-for-error-status-codes)

這些功能都需要[相容性版本](xref:mvc/compatibility-version) 2.1 或更新版本。

### <a name="attribute-on-specific-controllers"></a>特定控制器上的屬性

`[ApiController]` 屬性可以套用至特定的控制器，如專案範本中的下列範例所示：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a>多個控制器上的屬性

在多個控制站上使用同一屬性的方法之一，就是建立以 `[ApiController]` 屬性標註的自訂基底控制器類別。 下列範例顯示自訂基類，以及從它衍生的控制器：

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a>元件上的屬性

如果[相容性版本](xref:mvc/compatibility-version)設定為 2.2 或更新版本，`[ApiController]` 屬性就可以套用至組件。 以這種方式標註會將 Web API 行為套用至組件中的所有控制器。 沒有任何方法可以退出個別控制器。 將元件層級屬性套用至`Startup`類別周圍的命名空間宣告：

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

::: moniker-end

## <a name="attribute-routing-requirement"></a>屬性路由需求

`[ApiController]` 屬性會讓屬性路由需求。 例如：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

在中<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*>`UseEndpoints`，您`Startup.Configure`可以透過、或所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)來存取動作。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

無法透過 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 中所定義的[慣例路由](xref:mvc/controllers/routing#conventional-routing)或 `Startup.Configure` 中的 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 來存取動作。

::: moniker-end

### <a name="automatic-http-400-responses"></a>HTTP 400 自動回應

`[ApiController]` 屬性會讓模型驗證錯誤自動觸發 HTTP 400 回應。 因此，動作方法不再需要下列程式碼：

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

ASP.NET Core MVC 會使用<xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter>動作篩選準則來執行上述檢查。

### <a name="default-badrequest-response"></a>預設 BadRequest 回應

當相容性版本為2.1 時，HTTP 400 回應的預設回應類型為<xref:Microsoft.AspNetCore.Mvc.SerializableError>。 下列要求主體是序列化類型的範例：

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

當相容性版本為2.2 或更新版本時，HTTP 400 回應的預設回應類型為<xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>。 下列要求主體是序列化類型的範例：

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

`ValidationProblemDetails`類型：

* 提供電腦可讀取的格式，以指定 Web API 回應中的錯誤。
* 符合[RFC 7807 規格](https://tools.ietf.org/html/rfc7807)。

::: moniker-end

### <a name="log-automatic-400-responses"></a>記錄自動 400 回應

請參閱[如何在模型驗證錯誤上記錄自動 400 回應 (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157) \(英文\)。

### <a name="disable-automatic-400-response"></a>停用自動400回應

若要停用自動 400 行為，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 屬性設定為 `true`。 在 `Startup.ConfigureServices` 中新增下列醒目提示的程式碼：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a>繫結來源參數推斷

繫結來源屬性可定義動作參數值的所在位置。 系統提供下列繫結來源屬性：

|屬性|繫結來源 |
|---------|---------|
|[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | 要求本文 |
|[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | 要求本文中的表單資料 |
|[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | 要求標頭 |
|[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | 要求查詢字串參數 |
|[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | 來自目前要求的路由資料 |
|[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | 作為動作參數插入的要求服務 |

> [!WARNING]
> 當值可能包含 `%2f` (也就是 `/`) 時，請勿使用 `[FromRoute]`。 `%2f` 不會是未逸出的 `/`。 如果值可能包含 `%2f`，請使用 `[FromQuery]`。

若沒有 `[ApiController]` 屬性或繫結來源屬性 (例如 `[FromQuery]`)，ASP.NET Core 執行階段會嘗試使用複雜物件模型繫結器。 複雜物件模型繫結器會以定義的順序從值提供者提取資料。

在下列範例中，`[FromQuery]` 屬性表示 `discontinuedOnly` 參數值是在要求 URL 查詢字串中提供：

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

`[ApiController]` 屬性會依據動作參數的預設資料來源套用推斷規則。 這些規則透過將屬性套用至動作參數，讓您不必以手動方式識別的繫結來源。 繫結來源推斷規則的行為如下所示：

* 系統會依據複雜類型參數推斷 `[FromBody]`。 如果是任何具有像是 <xref:Microsoft.AspNetCore.Http.IFormCollection> 與 <xref:System.Threading.CancellationToken> 等特殊意義的複雜內建類型，則為 `[FromBody]` 推斷規則的例外。 繫結來源推斷程式碼會忽略這些特殊的類型。
* `[FromForm]` 針對類型 <xref:Microsoft.AspNetCore.Http.IFormFile> 與 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 的動作參數推斷的。 而不會依據任何簡單或使用者定義的類型進行推斷。
* 系統會依據符合路由範本參數的任何動作參數名稱推斷 `[FromRoute]`。 如果有多個路由符合動作參數，則會將任何路由值視為 `[FromRoute]`。
* 系統會依據任何其他動作參數推斷 `[FromQuery]`。

### <a name="frombody-inference-notes"></a>FromBody 推斷備註

並不會為像是 `string` 或 `int` 等簡單型別，推斷 `[FromBody]`。 因此，當需要使用該功能時，應為簡單型別使用 `[FromBody]` 屬性。

當動作有多個參數從要求主體繫結時，會擲回例外狀況。 例如，下列所有動作方法簽章都會造成例外狀況：

* 因兩個參數都是複雜類型，而推斷 `[FromBody]`。

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* 因為其為複雜類型，所以一個有 `[FromBody]` 屬性，而推斷另一個。

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* 兩者均有 `[FromBody]` 屬性。

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> 在 ASP.NET Core 2.1 中，集合類型參數 (例如清單與陣列) 不正確地推斷為 `[FromQuery]`。 `[FromBody]` 屬性應該用於這些參數 (若它們將從要求本文繫結)。 此行為在 ASP.NET Core 2.2 或更新版本中已修正，其中集合類型參數預設推斷為從本文繫結。

::: moniker-end

### <a name="disable-inference-rules"></a>停用推斷規則

若要停用繫結來源推斷，請將 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 設定為 `true`。 將下列程式碼加入 `Startup.ConfigureServices`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a>多部分/表單資料要求推斷

當`[ApiController]`動作參數以[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)屬性標注時，屬性會套用推斷規則。 會`multipart/form-data`推斷要求內容類型。

若要停用預設行為，請<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>將屬性`true`設定`Startup.ConfigureServices`為中的：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

## <a name="problem-details-for-error-status-codes"></a>錯誤狀態碼的問題詳細資料

當相容性版本為 2.2 或更新版本時，MVC 會將錯誤結果 (具有狀態碼 400 以上的結果) 為具有 <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> 的結果。 以 [RFC 7807 規格](https://tools.ietf.org/html/rfc7807)為基礎的 `ProblemDetails` 類型，在 HTTP 回應中提供電腦可讀取的錯誤詳細資料。

請考慮下列控制器動作中的程式碼：

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

方法會產生`ProblemDetails`具有主體的 HTTP 404 狀態碼。 `NotFound` 例如：

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a>停用 ProblemDetails 回應

當屬性設定為`ProblemDetails` `true`時，會停用實例的自動建立。 <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> 將下列程式碼加入 `Startup.ConfigureServices`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
