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
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="ba45c-104">빠른 시작: Azure SDK for Go를 사용하여 템플릿에서 Azure 가상 머신 배포</span><span class="sxs-lookup"><span data-stu-id="ba45c-104">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="ba45c-105">이 빠른 시작에서는 Azure SDK for Go를 사용하여 템플릿에서 리소스를 배포하는 방법에 대해 자세히 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-105">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="ba45c-106">템플릿은 [Azure 리소스 그룹](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) 내에 포함된 모든 리소스에 대한 스냅샷입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-106">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="ba45c-107">과정을 진행하는 동안 유용한 작업을 수행하면서 SDK의 기능 및 규칙에 익숙해질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-107">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="ba45c-108">이 빠른 시작을 마치면 가상 머신을 실행하고 사용자 이름 및 암호를 사용해서 로그인하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-108">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="ba45c-109">Azure CLI의 로컬 설치를 사용할 경우, 이 빠른 시작을 위해서는 CLI 버전 2.0.24 이상이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-109">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="ba45c-110">`az --version`을(를) 실행하여 CLI 설치가 이 요구 사항을 충족하는지 확인하십시오.</span><span class="sxs-lookup"><span data-stu-id="ba45c-110">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="ba45c-111">설치 또는 업그레이드가 필요한 경우에는 [Azure CLI 2.0 설치](/cli/azure/install-azure-cli)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="ba45c-111">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="ba45c-112">Azure SDK for Go 설치</span><span class="sxs-lookup"><span data-stu-id="ba45c-112">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="ba45c-113">서비스 주체 만들기</span><span class="sxs-lookup"><span data-stu-id="ba45c-113">Create a service principal</span></span>

<span data-ttu-id="ba45c-114">응용 프로그램에 비 대화형으로 로그인하려면 서비스 주체가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-114">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="ba45c-115">서비스 주체는 고유한 사용자 ID를 만드는 RBAC(역할 기반 액세스 제어)의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-115">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="ba45c-116">CLI를 사용하여 새로운 서비스 주체를 만들려면 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-116">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="ba45c-117">출력에 `appId`, `password` 및 `tenant` 값이 기록되는지 __확인하십시오__.</span><span class="sxs-lookup"><span data-stu-id="ba45c-117">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="ba45c-118">이러한 값은 응용 프로그램이 Azure에서 인증을 수행하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-118">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="ba45c-119">Azure CLI 2.0을 사용한 서비스 주체 만들기 및 관리에 대한 자세한 내용은 [Azure CLI 2.0을 사용하여 Azure 서비스 주체 만들기](/cli/azure/create-an-azure-service-principal-azure-cli)를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="ba45c-119">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="ba45c-120">코드 가져오기</span><span class="sxs-lookup"><span data-stu-id="ba45c-120">Get the code</span></span>

<span data-ttu-id="ba45c-121">빠른 시작 코드 및 `go get`의 모든 종속성을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="ba45c-122">이 코드는 컴파일할 수 있지만, Azure 계정 및 생성된 서비스 주체에 대한 정보를 제공할 때까지 올바르게 실행되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-122">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="ba45c-123">`main.go`에는 `authInfo` 구조체를 포함하는 `config` 변수가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-123">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="ba45c-124">올바른 인증을 위해서는 이 구조체에서 해당 필드 값을 바꿔야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-124">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="ba45c-125">`SubscriptionID`: CLI 명령으로 가져올 수 있는 구독 ID</span><span class="sxs-lookup"><span data-stu-id="ba45c-125">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="ba45c-126">`TenantID`: 서비스 주체를 만들 때 기록된 `tenant` 값인 테넌트 ID</span><span class="sxs-lookup"><span data-stu-id="ba45c-126">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="ba45c-127">`ServicePrincipalID`: 서비스 주체를 만들 때 기록된 `appId` 값</span><span class="sxs-lookup"><span data-stu-id="ba45c-127">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="ba45c-128">`ServicePrincipalSecret`: 서비스 주체를 만들 때 기록된 `password` 값</span><span class="sxs-lookup"><span data-stu-id="ba45c-128">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="ba45c-129">또한 `vm-quickstart-params.json` 파일의 값을 편집해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-129">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="ba45c-130">`vm_password`: 가상 머신 사용자 계정의 암호입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-130">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="ba45c-131">길이는 12-72자여야 하며 다음 문자 중 3개를 포함해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-131">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="ba45c-132">소문자</span><span class="sxs-lookup"><span data-stu-id="ba45c-132">A lowercase letter</span></span>
  * <span data-ttu-id="ba45c-133">대문자</span><span class="sxs-lookup"><span data-stu-id="ba45c-133">An uppercase letter</span></span>
  * <span data-ttu-id="ba45c-134">숫자</span><span class="sxs-lookup"><span data-stu-id="ba45c-134">A number</span></span>
  * <span data-ttu-id="ba45c-135">기호</span><span class="sxs-lookup"><span data-stu-id="ba45c-135">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="ba45c-136">코드 실행</span><span class="sxs-lookup"><span data-stu-id="ba45c-136">Running the code</span></span>

