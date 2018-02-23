---
title: "Go에서 Azure 가상 머신 배포"
description: "Azure SDK for Go를 사용하여 가상 머신을 배포합니다."
keywords: "azure, 가상 머신, vm, go, golang, azure sdk"
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: e530d944deca40e9e6c29b6c2768e2367822714e
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>빠른 시작: Azure SDK for Go를 사용하여 템플릿에서 Azure 가상 머신 배포

이 빠른 시작에서는 Azure SDK for Go를 사용하여 템플릿에서 리소스를 배포하는 방법에 대해 자세히 다룹니다. 템플릿은 [Azure 리소스 그룹](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) 내에 포함된 모든 리소스에 대한 스냅샷입니다. 과정을 진행하는 동안 유용한 작업을 수행하면서 SDK의 기능 및 규칙에 익숙해질 수 있습니다.

이 빠른 시작을 마치면 가상 머신을 실행하고 사용자 이름 및 암호를 사용해서 로그인하게 됩니다.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Azure CLI의 로컬 설치를 사용할 경우, 이 빠른 시작을 위해서는 CLI 버전 2.0.24 이상이 필요합니다. `az --version`을(를) 실행하여 CLI 설치가 이 요구 사항을 충족하는지 확인하십시오. 설치 또는 업그레이드가 필요한 경우에는 [Azure CLI 2.0 설치](/cli/azure/install-azure-cli)를 참조하십시오.

## <a name="install-the-azure-sdk-for-go"></a>Azure SDK for Go 설치 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>서비스 주체 만들기

응용 프로그램에 비 대화형으로 로그인하려면 서비스 주체가 필요합니다. 서비스 주체는 고유한 사용자 ID를 만드는 RBAC(역할 기반 인증)의 일부입니다. CLI를 사용하여 새로운 서비스 주체를 만들려면 다음 명령을 실행합니다.

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

출력에 `appId`, `password` 및 `tenant` 값이 기록되는지 __확인하십시오__. 이러한 값은 응용 프로그램이 Azure에서 인증을 수행하는 데 사용됩니다.

Azure CLI 2.0을 사용한 서비스 주체 만들기 및 관리에 대한 자세한 내용은 [Azure CLI 2.0을 사용하여 서비스 주체 만들기](/cli/azure/create-an-azure-service-principal-azure-cli)를 참조하십시오.

## <a name="get-the-code"></a>코드 가져오기

빠른 시작 코드 및 `go get`의 모든 종속성을 가져옵니다.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

이 코드는 컴파일할 수 있지만, Azure 계정 및 생성된 서비스 주체에 대한 정보를 제공할 때까지 올바르게 실행되지 않습니다. `main.go`에는 `authInfo` 구조체를 포함하는 `config` 변수가 있습니다. 올바른 인증을 위해서는 이 구조체에서 해당 필드 값을 바꿔야 합니다.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: CLI 명령으로 가져올 수 있는 구독 ID

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: 서비스 주체를 만들 때 기록된 `tenant` 값인 테넌트 ID
* `ServicePrincipalID`: 서비스 주체를 만들 때 기록된 `appId` 값
* `ServicePrincipalSecret`: 서비스 주체를 만들 때 기록된 `password` 값

또한 `vm-quickstart-params.json` 파일의 값을 편집해야 합니다.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: 가상 머신 사용자 계정의 암호입니다. 길이는 6-72자여야 하며 다음 문자 중 3개를 포함해야 합니다.
  * 소문자
  * 대문자
  * 숫자
  * 기호

## <a name="running-the-code"></a>코드 실행

`go run` 명령으로 빠른 시작을 실행합니다.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

배포 중 오류가 발생하면 자세한 설명은 없지만, 문제가 있음을 나타내는 메시지가 표시됩니다. Azure CLI에서 다음 명령을 사용하여 배포 오류에 대한 세부 정보를 가져옵니다.

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

배포가 성공하면 새롭게 생성된 가상 머신에 로그인하기 위한 사용자 이름, IP 주소 및 암호를 제공하는 메시지가 표시됩니다. 이 시스템에 SSH로 로그인하여 시스템이 작동되어 실행 중인지 확인합니다.

## <a name="cleaning-up"></a>정리

