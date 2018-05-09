---
title: Azure SDK for Go 설치
description: Azure SDK for Go 설치, 공급 및 구성 방법.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: ad77bdff881770512a828b19dc7af4821f4a55ad
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="28ab2-103">Azure SDK for Go 설치</span><span class="sxs-lookup"><span data-stu-id="28ab2-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="28ab2-104">Azure SDK for Go 시작!</span><span class="sxs-lookup"><span data-stu-id="28ab2-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="28ab2-105">SDK를 사용하면 Go 응용 프로그램에서 Azure 서비스를 관리하고 상호 작용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="28ab2-106">Azure SDK for Go 가져오기</span><span class="sxs-lookup"><span data-stu-id="28ab2-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="28ab2-107">Azure Storage Blob을 사용하려면 별도의 SDK가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="28ab2-108">Azure SDK for Go 공급하기</span><span class="sxs-lookup"><span data-stu-id="28ab2-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="28ab2-109">Azure SDK for Go는 [dep](https://github.com/golang/dep)를 통해 공급할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="28ab2-110">안정성을 위해서는 공급 방식이 권장됩니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="28ab2-111">`dep` 지원을 사용하려면 `github.com/Azure/azure-sdk-for-go`을(를) `Gopkg.toml`의 `[[constraint]]` 섹션에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="28ab2-112">예를 들어 버전 `14.0.0`에 공급하려면 다음 항목을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="28ab2-113">프로젝트에 Azure SDK for Go 포함하기</span><span class="sxs-lookup"><span data-stu-id="28ab2-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="28ab2-114">Go 코드에서 Azure 서비스를 사용하려면 상호 작용하려는 모든 서비스 및 필요한 `autorest` 모듈을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="28ab2-115">GoDoc에서 제공되는 전체 모듈 목록에서는 [사용 가능한 서비스](https://godoc.org/github.com/Azure/azure-sdk-for-go) 및 [AutoRest 패키지](https://godoc.org/github.com/Azure/go-autorest)를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="28ab2-116">`go-autorest`에서 가장 일반적으로 필요한 패키지는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="28ab2-117">패키지</span><span class="sxs-lookup"><span data-stu-id="28ab2-117">Package</span></span> | <span data-ttu-id="28ab2-118">설명</span><span class="sxs-lookup"><span data-stu-id="28ab2-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="28ab2-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="28ab2-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="28ab2-120">서비스 클라이언트 인증을 처리하기 위한 개체</span><span class="sxs-lookup"><span data-stu-id="28ab2-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="28ab2-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="28ab2-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="28ab2-122">Azure 서비스와의 상호 작용을 위한 상수</span><span class="sxs-lookup"><span data-stu-id="28ab2-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="28ab2-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="28ab2-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="28ab2-124">Azure 서비스에 액세스하기 위한 인증 메커니즘</span><span class="sxs-lookup"><span data-stu-id="28ab2-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="28ab2-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="28ab2-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="28ab2-126">Azure SDK 데이터 구조를 사용하기 위한 형식 어설션 도우미</span><span class="sxs-lookup"><span data-stu-id="28ab2-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="28ab2-127">Azure 서비스를 위한 모듈은 해당 모듈을 위한 SDK API에서 개별적으로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="28ab2-128">이러한 버전은 모듈 가져오기 경로에 속하며 _서비스 버전_ 또는 _프로필_로 제공됩니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="28ab2-129">현재까지는 개발 및 릴리스 용도의 특정 서비스 버전을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="28ab2-130">서비스는 `services` 모듈 아래에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-130">Services are located under the `services` module.</span></span> <span data-ttu-id="28ab2-131">가져오기의 전체 경로는 서비스 이름과 `YYYY-MM-DD` 형식의 버전 그리고 다시 서비스 이름으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="28ab2-132">예를 들어 Compute 서비스의 `2017-03-30` 버전을 포함하기 위해서는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="28ab2-133">현재까지는 특별한 이유가 없는 한 최신 서비스 버전을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="28ab2-134">서비스에 대해 총체적 스냅샷이 필요한 경우 단일 프로필 버전을 선택할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="28ab2-135">현재까지 유일하게 잠긴 프로필은 버전 `2017-03-09`이며, 여기에는 최신 서비스 기능이 포함되지 않았을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-135">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="28ab2-136">프로필은 `profiles` 모듈 아래에 있으며, 버전이 `YYYY-MM-DD` 형식으로 표시되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="28ab2-137">서비스는 해당 프로필 버전 아래에 그룹화되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="28ab2-138">예를 들어 `2017-03-09` 프로필에서 Azure 리소스 관리 모듈을 가져오려면:</span><span class="sxs-lookup"><span data-stu-id="28ab2-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="28ab2-139">또한 `preview` 및 `latest` 프로필도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="28ab2-140">이것들은 사용하지 않는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-140">Using them is not recommended.</span></span> <span data-ttu-id="28ab2-141">이러한 프로필은 롤링 버전이므로, 서비스 동작이 언제든지 변경될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="28ab2-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="28ab2-142">다음 단계</span><span class="sxs-lookup"><span data-stu-id="28ab2-142">Next steps</span></span>

<span data-ttu-id="28ab2-143">Azure SDK for Go 사용을 시작하려면 빠른 시작을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="28ab2-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="28ab2-144">템플릿에서 가상 머신 배포</span><span class="sxs-lookup"><span data-stu-id="28ab2-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="28ab2-145">Azure Blob SDK for Go를 사용하여 Azure Blob Storage에 개체 전송</span><span class="sxs-lookup"><span data-stu-id="28ab2-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="28ab2-146">Azure Database for PostgreSQL에 연결</span><span class="sxs-lookup"><span data-stu-id="28ab2-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="28ab2-147">Go SDK에서 직접 다른 서비스를 시작하려면, 제공되는 샘플 코드 중 일부를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="28ab2-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="28ab2-148">Azure 서비스를 사용하여 인증</span><span class="sxs-lookup"><span data-stu-id="28ab2-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="28ab2-149">SSH 인증으로 새로운 가상 머신 배포</span><span class="sxs-lookup"><span data-stu-id="28ab2-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="28ab2-150">Azure Container Instances에 컨테이너 이미지 배포</span><span class="sxs-lookup"><span data-stu-id="28ab2-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="28ab2-151">Azure Kubernetes Service에서 클러스터 만들기</span><span class="sxs-lookup"><span data-stu-id="28ab2-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="28ab2-152">Azure Storage 서비스 작업</span><span class="sxs-lookup"><span data-stu-id="28ab2-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="28ab2-153">Azure SDK for Go를 위한 모든 샘플</span><span class="sxs-lookup"><span data-stu-id="28ab2-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