<span data-ttu-id="ba45c-137">`go run` 명령으로 빠른 시작을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-137">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="ba45c-138">배포 중 오류가 발생하면 자세한 설명은 없지만, 문제가 있음을 나타내는 메시지가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-138">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="ba45c-139">Azure CLI에서 다음 명령을 사용하여 배포 오류에 대한 세부 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-139">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="ba45c-140">배포가 성공하면 새롭게 생성된 가상 머신에 로그인하기 위한 사용자 이름, IP 주소 및 암호를 제공하는 메시지가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-140">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="ba45c-141">이 시스템에 SSH로 로그인하여 시스템이 작동되어 실행 중인지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-141">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="ba45c-142">정리</span><span class="sxs-lookup"><span data-stu-id="ba45c-142">Cleaning up</span></span>

<span data-ttu-id="ba45c-143">CLI를 사용해서 리소스 그룹을 삭제하여 이 빠른 시작 중 생성된 리소스를 정리합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-143">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="ba45c-144">코드 심화 안내</span><span class="sxs-lookup"><span data-stu-id="ba45c-144">Code in depth</span></span>

<span data-ttu-id="ba45c-145">빠른 시작 코드는 변수 및 여러 작은 기능들의 블록으로 세분화됩니다. 여기에서는 이러한 각 블록에 대해 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-145">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="ba45c-146">변수 할당 및 구조체</span><span class="sxs-lookup"><span data-stu-id="ba45c-146">Variable assignments and structs</span></span>

