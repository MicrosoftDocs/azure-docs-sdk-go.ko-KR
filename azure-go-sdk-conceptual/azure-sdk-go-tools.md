---
title: Go 개발자를 위한 도구
description: Azure SDK for Go 및 Azure 서비스 작업을 위한 도구
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039508"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Azure SDK for Go 사용 개발자를 위한 도구

Go 코드를 효율적으로 작성하고 Azure 서비스와 원활하게 작동하기 위해 몇 가지 권장 도구가 제공됩니다.

## <a name="azure-cli"></a>Azure CLI

Azure CLI는 구독에 Azure 리소스를 만들고 구성할 수 있는 명령줄 인터페이스를 제공합니다. CLI는 공통 및 공유 Azure 리소스 구축을 빠르게 시작할 수 있도록 도와줌으로써 더 복잡한 서비스 사용에 집중할 수 있게 해줍니다. CLI에는 쿼리 및 필터링 기능이 포함되므로, 원하는 명령줄 도구로 출력을 직접 파이프할 수 있습니다. CLI는 Docker 이미지로 또는 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)을 통해 로컬 시스템에 설치할 수 있도록 제공됩니다.

> [!div class="nextstepaction"]
> [Azure CLI 설치](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code는 확장을 통해 Go 언어에 대해 포괄적인 지원을 제공하는 경량형 편집기입니다. 이러한 확장에는 자동 완성, `impl` 템플릿, 리팩터링 및 디버깅과 같은 기능에 대한 지원이 포함됩니다. Visual Studio Code는 또한 소스 제어 등의 공통 개발자 도구를 위한 여러 확장은 물론 Azure 서비스와의 직접 상호 작용을 위한 확장도 제공합니다. Microsoft는 Azure CLI에 대한 대화형 인터페이스를 포함하여 이러한 Azure 확장이 포함된 공식 메타 확장을 지원하고 있습니다.

* [Visual Studio Code 설치](https://code.visualstudio.com/Download)
* [Visual Studio Code Go 확장 얻기](https://code.visualstudio.com/docs/languages/go)
* [Azure 도구 확장 얻기](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>Azure DevOps 프로젝트를 사용하는 CI/CD

Azure DevOps 프로젝트 파이프라인을 사용하여 Go 응용 프로그램을 지속적으로 빌드 및 배포를 설정할 수 있습니다. 사용 가능한 git 리포지토리만 있으면 Azure 리소스에서 직접 배포하고 테스트하도록 설정할 수 있습니다. 구성 파이프라인은 쉽게 만들고 관리할 수 있으며 Azure에 직접 프로비저닝되므로 다른 Azure 리소스를 처리하는 것과 같은 방식으로 제어할 수 있습니다.

> [!div class="nextstepaction"]
> [Azure DevOps 프로젝트를 사용하여 CI/CD 파이프라인을 만드는 방법 알아보기](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>dep로 종속성 관리

아직까지는 공식 솔루션이 없기 때문에 패키지 종속성을 관리하고 Go를 통한 공급 수행 방식은 여러 가지가 존재합니다. 이러한 관리를 수행하기 위해 권장되는 방법은 `dep` 종속성 관리자를 사용하는 것입니다. Azure SDK for Go는 dep를 사용하여 공급을 수행하며, dep를 사용하는 다른 프로젝트에 대해 종속성을 올바르게 가져오는 것으로 확인됩니다.

> [!div class="nextstepaction"]
> [dep 종속성 관리자 얻기](https://github.com/golang/dep)
