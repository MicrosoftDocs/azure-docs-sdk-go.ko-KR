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
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a>계산 및 네트워킹을 위한 Azure SDK for Go 샘플

다음 표는 Go용 Azure SDK에서 계산 및 가상 네트워크 리소스를 관리하는 방법을 보여주는 선정된 예제에 대한 링크입니다.

Azure SDK for Go의 모든 샘플은 [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples)에서 사용할 수 있습니다.

| Name | 설명 |
|------|-------------|
| [network/network](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | 가상 네트워크, 서브넷 및 네트워크 보안 그룹을 포함한 네트워크 리소스를 생성, 업데이트, 삭제 및 쿼리합니다. |
| [compute/vm_disk](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | VM을 위해 데이터 디스크를 생성, 첨부, 분리, 업데이트 및 암호화합니다. |
| [compute/vm](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | VM을 생성, 업데이트, 비활성화 및 관리합니다. |
| [compute/vm_with_availabilityset](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | VM에에 가용성 집합과 부하 분산 장치를 만듭니다. |
| [compute/vm_with_identity](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | VM에 MSI(관리 서비스 ID)를 만들고 관리합니다. |
