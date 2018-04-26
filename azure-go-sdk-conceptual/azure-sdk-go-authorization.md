---
title: Azure SDK for Go를 사용한 인증
description: Azure SDK for Go에서 사용할 수 있는 인증 방법 및 그 사용 방법에 대해 알아봅니다.
services: azure
author: sptramer
ms.author: sttramer
ms.date: 04/03/2018
ms.topic: article
ms.service: azure
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 39f9dc5a7cdf9ab84cfd9264446bacb31392ca80
ms.sourcegitcommit: 59d2b4c9d8da15fbbd15e36551093219fdaf256e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Azure SDK for Go에서의 인증 방법

Azure SDK for Go에서는 응용 프로그램에서 사용할 수 있는 다양한 인증 유형 및 방법을 제공합니다. 지원되는 인증 방법은 환경 변수에서 정보를 가져오는 방법부터 대화형 웹 기반 인증 방법까지 다양합니다. 이 문서에서는 SDK에서 사용 가능한 인증 유형 및 해당 인증 유형을 사용하기 위한 방법을 소개합니다. 또한 응용 프로그램에 적합한 인증 유형을 선택하기 위한 모범 사례를 설명합니다.

## <a name="available-authentication-types-and-methods"></a>사용 가능한 인증 유형 및 방법

Azure SDK for Go는 서로 다른 자격 증명 집합을 사용하여 여러 가지 유형의 인증을 제공합니다. 각 인증 유형은 서로 다른 인증 방법을 통해 사용할 수 있으며, 인증 방법은 SDK가 이러한 자격 증명을 입력으로 사용하는 방법을 나타냅니다. 다음 표에서는 사용할 수 있는 인증 유형 및 응용 프로그램에서 해당 인증 유형의 사용이 권장되는 상황을 설명합니다.

| 인증 유형 | 권장되는 경우... |
|---------------------|---------------------|
| 인증서 기반 인증 | AAD(Azure Active Directory) 사용자 또는 서비스 주체에 대해 구성된 X509 인증서가 있습니다. 자세한 내용은 [Azure Active Directory에서 인증서 기반 인증 시작]을 참조하세요. |
| 클라이언트 자격 증명 | 이 응용 프로그램에 대해 설정된 구성된 서비스 주체가 있거나 해당 서비스 주체가 속한 응용 프로그램 클래스가 있습니다. 자세한 내용은 [Azure CLI 2.0에서 서비스 주체 만들기]를 참조하세요. |
| MSI(관리 서비스 ID) | 응용 프로그램이 MSI(관리 서비스 ID)로 구성된 Azure 리소스에서 실행됩니다. 자세한 내용은 [Azure 리소스용 MSI(관리 서비스 ID)]를 참조하세요. |
| 장치 토큰 | 응용 프로그램이 대화형으로__만__ 사용되며 잠재적으로 여러 AAD 테넌트의 다양한 사용자가 이 응용 프로그램을 사용하게 됩니다. 사용자에게 로그인할 웹 브라우저에 대한 액세스 권한이 있습니다. 자세한 내용은 [장치 토큰 인증 사용](#use-device-token-authentication)을 참조하세요.|
| 사용자 이름/암호 | 다른 인증 방법을 사용할 수 없는 대화형 응용 프로그램이 있습니다. 사용자에게 AAD 로그인에 대해 사용하도록 설정된 다단계 인증이 없습니다. |

> [!IMPORTANT]
> 클라이언트 자격 증명 외의 인증 유형을 사용하는 경우 응용 프로그램이 Azure Active Directory에 등록되어야 합니다. 자세한 내용은 [Azure Active Directory와 응용 프로그램 통합](/azure/active-directory/develop/active-directory-integrating-applications)을 참조하세요.

> [!NOTE]
> 특별한 요구 사항이 없으면 사용자 이름/암호 인증을 사용하지 마십시오. 사용자 기반 로그인이 적절한 경우에는 일반적으로 장치 토큰 인증을 대신 사용할 수 있습니다.

[Azure Active Directory에서 인증서 기반 인증 시작]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Azure CLI 2.0에서 서비스 주체 만들기]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure 리소스용 MSI(관리 서비스 ID)]: /azure/active-directory/managed-service-identity/overview

