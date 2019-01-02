---
title: Azure SDK for Go를 사용한 인증
description: Azure SDK for Go에서 사용할 수 있는 인증 방법 및 그 사용 방법에 대해 알아봅니다.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.component: authentication
ms.openlocfilehash: c2c3dccfa8da5cfe57fee0b90139002068982560
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "49481985"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a><span data-ttu-id="5be9f-103">Azure SDK for Go에서의 인증 방법</span><span class="sxs-lookup"><span data-stu-id="5be9f-103">Authentication methods in the Azure SDK for Go</span></span>

<span data-ttu-id="5be9f-104">Go용 Azure SDK는 Azure를 사용하여 인증하는 여러 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-104">The Azure SDK for Go offers multiple ways to authenticate with Azure.</span></span> <span data-ttu-id="5be9f-105">이러한 인증 _유형_은 다양한 인증 _방법_을 통해 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-105">These authentication _types_ are invoked through different authentication _methods_.</span></span> <span data-ttu-id="5be9f-106">이 문서에서는 사용 가능한 유형, 메서드 및 애플리케이션에 가장 적합한 것을 선택하는 방법을 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-106">This article covers the available types, methods, and how to choose which are best for your application.</span></span>

## <a name="available-authentication-types-and-methods"></a><span data-ttu-id="5be9f-107">사용 가능한 인증 유형 및 방법</span><span class="sxs-lookup"><span data-stu-id="5be9f-107">Available authentication types and methods</span></span>

<span data-ttu-id="5be9f-108">Azure SDK for Go는 서로 다른 자격 증명 집합을 사용하여 여러 가지 유형의 인증을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-108">The Azure SDK for Go offers several different types of authentication, using different credentials sets.</span></span> <span data-ttu-id="5be9f-109">각 인증 유형은 서로 다른 인증 방법을 통해 사용할 수 있으며, 인증 방법은 SDK가 이러한 자격 증명을 입력으로 사용하는 방법을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-109">Each authentication type is available through different authentication methods, which are how the SDK takes these credentials as input.</span></span> <span data-ttu-id="5be9f-110">다음 표에서는 사용할 수 있는 인증 유형 및 애플리케이션에서 해당 인증 유형의 사용이 권장되는 상황을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-110">The following table describes the available types of authentication and situations in which they're recommended for use by your application.</span></span>

| <span data-ttu-id="5be9f-111">인증 유형</span><span class="sxs-lookup"><span data-stu-id="5be9f-111">Authentication type</span></span> | <span data-ttu-id="5be9f-112">권장되는 경우...</span><span class="sxs-lookup"><span data-stu-id="5be9f-112">Recommended when...</span></span> |
|---------------------|---------------------|
| <span data-ttu-id="5be9f-113">인증서 기반 인증</span><span class="sxs-lookup"><span data-stu-id="5be9f-113">Certificate-based authentication</span></span> | <span data-ttu-id="5be9f-114">AAD(Azure Active Directory) 사용자 또는 서비스 주체에 대해 구성된 X509 인증서가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-114">You have an X509 certificate that was configured for an Azure Active Directory (AAD) user or service principal.</span></span> <span data-ttu-id="5be9f-115">자세한 내용은 [Azure Active Directory에서 인증서 기반 인증 시작]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-115">To learn more, see [Get started with certificate-based authentication in Azure Active Directory].</span></span> |
| <span data-ttu-id="5be9f-116">클라이언트 자격 증명</span><span class="sxs-lookup"><span data-stu-id="5be9f-116">Client credentials</span></span> | <span data-ttu-id="5be9f-117">이 애플리케이션에 대해 설정된 구성된 서비스 주체가 있거나 해당 서비스 주체가 속한 애플리케이션 클래스가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-117">You have a configured service principal that is set up for this application or a class of applications it belongs to.</span></span> <span data-ttu-id="5be9f-118">자세한 내용은 [Azure CLI에서 서비스 주체 만들기]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-118">To learn more, see [Create a service principal with Azure CLI].</span></span> |
| <span data-ttu-id="5be9f-119">Azure 리소스에 대한 관리 ID</span><span class="sxs-lookup"><span data-stu-id="5be9f-119">Managed identities for Azure resources</span></span> | <span data-ttu-id="5be9f-120">관리 ID로 구성된 Azure 리소스에서 애플리케이션을 실행하는 중입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-120">Your application is running on an Azure resource that has been configured with a managed identity.</span></span> <span data-ttu-id="5be9f-121">자세히 알아보려면 [Azure 리소스에 대한 관리 ID]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-121">To learn more, see [Managed identities for Azure resources].</span></span> |
| <span data-ttu-id="5be9f-122">장치 토큰</span><span class="sxs-lookup"><span data-stu-id="5be9f-122">Device token</span></span> | <span data-ttu-id="5be9f-123">애플리케이션을 대화형으로__만__ 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-123">Your application is meant to be used interactively __only__.</span></span> <span data-ttu-id="5be9f-124">사용자에게 활성화된 다단계 인증이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-124">Users may have multi-factor authentication enabled.</span></span> <span data-ttu-id="5be9f-125">사용자에게 로그인할 웹 브라우저에 대한 액세스 권한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-125">Users have access to a web browser to sign in.</span></span> <span data-ttu-id="5be9f-126">자세한 내용은 [디바이스 토큰 인증 사용](#use-device-token-authentication)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-126">For more information, see [Use device token authentication](#use-device-token-authentication).</span></span>|
| <span data-ttu-id="5be9f-127">사용자 이름/암호</span><span class="sxs-lookup"><span data-stu-id="5be9f-127">Username/password</span></span> | <span data-ttu-id="5be9f-128">다른 인증 방법을 사용할 수 없는 대화형 애플리케이션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-128">You have an interactive application that can't use any other authentication method.</span></span> <span data-ttu-id="5be9f-129">사용자에게 AAD 로그인에 대해 사용하도록 설정된 다단계 인증이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-129">Your users don't have multi-factor authentication enabled for their AAD sign-in.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="5be9f-130">클라이언트 자격 증명 외의 인증 유형을 사용하는 경우 애플리케이션이 Azure Active Directory에 등록되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-130">If you use an authentication type other than client credentials, your application must be registered in Azure Active Directory.</span></span> <span data-ttu-id="5be9f-131">자세한 내용은 [Azure Active Directory와 애플리케이션 통합](/azure/active-directory/develop/active-directory-integrating-applications)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-131">To learn how, see [Integrating applications with Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).</span></span>
>
> [!NOTE]
> <span data-ttu-id="5be9f-132">특별한 요구 사항이 없으면 사용자 이름/암호 인증을 사용하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="5be9f-132">Unless you have special requirements, avoid username/password authentication.</span></span> <span data-ttu-id="5be9f-133">사용자 기반 로그인이 적절한 경우에는 일반적으로 장치 토큰 인증을 대신 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-133">In situations where user-based sign in is appropriate, device token authentication can usually be used instead.</span></span>

[Azure Active Directory에서 인증서 기반 인증 시작]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Get started with certificate-based authentication in Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Azure CLI에서 서비스 주체 만들기]: /cli/azure/create-an-azure-service-principal-azure-cli
[Create a service principal with Azure CLI]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure 리소스에 대한 관리 ID]: /azure/active-directory/managed-identities-azure-resources/overview
[Managed identities for Azure resources]: /azure/active-directory/managed-identities-azure-resources/overview

<span data-ttu-id="5be9f-137">이러한 인증 유형은 다양한 방법을 통해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-137">These authentication types are available through different methods.</span></span>

