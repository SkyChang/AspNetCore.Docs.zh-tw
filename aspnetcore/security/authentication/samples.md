---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: d49aef198e926d88f1a6727f84b06f0861c8812d
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187294"
---
# <a name="authentication-samples-for-aspnet-core"></a>ASP.NET Core 的驗證範例

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：

* [宣告轉換](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [Cookie 驗證](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [自訂原則提供者-IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [動態驗證配置和選項](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [外部宣告](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [根據要求在 cookie 和另一個驗證配置之間進行選取](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [限制靜態檔案的存取](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>執行範例

* 選取[分支](https://github.com/aspnet/AspNetCore)。 例如：`Tag:v3.0.0`
* 複製或下載[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)。
* 確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://www.microsoft.com/net/download/all)版本。
* 流覽至*AspNetCore/src/Security/samples*中的範例，並使用`dotnet run`來執行範例。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：

* [宣告轉換](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Cookie 驗證](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [自訂原則提供者-IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [動態驗證配置和選項](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [外部宣告](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [根據要求在 cookie 和另一個驗證配置之間進行選取](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [限制靜態檔案的存取](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>執行範例

* 選取[分支](https://github.com/aspnet/AspNetCore)。 例如：`release/2.2`
* 複製或下載[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)。
* 確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://www.microsoft.com/net/download/all)版本。
* 流覽至*AspNetCore/src/Security/samples*中的範例，並使用`dotnet run`來執行範例。

::: moniker-end