CLI를 사용해서 리소스 그룹을 삭제하여 이 빠른 시작 중 생성된 리소스를 정리합니다.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>코드 심화 안내

빠른 시작 코드는 변수 및 여러 작은 기능들의 블록으로 세분화됩니다. 여기에서는 이러한 각 블록에 대해 자세히 설명합니다.

### <a name="variable-assignments-and-structs"></a>변수 할당 및 구조체

빠른 시작은 자체 포함된 방식이므로, 명령줄 옵션 또는 환경 변수 대신 전역 변수를 사용합니다.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

`authInfo` 구조체는 Azure 서비스에 대한 권한 부여를 위해 필요한 모든 정보를 캡슐화하도록 선언됩니다.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

값은 생성된 리소스의 이름을 제공하도록 선언됩니다. 또한 여기에 지정된 위치는 다른 데이터 센터에서의 배포 동작을 확인할 수 있도록 변경할 수 있습니다. 일부 데이터 센터에는 필요한 리소스가 없을 수도 있습니다.

`templateFile` 및 `parametersFile` 상수는 배포에 필요한 파일을 가리킵니다. 보안 주체 토큰은 나중에 다뤄지며, `ctx` 변수는 네트워크에 대한 [Go 컨텍스트](https://blog.golang.org/context)입니다.

### <a name="init-and-authorization"></a>init() 및 권한 부여

코드에 대한 `init()` 메서드는 권한 부여를 설정합니다. 권한 부여는 빠른 시작에 포함된 모든 항목들의 사전 조건이므로, 초기화 중에 준비하는 것이 좋습니다. 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

이 코드는 권한 부여를 위한 두 단계를 완료합니다.

* `TenantID`에 대한 OAuth 구성 정보는 Azure Active Directory와의 상호 작용을 통해 검색됩니다. [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) 개체에는 표준 Azure 구성에 사용되는 끝점이 포함됩니다.
* [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) 함수가 호출됩니다. 이 함수는 서비스 주체 로그인과 함께 OAuth 정보는 물론 사용 중인 Azure 관리 스타일을 가져옵니다. 특별한 요구 사항이 있고 해당 작업을 잘 알고 있지 않은 한, 이 값은 항상 `.ResourceManagerEndpoint`이어야 합니다.

### <a name="flow-of-operations-in-main"></a>main()의 작업 흐름

`main()` 함수는 작업 흐름만 나타내고 오류 검사만 수행하는 간단한 함수입니다.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

코드가 실행하는 단계는 순서대로 다음과 같습니다.

* (`createGroup()`)에 배포할 리소스 그룹 만들기
* 이 그룹(`createDeployment()`) 내에 배포 만들기
* 배포된 가상 머신(`getLogin()`)에 대한 로그인 정보 가져오기 및 표시

### <a name="creating-the-resource-group"></a>리소스 그룹 만들기

`createGroup()` 함수는 리소스 그룹을 만듭니다. 호출 흐름 및 인수를 보면 서비스 상호 작용이 SDK에 구성된 방식을 알 수 있습니다.

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Azure 서비스와의 일반적인 상호 작용 흐름은 다음과 같습니다.

* `service.NewXClient()` 메서드를 사용하여 클라이언트를 만듭니다. 여기서 `X`은(는) 상호 작용하려는 `service`의 리소스 유형입니다. 이 함수는 항상 구독 ID를 사용합니다.
* 클라이언트에 대한 권한 부여 방법을 설정해서 원격 API와 상호 작용할 수 있도록 허용합니다.
* 원격 API에 따라 클라이언트에서 메서드 호출을 수행합니다. 서비스 클라이언트 메서드는 일반적으로 리소스 이름 및 메타데이터 개체를 사용합니다.

여기에서 [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 함수는 형식 변환을 수행하기 위해 사용됩니다. SDK의 메서드에 대한 매개 변수 구조체는 거의 배타적으로 포인터를 사용하므로, 이러한 메서드는 형식 변환을 쉽게 만들기 위해 제공되었습니다. 편리한 변환기의 전체 목록 및 동작을 보려면 [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) 모듈 설명서를 참조하십시오.

`groupsClient.CreateOrUpdate()` 작업은 리소스 그룹을 나타내는 데이터 구조체에 대한 포인터를 반환합니다. 이러한 종류의 직접 반환 값은 동기적으로 수행되어야 하는 단기 실행 작업을 나타냅니다. 다음 섹션에서는 장기 실행 작업의 예 및 이와 상호 작용하는 방법을 배웁니다.

### <a name="performing-the-deployment"></a>배포 수행

해당 리소스를 포함할 그룹을 만든 다음에는 배포를 실행해야 합니다. 이 코드는 여러 논리 부분을 강조하기 위해 더 작은 섹션으로 구분됩니다.


```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }

        // ...
```

배포 파일은 `readJSON`에서 로드되며, 이에 대한 세부 사항은 생략되어 있습니다. 이 함수는 리소스 배포 호출을 위한 메타 데이터를 생성하는 데 사용되는 형식인 `*map[string]interface{}`을(를) 반환합니다.

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        deploymentFuture, err := deploymentsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                deploymentName,
                resources.Deployment{
                        Properties: &resources.DeploymentProperties{
                                Template:   template,
                                Parameters: params,
                                Mode:       resources.Incremental,
                        },
                },
        )
        if err != nil {
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

이 코드는 리소스 그룹을 만들 때와 동일한 패턴을 따릅니다. 새로운 클라이언트가 생성되고 Azure에서 인증을 수행할 수 있는 기능이 제공된 다음, 메서드가 호출됩니다. 이 메서드도 리소스 그룹의 해당 메서드와 동일한 이름(`CreateOrUpdate`)을 갖습니다. 이 패턴은 SDK에서 반복적으로 확인할 수 있습니다. 일반적으로 비슷한 작업을 수행하는 메서드는 동일한 이름을 갖습니다.

가장 큰 차이점은 `deploymentsClient.CreateOrUpdate()` 메서드의 반환 값에 있습니다. 이 값은 [미래 디자인 패턴](https://en.wikipedia.org/wiki/Futures_and_promises)을 따르는 `Future` 개체입니다. 미래는 Azure에서 장기 실행 작업을 나타내며, 다른 작업을 수행하는 동안 간헐적으로 폴링해야 할 수 있습니다.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

이 예에서 수행할 최상의 작업은 이 작업이 완료될 때까지 기다리는 것입니다. 미래 개체를 기다리기 위해서는 [컨텍스트 개체](https://blog.golang.org/context) 및 미래 개체를 생성한 클라이언트가 모두 필요합니다. 여기에서 가능한 두 가지 오류 소스는 메서드를 호출하려고 시도할 때 클라이언트 측에서 발생한 오류와 서버에서의 오류 응답입니다. 후자는 `deploymentFuture.Result()` 호출 중에 반환됩니다.

### <a name="obtaining-the-assigned-ip-address"></a>할당된 IP 주소 가져오기

새로 만든 가상 머신에서 어떤 작업을 수행하려면 할당된 IP 주소가 필요합니다. IP 주소는 NIC(네트워크 인터페이스 컨트롤러) 리소스에 바인딩된 고유의 개별 Azure 리소스입니다.

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

이 메서드는 매개 변수 파일에 저장된 정보에 의존합니다. 이 코드는 가상 머신에 직접 쿼리해서 해당 NIC를 가져오고, NIC에 쿼리해서 해당 IP 주소를 가져온 후, IP 리소스를 직접 쿼리할 수 있습니다. 이것은 종속성 및 작업이 길게 늘어진 분석 체인이기 때문에 비용이 높습니다. JSON 정보는 로컬이기 때문에 대신 로드할 수 있습니다.

가상 머신 사용자 및 암호 값도 JSON에서 로드될 수 있습니다.

## <a name="next-steps"></a>다음 단계

이 빠른 시작에서는 기존 템플릿을 사용해서 Go를 통해 배포했습니다. 그런 다음 SSH를 통해 새롭게 생성된 가상 머신에 연결하여 실행하는지 확인했습니다.

Go를 사용한 Azure 환경에서 가상 머신을 사용하는 방법에 대해 학습을 계속하려면 [Go용 Azure 컴퓨팅 샘플](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) 또는 [Go용 Azure 리소스 관리 샘플](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)을 참조하십시오.
