---
title: 계산 및 네트워킹을 위한 Azure SDK for Go 샘플
description: Azure SDK for Go의 VM 및 가상 네트워크와 같은 계산 리소스로 작업하기 위해 선택한 샘플입니다.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/21/2018
ms.topic: sample
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 3b31716ee42c638bab4a6dd99b9eb0d7c07e51a4
ms.sourcegitcommit: 0f581979216f7c9d4913681a6d9f6fe09af26e43
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39475792"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="bc04c-103">계산 및 네트워킹을 위한 Azure SDK for Go 샘플</span><span class="sxs-lookup"><span data-stu-id="bc04c-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="bc04c-104">다음 표는 VM, 가상 네트워크 및 Azure의 서브넷을 관리하기 위해 사용할 수 있도록 선택한 Go 소스 코드로 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="bc04c-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="bc04c-105">Azure SDK for Go의 모든 샘플은 [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="bc04c-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="bc04c-106">Name</span><span class="sxs-lookup"><span data-stu-id="bc04c-106">Name</span></span> | <span data-ttu-id="bc04c-107">설명</span><span class="sxs-lookup"><span data-stu-id="bc04c-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="bc04c-108">network/network</span><span class="sxs-lookup"><span data-stu-id="bc04c-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="bc04c-109">가상 네트워크, 서브넷 및 네트워크 보안 그룹을 포함한 네트워크 리소스를 생성, 업데이트, 삭제 및 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="bc04c-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="bc04c-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="bc04c-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="bc04c-111">VM에에 데이터 디스크를 생성, 첨부, 분리 및 암호화합니다.</span><span class="sxs-lookup"><span data-stu-id="bc04c-111">Create, attach, detatch, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="bc04c-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="bc04c-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="bc04c-113">VM을 생성, 업데이트, 비활성화 및 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="bc04c-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="bc04c-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="bc04c-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="bc04c-115">VM에에 가용성 집합과 부하 분산 장치를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="bc04c-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="bc04c-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="bc04c-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="bc04c-117">VM에 MSI(관리 서비스 ID)를 만들고 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="bc04c-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |
