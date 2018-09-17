---
title: 계산 및 네트워킹을 위한 Azure SDK for Go 샘플
description: Azure SDK for Go의 VM 및 가상 네트워크와 같은 계산 리소스로 작업하기 위해 선택한 샘플입니다.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: sample
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: d570ad8598ae06633d0010245c207641161ee446
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059087"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="e3751-103">계산 및 네트워킹을 위한 Azure SDK for Go 샘플</span><span class="sxs-lookup"><span data-stu-id="e3751-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="e3751-104">다음 표는 Go용 Azure SDK에서 계산 및 가상 네트워크 리소스를 관리하는 방법을 보여주는 선정된 예제에 대한 링크입니다.</span><span class="sxs-lookup"><span data-stu-id="e3751-104">The following table links to selected samples that demonstrate the management of compute and virtual network resources in the Azure SDK for Go.</span></span>

<span data-ttu-id="e3751-105">Azure SDK for Go의 모든 샘플은 [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3751-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="e3751-106">Name</span><span class="sxs-lookup"><span data-stu-id="e3751-106">Name</span></span> | <span data-ttu-id="e3751-107">설명</span><span class="sxs-lookup"><span data-stu-id="e3751-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="e3751-108">network/network</span><span class="sxs-lookup"><span data-stu-id="e3751-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="e3751-109">가상 네트워크, 서브넷 및 네트워크 보안 그룹을 포함한 네트워크 리소스를 생성, 업데이트, 삭제 및 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="e3751-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="e3751-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="e3751-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="e3751-111">VM을 위해 데이터 디스크를 생성, 첨부, 분리, 업데이트 및 암호화합니다.</span><span class="sxs-lookup"><span data-stu-id="e3751-111">Create, attach, detach, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="e3751-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="e3751-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="e3751-113">VM을 생성, 업데이트, 비활성화 및 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="e3751-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="e3751-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="e3751-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="e3751-115">VM에에 가용성 집합과 부하 분산 장치를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="e3751-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="e3751-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="e3751-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="e3751-117">VM에 MSI(관리 서비스 ID)를 만들고 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="e3751-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |
