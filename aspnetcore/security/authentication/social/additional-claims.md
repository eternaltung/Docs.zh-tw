---
title: 保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖
author: guardrex
description: 了解如何建立額外的宣告並從外部提供者的權杖。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6386dd5f0bd901be01cf56081a6b9ca7f408f9f9
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336188"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="9eeff-103">保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖</span><span class="sxs-lookup"><span data-stu-id="9eeff-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="9eeff-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9eeff-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9eeff-105">將 ASP.NET Core 應用程式可以建立額外的宣告和外部驗證提供者，例如 Facebook、 Google、 Microsoft 及 Twitter 的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="9eeff-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="9eeff-106">每個提供者會顯示其平台上，使用者的不同資訊但接收，以及將使用者資料轉換成其他宣告的模式相同。</span><span class="sxs-lookup"><span data-stu-id="9eeff-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="9eeff-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9eeff-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisite"></a><span data-ttu-id="9eeff-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="9eeff-108">Prerequisite</span></span>

<span data-ttu-id="9eeff-109">決定哪些應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="9eeff-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="9eeff-110">每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="9eeff-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="9eeff-111">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="9eeff-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="9eeff-112">[範例應用程式](#sample-app-instructions)會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="9eeff-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="authentication-provider-configuration"></a><span data-ttu-id="9eeff-113">驗證提供者組態</span><span class="sxs-lookup"><span data-stu-id="9eeff-113">Authentication provider configuration</span></span>

### <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="9eeff-114">設定用戶端識別碼和用戶端祕密</span><span class="sxs-lookup"><span data-stu-id="9eeff-114">Set the client ID and client secret</span></span>

<span data-ttu-id="9eeff-115">OAuth 驗證提供者會建立信任關係，使用用戶端識別碼和用戶端祕密應用程式。</span><span class="sxs-lookup"><span data-stu-id="9eeff-115">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="9eeff-116">用戶端識別碼和用戶端密碼值會針對應用程式外部驗證提供者所使用的提供者註冊應用程式時。</span><span class="sxs-lookup"><span data-stu-id="9eeff-116">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="9eeff-117">應用程式使用每個外部提供者必須獨立設定，提供者的用戶端識別碼和用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="9eeff-117">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="9eeff-118">如需詳細資訊，請參閱適用於您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="9eeff-118">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="9eeff-119">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="9eeff-119">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="9eeff-120">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="9eeff-120">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="9eeff-121">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="9eeff-121">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="9eeff-122">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="9eeff-122">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="9eeff-123">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="9eeff-123">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="9eeff-124">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="9eeff-124">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="9eeff-125">範例應用程式設定 Google 驗證提供者用戶端識別碼和由 Google 提供的用戶端祕密：</span><span class="sxs-lookup"><span data-stu-id="9eeff-125">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a><span data-ttu-id="9eeff-126">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="9eeff-126">Establish the authentication scope</span></span>

<span data-ttu-id="9eeff-127">指定從提供者擷取所指定的權限清單<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>。</span><span class="sxs-lookup"><span data-stu-id="9eeff-127">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="9eeff-128">常見的外部提供者的驗證領域會出現在下表中。</span><span class="sxs-lookup"><span data-stu-id="9eeff-128">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="9eeff-129">提供者</span><span class="sxs-lookup"><span data-stu-id="9eeff-129">Provider</span></span>  | <span data-ttu-id="9eeff-130">範圍</span><span class="sxs-lookup"><span data-stu-id="9eeff-130">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="9eeff-131">Facebook</span><span class="sxs-lookup"><span data-stu-id="9eeff-131">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="9eeff-132">Google</span><span class="sxs-lookup"><span data-stu-id="9eeff-132">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="9eeff-133">Microsoft</span><span class="sxs-lookup"><span data-stu-id="9eeff-133">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="9eeff-134">Twitter</span><span class="sxs-lookup"><span data-stu-id="9eeff-134">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="9eeff-135">範例應用程式會新增 Google`plus.login`要求 Google + 登入權限的範圍：</span><span class="sxs-lookup"><span data-stu-id="9eeff-135">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="9eeff-136">將使用者資料的索引鍵對應，並建立宣告</span><span class="sxs-lookup"><span data-stu-id="9eeff-136">Map user data keys and create claims</span></span>

<span data-ttu-id="9eeff-137">在 提供者的選項，指定<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>每個索引鍵的外部提供者的 JSON 使用者資料，來讀取登入的應用程式身分識別。</span><span class="sxs-lookup"><span data-stu-id="9eeff-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="9eeff-138">如需有關宣告類型的詳細資訊，請參閱<xref:System.Security.Claims.ClaimTypes>。</span><span class="sxs-lookup"><span data-stu-id="9eeff-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="9eeff-139">範例應用程式會建立<xref:System.Security.Claims.ClaimTypes.Gender>來自宣告`gender`Google 使用者資料中的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="9eeff-139">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="9eeff-140">在  <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>，則<xref:Microsoft.AspNetCore.Identity.IdentityUser>(`ApplicationUser`) 登入應用程式與<xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>。</span><span class="sxs-lookup"><span data-stu-id="9eeff-140">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="9eeff-141">登入程序期間<xref:Microsoft.AspNetCore.Identity.UserManager`1>可以儲存`ApplicationUser`宣告的使用者資料可從<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>。</span><span class="sxs-lookup"><span data-stu-id="9eeff-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="9eeff-142">範例應用程式中`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*) 建立<xref:System.Security.Claims.ClaimTypes.Gender>宣告為帶正負號的`ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="9eeff-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a><span data-ttu-id="9eeff-143">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="9eeff-143">Save the access token</span></span>

<span data-ttu-id="9eeff-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義存取和重新整理權杖是否應該儲存在<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功授權之後。</span><span class="sxs-lookup"><span data-stu-id="9eeff-144"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="9eeff-145">`SaveTokens` 設定為`false`預設情況下，以減少最終的驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="9eeff-145">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="9eeff-146">範例應用程式設定的值`SaveTokens`要`true`在<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="9eeff-146">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="9eeff-147">當`OnPostConfirmationAsync`執行時，儲存的存取權杖 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) 從外部提供者`ApplicationUser`的`AuthenticationProperties`。</span><span class="sxs-lookup"><span data-stu-id="9eeff-147">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="9eeff-148">範例應用程式會將儲存存取權杖：</span><span class="sxs-lookup"><span data-stu-id="9eeff-148">The sample app saves the access token in:</span></span>

* <span data-ttu-id="9eeff-149">`OnPostConfirmationAsync` &ndash; 執行新的使用者註冊。</span><span class="sxs-lookup"><span data-stu-id="9eeff-149">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="9eeff-150">`OnGetCallbackAsync` &ndash; 當先前已註冊的使用者登入應用程式時執行。</span><span class="sxs-lookup"><span data-stu-id="9eeff-150">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="9eeff-151">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="9eeff-151">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="9eeff-152">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="9eeff-152">How to add additional custom tokens</span></span>

<span data-ttu-id="9eeff-153">若要示範如何新增自訂權杖，它會儲存為一部分`SaveTokens`，範例應用程式會新增<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>與目前<xref:System.DateTime>如[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)的`TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="9eeff-153">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="9eeff-154">範例應用程式的指示</span><span class="sxs-lookup"><span data-stu-id="9eeff-154">Sample app instructions</span></span>

<span data-ttu-id="9eeff-155">範例應用程式示範如何：</span><span class="sxs-lookup"><span data-stu-id="9eeff-155">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="9eeff-156">從 Google 取得使用者的性別和儲存性別宣告的值。</span><span class="sxs-lookup"><span data-stu-id="9eeff-156">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="9eeff-157">將 Google 存取權杖儲存在使用者的`AuthenticationProperties`。</span><span class="sxs-lookup"><span data-stu-id="9eeff-157">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="9eeff-158">若要使用範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="9eeff-158">To use the sample app:</span></span>

1. <span data-ttu-id="9eeff-159">註冊應用程式，並取得有效的用戶端識別碼和 Google 驗證的用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="9eeff-159">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="9eeff-160">如需詳細資訊，請參閱<xref:security/authentication/google-logins>。</span><span class="sxs-lookup"><span data-stu-id="9eeff-160">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="9eeff-161">提供用戶端識別碼和應用程式中的用戶端祕密<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>的`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="9eeff-161">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="9eeff-162">執行應用程式，並要求我宣告頁面。</span><span class="sxs-lookup"><span data-stu-id="9eeff-162">Run the app and request the My Claims page.</span></span> <span data-ttu-id="9eeff-163">當使用者未登入時，應用程式重新導向至 Google。</span><span class="sxs-lookup"><span data-stu-id="9eeff-163">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="9eeff-164">使用 Google 登入。</span><span class="sxs-lookup"><span data-stu-id="9eeff-164">Sign in with Google.</span></span> <span data-ttu-id="9eeff-165">Google 使用者重新導向回到應用程式 (`/Home/MyClaims`)。</span><span class="sxs-lookup"><span data-stu-id="9eeff-165">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="9eeff-166">使用者經過驗證，而且我的宣告頁面載入。</span><span class="sxs-lookup"><span data-stu-id="9eeff-166">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="9eeff-167">性別宣告是底下**使用者宣告**從 Google 取得的值。</span><span class="sxs-lookup"><span data-stu-id="9eeff-167">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="9eeff-168">存取權杖會出現在**驗證屬性**。</span><span class="sxs-lookup"><span data-stu-id="9eeff-168">The access token appears in the **Authentication Properties**.</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```