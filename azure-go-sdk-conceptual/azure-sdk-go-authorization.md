---
title: Azure SDK for Go를 사용한 인증
description: Azure SDK for Go에서 사용할 수 있는 인증 방법 및 그 사용 방법에 대해 알아봅니다.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.service: active-directory
ms.component: authentication
ms.openlocfilehash: c7970167070bdf1f3fc75692f3e34268801c65df
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067002"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a><span data-ttu-id="c4958-103">Azure SDK for Go에서의 인증 방법</span><span class="sxs-lookup"><span data-stu-id="c4958-103">Authentication methods in the Azure SDK for Go</span></span>

<span data-ttu-id="c4958-104">Azure SDK for Go에서는 응용 프로그램에서 사용할 수 있는 다양한 인증 유형 및 방법을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-104">The Azure SDK for Go offers a variety of authentication types and methods that your application can use.</span></span> <span data-ttu-id="c4958-105">지원되는 인증 방법은 환경 변수에서 정보를 가져오는 방법부터 대화형 웹 기반 인증 방법까지 다양합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-105">Supported authentication methods range from pulling information from environment variables to interactive web-based authentication.</span></span> <span data-ttu-id="c4958-106">이 문서에서는 SDK에서 사용 가능한 인증 유형 및 해당 인증 유형을 사용하기 위한 방법을 소개합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-106">This article introduces you to the available types of authentication in the SDK, and the methods for using them.</span></span> <span data-ttu-id="c4958-107">또한 응용 프로그램에 적합한 인증 유형을 선택하기 위한 모범 사례를 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-107">You'll also learn best practices for selecting which authentication type is right for your application.</span></span>

## <a name="available-authentication-types-and-methods"></a><span data-ttu-id="c4958-108">사용 가능한 인증 유형 및 방법</span><span class="sxs-lookup"><span data-stu-id="c4958-108">Available authentication types and methods</span></span>

<span data-ttu-id="c4958-109">Azure SDK for Go는 서로 다른 자격 증명 집합을 사용하여 여러 가지 유형의 인증을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-109">The Azure SDK for Go offers several different types of authentication, using different credentials sets.</span></span> <span data-ttu-id="c4958-110">각 인증 유형은 서로 다른 인증 방법을 통해 사용할 수 있으며, 인증 방법은 SDK가 이러한 자격 증명을 입력으로 사용하는 방법을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-110">Each of these authentication types are available through different authentication methods, which are how the SDK takes these credentials as input.</span></span> <span data-ttu-id="c4958-111">다음 표에서는 사용할 수 있는 인증 유형 및 응용 프로그램에서 해당 인증 유형의 사용이 권장되는 상황을 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-111">The following table describes the available types of authentication and situations in which they're recommended for use by your application.</span></span>