이러한 인증 유형은 다양한 방법을 통해 사용할 수 있습니다. [_환경 기반 인증_](#use-environment-based-authentication)은 프로그램의 환경에서 직접 자격 증명을 읽습니다. [_파일 기반 인증_](#use-file-based-authentication)은 서비스 주체 자격 증명을 포함하는 파일을 로드합니다. [_클라이언트 기반 인증_](#use-an-authentication-client)은 Go 코드의 개체를 사용하며 프로그램 실행 중에 사용자가 자격 증명을 제공해야 합니다. 마지막으로, [_장치 토큰 인증_](#use-device-token-authentication)은 사용자가 토큰을 사용하여 웹 브라우저를 통해 대화형으로 로그인해야 하며 환경 또는 파일 기반 인증과 함께 사용할 수는 없습니다.

모든 인증 기능 및 유형을 `github.com/Azure/go-autorest/autorest/azure/auth` 패키지에서 사용할 수 있습니다.

> [!NOTE]
> 특별한 요구 사항이 없으면 클라이언트 기반 인증을 사용하지 마십시오. 이 인증 방법은 잘못된 사례를 조장합니다. 특히, 클라이언트 기반 인증을 사용하면 자격 증명을 하드 코딩하고 싶어지게 됩니다. 또한 인증을 위해 사용자 지정 코드를 작성하는 방식은 인증 요구 사항이 변경되는 경우 향후 SDK에서 중단될 수 있습니다.

## <a name="use-environment-based-authentication"></a>환경 기반 인증 사용

컨테이너와 같이 밀접하게 제어된 환경에서 응용 프로그램을 실행하는 경우, 환경 기반 인증은 자연스러운 선택입니다. 응용 프로그램을 실행하기 전에 사용자가 셸 환경을 구성하면 Go SDK에서 런타임 시 이러한 환경 변수를 읽어 Azure에서 인증합니다. 

환경 기반 인증은 장치 토큰을 제외한 모든 인증 방법에 대해 지원되며 클라이언트 자격 증명, 인증서, 사용자 이름/암호, MSI(관리 서비스 ID)의 순서로 평가됩니다. 필요한 환경 변수가 설정 해제되었거나 SDK가 인증 서비스에서 거절되는 경우, 그 다음 인증 유형을 시도합니다. SDK가 해당 환경에서 인증할 수 없는 경우에는 오류를 반환합니다.

다음 표에서는 환경 기반 인증에서 지원하는 각 인증 유형에 대해 설정해야 하는 환경 변수를 자세히 설명합니다.

| 인증 유형 | 환경 변수 | 설명 |
| ------------------- | -------------------- | ----------- |
| __클라이언트 자격 증명__ | `AZURE_TENANT_ID` | 서비스 주체가 속하는 Active Directory 테넌트의 ID입니다. |
| | `AZURE_CLIENT_ID` | 서비스 주체의 이름 또는 ID입니다. |
| | `AZURE_CLIENT_SECRET` | 서비스 사용자와 연결된 비밀입니다. |
| __인증서__ | `AZURE_TENANT_ID` | 인증서가 등록된 Active Directory 테넌트의 ID입니다. |
| | `AZURE_CLIENT_ID` | 인증서와 연결된 응용 프로그램 클라이언트 ID입니다. |
| | `AZURE_CERTIFICATE_PATH` | 클라이언트 인증서 파일 경로입니다. |
| | `AZURE_CERTIFICATE_PASSWORD` | 클라이언트 인증서에 대한 암호입니다. |
| __사용자 이름/암호__ | `AZURE_TENANT_ID` | 사용자가 속하는 Active Directory 테넌트의 ID입니다. |
| | `AZURE_CLIENT_ID` | 응용 프로그램 클라이언트 ID입니다. |
| | `AZURE_USERNAME` | 로그인에 사용하는 사용자 이름입니다. |
| | `AZURE_PASSWORD` | 로그인에 사용하는 암호입니다. |
| __MSI__ | | MSI에서는 자격 증명을 설정할 필요가 없습니다. 응용 프로그램은 MSI를 사용하도록 구성된 Azure 리소스에서 실행되어야 합니다. 자세한 내용은 [Azure 리소스용 MSI(관리 서비스 ID)]를 참조하세요. |

또한 기본 Azure 공용 클라우드가 아닌 다른 클라우드 또는 관리 끝점에 연결해야 할 경우, 다음과 같은 환경 변수를 설정할 수 있습니다. 환경 변수를 설정하는 가장 일반적인 이유는 Azure Stack, 지리적으로 다른 지역의 클라우드 또는 Azure 클래식 배포 모델을 사용하는 경우입니다.

| 환경 변수 | 설명  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | 연결할 클라우드 환경의 이름입니다. |
| `AZURE_AD_RESOURCE` | 연결할 때 사용할 Active Directory 리소스 ID입니다. 이 값은 관리 끝점을 가리키는 URI여야 합니다. |

환경 기반 인증을 사용할 때는 [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) 함수를 호출하여 권한 부여자 개체를 가져옵니다. 그 다음, 이 개체는 클라이언트의 `Authorizer` 속성에서 Azure에 액세스할 수 있도록 설정됩니다.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

## <a name="use-file-based-authentication"></a>파일 기반 인증 사용

파일 기반 인증은 클라이언트 자격 증명이 [Azure CLI 2.0](/cli/azure)에서 생성된 로컬 파일 형식으로 저장된 경우 해당 자격 증명으로만 작동됩니다. `--sdk-auth` 매개 변수를 사용하여 새 서비스 주체를 만들 때 이 파일을 쉽게 만들 수 있습니다. 파일 기반 인증을 사용하려고 계획하는 경우 서비스 주체를 만들 때 이 인수가 제공되도록 해야 합니다. CLI에서는 출력을 `stdout`에 인쇄하므로 출력을 파일로 리디렉션합니다.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

`AZURE_AUTH_LOCATION` 환경 변수를 권한 부여 파일이 있는 위치로 설정합니다. 응용 프로그램에서 이 환경 변수를 읽고 그 안에 포함된 자격 증명을 구문 분석합니다. 런타임 시 권한 부여 파일을 선택해야 하는 경우, [os.Setenv](https://golang.org/pkg/os/#Setenv) 함수를 사용하여 프로그램의 환경을 조작합니다.

인증 정보를 로드하려면 [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) 함수를 호출합니다. 환경 기반 권한 부여와 달리, 파일 기반 권한 부여에는 리소스 끝점이 필요합니다.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

서비스 주체 사용 및 액세스 권한 관리에 대한 자세한 내용은 [Azure CLI 2.0에서 서비스 주체 만들기]를 참조하세요.

## <a name="use-device-token-authentication"></a>장치 토큰 인증 사용

사용자가 대화형으로 로그인하도록 하려면 장치 토큰 인증을 통해 해당 기능을 제공하는 것이 가장 좋은 방법입니다. 이 인증 흐름에서 Microsoft 로그인 사이트에 붙여 넣을 토큰을 사용자에게 전달하면, 사용자가 이 로그인 사이트에서 AAD(Azure Active Directory) 계정을 사용하여 로그인합니다. 표준 사용자 이름/암호 인증과 달리, 이 인증 방법은 다단계 인증을 사용하도록 설정된 계정을 지원합니다.

장치 토큰 인증을 사용하려면 [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) 함수를 사용하여 [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) 권한 부여자를 만듭니다. 인증 프로세스를 시작하려면 결과 개체에서 [권한 부여자](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer)를 호출합니다. 전체 인증 흐름이 완료될 때까지 장치 흐름 인증에서 프로그램 실행을 차단합니다.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>인증 클라이언트 사용

특정 인증 유형이 필요하고 프로그램에서 사용자로부터 인증 정보를 로드하는 작업을 수행하도록 하려는 경우, [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) 인터페이스를 준수하는 모든 클라이언트를 사용할 수 있습니다. 대화형 프로그램을 원하거나, 특수한 구성 파일을 사용하거나, 다른 인증 방법을 사용하지 않도록 차단해야 하는 요구 사항이 있는 경우, 이 인터페이스를 구현하는 유형을 사용하세요.

> [!WARNING]
> 절대로 Azure 자격 증명을 응용 프로그램에 하드 코딩하지 마세요. 응용 프로그램 이진 코드에 비밀 정보를 삽입하면 응용 프로그램 실행 여부에 관계없이 공격자가 정보를 쉽게 추출할 수 있습니다. 그러면 해당 자격 증명에 대해 권한이 부여된 모든 Azure 리소스가 위험에 처하게 됩니다.

다음 표에는 `AuthorizerConfig` 인터페이스를 따르는 SDK의 형식이 나열되어 있습니다.

| 인증 유형 | 권한 부여자 유형 |
|---------------------|-----------------------|
| 인증서 기반 인증 | [ClientCertificateConfig] |
| 클라이언트 자격 증명 | [ClientCredentialsConfig] |
| MSI(관리 서비스 ID) | [MSIConfig] |
| 사용자 이름/암호 | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

연관된 `New` 함수를 사용하여 인증자를 만든 다음, 결과 개체에서 `Authorize`를 호출하여 인증을 수행합니다. 예를 들어 인증서 기반 인증을 사용하려면:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
