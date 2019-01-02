---
title: Azure SDK for Go 사용 개발자를 위한 도구
description: Azure SDK for Go 및 Azure 서비스 작업을 위한 도구
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059206"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="fa11f-103">Azure SDK for Go 사용 개발자를 위한 도구</span><span class="sxs-lookup"><span data-stu-id="fa11f-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="fa11f-104">Go 코드를 효율적으로 작성하고 Azure 서비스와 원활하게 작동하기 위해 몇 가지 권장 도구가 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="fa11f-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fa11f-105">Azure CLI</span></span>

<span data-ttu-id="fa11f-106">Azure CLI는 구독에 Azure 리소스를 만들고 구성할 수 있는 명령줄 인터페이스를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="fa11f-107">CLI는 공통 및 공유 Azure 리소스 구축을 빠르게 시작할 수 있도록 도와줌으로써 더 복잡한 서비스 사용에 집중할 수 있게 해줍니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="fa11f-108">CLI에는 쿼리 및 필터링 기능이 포함되므로, 원하는 명령줄 도구로 출력을 직접 파이프할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="fa11f-109">CLI는 Docker 이미지로 또는 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)을 통해 로컬 시스템에 설치할 수 있도록 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fa11f-110">Azure CLI 설치</span><span class="sxs-lookup"><span data-stu-id="fa11f-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="fa11f-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fa11f-111">Visual Studio Code</span></span>

<span data-ttu-id="fa11f-112">Visual Studio Code는 Go 지원을 제공하는 경량 편집기입니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="fa11f-113">이러한 확장에는 자동 완성, `impl` 템플릿, 리팩터링 및 디버깅과 같은 기능이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="fa11f-114">또한 Visual Studio Code는 소스 제어에 대한 편집기 내 액세스 및 Azure 서비스 작업을 위한 확장을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="fa11f-115">Visual Studio Code 설치</span><span class="sxs-lookup"><span data-stu-id="fa11f-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="fa11f-116">Visual Studio Code Go 확장 얻기</span><span class="sxs-lookup"><span data-stu-id="fa11f-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="fa11f-117">Visual Studio Code Azure 도구 확장 얻기</span><span class="sxs-lookup"><span data-stu-id="fa11f-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="fa11f-118">Azure DevOps 프로젝트를 사용하는 CI/CD</span><span class="sxs-lookup"><span data-stu-id="fa11f-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="fa11f-119">Azure DevOps Project 파이프라인을 사용하여 Go 애플리케이션에 대한 지속적인 통합 시스템을 구축할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="fa11f-120">git 리포지토리만 있으면, Azure에서 직접 배포 및 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fa11f-121">Azure DevOps 프로젝트를 사용하여 CI/CD 파이프라인을 만드는 방법 알아보기</span><span class="sxs-lookup"><span data-stu-id="fa11f-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="fa11f-122">dep로 종속성 관리</span><span class="sxs-lookup"><span data-stu-id="fa11f-122">Dependency management with dep</span></span>

<span data-ttu-id="fa11f-123">Go용 Azure SDK는 종속성 관리를 위한 dep를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="fa11f-124">dep 명령을 사용하면 버전 충돌을 피하고 프로젝트가 올바르게 작동하는지 확인하여 Go 애플리케이션에 대한 벤더 요구 사항을 끌어올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fa11f-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fa11f-125">dep 종속성 관리자 얻기</span><span class="sxs-lookup"><span data-stu-id="fa11f-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
