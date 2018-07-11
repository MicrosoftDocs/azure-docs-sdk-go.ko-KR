---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 0febc2cc42ad95a1b7a5032c7987e37cc82f374e
ms.sourcegitcommit: 181d4e0b164cf39b3feac346f559596bd19c94db
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38067036"
---
<span data-ttu-id="039bb-101">[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go)는 Go 버전 1.8 이상과 호환됩니다.</span><span class="sxs-lookup"><span data-stu-id="039bb-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="039bb-102">[Azure Stack 프로필](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles)을 사용하는 환경의 경우 최소 요구 사항은 Go 버전 1.9입니다.</span><span class="sxs-lookup"><span data-stu-id="039bb-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="039bb-103">시스템에서 Go를 사용할 수 없는 경우 [Go 설치 지침](https://golang.org/doc/install)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="039bb-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="039bb-104">Azure SDK for Go 및 해당 종속성은 `go get`을(를) 통해 얻을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="039bb-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="039bb-105">URL에서 `Azure`은(는) 대문자로 표시해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="039bb-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="039bb-106">그렇지 않으면 SDK를 사용할 때 대소문자 관련 가져오기 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="039bb-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="039bb-107">또한 가져오기 구문에서도 `Azure`을(를) 대문자로 표시해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="039bb-107">You also need to capitalize `Azure` in your import statements.</span></span>