<span data-ttu-id="ba45c-147">빠른 시작은 자체 포함된 방식이므로, 명령줄 옵션 또는 환경 변수 대신 전역 변수를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-147">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="ba45c-148">`authInfo` 구조체는 Azure 서비스에 대한 권한 부여를 위해 필요한 모든 정보를 캡슐화하도록 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-148">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="ba45c-149">값은 생성된 리소스의 이름을 제공하도록 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-149">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="ba45c-150">또한 여기에 지정된 위치는 다른 데이터 센터에서의 배포 동작을 확인할 수 있도록 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-150">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="ba45c-151">일부 데이터 센터에는 필요한 리소스가 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-151">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="ba45c-152">`templateFile` 및 `parametersFile` 상수는 배포에 필요한 파일을 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-152">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="ba45c-153">보안 주체 토큰은 나중에 다뤄지며, `ctx` 변수는 네트워크에 대한 [Go 컨텍스트](https://blog.golang.org/context)입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-153">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="ba45c-154">init() 및 권한 부여</span><span class="sxs-lookup"><span data-stu-id="ba45c-154">init() and authorization</span></span>

<span data-ttu-id="ba45c-155">코드에 대한 `init()` 메서드는 권한 부여를 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-155">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="ba45c-156">권한 부여는 빠른 시작에 포함된 모든 항목들의 사전 조건이므로, 초기화 중에 준비하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-156">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="ba45c-157">이 코드는 권한 부여를 위한 두 단계를 완료합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-157">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="ba45c-158">`TenantID`에 대한 OAuth 구성 정보는 Azure Active Directory와의 상호 작용을 통해 검색됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-158">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="ba45c-159">[`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) 개체에는 표준 Azure 구성에 사용되는 끝점이 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-159">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="ba45c-160">[`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) 함수가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-160">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="ba45c-161">이 함수는 서비스 주체 로그인과 함께 OAuth 정보는 물론 사용 중인 Azure 관리 스타일을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-161">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="ba45c-162">특별한 요구 사항이 있고 해당 작업을 잘 알고 있지 않은 한, 이 값은 항상 `.ResourceManagerEndpoint`이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-162">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="ba45c-163">main()의 작업 흐름</span><span class="sxs-lookup"><span data-stu-id="ba45c-163">Flow of operations in main()</span></span>

<span data-ttu-id="ba45c-164">`main()` 함수는 작업 흐름만 나타내고 오류 검사만 수행하는 간단한 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-164">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="ba45c-165">코드가 실행하는 단계는 순서대로 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-165">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="ba45c-166">(`createGroup()`)에 배포할 리소스 그룹 만들기</span><span class="sxs-lookup"><span data-stu-id="ba45c-166">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="ba45c-167">이 그룹(`createDeployment()`) 내에 배포 만들기</span><span class="sxs-lookup"><span data-stu-id="ba45c-167">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="ba45c-168">배포된 가상 머신(`getLogin()`)에 대한 로그인 정보 가져오기 및 표시</span><span class="sxs-lookup"><span data-stu-id="ba45c-168">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="ba45c-169">리소스 그룹 만들기</span><span class="sxs-lookup"><span data-stu-id="ba45c-169">Creating the resource group</span></span>

<span data-ttu-id="ba45c-170">`createGroup()` 함수는 리소스 그룹을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-170">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="ba45c-171">호출 흐름 및 인수를 보면 서비스 상호 작용이 SDK에 구성된 방식을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-171">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="ba45c-172">Azure 서비스와의 일반적인 상호 작용 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-172">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="ba45c-173">`service.NewXClient()` 메서드를 사용하여 클라이언트를 만듭니다. 여기서 `X`은(는) 상호 작용하려는 `service`의 리소스 유형입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-173">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="ba45c-174">이 함수는 항상 구독 ID를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-174">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="ba45c-175">클라이언트에 대한 권한 부여 방법을 설정해서 원격 API와 상호 작용할 수 있도록 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-175">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="ba45c-176">원격 API에 따라 클라이언트에서 메서드 호출을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-176">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="ba45c-177">서비스 클라이언트 메서드는 일반적으로 리소스 이름 및 메타데이터 개체를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-177">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="ba45c-178">여기에서 [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 함수는 형식 변환을 수행하기 위해 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-178">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="ba45c-179">SDK의 메서드에 대한 매개 변수 구조체는 거의 배타적으로 포인터를 사용하므로, 이러한 메서드는 형식 변환을 쉽게 만들기 위해 제공되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-179">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="ba45c-180">편리한 변환기의 전체 목록 및 동작을 보려면 [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) 모듈 설명서를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="ba45c-180">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="ba45c-181">`groupsClient.CreateOrUpdate()` 작업은 리소스 그룹을 나타내는 데이터 구조체에 대한 포인터를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-181">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="ba45c-182">이러한 종류의 직접 반환 값은 동기적으로 수행되어야 하는 단기 실행 작업을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-182">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="ba45c-183">다음 섹션에서는 장기 실행 작업의 예 및 이와 상호 작용하는 방법을 배웁니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-183">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="ba45c-184">배포 수행</span><span class="sxs-lookup"><span data-stu-id="ba45c-184">Performing the deployment</span></span>

<span data-ttu-id="ba45c-185">해당 리소스를 포함할 그룹을 만든 다음에는 배포를 실행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-185">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="ba45c-186">이 코드는 여러 논리 부분을 강조하기 위해 더 작은 섹션으로 구분됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-186">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="ba45c-187">배포 파일은 `readJSON`에서 로드되며, 이에 대한 세부 사항은 생략되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-187">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="ba45c-188">이 함수는 리소스 배포 호출을 위한 메타 데이터를 생성하는 데 사용되는 형식인 `*map[string]interface{}`을(를) 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-188">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="ba45c-189">이 코드는 리소스 그룹을 만들 때와 동일한 패턴을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-189">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="ba45c-190">새로운 클라이언트가 생성되고 Azure에서 인증을 수행할 수 있는 기능이 제공된 다음, 메서드가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-190">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="ba45c-191">이 메서드도 리소스 그룹의 해당 메서드와 동일한 이름(`CreateOrUpdate`)을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-191">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="ba45c-192">이 패턴은 SDK에서 반복적으로 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-192">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="ba45c-193">일반적으로 비슷한 작업을 수행하는 메서드는 동일한 이름을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-193">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="ba45c-194">가장 큰 차이점은 `deploymentsClient.CreateOrUpdate()` 메서드의 반환 값에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-194">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="ba45c-195">이 값은 [미래 디자인 패턴](https://en.wikipedia.org/wiki/Futures_and_promises)을 따르는 `Future` 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-195">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="ba45c-196">미래는 Azure에서 장기 실행 작업을 나타내며, 다른 작업을 수행하는 동안 간헐적으로 폴링해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-196">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="ba45c-197">이 예에서 수행할 최상의 작업은 이 작업이 완료될 때까지 기다리는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-197">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="ba45c-198">미래 개체를 기다리기 위해서는 [컨텍스트 개체](https://blog.golang.org/context) 및 미래 개체를 생성한 클라이언트가 모두 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-198">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="ba45c-199">여기에서 가능한 두 가지 오류 소스는 메서드를 호출하려고 시도할 때 클라이언트 측에서 발생한 오류와 서버에서의 오류 응답입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-199">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="ba45c-200">후자는 `deploymentFuture.Result()` 호출 중에 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-200">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="ba45c-201">할당된 IP 주소 가져오기</span><span class="sxs-lookup"><span data-stu-id="ba45c-201">Obtaining the assigned IP address</span></span>

<span data-ttu-id="ba45c-202">새로 만든 가상 머신에서 어떤 작업을 수행하려면 할당된 IP 주소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-202">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="ba45c-203">IP 주소는 NIC(네트워크 인터페이스 컨트롤러) 리소스에 바인딩된 고유의 개별 Azure 리소스입니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-203">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="ba45c-204">이 메서드는 매개 변수 파일에 저장된 정보에 의존합니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-204">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="ba45c-205">이 코드는 가상 머신에 직접 쿼리해서 해당 NIC를 가져오고, NIC에 쿼리해서 해당 IP 주소를 가져온 후, IP 리소스를 직접 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-205">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="ba45c-206">이것은 종속성 및 작업이 길게 늘어진 분석 체인이기 때문에 비용이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-206">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="ba45c-207">JSON 정보는 로컬이기 때문에 대신 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-207">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="ba45c-208">가상 머신 사용자 및 암호 값도 JSON에서 로드될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-208">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba45c-209">다음 단계</span><span class="sxs-lookup"><span data-stu-id="ba45c-209">Next steps</span></span>

<span data-ttu-id="ba45c-210">이 빠른 시작에서는 기존 템플릿을 사용해서 Go를 통해 배포했습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-210">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="ba45c-211">그런 다음 SSH를 통해 새롭게 생성된 가상 머신에 연결하여 실행하는지 확인했습니다.</span><span class="sxs-lookup"><span data-stu-id="ba45c-211">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="ba45c-212">Go를 사용한 Azure 환경에서 가상 머신을 사용하는 방법에 대해 학습을 계속하려면 [Go용 Azure 컴퓨팅 샘플](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) 또는 [Go용 Azure 리소스 관리 샘플](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="ba45c-212">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
