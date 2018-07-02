---
title: Go 개발자를 위한 도구
description: Azure SDK for Go 및 Azure 서비스 작업을 위한 도구
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 25b46e3a1636c39e261ba11c6f8939d8721cc693
ms.sourcegitcommit: 79d216c6b0442d0f3b3660ff2a34dc8b2049390c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093161"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="4dcd7-103">Azure SDK for Go 사용 개발자를 위한 도구</span><span class="sxs-lookup"><span data-stu-id="4dcd7-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="4dcd7-104">Go 코드를 효율적으로 작성하고 Azure 서비스와 원활하게 작동하기 위해 몇 가지 권장 도구가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="4dcd7-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4dcd7-105">Azure CLI 2.0</span></span>

<span data-ttu-id="4dcd7-106">Azure 2.0 CLI는 구독에 Azure 리소스를 만들고 구성하기 위한 명령줄 인터페이스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="4dcd7-107">CLI는 공통 및 공유 Azure 리소스 구축을 빠르게 시작할 수 있도록 도와줌으로써 더 복잡한 서비스 사용에 집중할 수 있게 해줍니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="4dcd7-108">CLI에는 쿼리 및 필터링 기능이 포함되므로, 원하는 명령줄 도구로 출력을 직접 파이프할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="4dcd7-109">CLI는 Docker 이미지로 또는 [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)을 통해 로컬 시스템에 설치할 수 있도록 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4dcd7-110">Azure CLI 2.0 설치</span><span class="sxs-lookup"><span data-stu-id="4dcd7-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="4dcd7-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4dcd7-111">Visual Studio Code</span></span>

<span data-ttu-id="4dcd7-112">Visual Studio Code는 확장을 통해 Go 언어에 대해 포괄적인 지원을 제공하는 경량형 편집기입니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="4dcd7-113">이러한 확장에는 자동 완성, `impl` 템플릿, 리팩터링 및 디버깅과 같은 기능에 대한 지원이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="4dcd7-114">Visual Studio Code는 또한 소스 제어 등의 공통 개발자 도구를 위한 여러 확장은 물론 Azure 서비스와의 직접 상호 작용을 위한 확장도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="4dcd7-115">Microsoft는 Azure CLI에 대한 대화형 인터페이스를 포함하여 이러한 Azure 확장이 포함된 공식 메타 확장을 지원하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="4dcd7-116">Visual Studio Code 설치</span><span class="sxs-lookup"><span data-stu-id="4dcd7-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="4dcd7-117">Visual Studio Code Go 확장 얻기</span><span class="sxs-lookup"><span data-stu-id="4dcd7-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="4dcd7-118">Azure 도구 확장 얻기</span><span class="sxs-lookup"><span data-stu-id="4dcd7-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="4dcd7-119">dep로 종속성 관리</span><span class="sxs-lookup"><span data-stu-id="4dcd7-119">Dependency management with dep</span></span>

<span data-ttu-id="4dcd7-120">아직까지는 공식 솔루션이 없기 때문에 패키지 종속성을 관리하고 Go를 통한 공급 수행 방식은 여러 가지가 존재합니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="4dcd7-121">이러한 관리를 수행하기 위해 권장되는 방법은 `dep` 종속성 관리자를 사용하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="4dcd7-122">Azure SDK for Go는 dep를 사용하여 공급을 수행하며, dep를 사용하는 다른 프로젝트에 대해 종속성을 올바르게 가져오는 것으로 확인됩니다.</span><span class="sxs-lookup"><span data-stu-id="4dcd7-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4dcd7-123">dep 종속성 관리자 얻기</span><span class="sxs-lookup"><span data-stu-id="4dcd7-123">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
