---
title: 계산 및 네트워킹을 위한 Azure SDK for Go 샘플
description: Azure SDK for Go의 VM 및 가상 네트워크와 같은 계산 리소스로 작업하기 위해 선택한 샘플입니다.
author: sptramer
ms.author: sttramer
ms.date: 03/21/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 1e6f9d848213f0f70ab59d5e0bcc5c52127e97c5
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a>계산 및 네트워킹을 위한 Azure SDK for Go 샘플

다음 표는 VM, 가상 네트워크 및 Azure의 서브넷을 관리하기 위해 사용할 수 있도록 선택한 Go 소스 코드로 연결됩니다. 

Azure SDK for Go의 모든 샘플은 [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples)에서 사용할 수 있습니다.

| 이름 | 설명 |
|------|-------------|
| [network/network](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | 가상 네트워크, 서브넷 및 네트워크 보안 그룹을 포함한 네트워크 리소스를 생성, 업데이트, 삭제 및 쿼리합니다. |
| [compute/loadbalancer](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/loadbalancer.go) | 가용성 그룹을 만들고 쿼리하고 부하 분산 장치로 VM을 만듭니다. |
| [compute/compute](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/compute.go) | VM을 생성, 삭제, 업데이트 및 전원 관리합니다. 데이터 디스크로 작업하고 VM의 OS 디스크를 관리합니다. |