| <span data-ttu-id="c4958-112">인증 유형</span><span class="sxs-lookup"><span data-stu-id="c4958-112">Authentication type</span></span> | <span data-ttu-id="c4958-113">권장되는 경우...</span><span class="sxs-lookup"><span data-stu-id="c4958-113">Recommended when...</span></span> |
|---------------------|---------------------|
| <span data-ttu-id="c4958-114">인증서 기반 인증</span><span class="sxs-lookup"><span data-stu-id="c4958-114">Certificate-based authentication</span></span> | <span data-ttu-id="c4958-115">AAD(Azure Active Directory) 사용자 또는 서비스 주체에 대해 구성된 X509 인증서가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-115">You have an X509 certificate that was configured for an Azure Active Directory (AAD) user or service principal.</span></span> <span data-ttu-id="c4958-116">자세한 내용은 [Azure Active Directory에서 인증서 기반 인증 시작]을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-116">To learn more, see [Get started with certificate-based authentication in Azure Active Directory].</span></span> |
| <span data-ttu-id="c4958-117">클라이언트 자격 증명</span><span class="sxs-lookup"><span data-stu-id="c4958-117">Client credentials</span></span> | <span data-ttu-id="c4958-118">이 응용 프로그램에 대해 설정된 구성된 서비스 주체가 있거나 해당 서비스 주체가 속한 응용 프로그램 클래스가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-118">You have a configured service principal that is set up for this application or a class of applications it belongs to.</span></span> <span data-ttu-id="c4958-119">자세한 내용은 [Azure CLI 2.0에서 서비스 주체 만들기]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-119">To learn more, see [Create a service principal with Azure CLI 2.0].</span></span> |
| <span data-ttu-id="c4958-120">MSI(관리 서비스 ID)</span><span class="sxs-lookup"><span data-stu-id="c4958-120">Managed Service Identity (MSI)</span></span> | <span data-ttu-id="c4958-121">응용 프로그램이 MSI(관리 서비스 ID)로 구성된 Azure 리소스에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-121">Your application is running on an Azure resource that has been configured with Managed Service Identity (MSI).</span></span> <span data-ttu-id="c4958-122">자세한 내용은 [Azure 리소스용 MSI(관리 서비스 ID)]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-122">To learn more, see [Managed Service Identity (MSI) for Azure resources].</span></span> |
| <span data-ttu-id="c4958-123">장치 토큰</span><span class="sxs-lookup"><span data-stu-id="c4958-123">Device token</span></span> | <span data-ttu-id="c4958-124">응용 프로그램이 대화형으로__만__ 사용되며 잠재적으로 여러 AAD 테넌트의 다양한 사용자가 이 응용 프로그램을 사용하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-124">Your application is meant to be used interactively __only__ and will have a variety of users, potentially from multiple AAD tenants.</span></span> <span data-ttu-id="c4958-125">사용자에게 로그인할 웹 브라우저에 대한 액세스 권한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-125">Users have access to a web browser to sign in.</span></span> <span data-ttu-id="c4958-126">자세한 내용은 [장치 토큰 인증 사용](#use-device-token-authentication)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-126">For more information, see [Use device token authentication](#use-device-token-authentication).</span></span>|
| <span data-ttu-id="c4958-127">사용자 이름/암호</span><span class="sxs-lookup"><span data-stu-id="c4958-127">Username/password</span></span> | <span data-ttu-id="c4958-128">다른 인증 방법을 사용할 수 없는 대화형 응용 프로그램이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-128">You have an interactive application that cannot use any other authentication method.</span></span> <span data-ttu-id="c4958-129">사용자에게 AAD 로그인에 대해 사용하도록 설정된 다단계 인증이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-129">Your users do not have multi-factor authentication enabled for their AAD sign in.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c4958-130">클라이언트 자격 증명 외의 인증 유형을 사용하는 경우 응용 프로그램이 Azure Active Directory에 등록되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-130">If you use an authentication type other than client credentials, your application must be registered in Azure Active Directory.</span></span> <span data-ttu-id="c4958-131">자세한 내용은 [Azure Active Directory와 응용 프로그램 통합](/azure/active-directory/develop/active-directory-integrating-applications)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-131">To learn how, see [Integrating applications with Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).</span></span>

> [!NOTE]
> <span data-ttu-id="c4958-132">특별한 요구 사항이 없으면 사용자 이름/암호 인증을 사용하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="c4958-132">Unless you have special requirements, avoid username/password authentication.</span></span> <span data-ttu-id="c4958-133">사용자 기반 로그인이 적절한 경우에는 일반적으로 장치 토큰 인증을 대신 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-133">In situations where user-based sign in is appropriate, device token authentication can usually be used instead.</span></span>

[Azure Active Directory에서 인증서 기반 인증 시작]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Get started with certificate-based authentication in Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Azure CLI 2.0에서 서비스 주체 만들기]: /cli/azure/create-an-azure-service-principal-azure-cli
[Create a service principal with Azure CLI 2.0]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure 리소스용 MSI(관리 서비스 ID)]: /azure/active-directory/managed-service-identity/overview
[Managed Service Identity (MSI) for Azure resources]: /azure/active-directory/managed-service-identity/overview

