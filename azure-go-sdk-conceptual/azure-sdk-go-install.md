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
ms.openlocfilehash: 7990ec8bde5622078aa822fc7e66ba5c4384d682
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337146"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="4b64b-103">Azure SDK for Go 설치</span><span class="sxs-lookup"><span data-stu-id="4b64b-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="4b64b-104">Azure SDK for Go 시작!</span><span class="sxs-lookup"><span data-stu-id="4b64b-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="4b64b-105">SDK를 사용하면 Go 애플리케이션에서 Azure 서비스를 관리하고 상호 작용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="4b64b-106">Azure SDK for Go 가져오기</span><span class="sxs-lookup"><span data-stu-id="4b64b-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="4b64b-107">일부 Azure 서비스에는 자체적으로 Go SDK가 있고, 코어 Azure SDK Go 패키지에는 포함되어 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="4b64b-108">다음 표에서 고유의 SDK가 포함된 서비스 및 해당 패키지 이름이 리스트되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="4b64b-109">이러한 패키지는 모두 미리 보기에 있는 것으로 간주됩니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="4b64b-110">서비스</span><span class="sxs-lookup"><span data-stu-id="4b64b-110">Service</span></span> | <span data-ttu-id="4b64b-111">패키지</span><span class="sxs-lookup"><span data-stu-id="4b64b-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="4b64b-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="4b64b-112">Blob Storage</span></span> | [<span data-ttu-id="4b64b-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="4b64b-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="4b64b-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="4b64b-114">File Storage</span></span> | [<span data-ttu-id="4b64b-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="4b64b-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="4b64b-116">저장소 큐</span><span class="sxs-lookup"><span data-stu-id="4b64b-116">Storage Queue</span></span> | [<span data-ttu-id="4b64b-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="4b64b-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="4b64b-118">이벤트 허브</span><span class="sxs-lookup"><span data-stu-id="4b64b-118">Event Hub</span></span> | [<span data-ttu-id="4b64b-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="4b64b-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="4b64b-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="4b64b-120">Service Bus</span></span> | [<span data-ttu-id="4b64b-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="4b64b-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="4b64b-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="4b64b-122">Application Insights</span></span> | [<span data-ttu-id="4b64b-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="4b64b-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="4b64b-124">Azure SDK for Go 공급하기</span><span class="sxs-lookup"><span data-stu-id="4b64b-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="4b64b-125">Azure SDK for Go는 [dep](https://github.com/golang/dep)를 통해 공급할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="4b64b-126">안정성을 위해서는 공급 방식이 권장됩니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="4b64b-127">고유한 프로젝트에 `dep`를 사용하려면 `Gopkg.toml`의 `[[constraint]]` 섹션에 `github.com/Azure/azure-sdk-for-go`를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="4b64b-128">예를 들어 버전 `14.0.0`에 공급하려면 다음 항목을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="4b64b-129">프로젝트에 Azure SDK for Go 포함하기</span><span class="sxs-lookup"><span data-stu-id="4b64b-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="4b64b-130">Go 코드에서 Azure 서비스를 사용하려면 상호 작용하려는 모든 서비스 및 필요한 `autorest` 모듈을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="4b64b-131">GoDoc에서 제공되는 전체 모듈 목록에서는 [사용 가능한 서비스](https://godoc.org/github.com/Azure/azure-sdk-for-go) 및 [AutoRest 패키지](https://godoc.org/github.com/Azure/go-autorest)를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="4b64b-132">`go-autorest`에서 가장 일반적으로 필요한 패키지는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="4b64b-133">패키지</span><span class="sxs-lookup"><span data-stu-id="4b64b-133">Package</span></span> | <span data-ttu-id="4b64b-134">설명</span><span class="sxs-lookup"><span data-stu-id="4b64b-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="4b64b-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="4b64b-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="4b64b-136">서비스 클라이언트 인증을 처리하기 위한 개체</span><span class="sxs-lookup"><span data-stu-id="4b64b-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="4b64b-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="4b64b-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="4b64b-138">Azure 서비스와의 상호 작용을 위한 상수</span><span class="sxs-lookup"><span data-stu-id="4b64b-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="4b64b-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="4b64b-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="4b64b-140">Azure 서비스에 액세스하기 위한 인증 메커니즘</span><span class="sxs-lookup"><span data-stu-id="4b64b-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="4b64b-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="4b64b-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="4b64b-142">Azure SDK 데이터 구조를 사용하기 위한 형식 어설션 도우미</span><span class="sxs-lookup"><span data-stu-id="4b64b-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="4b64b-143">Go 패키지 및 Azure 서비스는 독립적으로 버전 관리됩니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="4b64b-144">서비스 버전은 `services` 모듈 아래의 모듈 가져오기 경로의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="4b64b-145">모듈의 전체 경로는 서비스 이름과 `YYYY-MM-DD` 형식의 버전 그리고 다시 서비스 이름으로 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="4b64b-146">예를 들어 Compute 서비스의 `2017-03-30` 버전을 가져오기 위해서는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="4b64b-147">개발을 시작할 때 서비스의 최신 버전을 사용하고 일관되게 유지하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="4b64b-148">Go SDK 업데이트가 없더라도 버전이 다르면 서비스 요구 사항이 변경되어 코드가 손상될 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="4b64b-149">서비스에 대해 총체적 스냅샷이 필요한 경우 단일 프로필 버전을 선택할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="4b64b-150">현재까지 유일하게 잠긴 프로필은 버전 `2017-03-09`이며, 여기에는 최신 서비스 기능이 포함되지 않았을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="4b64b-151">프로필은 `profiles` 모듈 아래에 있으며, 버전이 `YYYY-MM-DD` 형식으로 표시되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="4b64b-152">서비스는 해당 프로필 버전 아래에 그룹화되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="4b64b-153">예를 들어 `2017-03-09` 프로필에서 Azure 리소스 관리 모듈을 가져오려면:</span><span class="sxs-lookup"><span data-stu-id="4b64b-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="4b64b-154">또한 `preview` 및 `latest` 프로필도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="4b64b-155">이것들은 사용하지 않는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-155">Using them is not recommended.</span></span> <span data-ttu-id="4b64b-156">이러한 프로필은 롤링 버전이므로, 서비스 동작이 언제든지 변경될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4b64b-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b64b-157">다음 단계</span><span class="sxs-lookup"><span data-stu-id="4b64b-157">Next steps</span></span>

<span data-ttu-id="4b64b-158">Azure SDK for Go 사용을 시작하려면 빠른 시작을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="4b64b-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="4b64b-159">템플릿에서 가상 머신 배포</span><span class="sxs-lookup"><span data-stu-id="4b64b-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="4b64b-160">Azure Blob SDK for Go를 사용하여 Azure Blob Storage에 개체 전송</span><span class="sxs-lookup"><span data-stu-id="4b64b-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="4b64b-161">Azure Database for PostgreSQL에 연결</span><span class="sxs-lookup"><span data-stu-id="4b64b-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="4b64b-162">Go SDK에서 직접 다른 서비스를 시작하려면, 제공되는 샘플 코드 중 일부를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="4b64b-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="4b64b-163">Azure 서비스를 사용하여 인증</span><span class="sxs-lookup"><span data-stu-id="4b64b-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="4b64b-164">SSH 인증으로 새로운 가상 머신 배포</span><span class="sxs-lookup"><span data-stu-id="4b64b-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="4b64b-165">Azure Container Instances에 컨테이너 이미지 배포</span><span class="sxs-lookup"><span data-stu-id="4b64b-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="4b64b-166">Azure Kubernetes Service에서 클러스터 만들기</span><span class="sxs-lookup"><span data-stu-id="4b64b-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="4b64b-167">Azure Storage 서비스 작업</span><span class="sxs-lookup"><span data-stu-id="4b64b-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="4b64b-168">Azure SDK for Go를 위한 모든 샘플</span><span class="sxs-lookup"><span data-stu-id="4b64b-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
