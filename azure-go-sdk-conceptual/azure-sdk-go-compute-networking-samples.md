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
ms.openlocfilehash: 4837572a50ae934e71bfe49d916c01e131bb6d83
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319699"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="7be53-103">계산 및 네트워킹을 위한 Azure SDK for Go 샘플</span><span class="sxs-lookup"><span data-stu-id="7be53-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="7be53-104">다음 표는 VM, 가상 네트워크 및 Azure의 서브넷을 관리하기 위해 사용할 수 있도록 선택한 Go 소스 코드로 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="7be53-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="7be53-105">Azure SDK for Go의 모든 샘플은 [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples)에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7be53-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="7be53-106">Name</span><span class="sxs-lookup"><span data-stu-id="7be53-106">Name</span></span> | <span data-ttu-id="7be53-107">설명</span><span class="sxs-lookup"><span data-stu-id="7be53-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="7be53-108">network/network</span><span class="sxs-lookup"><span data-stu-id="7be53-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="7be53-109">가상 네트워크, 서브넷 및 네트워크 보안 그룹을 포함한 네트워크 리소스를 생성, 업데이트, 삭제 및 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="7be53-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="7be53-110">compute/loadbalancer</span><span class="sxs-lookup"><span data-stu-id="7be53-110">compute/loadbalancer</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/loadbalancer.go) | <span data-ttu-id="7be53-111">가용성 그룹을 만들고 쿼리하고 부하 분산 장치로 VM을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7be53-111">Create and query availability groups, and create VMs with a load balancer.</span></span> |
| [<span data-ttu-id="7be53-112">compute/compute</span><span class="sxs-lookup"><span data-stu-id="7be53-112">compute/compute</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/compute.go) | <span data-ttu-id="7be53-113">VM을 생성, 삭제, 업데이트 및 전원 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="7be53-113">Create, delete, update, and power-manage VMs.</span></span> <span data-ttu-id="7be53-114">데이터 디스크로 작업하고 VM의 OS 디스크를 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="7be53-114">Work with data disks and managing the OS disk of the VM.</span></span> |