<span data-ttu-id="c4958-137">이러한 인증 유형은 다양한 방법을 통해 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-137">These authentication types are available through different methods.</span></span> <span data-ttu-id="c4958-138">[_환경 기반 인증_](#use-environment-based-authentication)은 프로그램의 환경에서 직접 자격 증명을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-138">[_Environment-based authentication_](#use-environment-based-authentication) reads credentials directly from the program's environment.</span></span> <span data-ttu-id="c4958-139">[_파일 기반 인증_](#use-file-based-authentication)은 서비스 주체 자격 증명을 포함하는 파일을 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-139">[_File-based authentication_](#use-file-based-authentication) loads a file containing service principal credentials.</span></span> <span data-ttu-id="c4958-140">[_클라이언트 기반 인증_](#use-an-authentication-client)은 Go 코드의 개체를 사용하며 프로그램 실행 중에 사용자가 자격 증명을 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-140">[_Client-based authentication_](#use-an-authentication-client) uses an object in Go code and makes you responsible for providing the credentials during program execution.</span></span> <span data-ttu-id="c4958-141">마지막으로, [_장치 토큰 인증_](#use-device-token-authentication)은 사용자가 토큰을 사용하여 웹 브라우저를 통해 대화형으로 로그인해야 하며 환경 또는 파일 기반 인증과 함께 사용할 수는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-141">Finally, [_Device token authentication_](#use-device-token-authentication) requires users to sign in interactively through a web browser with a token, and cannot be used with environment- or file-based authentication.</span></span>

<span data-ttu-id="c4958-142">모든 인증 기능 및 유형을 `github.com/Azure/go-autorest/autorest/azure/auth` 패키지에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-142">All authentication functions and types are available in the `github.com/Azure/go-autorest/autorest/azure/auth` package.</span></span>

> [!NOTE]
> <span data-ttu-id="c4958-143">특별한 요구 사항이 없으면 클라이언트 기반 인증을 사용하지 마십시오.</span><span class="sxs-lookup"><span data-stu-id="c4958-143">Unless you have special requirements, avoid client-based authentication.</span></span> <span data-ttu-id="c4958-144">이 인증 방법은 잘못된 사례를 조장합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-144">This method of authentication encourages bad practices.</span></span> <span data-ttu-id="c4958-145">특히, 클라이언트 기반 인증을 사용하면 자격 증명을 하드 코딩하고 싶어지게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-145">In particular, using client-based authentication makes it tempting to hard-code credentials.</span></span> <span data-ttu-id="c4958-146">또한 인증을 위해 사용자 지정 코드를 작성하는 방식은 인증 요구 사항이 변경되는 경우 향후 SDK에서 중단될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-146">Writing custom code for authentication may also break under future SDK releases if authentication requirements change.</span></span>

## <a name="use-environment-based-authentication"></a><span data-ttu-id="c4958-147">환경 기반 인증 사용</span><span class="sxs-lookup"><span data-stu-id="c4958-147">Use environment-based authentication</span></span>

<span data-ttu-id="c4958-148">컨테이너와 같이 밀접하게 제어된 환경에서 응용 프로그램을 실행하는 경우, 환경 기반 인증은 자연스러운 선택입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-148">If you're running your application in a tightly controlled environment such as in a container, environment-based authentication is a natural choice.</span></span> <span data-ttu-id="c4958-149">응용 프로그램을 실행하기 전에 사용자가 셸 환경을 구성하면 Go SDK에서 런타임 시 이러한 환경 변수를 읽어 Azure에서 인증합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-149">You configure the shell environment before running your application and the Go SDK reads these environment variables at runtime to authenticate with Azure.</span></span> 

<span data-ttu-id="c4958-150">환경 기반 인증은 장치 토큰을 제외한 모든 인증 방법에 대해 지원되며 클라이언트 자격 증명, 인증서, 사용자 이름/암호, MSI(관리 서비스 ID)의 순서로 평가됩니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-150">Environment-based authentication has support for all authentication methods except device tokens, evaluated in the following order: Client credentials, certificates, username/password, and Managed Service Identity (MSI).</span></span> <span data-ttu-id="c4958-151">필요한 환경 변수가 설정 해제되었거나 SDK가 인증 서비스에서 거절되는 경우, 그 다음 인증 유형을 시도합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-151">If a required environment variable is unset or the SDK gets a refusal from the authentication service, the next authentication type is tried.</span></span> <span data-ttu-id="c4958-152">SDK가 해당 환경에서 인증할 수 없는 경우에는 오류를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-152">If the SDK cannot authenticate from the environment, it returns an error.</span></span>

<span data-ttu-id="c4958-153">다음 표에서는 환경 기반 인증에서 지원하는 각 인증 유형에 대해 설정해야 하는 환경 변수를 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-153">The following table details the environment variables that need to be set for each authentication type supported by environment-based authentication.</span></span>

| <span data-ttu-id="c4958-154">인증 유형</span><span class="sxs-lookup"><span data-stu-id="c4958-154">Authentication type</span></span> | <span data-ttu-id="c4958-155">환경 변수</span><span class="sxs-lookup"><span data-stu-id="c4958-155">Environment variable</span></span> | <span data-ttu-id="c4958-156">설명</span><span class="sxs-lookup"><span data-stu-id="c4958-156">Description</span></span> |
| ------------------- | -------------------- | ----------- |
| <span data-ttu-id="c4958-157">__클라이언트 자격 증명__</span><span class="sxs-lookup"><span data-stu-id="c4958-157">__Client credentials__</span></span> | `AZURE_TENANT_ID` | <span data-ttu-id="c4958-158">서비스 주체가 속하는 Active Directory 테넌트의 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-158">The ID for the Active Directory tenant that the service principal belongs to.</span></span> |
| | `AZURE_CLIENT_ID` | <span data-ttu-id="c4958-159">서비스 주체의 이름 또는 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-159">The name or ID of the service principal.</span></span> |
| | `AZURE_CLIENT_SECRET` | <span data-ttu-id="c4958-160">서비스 사용자와 연결된 비밀입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-160">The secret associated with the service principal.</span></span> |
| <span data-ttu-id="c4958-161">__인증서__</span><span class="sxs-lookup"><span data-stu-id="c4958-161">__Certificate__</span></span> | `AZURE_TENANT_ID` | <span data-ttu-id="c4958-162">인증서가 등록된 Active Directory 테넌트의 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-162">The ID for the Active Directory tenant that the certificate is registered with.</span></span> |
| | `AZURE_CLIENT_ID` | <span data-ttu-id="c4958-163">인증서와 연결된 응용 프로그램 클라이언트 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-163">The application client ID associated with the certificate.</span></span> |
| | `AZURE_CERTIFICATE_PATH` | <span data-ttu-id="c4958-164">클라이언트 인증서 파일 경로입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-164">The path to the client certificate file.</span></span> |
| | `AZURE_CERTIFICATE_PASSWORD` | <span data-ttu-id="c4958-165">클라이언트 인증서에 대한 암호입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-165">The password for the client certificate.</span></span> |
| <span data-ttu-id="c4958-166">__사용자 이름/암호__</span><span class="sxs-lookup"><span data-stu-id="c4958-166">__Username/Password__</span></span> | `AZURE_TENANT_ID` | <span data-ttu-id="c4958-167">사용자가 속하는 Active Directory 테넌트의 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-167">The ID for the Active Directory tenant that the user belongs to.</span></span> |
| | `AZURE_CLIENT_ID` | <span data-ttu-id="c4958-168">응용 프로그램 클라이언트 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-168">The application client ID.</span></span> |
| | `AZURE_USERNAME` | <span data-ttu-id="c4958-169">로그인에 사용하는 사용자 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-169">The username to sign in with.</span></span> |
| | `AZURE_PASSWORD` | <span data-ttu-id="c4958-170">로그인에 사용하는 암호입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-170">The password to sign in with.</span></span> |
| <span data-ttu-id="c4958-171">__MSI__</span><span class="sxs-lookup"><span data-stu-id="c4958-171">__MSI__</span></span> | | <span data-ttu-id="c4958-172">MSI에서는 자격 증명을 설정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-172">MSI does not require any credentials to be set.</span></span> <span data-ttu-id="c4958-173">응용 프로그램은 MSI를 사용하도록 구성된 Azure 리소스에서 실행되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-173">The application must be running on an Azure resource configured to use MSI.</span></span> <span data-ttu-id="c4958-174">자세한 내용은 [Azure 리소스용 MSI(관리 서비스 ID)]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-174">For details, see [Managed Service Identity (MSI) for Azure resources].</span></span> |

<span data-ttu-id="c4958-175">또한 기본 Azure 공용 클라우드가 아닌 다른 클라우드 또는 관리 끝점에 연결해야 할 경우, 다음과 같은 환경 변수를 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-175">If you need to connect to a cloud or management endpoint other than the default Azure public cloud, you can also set the following environment variables.</span></span> <span data-ttu-id="c4958-176">환경 변수를 설정하는 가장 일반적인 이유는 Azure Stack, 지리적으로 다른 지역의 클라우드 또는 Azure 클래식 배포 모델을 사용하는 경우입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-176">The most common reasons to set them are if you use Azure Stack, a cloud in a different geographic region, or the Azure Classic deployment model.</span></span>

| <span data-ttu-id="c4958-177">환경 변수</span><span class="sxs-lookup"><span data-stu-id="c4958-177">Environment variable</span></span> | <span data-ttu-id="c4958-178">설명</span><span class="sxs-lookup"><span data-stu-id="c4958-178">Description</span></span>  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | <span data-ttu-id="c4958-179">연결할 클라우드 환경의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-179">The name of the cloud environment to connect to.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="c4958-180">연결할 때 사용할 Active Directory 리소스 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-180">The Active Directory resource ID to use when connecting.</span></span> <span data-ttu-id="c4958-181">이 값은 관리 끝점을 가리키는 URI여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-181">This should be a URI pointing to your management endpoint.</span></span> |

<span data-ttu-id="c4958-182">환경 기반 인증을 사용할 때는 [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) 함수를 호출하여 권한 부여자 개체를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-182">When using environment-based authentication, call the [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) function to get your authorizer object.</span></span> <span data-ttu-id="c4958-183">그 다음, 이 개체는 클라이언트의 `Authorizer` 속성에서 Azure에 액세스할 수 있도록 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-183">This object is then set on the `Authorizer` property of clients to allow them access to Azure.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a><span data-ttu-id="c4958-184">Azure Stack 인증</span><span class="sxs-lookup"><span data-stu-id="c4958-184">Authentication on Azure Stack</span></span>

<span data-ttu-id="c4958-185">Azure Stack을 인증하려면 다음 변수를 설정해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-185">To authenticate on Azure Stack, you need to set the following variables:</span></span>

| <span data-ttu-id="c4958-186">환경 변수</span><span class="sxs-lookup"><span data-stu-id="c4958-186">Environment variable</span></span> | <span data-ttu-id="c4958-187">설명</span><span class="sxs-lookup"><span data-stu-id="c4958-187">Description</span></span>  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | <span data-ttu-id="c4958-188">Azure Active Directory 엔드포인트입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-188">The Active Directory endpoint.</span></span> |
| `AZURE_AD_RESOURCE` | <span data-ttu-id="c4958-189">Active Directory 리소스 ID입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-189">The Active Directory resource ID.</span></span> |

<span data-ttu-id="c4958-190">Azure Stack 메타 데이터 정보에서 이러한 변수를 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-190">These variables can be retrieved from Azure Stack metadata information.</span></span> <span data-ttu-id="c4958-191">메타 데이터를 검색하려면 Azure Stack 환경에서 웹 브라우저를 열고 다음 url을 사용 합니다.`(ResourceManagerURL)/metadata/endpoints?api-version=1.0`</span><span class="sxs-lookup"><span data-stu-id="c4958-191">To retrieve the metadata, open a web browser in your Azure Stack environment and use the url: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`</span></span>

<span data-ttu-id="c4958-192">`ResourceManagerURL`은 지역 이름, 컴퓨터 이름 및 Azure Stack 배포의 외부 정규화된 도메인 이름(FQDN)에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-192">The `ResourceManagerURL` varies based on the region name, machine name and external fully qualified domain name (FQDN) of your Azure Stack deployment:</span></span>

| <span data-ttu-id="c4958-193">Environment</span><span class="sxs-lookup"><span data-stu-id="c4958-193">Environment</span></span> | <span data-ttu-id="c4958-194">ResourceManagerURL</span><span class="sxs-lookup"><span data-stu-id="c4958-194">ResourceManagerURL</span></span> |
|----------------------|--------------|
| <span data-ttu-id="c4958-195">Development Kit</span><span class="sxs-lookup"><span data-stu-id="c4958-195">Development Kit</span></span> | `https://management.local.azurestack.external/` |
| <span data-ttu-id="c4958-196">통합 시스템</span><span class="sxs-lookup"><span data-stu-id="c4958-196">Integrated Systems</span></span> | `https://management.(region).ext-(machine-name).(FQDN)` |

<span data-ttu-id="c4958-197">Azure Stack에서 Azure SDK for Go를 사용하는 방법에 대한 자세한 내용은 [Azure Stack에서 Go를 사용한 API 버전 프로필 사용](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-197">For more details on how to use Azure SDK for Go on Azure Stack see [Use API version profiles with Go in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)</span></span>


## <a name="use-file-based-authentication"></a><span data-ttu-id="c4958-198">파일 기반 인증 사용</span><span class="sxs-lookup"><span data-stu-id="c4958-198">Use file-based authentication</span></span>

<span data-ttu-id="c4958-199">파일 기반 인증은 클라이언트 자격 증명이 [Azure CLI 2.0](/cli/azure)에서 생성된 로컬 파일 형식으로 저장된 경우 해당 자격 증명으로만 작동됩니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-199">File-based authentication only works with client credentials when they are stored in a local file format generated by [the Azure CLI 2.0](/cli/azure).</span></span> <span data-ttu-id="c4958-200">`--sdk-auth` 매개 변수를 사용하여 새 서비스 주체를 만들 때 이 파일을 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-200">You can easily create this file when creating a new service principal with the `--sdk-auth` parameter.</span></span> <span data-ttu-id="c4958-201">파일 기반 인증을 사용하려고 계획하는 경우 서비스 주체를 만들 때 이 인수가 제공되도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-201">If you plan on using file-based authentication, make sure that this argument is provided when creating a service principal.</span></span> <span data-ttu-id="c4958-202">CLI에서는 출력을 `stdout`에 인쇄하므로 출력을 파일로 리디렉션합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-202">Since the CLI prints output to `stdout`, redirect output to a file.</span></span>

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

<span data-ttu-id="c4958-203">`AZURE_AUTH_LOCATION` 환경 변수를 권한 부여 파일이 있는 위치로 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-203">Set the `AZURE_AUTH_LOCATION` environment variable to where the authorization file is located.</span></span> <span data-ttu-id="c4958-204">응용 프로그램에서 이 환경 변수를 읽고 그 안에 포함된 자격 증명을 구문 분석합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-204">This environment variable is read by the application, and the credentials within it are parsed.</span></span> <span data-ttu-id="c4958-205">런타임 시 권한 부여 파일을 선택해야 하는 경우, [os.Setenv](https://golang.org/pkg/os/#Setenv) 함수를 사용하여 프로그램의 환경을 조작합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-205">If you need to select the authorization file at runtime, manipulate the program's environment using the [os.Setenv](https://golang.org/pkg/os/#Setenv) function.</span></span>

<span data-ttu-id="c4958-206">인증 정보를 로드하려면 [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) 함수를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-206">To load the authentication information, call the [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) function.</span></span> <span data-ttu-id="c4958-207">환경 기반 권한 부여와 달리, 파일 기반 권한 부여에는 리소스 끝점이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-207">Unlike environment-based authorization, file-based authorization requires a resource endpoint.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

<span data-ttu-id="c4958-208">서비스 주체 사용 및 액세스 권한 관리에 대한 자세한 내용은 [Azure CLI 2.0에서 서비스 주체 만들기]를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-208">For more on using service principals and managing their access permissions, see [Create a service principal with Azure CLI 2.0].</span></span>

## <a name="use-device-token-authentication"></a><span data-ttu-id="c4958-209">장치 토큰 인증 사용</span><span class="sxs-lookup"><span data-stu-id="c4958-209">Use device token authentication</span></span>

<span data-ttu-id="c4958-210">사용자가 대화형으로 로그인하도록 하려면 장치 토큰 인증을 통해 해당 기능을 제공하는 것이 가장 좋은 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-210">If you want users to sign in interactively, the best way to offer that capability is through device token authentication.</span></span> <span data-ttu-id="c4958-211">이 인증 흐름에서 Microsoft 로그인 사이트에 붙여 넣을 토큰을 사용자에게 전달하면, 사용자가 이 로그인 사이트에서 AAD(Azure Active Directory) 계정을 사용하여 인증합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-211">This authentication flow passes the user a token to paste into a Microsoft sign-in site, where they then authenticate with an Azure Active Directory (AAD) account.</span></span> <span data-ttu-id="c4958-212">표준 사용자 이름/암호 인증과 달리, 이 인증 방법은 다단계 인증을 사용하도록 설정된 계정을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-212">This authentication method supports accounts that have multi-factor authentication enabled, unlike standard username/password authentication.</span></span>

<span data-ttu-id="c4958-213">장치 토큰 인증을 사용하려면 [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) 함수를 사용하여 [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) 권한 부여자를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-213">To use device token authentication, create a [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) authorizer with the [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) function.</span></span> <span data-ttu-id="c4958-214">인증 프로세스를 시작하려면 결과 개체에서 [권한 부여자](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer)를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-214">Call [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) on the resulting object to start the authentication process.</span></span> <span data-ttu-id="c4958-215">전체 인증 흐름이 완료될 때까지 장치 흐름 인증에서 프로그램 실행을 차단합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-215">Device flow authentication blocks program execution until the whole authentication flow is complete.</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a><span data-ttu-id="c4958-216">인증 클라이언트 사용</span><span class="sxs-lookup"><span data-stu-id="c4958-216">Use an authentication client</span></span>

<span data-ttu-id="c4958-217">특정 인증 유형이 필요하고 프로그램에서 사용자로부터 인증 정보를 로드하는 작업을 수행하도록 하려는 경우, [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) 인터페이스를 준수하는 모든 클라이언트를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-217">If you require a specific type of authentication and are willing to have your program do the work to load authentication information from the user, you can use any client that conforms to the [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) interface.</span></span> <span data-ttu-id="c4958-218">대화형 프로그램을 원하거나, 특수한 구성 파일을 사용하거나, 다른 인증 방법을 사용하지 않도록 차단해야 하는 요구 사항이 있는 경우, 이 인터페이스를 구현하는 유형을 사용하세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-218">Use a type that implements this interface when you want an interactive program, use specialized configuration files, or have a requirement that prevents you from using another authentication method.</span></span>

> [!WARNING]
> <span data-ttu-id="c4958-219">절대로 Azure 자격 증명을 응용 프로그램에 하드 코딩하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="c4958-219">Never hard-code Azure credentials into an application.</span></span> <span data-ttu-id="c4958-220">응용 프로그램 이진 코드에 비밀 정보를 삽입하면 응용 프로그램 실행 여부에 관계없이 공격자가 정보를 쉽게 추출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-220">Putting secrets into an application binary makes it easier for an attacker to extract them, whether the application is running or not.</span></span> <span data-ttu-id="c4958-221">그러면 해당 자격 증명에 대해 권한이 부여된 모든 Azure 리소스가 위험에 처하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-221">This puts all Azure resources the credentials are authorized for at risk!</span></span>

<span data-ttu-id="c4958-222">다음 표에는 `AuthorizerConfig` 인터페이스를 따르는 SDK의 형식이 나열되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-222">The following table lists the types in the SDK that conform to the `AuthorizerConfig` interface.</span></span>

| <span data-ttu-id="c4958-223">인증 유형</span><span class="sxs-lookup"><span data-stu-id="c4958-223">Authentication type</span></span> | <span data-ttu-id="c4958-224">권한 부여자 유형</span><span class="sxs-lookup"><span data-stu-id="c4958-224">Authorizer type</span></span> |
|---------------------|-----------------------|
| <span data-ttu-id="c4958-225">인증서 기반 인증</span><span class="sxs-lookup"><span data-stu-id="c4958-225">Certificate-based authentication</span></span> | <span data-ttu-id="c4958-226">[ClientCertificateConfig]</span><span class="sxs-lookup"><span data-stu-id="c4958-226">[ClientCertificateConfig]</span></span> |
| <span data-ttu-id="c4958-227">클라이언트 자격 증명</span><span class="sxs-lookup"><span data-stu-id="c4958-227">Client credentials</span></span> | <span data-ttu-id="c4958-228">[ClientCredentialsConfig]</span><span class="sxs-lookup"><span data-stu-id="c4958-228">[ClientCredentialsConfig]</span></span> |
| <span data-ttu-id="c4958-229">MSI(관리 서비스 ID)</span><span class="sxs-lookup"><span data-stu-id="c4958-229">Managed Service Identity (MSI)</span></span> | <span data-ttu-id="c4958-230">[MSIConfig]</span><span class="sxs-lookup"><span data-stu-id="c4958-230">[MSIConfig]</span></span> |
| <span data-ttu-id="c4958-231">사용자 이름/암호</span><span class="sxs-lookup"><span data-stu-id="c4958-231">Username/password</span></span> | <span data-ttu-id="c4958-232">[UsernamePasswordConfig]</span><span class="sxs-lookup"><span data-stu-id="c4958-232">[UsernamePasswordConfig]</span></span> |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

<span data-ttu-id="c4958-237">연관된 `New` 함수를 사용하여 인증자를 만든 다음, 결과 개체에서 `Authorize`를 호출하여 인증을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="c4958-237">Create an authenticator with its associated `New` function, and then call `Authorize` on the resulting object to perform authentication.</span></span> <span data-ttu-id="c4958-238">예를 들어 인증서 기반 인증을 사용하려면:</span><span class="sxs-lookup"><span data-stu-id="c4958-238">For example, to use certificate-based authentication:</span></span>

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
