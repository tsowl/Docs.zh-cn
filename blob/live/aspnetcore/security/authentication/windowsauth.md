---
title: "在 ASP.NET 核心中配置 Windows 身份验证"
author: ardalis
description: "本文介绍如何在 ASP.NET Core 上使用 IIS Express、 IIS、 HTTP.sys，和 WebListener 中配置 Windows 身份验证。"
keywords: "ASP.NET 核心，Windows 身份验证、 Authorize 属性、 AllowAnonymous 特性"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: e5ceffe5b7f7e3ef4f6158b6b7b7d571a21ee130
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="71144-104">在 ASP.NET Core 应用程序配置 Windows 身份验证</span><span class="sxs-lookup"><span data-stu-id="71144-104">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="71144-105">通过[Steve Smith](https://ardalis.com)和[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="71144-105">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="71144-106">Windows 身份验证可以配置用于与 IIS 托管的 ASP.NET Core 应用[HTTP.sys](xref:fundamentals/servers/httpsys)，或[WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="71144-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="71144-107">什么是 Windows 身份验证？</span><span class="sxs-lookup"><span data-stu-id="71144-107">What is Windows authentication?</span></span>

<span data-ttu-id="71144-108">Windows 身份验证依赖于操作系统的 ASP.NET Core 应用的用户进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="71144-109">在使用 Active Directory 域标识或其他 Windows 帐户来标识用户的企业网络上运行你的服务器时，你可以使用 Windows 身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="71144-110">Windows 身份验证是最适合 intranet 环境中的用户、 客户端应用程序和 web 服务器属于同一 Windows 域。</span><span class="sxs-lookup"><span data-stu-id="71144-110">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="71144-111">[了解有关 Windows 身份验证和为 IIS 安装](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)。</span><span class="sxs-lookup"><span data-stu-id="71144-111">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="71144-112">启用 ASP.NET Core 应用程序中的 Windows 身份验证</span><span class="sxs-lookup"><span data-stu-id="71144-112">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="71144-113">Visual Studio Web 应用程序模板可以配置为支持 Windows 身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="71144-114">使用 Windows 身份验证应用程序模板</span><span class="sxs-lookup"><span data-stu-id="71144-114">Use the Windows authentication app template</span></span>

<span data-ttu-id="71144-115">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="71144-115">In Visual Studio:</span></span>
1. <span data-ttu-id="71144-116">创建新的 ASP.NET Core Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="71144-116">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="71144-117">从模板列表中选择 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="71144-117">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="71144-118">选择**更改身份验证**按钮，然后选择**Windows 身份验证**。</span><span class="sxs-lookup"><span data-stu-id="71144-118">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="71144-119">运行应用。</span><span class="sxs-lookup"><span data-stu-id="71144-119">Run the app.</span></span> <span data-ttu-id="71144-120">用户名已显示在顶部的应用程序的权限。</span><span class="sxs-lookup"><span data-stu-id="71144-120">The username appears in the top right of the app.</span></span>

![Windows 身份验证浏览器屏幕快照](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="71144-122">对于使用 IIS Express 的开发工作，该模板提供所有使用 Windows 身份验证所需的配置。</span><span class="sxs-lookup"><span data-stu-id="71144-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="71144-123">以下部分说明如何手动配置 Windows 身份验证的 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="71144-123">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="71144-124">为 Windows 以及匿名身份验证的 visual Studio 设置</span><span class="sxs-lookup"><span data-stu-id="71144-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="71144-125">Visual Studio 项目**属性**页面的**调试**选项卡为 Windows 身份验证和匿名身份验证提供的复选框。</span><span class="sxs-lookup"><span data-stu-id="71144-125">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows 身份验证浏览器屏幕快照](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="71144-127">或者，在中配置这两个属性*launchSettings.json*文件：</span><span class="sxs-lookup"><span data-stu-id="71144-127">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="71144-128">启用与 IIS 的 Windows 身份验证</span><span class="sxs-lookup"><span data-stu-id="71144-128">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="71144-129">IIS 使用[ASP.NET 核心模块](xref:fundamentals/servers/aspnet-core-module)(ANCM) 来承载 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="71144-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="71144-130">ANCM 流 Windows 身份验证到 IIS 默认情况下。</span><span class="sxs-lookup"><span data-stu-id="71144-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="71144-131">IIS，不是应用程序项目中进行配置的 Windows 身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="71144-132">以下部分说明如何使用 IIS 管理器来配置 ASP.NET Core 应用以使用 Windows 身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="71144-133">创建一个新的 IIS 站点</span><span class="sxs-lookup"><span data-stu-id="71144-133">Create a new IIS site</span></span>

<span data-ttu-id="71144-134">指定名称和文件夹，并允许它创建新的应用程序池。</span><span class="sxs-lookup"><span data-stu-id="71144-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="71144-135">自定义身份验证</span><span class="sxs-lookup"><span data-stu-id="71144-135">Customize authentication</span></span>

<span data-ttu-id="71144-136">打开身份验证菜单上的站点。</span><span class="sxs-lookup"><span data-stu-id="71144-136">Open the Authentication menu for the site.</span></span>

![IIS 身份验证菜单](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="71144-138">禁用匿名身份验证并启用 Windows 身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS 身份验证设置](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="71144-140">将你的项目发布到 IIS 站点文件夹</span><span class="sxs-lookup"><span data-stu-id="71144-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="71144-141">使用 Visual Studio 或.NET 核心 CLI，将应用发布到目标文件夹。</span><span class="sxs-lookup"><span data-stu-id="71144-141">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio 发布对话框](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="71144-143">详细了解[发布到 IIS](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="71144-143">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="71144-144">启动应用程序来验证使用 Windows 身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="71144-145">启用 Windows 身份验证与 HTTP.sys 或 WebListener</span><span class="sxs-lookup"><span data-stu-id="71144-145">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="71144-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="71144-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="71144-147">尽管 Kestrel 不支持 Windows 身份验证，你可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)以在 Windows 上支持自承载的方案。</span><span class="sxs-lookup"><span data-stu-id="71144-147">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="71144-148">下面的示例将配置应用程序的 web 主机 HTTP.sys 用于 Windows 身份验证：</span><span class="sxs-lookup"><span data-stu-id="71144-148">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="71144-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="71144-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="71144-150">尽管 Kestrel 不支持 Windows 身份验证，你可以使用[WebListener](xref:fundamentals/servers/weblistener)以在 Windows 上支持自承载的方案。</span><span class="sxs-lookup"><span data-stu-id="71144-150">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="71144-151">下面的示例将配置应用程序的 web 主机 WebListener 用于 Windows 身份验证：</span><span class="sxs-lookup"><span data-stu-id="71144-151">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="71144-152">使用 Windows 身份验证</span><span class="sxs-lookup"><span data-stu-id="71144-152">Work with Windows authentication</span></span>

<span data-ttu-id="71144-153">匿名访问的配置状态确定的方式`[Authorize]`和`[AllowAnonymous]`特性用于应用程序中。</span><span class="sxs-lookup"><span data-stu-id="71144-153">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="71144-154">以下两节介绍如何处理的匿名访问权限不允许的和允许配置状态。</span><span class="sxs-lookup"><span data-stu-id="71144-154">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="71144-155">不允许匿名访问</span><span class="sxs-lookup"><span data-stu-id="71144-155">Disallow anonymous access</span></span>

<span data-ttu-id="71144-156">当启用了 Windows 身份验证并且禁用了匿名访问，`[Authorize]`和`[AllowAnonymous]`属性产生任何影响。</span><span class="sxs-lookup"><span data-stu-id="71144-156">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="71144-157">如果 IIS 站点 （或 HTTP.sys 或 WebListener 服务器） 配置为不允许匿名访问，请求将永远不会到达你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="71144-157">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="71144-158">为此，`[AllowAnonymous]`属性不适用。</span><span class="sxs-lookup"><span data-stu-id="71144-158">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="71144-159">允许匿名访问</span><span class="sxs-lookup"><span data-stu-id="71144-159">Allow anonymous access</span></span>

<span data-ttu-id="71144-160">当启用了 Windows 身份验证和匿名访问时，使用`[Authorize]`和`[AllowAnonymous]`属性。</span><span class="sxs-lookup"><span data-stu-id="71144-160">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="71144-161">`[Authorize]`属性允许你保护的应用程序的部分，真正需要 Windows 身份验证。</span><span class="sxs-lookup"><span data-stu-id="71144-161">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="71144-162">`[AllowAnonymous]`特性重写`[Authorize]`属性中允许匿名访问的应用程序的使用情况。</span><span class="sxs-lookup"><span data-stu-id="71144-162">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="71144-163">请参阅[简单授权](xref:security/authorization/simple)对于特性用法的详细信息。</span><span class="sxs-lookup"><span data-stu-id="71144-163">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="71144-164">在 ASP.NET Core 2.x，`[Authorize]`属性要求中的其他配置*Startup.cs*质询对于 Windows 身份验证的匿名请求。</span><span class="sxs-lookup"><span data-stu-id="71144-164">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="71144-165">建议的配置略有根据正在使用的 web 服务器而有所不同。</span><span class="sxs-lookup"><span data-stu-id="71144-165">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="71144-166">IIS</span><span class="sxs-lookup"><span data-stu-id="71144-166">IIS</span></span>

<span data-ttu-id="71144-167">如果使用 IIS，添加到以下`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="71144-167">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="71144-168">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="71144-168">HTTP.sys</span></span>

<span data-ttu-id="71144-169">如果使用 HTTP.sys，添加到以下`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="71144-169">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="71144-170">Impersonation</span><span class="sxs-lookup"><span data-stu-id="71144-170">Impersonation</span></span>

<span data-ttu-id="71144-171">ASP.NET 核心未实现模拟。</span><span class="sxs-lookup"><span data-stu-id="71144-171">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="71144-172">使用应用程序池或进程标识的所有请求的应用程序标识使用运行应用。</span><span class="sxs-lookup"><span data-stu-id="71144-172">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="71144-173">如果你需要显式执行代表用户操作，使用`WindowsIdentity.RunImpersonated`。</span><span class="sxs-lookup"><span data-stu-id="71144-173">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="71144-174">在此上下文中运行的单个操作，然后关闭上下文。</span><span class="sxs-lookup"><span data-stu-id="71144-174">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="71144-175">请注意，`RunImpersonated`不支持异步操作，并且不应可用于复杂的方案。</span><span class="sxs-lookup"><span data-stu-id="71144-175">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="71144-176">例如，包装整个请求或中间件链不受支持或建议。</span><span class="sxs-lookup"><span data-stu-id="71144-176">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---