---
title: Go 개발자를 위한 도구
description: Azure SDK for Go 및 Azure 서비스 작업을 위한 도구
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 054965eb1ea4f1a7556e2968dfbe07b2db69d26f
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="bbc2a-103">Azure SDK for Go 사용 개발자를 위한 도구</span><span class="sxs-lookup"><span data-stu-id="bbc2a-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="bbc2a-104">Go 코드를 효율적으로 작성하고 Azure 서비스와 원활하게 작동하기 위해 몇 가지 권장 도구가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="bbc2a-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bbc2a-105">Azure CLI 2.0</span></span>

<span data-ttu-id="bbc2a-106">Azure 2.0 CLI는 구독에 Azure 리소스를 만들고 구성하기 위한 명령줄 인터페이스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="bbc2a-107">CLI는 공통 및 공유 Azure 리소스 구축을 빠르게 시작할 수 있도록 도와줌으로써 더 복잡한 서비스 사용에 집중할 수 있게 해줍니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="bbc2a-108">CLI에는 쿼리 및 필터링 기능이 포함되므로, 원하는 명령줄 도구로 출력을 직접 파이프할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="bbc2a-109">CLI는 Docker 이미지로 또는 [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)을 통해 로컬 시스템에 설치할 수 있도록 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bbc2a-110">Azure CLI 2.0 설치</span><span class="sxs-lookup"><span data-stu-id="bbc2a-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="bbc2a-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bbc2a-111">Visual Studio Code</span></span>

<span data-ttu-id="bbc2a-112">Visual Studio Code는 확장을 통해 Go 언어에 대해 포괄적인 지원을 제공하는 경량형 편집기입니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="bbc2a-113">이러한 확장에는 자동 완성, `impl` 템플릿, 리팩터링 및 디버깅과 같은 기능에 대한 지원이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="bbc2a-114">Visual Studio Code는 또한 소스 제어 등의 공통 개발자 도구를 위한 여러 확장은 물론 Azure 서비스와의 직접 상호 작용을 위한 확장도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="bbc2a-115">Microsoft는 Azure CLI에 대한 대화형 인터페이스를 포함하여 이러한 Azure 확장이 포함된 공식 메타 확장을 지원하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="bbc2a-116">Visual Studio Code 설치</span><span class="sxs-lookup"><span data-stu-id="bbc2a-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="bbc2a-117">Visual Studio Code Go 확장 얻기</span><span class="sxs-lookup"><span data-stu-id="bbc2a-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="bbc2a-118">Azure 도구 확장 얻기</span><span class="sxs-lookup"><span data-stu-id="bbc2a-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="bbc2a-119">dep로 종속성 관리</span><span class="sxs-lookup"><span data-stu-id="bbc2a-119">Dependency management with dep</span></span>

<span data-ttu-id="bbc2a-120">아직까지는 공식 솔루션이 없기 때문에 패키지 종속성을 관리하고 Go를 통한 공급 수행 방식은 여러 가지가 존재합니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="bbc2a-121">이러한 관리를 수행하기 위해 권장되는 방법은 `dep` 종속성 관리자를 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="bbc2a-122">Azure SDK for Go는 dep를 사용하여 공급을 수행하며, dep를 사용하는 다른 프로젝트에 대해 종속성을 올바르게 가져오는 것으로 확인됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bbc2a-123">dep 종속성 관리자 얻기</span><span class="sxs-lookup"><span data-stu-id="bbc2a-123">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="bbc2a-124">Application Insights를 사용하여 원격 분석</span><span class="sxs-lookup"><span data-stu-id="bbc2a-124">Telemetry with Application Insights</span></span>

<span data-ttu-id="bbc2a-125">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/)는 응용 프로그램에서 원격 분석 정보를 쉽게 수집할 수 있게 해주는 분석 제품으로서, Azure 에코시스템, Visual Studio Team Services 및 GitHub와 통합됩니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-125">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="bbc2a-126">여러 응용 프로그램에서 사용할 수 있으며, Microsoft는 Application Insights에 사용할 수 있는 Go SDK를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="bbc2a-126">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bbc2a-127">Go SDK용 Application Insights 얻기</span><span class="sxs-lookup"><span data-stu-id="bbc2a-127">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
