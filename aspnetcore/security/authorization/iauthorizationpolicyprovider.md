---
title: ASP.NET Core 中的自訂授權原則提供者
author: mjrousos
description: 瞭解如何在 ASP.NET Core 應用程式中使用自訂的 IAuthorizationPolicyProvider，以動態方式產生授權原則。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: b11f7f94e1042e65d5f2ff8ab0fe9ea7838bebeb
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108059"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>在 ASP.NET Core 中使用 IAuthorizationPolicyProvider 的自訂授權原則提供者 

由[Mike Rousos](https://github.com/mjrousos)

通常在使用以[原則為基礎的授權](xref:security/authorization/policies)時，會藉`AuthorizationOptions.AddPolicy`由呼叫作為授權服務設定的一部分來註冊原則。 在某些情況下，不可能（或需要）以這種方式註冊所有授權原則。 在這些情況下，您可以使用自`IAuthorizationPolicyProvider`定義來控制授權原則的提供方式。

自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能有用的案例範例包括：

* 使用外部服務來提供原則評估。
* 使用較大範圍的原則（例如，針對不同的房間號碼或年齡），因此使用`AuthorizationOptions.AddPolicy`呼叫來新增每個授權原則並不合理。
* 在執行時間根據外部資料源（例如資料庫）中的資訊建立原則，或透過另一個機制動態判斷授權需求。

從[AspNetCore GitHub 存放庫](https://github.com/aspnet/AspNetCore)中[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)。 下載 aspnet/AspNetCore 存放庫 ZIP 檔案。 解壓縮檔案。 流覽至*src/Security/samples/CustomPolicyProvider*專案資料夾。

## <a name="customize-policy-retrieval"></a>自訂原則抓取

ASP.NET Core 應用程式會使用`IAuthorizationPolicyProvider`介面的執行來取出授權原則。 根據預設，會註冊並使用[DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) 。 `DefaultAuthorizationPolicyProvider`傳回`AuthorizationOptions` 呼叫`IServiceCollection.AddAuthorization`中所提供的原則。

您可以在應用程式的相依性`IAuthorizationPolicyProvider` [插入](xref:fundamentals/dependency-injection)容器中註冊不同的執行，以自訂此行為。 

此`IAuthorizationPolicyProvider`介面包含兩個 api：

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)會傳回指定名稱的授權原則。
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)會傳回預設授權原則（未指定原則的`[Authorize]`屬性所使用的原則）。 

藉由執行這兩個 Api，您可以自訂如何提供授權原則。

## <a name="parameterized-authorize-attribute-example"></a>參數化授權屬性範例

其中一個有用`IAuthorizationPolicyProvider`的案例，就是`[Authorize]`啟用其需求取決於參數的自訂屬性。 例如，在以[原則為基礎的授權](xref:security/authorization/policies)檔中，以存留期為基礎的（"AtLeast21"）原則是用來做為範例。 如果應用程式中的不同控制器動作應該提供給*不同*年齡的使用者使用，則有許多不同的以年齡為基礎的原則可能會很有用。 您可以使用自訂`AuthorizationOptions` `IAuthorizationPolicyProvider`動態產生原則，而不是註冊應用程式在中所需的所有不同以年齡為基礎的原則。 若要更輕鬆地使用原則，您可以使用像`[MinimumAgeAuthorize(20)]`是的自訂授權屬性來標注動作。

## <a name="custom-authorization-attributes"></a>自訂授權屬性

授權原則是以其名稱來識別。 先前所`MinimumAgeAuthorizeAttribute`述的自訂必須將引數對應到可以用來抓取對應授權原則的字串。 您可以藉由衍生自`AuthorizeAttribute`並`Age`讓屬性包裝`AuthorizeAttribute.Policy`屬性，來執行這項操作。

```csharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

這個屬性型別具有`Policy`以硬式編碼前置詞（`"MinimumAge"`）為基礎的字串，以及透過函式傳入的整數。

您可以使用與其他`Authorize`屬性相同的方式將它套用至動作，不同之處在于它會使用整數做為參數。

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>自訂 IAuthorizationPolicyProvider

自訂`MinimumAgeAuthorizeAttribute`可讓您輕鬆地要求授權原則，以取得所需的最短存留期。 下一個要解決的問題是確保授權原則適用于所有這些不同的年齡。 這就是有用`IAuthorizationPolicyProvider`的地方。

使用`MinimumAgeAuthorizationAttribute`時，授權原則名稱會遵循模式`"MinimumAge" + Age`，因此自訂`IAuthorizationPolicyProvider`應該會藉由下列方式產生授權原則：

* 正在剖析原則名稱中的年齡。
* 使用`AuthorizationPolicyBuilder`建立新的`AuthorizationPolicy`
* 在此和下列範例中，會假設使用者是透過 cookie 進行驗證。 應該`AuthorizationPolicyBuilder`使用至少一個授權配置名稱來建立，或一律成功。 否則，沒有關于如何為使用者提供挑戰的資訊，將會擲回例外狀況。
* 根據年齡`AuthorizationPolicyBuilder.AddRequirements`將需求新增至原則。 在其他案例中，您`RequireClaim`可以改用、 `RequireRole`或`RequireUserName` 。

```csharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a>多個授權原則提供者

使用自訂`IAuthorizationPolicyProvider`的執行時，請記住，ASP.NET Core 只會使用一個`IAuthorizationPolicyProvider`的實例。 如果自訂提供者無法針對將使用的所有原則名稱提供授權原則，則應該切換回備份提供者。 

例如，假設有一個應用程式同時需要自訂年齡原則和更傳統的以角色為基礎的原則抓取。 這類應用程式可以使用自訂授權原則提供者：

* 嘗試剖析原則名稱。 
* 如果原則名稱不包含年齡，則`DefaultAuthorizationPolicyProvider`會呼叫不同的原則提供者（例如）。

如上所`IAuthorizationPolicyProvider`示的範例執行可以更新為使用， `DefaultAuthorizationPolicyProvider`方法是在其函式中建立回溯原則提供者（如果原則名稱不符合預期的 ' MinimumAge ' + age 模式，則會使用此功能）。

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

然後， `GetPolicyAsync`可以將方法更新為`FallbackPolicyProvider`使用，而不是傳回 null：

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>預設原則

除了提供命名的授權原則之外，自訂`IAuthorizationPolicyProvider`需要執行`GetDefaultPolicyAsync`以`[Authorize]`提供屬性的授權原則，而不需指定原則名稱。

在許多情況下，此授權屬性只需要已驗證的使用者，因此您可以透過呼叫來`RequireAuthenticatedUser`建立必要的原則：

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

如同自訂`IAuthorizationPolicyProvider`的所有層面，您可以視需要自訂此項。 在某些情況下，您可能會想要從`IAuthorizationPolicyProvider`回復取得預設原則。

## <a name="required-policy"></a>必要原則

自訂`IAuthorizationPolicyProvider`需要執行`GetRequiredPolicyAsync`的，可選擇性地提供一律需要的原則。 如果`GetRequiredPolicyAsync`傳回非 null 的原則，該原則將會與所要求的任何其他（名稱或預設）原則結合。

如果不需要任何必要的原則，則提供者可以只傳回 null 或延後至回復提供者：

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>使用自訂 IAuthorizationPolicyProvider

若要從`IAuthorizationPolicyProvider`使用自訂原則，您必須：

* 使用相依性`AuthorizationHandler`插入（如以[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers)中所述）來註冊適當的類型，如同所有以原則為基礎的授權案例。
* 在應用程式`IAuthorizationPolicyProvider`的相依性插入服務集合（在中`Startup.ConfigureServices`）中註冊自訂類型，以取代預設原則提供者。

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

您可以在`IAuthorizationPolicyProvider` [aspnet/AuthSamples GitHub 存放庫](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)中取得完整的自訂範例。
