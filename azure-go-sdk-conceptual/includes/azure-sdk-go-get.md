---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/14/2018
ms.locfileid: "55145552"
---
[Go용 Azure SDK](https://github.com/Azure/azure-sdk-for-go)는 Go 버전 1.8 이상과 호환됩니다. [Azure Stack 프로필](/azure/azure-stack/user/azure-stack-version-profiles-go)을 사용하는 환경의 경우 최소 요구 사항은 Go 버전 1.9입니다.
Go를 설치해야 할 경우 [Go 설치 지침](https://golang.org/doc/install)을 따릅니다.

Go용 Azure SDK 및 해당 종속성은 `go get`을 통해 다운로드할 수 있습니다.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> URL에서 `Azure`은(는) 대문자로 표시해야 합니다. 그렇지 않으면 SDK를 사용할 때 대소문자 관련 가져오기 문제가 발생할 수 있습니다. 또한 가져오기 구문에서도 `Azure`을(를) 대문자로 표시해야 합니다.