* <span data-ttu-id="5be9f-138">[_환경 기반 인증_](#use-environment-based-authentication)은 프로그램의 환경에서 직접 자격 증명을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-138">[_Environment-based authentication_](#use-environment-based-authentication) reads credentials directly from the program's environment.</span></span>
* <span data-ttu-id="5be9f-139">[_파일 기반 인증_](#use-file-based-authentication)은 서비스 주체 자격 증명을 포함하는 파일을 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-139">[_File-based authentication_](#use-file-based-authentication) loads a file containing service principal credentials.</span></span>
* <span data-ttu-id="5be9f-140">[_클라이언트 기반 인증_](#use-an-authentication-client)은 코드의 개체를 사용하며 프로그램 실행 중에 사용자가 자격 증명을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-140">[_Client-based authentication_](#use-an-authentication-client) uses an object in code and makes you responsible for providing the credentials during program execution.</span></span>
* <span data-ttu-id="5be9f-141">[_장치 토큰 인증_](#use-device-token-authentication)은 사용자에게 토큰으로 웹 브라우저를 통해 대화형으로 로그인하도록 요구합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-141">[_Device token authentication_](#use-device-token-authentication) requires users to sign in interactively through a web browser with a token.</span></span>

<span data-ttu-id="5be9f-142">모든 인증 기능 및 유형을 `github.com/Azure/go-autorest/autorest/azure/auth` 패키지에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-142">All authentication functions and types are available in the `github.com/Azure/go-autorest/autorest/azure/auth` package.</span></span>

> [!NOTE]
> <span data-ttu-id="5be9f-143">특별한 요구 사항이 없으면 클라이언트 기반 인증을 사용하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="5be9f-143">Unless you have special requirements, avoid client-based authentication.</span></span> <span data-ttu-id="5be9f-144">이 인증 방법은 잘못된 사례를 조장합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-144">This method of authentication encourages bad practices.</span></span> <span data-ttu-id="5be9f-145">특히, 클라이언트 기반 인증을 사용하면 자격 증명을 하드 코딩하고 싶어지게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-145">In particular, using client-based authentication makes it tempting to hard-code credentials.</span></span> <span data-ttu-id="5be9f-146">또한 인증을 위해 사용자 지정 코드를 작성하는 방식은 인증 요구 사항이 변경되는 경우 향후 SDK에서 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-146">Writing custom code for authentication may also break under future SDK releases if authentication requirements change.</span></span>

## <a name="use-environment-based-authentication"></a><span data-ttu-id="5be9f-147">환경 기반 인증 사용</span><span class="sxs-lookup"><span data-stu-id="5be9f-147">Use environment-based authentication</span></span>

<span data-ttu-id="5be9f-148">제어된 설정에서 애플리케이션을 실행하는 경우, 환경 기반 인증은 자연스러운 선택입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-148">If you're running your application in a controlled setting, environment-based authentication is a natural choice.</span></span> <span data-ttu-id="5be9f-149">이 인증 메서드를 사용하여, 애플리케이션을 실행하기 전에 셸 환경을 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-149">With this authentication method, you configure the shell environment before running your application.</span></span> <span data-ttu-id="5be9f-150">런타임 시 Go SDK는 Azure를 사용하여 인증하기 위해 이러한 환경 변수를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-150">At runtime, the Go SDK reads these environment variables to authenticate with Azure.</span></span>

<span data-ttu-id="5be9f-151">환경 기반 인증은 디바이스 토큰을 제외한 모든 인증 방법에 대해 지원되며, 다음 순서로 평가됩니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-151">Environment-based authentication has support for all authentication methods except device tokens, evaluated in the following order:</span></span>

* <span data-ttu-id="5be9f-152">클라이언트 자격 증명</span><span class="sxs-lookup"><span data-stu-id="5be9f-152">Client credentials</span></span>
* <span data-ttu-id="5be9f-153">X509 인증서</span><span class="sxs-lookup"><span data-stu-id="5be9f-153">X509 certificates</span></span>
* <span data-ttu-id="5be9f-154">사용자 이름/암호</span><span class="sxs-lookup"><span data-stu-id="5be9f-154">Username/password</span></span>
* <span data-ttu-id="5be9f-155">Azure 리소스에 대한 관리 ID</span><span class="sxs-lookup"><span data-stu-id="5be9f-155">Managed identities for Azure resources</span></span>

<span data-ttu-id="5be9f-156">인증 유형에 설정되지 않은 값이 있거나 인증 유형이 거부된 경우 SDK는 자동으로 다음 인증 유형을 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-156">If an authentication type has unset values or is refused, the SDK automatically tries the next authentication type.</span></span> <span data-ttu-id="5be9f-157">시도할 수 있는 유형이 더 이상 없는 경우 SDK는 오류를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-157">When no more types are available to try, the SDK returns an error.</span></span>

<span data-ttu-id="5be9f-158">다음 표에서는 환경 기반 인증에서 지원하는 각 인증 유형에 대해 설정해야 하는 환경 변수를 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-158">The following table details the environment variables that need to be set for each authentication type supported by environment-based authentication.</span></span>


|  <span data-ttu-id="5be9f-159">인증 유형</span><span class="sxs-lookup"><span data-stu-id="5be9f-159">Authentication type</span></span>   |     <span data-ttu-id="5be9f-160">환경 변수</span><span class="sxs-lookup"><span data-stu-id="5be9f-160">Environment variable</span></span>     |                                                                                                     <span data-ttu-id="5be9f-161">설명</span><span class="sxs-lookup"><span data-stu-id="5be9f-161">Description</span></span>                                                                                                      |
|------------------------|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5be9f-162">**클라이언트 자격 증명**</span><span class="sxs-lookup"><span data-stu-id="5be9f-162">**Client credentials**</span></span> |      `AZURE_TENANT_ID`       |                                                                    <span data-ttu-id="5be9f-163">서비스 주체가 속하는 Active Directory 테넌트의 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-163">The ID for the Active Directory tenant that the service principal belongs to.</span></span>                                                                     |
|                        |      `AZURE_CLIENT_ID`       |                                                                                       <span data-ttu-id="5be9f-164">서비스 주체의 이름 또는 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-164">The name or ID of the service principal.</span></span>                                                                                       |
|                        |    `AZURE_CLIENT_SECRET`     |                                                                                  <span data-ttu-id="5be9f-165">서비스 사용자와 연결된 비밀입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-165">The secret associated with the service principal.</span></span>                                                                                   |
|    <span data-ttu-id="5be9f-166">**인증서**</span><span class="sxs-lookup"><span data-stu-id="5be9f-166">**Certificate**</span></span>     |      `AZURE_TENANT_ID`       |                                                                   <span data-ttu-id="5be9f-167">인증서가 등록된 Active Directory 테넌트의 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-167">The ID for the Active Directory tenant that the certificate is registered with.</span></span>                                                                    |
|                        |      `AZURE_CLIENT_ID`       |                                                                              <span data-ttu-id="5be9f-168">인증서와 연결된 애플리케이션 클라이언트 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-168">The application client ID associated with the certificate.</span></span>                                                                              |
|                        |   `AZURE_CERTIFICATE_PATH`   |                                                                                       <span data-ttu-id="5be9f-169">클라이언트 인증서 파일 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-169">The path to the client certificate file.</span></span>                                                                                       |
|                        | `AZURE_CERTIFICATE_PASSWORD` |                                                                                       <span data-ttu-id="5be9f-170">클라이언트 인증서에 대한 암호입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-170">The password for the client certificate.</span></span>                                                                                       |
| <span data-ttu-id="5be9f-171">**사용자 이름/암호**</span><span class="sxs-lookup"><span data-stu-id="5be9f-171">**Username/Password**</span></span>  |      `AZURE_TENANT_ID`       |                                                                           <span data-ttu-id="5be9f-172">사용자가 속하는 Active Directory 테넌트의 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-172">The ID for the Active Directory tenant that the user belongs to.</span></span>                                                                           |
|                        |      `AZURE_CLIENT_ID`       |                                                                                              <span data-ttu-id="5be9f-173">애플리케이션 클라이언트 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-173">The application client ID.</span></span>                                                                                              |
|                        |       `AZURE_USERNAME`       |                                                                                            <span data-ttu-id="5be9f-174">로그인에 사용하는 사용자 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-174">The username to sign in with.</span></span>                                                                                             |
|                        |       `AZURE_PASSWORD`       |                                                                                            <span data-ttu-id="5be9f-175">로그인에 사용하는 암호입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-175">The password to sign in with.</span></span>                                                                                             |
|  <span data-ttu-id="5be9f-176">**관리 ID**</span><span class="sxs-lookup"><span data-stu-id="5be9f-176">**Managed identity**</span></span>  |                              | <span data-ttu-id="5be9f-177">관리 ID 인증에 자격 증명이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-177">No credentials are needed for managed identity authentication.</span></span> <span data-ttu-id="5be9f-178">관리 ID를 사용하도록 구성된 Azure 리소스에서 애플리케이션을 실행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-178">The application must be running on an Azure resource configured to use managed identities.</span></span> <span data-ttu-id="5be9f-179">자세한 내용은 [Azure 리소스에 대한 관리 ID]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-179">For details, see [Managed identities for Azure resources].</span></span> |

<span data-ttu-id="5be9f-180">또한 기본 Azure 공용 클라우드가 아닌 다른 클라우드 또는 관리 엔드포인트에 연결하려면, 다음과 같은 환경 변수를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-180">To connect to a cloud or management endpoint other than the default Azure public cloud, set the following environment variables.</span></span> <span data-ttu-id="5be9f-181">가장 일반적인 이유는 Azure Stack, 지리적으로 다른 지역의 클라우드 또는 클래식 배포 모델을 사용하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-181">The most common reasons are if you use Azure Stack, a cloud in a different geographic region, or the classic deployment model.</span></span>

| <span data-ttu-id="5be9f-182">환경 변수</span><span class="sxs-lookup"><span data-stu-id="5be9f-182">Environment variable</span></span> | <span data-ttu-id="5be9f-183">설명</span><span class="sxs-lookup"><span data-stu-id="5be9f-183">Description</span></span>  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | <span data-ttu-id="5be9f-184">연결할 클라우드 환경의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-184">The name of the cloud environment to connect to.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="5be9f-185">관리 엔드포인트의 URI로서, 연결에 사용할 Active Directory 리소스 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-185">The Active Directory resource ID to use when connecting, as a URI to your management endpoint.</span></span> |

<span data-ttu-id="5be9f-186">환경 기반 인증을 사용할 때는 [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) 함수를 호출하여 권한 부여자 개체를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-186">When using environment-based authentication, call the [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) function to get your authorizer object.</span></span> <span data-ttu-id="5be9f-187">그 다음, 이 개체는 클라이언트의 `Authorizer` 속성에서 Azure에 액세스할 수 있도록 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-187">This object is then set on the `Authorizer` property of clients to allow them access to Azure.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a><span data-ttu-id="5be9f-188">Azure Stack 인증</span><span class="sxs-lookup"><span data-stu-id="5be9f-188">Authentication on Azure Stack</span></span>

<span data-ttu-id="5be9f-189">Azure Stack을 인증하려면 다음 변수를 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-189">To authenticate on Azure Stack, you need to set the following variables:</span></span>

| <span data-ttu-id="5be9f-190">환경 변수</span><span class="sxs-lookup"><span data-stu-id="5be9f-190">Environment variable</span></span> | <span data-ttu-id="5be9f-191">설명</span><span class="sxs-lookup"><span data-stu-id="5be9f-191">Description</span></span>  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | <span data-ttu-id="5be9f-192">Azure Active Directory 엔드포인트입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-192">The Active Directory endpoint.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="5be9f-193">Active Directory 리소스 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-193">The Active Directory resource ID.</span></span> |

<span data-ttu-id="5be9f-194">Azure Stack 메타 데이터 정보에서 이러한 변수를 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-194">These variables can be retrieved from Azure Stack metadata information.</span></span> <span data-ttu-id="5be9f-195">메타 데이터를 검색하려면 Azure Stack 환경에서 웹 브라우저를 열고 다음 url을 사용 합니다.`(ResourceManagerURL)/metadata/endpoints?api-version=1.0`</span><span class="sxs-lookup"><span data-stu-id="5be9f-195">To retrieve the metadata, open a web browser in your Azure Stack environment and use the url: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`</span></span>

<span data-ttu-id="5be9f-196">`ResourceManagerURL`은 지역 이름, 머신 이름 및 Azure Stack 배포의 외부 정규화된 도메인 이름(FQDN)에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-196">The `ResourceManagerURL` varies based on the region name, machine name, and external fully qualified domain name (FQDN) of your Azure Stack deployment:</span></span>

| <span data-ttu-id="5be9f-197">Environment</span><span class="sxs-lookup"><span data-stu-id="5be9f-197">Environment</span></span> | <span data-ttu-id="5be9f-198">ResourceManagerURL</span><span class="sxs-lookup"><span data-stu-id="5be9f-198">ResourceManagerURL</span></span> |
|----------------------|--------------|
| <span data-ttu-id="5be9f-199">Development Kit</span><span class="sxs-lookup"><span data-stu-id="5be9f-199">Development Kit</span></span> | `https://management.local.azurestack.external/` |
| <span data-ttu-id="5be9f-200">통합 시스템</span><span class="sxs-lookup"><span data-stu-id="5be9f-200">Integrated Systems</span></span> | `https://management.(region).ext-(machine-name).(FQDN)` |

<span data-ttu-id="5be9f-201">Azure Stack에서 Go용 Azure SDK를 사용하는 방법에 대한 자세한 정보는 [Azure Stack에서 Go를 사용한 API 버전 프로필 사용](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-201">For more information on how to use the Azure SDK for Go on Azure Stack, see [Use API version profiles with Go in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)</span></span>

## <a name="use-file-based-authentication"></a><span data-ttu-id="5be9f-202">파일 기반 인증 사용</span><span class="sxs-lookup"><span data-stu-id="5be9f-202">Use file-based authentication</span></span>

<span data-ttu-id="5be9f-203">파일 기반 인증은 [Azure CLI](/cli/azure)로 생성된 파일 형식을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-203">File-based authentication uses a file format generated by [the Azure CLI](/cli/azure).</span></span> <span data-ttu-id="5be9f-204">`--sdk-auth` 매개 변수를 사용하여 새 서비스 주체를 만들 때 이 파일을 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-204">You can easily create this file when creating a new service principal with the `--sdk-auth` parameter.</span></span> <span data-ttu-id="5be9f-205">파일 기반 인증을 사용하려고 계획하는 경우 서비스 주체를 만들 때 이 인수가 제공되도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-205">If you plan on using file-based authentication, make sure that this argument is provided when creating a service principal.</span></span> <span data-ttu-id="5be9f-206">CLI에서는 출력을 `stdout`에 인쇄하므로 출력을 파일로 리디렉션합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-206">Since the CLI prints output to `stdout`, redirect output to a file.</span></span>

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

<span data-ttu-id="5be9f-207">`AZURE_AUTH_LOCATION` 환경 변수를 권한 부여 파일이 있는 위치로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-207">Set the `AZURE_AUTH_LOCATION` environment variable to where the authorization file is located.</span></span> <span data-ttu-id="5be9f-208">애플리케이션에서 이 환경 변수를 읽고 그 안에 포함된 자격 증명을 구문 분석합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-208">This environment variable is read by the application, and the credentials within it are parsed.</span></span> <span data-ttu-id="5be9f-209">런타임 시 권한 부여 파일을 선택해야 하는 경우, [os.Setenv](https://golang.org/pkg/os/#Setenv) 함수를 사용하여 프로그램의 환경을 조작합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-209">If you need to select the authorization file at runtime, manipulate the program's environment using the [os.Setenv](https://golang.org/pkg/os/#Setenv) function.</span></span>

<span data-ttu-id="5be9f-210">인증 정보를 로드하려면 [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) 함수를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-210">To load the authentication information, call the [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) function.</span></span> <span data-ttu-id="5be9f-211">환경 기반 권한 부여와 달리, 파일 기반 권한 부여에는 리소스 엔드포인트가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-211">Unlike environment-based authorization, file-based authorization requires a resource endpoint.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

<span data-ttu-id="5be9f-212">서비스 주체 사용 및 액세스 권한 관리에 대한 자세한 내용은 [Azure CLI에서 서비스 주체 만들기]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-212">For more on using service principals and managing their access permissions, see [Create a service principal with Azure CLI].</span></span>

## <a name="use-device-token-authentication"></a><span data-ttu-id="5be9f-213">장치 토큰 인증 사용</span><span class="sxs-lookup"><span data-stu-id="5be9f-213">Use device token authentication</span></span>

<span data-ttu-id="5be9f-214">사용자가 대화형으로 로그인하도록 하려면 디바이스 토큰 인증을 통하는 것이 가장 좋은 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-214">If you want users to sign in interactively, the best way is through device token authentication.</span></span> <span data-ttu-id="5be9f-215">이 인증 흐름에서 Microsoft 로그인 사이트에 붙여 넣을 토큰을 사용자에게 전달하면, 사용자가 이 로그인 사이트에서 AAD(Azure Active Directory) 계정을 사용하여 인증합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-215">This authentication flow passes the user a token to paste into a Microsoft sign-in site, where they then authenticate with an Azure Active Directory (AAD) account.</span></span> <span data-ttu-id="5be9f-216">표준 사용자 이름/암호 인증과 달리, 이 인증 방법은 다단계 인증을 사용하도록 설정된 계정을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-216">This authentication method supports accounts that have multi-factor authentication enabled, unlike standard username/password authentication.</span></span>

<span data-ttu-id="5be9f-217">디바이스 토큰 인증을 사용하려면 [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) 함수를 사용하여 [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) 권한 부여자를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-217">To use device token authentication, create a [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) authorizer with the [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) function.</span></span> <span data-ttu-id="5be9f-218">인증 프로세스를 시작하려면 결과 개체에서 [권한 부여자](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer)를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-218">Call [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) on the resulting object to start the authentication process.</span></span> <span data-ttu-id="5be9f-219">전체 인증 흐름이 완료될 때까지 장치 흐름 인증에서 프로그램 실행을 차단합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-219">Device flow authentication blocks program execution until the whole authentication flow is complete.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a><span data-ttu-id="5be9f-220">인증 클라이언트 사용</span><span class="sxs-lookup"><span data-stu-id="5be9f-220">Use an authentication client</span></span>

<span data-ttu-id="5be9f-221">특정 인증 유형이 필요하고 프로그램에서 사용자로부터 인증 정보를 로드하는 작업을 수행하도록 하려는 경우, [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) 인터페이스를 준수하는 모든 클라이언트를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-221">If you require a specific type of authentication and are willing to have your program do the work to load authentication information from the user, you can use any client that conforms to the [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) interface.</span></span> <span data-ttu-id="5be9f-222">다음의 경우 이 인터페이스를 구현하는 형식을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-222">Use a type that implements this interface when you:</span></span>

* <span data-ttu-id="5be9f-223">대화형 프로그램 작성</span><span class="sxs-lookup"><span data-stu-id="5be9f-223">Write an interactive program</span></span>
* <span data-ttu-id="5be9f-224">특수한 구성 파일 사용</span><span class="sxs-lookup"><span data-stu-id="5be9f-224">Use specialized configuration files</span></span>
* <span data-ttu-id="5be9f-225">기본 제공 인증 메서드를 사용하는 것을 금지하는 요구 사항이 있는 경우</span><span class="sxs-lookup"><span data-stu-id="5be9f-225">Have a requirement that prevents using a built-in authentication method</span></span>

> [!WARNING]
> <span data-ttu-id="5be9f-226">절대로 Azure 자격 증명을 애플리케이션에 하드 코딩하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="5be9f-226">Never hard-code Azure credentials into an application.</span></span> <span data-ttu-id="5be9f-227">애플리케이션 이진 코드에 비밀 정보를 삽입하면 애플리케이션 실행 여부에 관계없이 공격자가 정보를 쉽게 추출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-227">Putting secrets into an application binary makes it easier for an attacker to extract them, whether the application is running or not.</span></span> <span data-ttu-id="5be9f-228">그러면 해당 자격 증명에 대해 권한이 부여된 모든 Azure 리소스가 위험에 처하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-228">This puts all Azure resources the credentials are authorized for at risk!</span></span>

<span data-ttu-id="5be9f-229">다음 표에는 `AuthorizerConfig` 인터페이스를 따르는 SDK의 형식이 나열되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-229">The following table lists the types in the SDK that conform to the `AuthorizerConfig` interface.</span></span>

| <span data-ttu-id="5be9f-230">인증 유형</span><span class="sxs-lookup"><span data-stu-id="5be9f-230">Authentication type</span></span> | <span data-ttu-id="5be9f-231">권한 부여자 유형</span><span class="sxs-lookup"><span data-stu-id="5be9f-231">Authorizer type</span></span> |
|---------------------|-----------------------|
| <span data-ttu-id="5be9f-232">인증서 기반 인증</span><span class="sxs-lookup"><span data-stu-id="5be9f-232">Certificate-based authentication</span></span> | <span data-ttu-id="5be9f-233">[ClientCertificateConfig]</span><span class="sxs-lookup"><span data-stu-id="5be9f-233">[ClientCertificateConfig]</span></span> |
| <span data-ttu-id="5be9f-234">클라이언트 자격 증명</span><span class="sxs-lookup"><span data-stu-id="5be9f-234">Client credentials</span></span> | <span data-ttu-id="5be9f-235">[ClientCredentialsConfig]</span><span class="sxs-lookup"><span data-stu-id="5be9f-235">[ClientCredentialsConfig]</span></span> |
| <span data-ttu-id="5be9f-236">Azure 리소스에 대한 관리 ID</span><span class="sxs-lookup"><span data-stu-id="5be9f-236">Managed identities for Azure resources</span></span> | <span data-ttu-id="5be9f-237">[MSIConfig]</span><span class="sxs-lookup"><span data-stu-id="5be9f-237">[MSIConfig]</span></span> |
| <span data-ttu-id="5be9f-238">사용자 이름/암호</span><span class="sxs-lookup"><span data-stu-id="5be9f-238">Username/password</span></span> | <span data-ttu-id="5be9f-239">[UsernamePasswordConfig]</span><span class="sxs-lookup"><span data-stu-id="5be9f-239">[UsernamePasswordConfig]</span></span> |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

<span data-ttu-id="5be9f-244">연관된 `New` 함수를 사용하여 인증자를 만든 다음, 결과 개체에서 `Authorize`를 호출하여 인증합니다.</span><span class="sxs-lookup"><span data-stu-id="5be9f-244">Create an authenticator with its associated `New` function, and then call `Authorize` on the resulting object to authenticate.</span></span> <span data-ttu-id="5be9f-245">예를 들어 인증서 기반 인증을 사용하려면:</span><span class="sxs-lookup"><span data-stu-id="5be9f-245">For example, to use certificate-based authentication:</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
