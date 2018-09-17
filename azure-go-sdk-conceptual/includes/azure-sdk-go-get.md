---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059274"
---
<span data-ttu-id="eabed-101">[Go용 Azure SDK](https://github.com/Azure/azure-sdk-for-go)는 Go 버전 1.8 이상과 호환됩니다.</span><span class="sxs-lookup"><span data-stu-id="eabed-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="eabed-102">[Azure Stack 프로필](/azure/azure-stack/user/azure-stack-version-profiles-go)을 사용하는 환경의 경우 최소 요구 사항은 Go 버전 1.9입니다.</span><span class="sxs-lookup"><span data-stu-id="eabed-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="eabed-103">Go를 설치해야 할 경우 [Go 설치 지침](https://golang.org/doc/install)을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="eabed-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="eabed-104">Go용 Azure SDK 및 해당 종속성은 `go get`을 통해 다운로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eabed-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="eabed-105">URL에서 `Azure`은(는) 대문자로 표시해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eabed-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="eabed-106">그렇지 않으면 SDK를 사용할 때 대소문자 관련 가져오기 문제가 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="eabed-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="eabed-107">또한 가져오기 구문에서도 `Azure`을(를) 대문자로 표시해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="eabed-107">You also need to capitalize `Azure` in your import statements.</span></span>
