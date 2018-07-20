---
title: Go에서 Azure 가상 머신 배포
description: Azure SDK for Go를 사용하여 가상 머신을 배포합니다.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 6b1de35748fb7694d45715fa7f028d5730530d2e
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039559"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="600d4-103">빠른 시작: Azure SDK for Go를 사용하여 템플릿에서 Azure 가상 머신 배포</span><span class="sxs-lookup"><span data-stu-id="600d4-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="600d4-104">이 빠른 시작에서는 Azure SDK for Go를 사용하여 템플릿에서 리소스를 배포하는 방법에 대해 자세히 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="600d4-105">템플릿은 [Azure 리소스 그룹](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) 내에 포함된 모든 리소스에 대한 스냅샷입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="600d4-106">과정을 진행하는 동안 유용한 작업을 수행하면서 SDK의 기능 및 규칙에 익숙해질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="600d4-107">이 빠른 시작을 마치면 가상 머신을 실행하고 사용자 이름 및 암호를 사용해서 로그인하게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="600d4-108">Azure CLI의 로컬 설치를 사용할 경우, 이 빠른 시작을 위해서는 CLI 버전 __2.0.28__ 이상이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="600d4-109">`az --version`을(를) 실행하여 CLI 설치가 이 요구 사항을 충족하는지 확인하십시오.</span><span class="sxs-lookup"><span data-stu-id="600d4-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="600d4-110">설치 또는 업그레이드를 해야 할 경우 [Azure CLI 설치](/cli/azure/install-azure-cli)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="600d4-110">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="600d4-111">Azure SDK for Go 설치</span><span class="sxs-lookup"><span data-stu-id="600d4-111">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="600d4-112">서비스 주체 만들기</span><span class="sxs-lookup"><span data-stu-id="600d4-112">Create a service principal</span></span>

<span data-ttu-id="600d4-113">응용 프로그램으로 Azure에 비 대화형으로 로그인하려면 서비스 주체가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-113">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="600d4-114">서비스 주체는 고유한 사용자 ID를 만드는 RBAC(역할 기반 액세스 제어)의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="600d4-115">CLI를 사용하여 새로운 서비스 주체를 만들려면 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="600d4-116">환경 변수 `AZURE_AUTH_LOCATION`을 이 파일의 전체 경로가 되도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="600d4-117">그러면 어떠한 변경을 하거나 서비스 주체로부터 정보를 기록하지 않아도 SDK가 이 파일에서 직접 자격 증명을 찾아서 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="600d4-118">코드 가져오기</span><span class="sxs-lookup"><span data-stu-id="600d4-118">Get the code</span></span>

<span data-ttu-id="600d4-119">빠른 시작 코드 및 `go get`의 모든 종속성을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="600d4-120">`AZURE_AUTH_LOCATION` 변수가 적절히 설정되는 경우 소스 코드를 수정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="600d4-121">프로그램을 실행하면 여기에서 모든 필요한 인증 정보를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="600d4-122">코드 실행</span><span class="sxs-lookup"><span data-stu-id="600d4-122">Running the code</span></span>

<span data-ttu-id="600d4-123">`go run` 명령으로 빠른 시작을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="600d4-124">배포 중 오류가 발생하면 자세한 설명은 없지만, 문제가 있음을 나타내는 메시지가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="600d4-125">Azure CLI에서 다음 명령을 사용하여 배포 오류에 대한 완전한 세부 정보를 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="600d4-126">배포가 성공하면 새롭게 생성된 가상 머신에 로그인하기 위한 사용자 이름, IP 주소 및 암호를 제공하는 메시지가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="600d4-127">이 시스템에 SSH로 로그인하여 시스템이 작동되어 실행 중인지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="600d4-128">정리</span><span class="sxs-lookup"><span data-stu-id="600d4-128">Cleaning up</span></span>

<span data-ttu-id="600d4-129">CLI를 사용해서 리소스 그룹을 삭제하여 이 빠른 시작 중 생성된 리소스를 정리합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="600d4-130">코드 심화 안내</span><span class="sxs-lookup"><span data-stu-id="600d4-130">Code in depth</span></span>

<span data-ttu-id="600d4-131">빠른 시작 코드는 변수 및 여러 작은 기능들의 블록으로 세분화됩니다. 여기에서는 이러한 각 블록에 대해 자세히 설명합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="600d4-132">변수, 상수 및 형식</span><span class="sxs-lookup"><span data-stu-id="600d4-132">Variables, constants, and types</span></span>

<span data-ttu-id="600d4-133">빠른 시작은 자체 포함되므로, 전역 상수와 변수를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

<span data-ttu-id="600d4-134">값은 생성된 리소스의 이름을 제공하도록 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="600d4-135">또한 여기에 지정된 위치는 다른 데이터 센터에서의 배포 동작을 확인할 수 있도록 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="600d4-136">일부 데이터 센터에는 필요한 리소스가 없을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="600d4-137">`clientInfo` 형식은 SDK에서 클라이언트를 설정하고 VM 암호를 설정하기 위해 인증 파일에서 독립적으로 로드해야 하는 모든 정보를 캡슐화하도록 선언됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="600d4-138">`templateFile` 및 `parametersFile` 상수는 배포에 필요한 파일을 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="600d4-139">`authorizer`는 인증을 위해 Go SDK를 통해 구성되며 `ctx` 변수는 네트워크 작업에 대한 [Go 컨텍스트](https://blog.golang.org/context)입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="600d4-140">인증 및 초기화</span><span class="sxs-lookup"><span data-stu-id="600d4-140">Authentication and initialization</span></span>

<span data-ttu-id="600d4-141">`init` 함수가 인증을 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="600d4-142">인증은 빠른 시작에 포함된 모든 항목들의 사전 조건이므로, 초기화 중에 준비하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="600d4-143">또한 클라이언트와 VM을 구성하기 위해 인증 파일에서 필요한 일부 정보를 로드합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

<span data-ttu-id="600d4-144">먼저, `AZURE_AUTH_LOCATION`에 있는 파일에서 인증 정보를 로드하기 위해 [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile)이 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="600d4-145">그 다음, 이 파일을 `readJSON` 함수(여기서는 생략됨)에서 수동으로 로드하여 프로그램의 나머지 부분을 실행하는 데 필요한 두 개의 값(클라이언트의 구독 ID 및 VM의 암호에도 사용되는 서비스 주체의 비밀 정보)을 끌어옵니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="600d4-146">빠른 시작을 간단하게 유지하기 위해 서비스 주체 암호가 재사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="600d4-147">프로덕션 환경에서는 Azure 리소스에 대한 액세스 권한을 부여하는 암호를 __절대__ 재사용하지 않도록 하십시오.</span><span class="sxs-lookup"><span data-stu-id="600d4-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="600d4-148">main()의 작업 흐름</span><span class="sxs-lookup"><span data-stu-id="600d4-148">Flow of operations in main()</span></span>

<span data-ttu-id="600d4-149">`main` 함수는 작업 흐름만 나타내고 오류 검사만 수행하는 간단한 함수입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

<span data-ttu-id="600d4-150">코드가 실행하는 단계는 순서대로 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="600d4-151">(`createGroup`)에 배포할 리소스 그룹 만들기</span><span class="sxs-lookup"><span data-stu-id="600d4-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="600d4-152">이 그룹(`createDeployment`) 내에 배포 만들기</span><span class="sxs-lookup"><span data-stu-id="600d4-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="600d4-153">배포된 가상 머신(`getLogin`)에 대한 로그인 정보 가져오기 및 표시</span><span class="sxs-lookup"><span data-stu-id="600d4-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="600d4-154">리소스 그룹 만들기</span><span class="sxs-lookup"><span data-stu-id="600d4-154">Creating the resource group</span></span>

<span data-ttu-id="600d4-155">`createGroup` 함수는 리소스 그룹을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="600d4-156">호출 흐름 및 인수를 보면 서비스 상호 작용이 SDK에 구성된 방식을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="600d4-157">Azure 서비스와의 일반적인 상호 작용 흐름은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="600d4-158">`service.New*Client()` 메서드를 사용하여 클라이언트를 만듭니다. 여기서 `*`은(는) 상호 작용하려는 `service`의 리소스 유형입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="600d4-159">이 함수는 항상 구독 ID를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="600d4-160">클라이언트에 대한 권한 부여 방법을 설정해서 원격 API와 상호 작용할 수 있도록 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="600d4-161">원격 API에 따라 클라이언트에서 메서드 호출을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="600d4-162">서비스 클라이언트 메서드는 일반적으로 리소스 이름 및 메타데이터 개체를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="600d4-163">여기에서 [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) 함수는 형식 변환을 수행하기 위해 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="600d4-164">SDK 메서드에 대한 매개 변수는 거의 배타적으로 포인터를 사용하므로, 형식 변환을 쉽게 하기 위해 편리한 메서드가 제공되었습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="600d4-165">편리한 변환기의 전체 목록 및 동작을 보려면 [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) 모듈 설명서를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="600d4-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="600d4-166">`groupsClient.CreateOrUpdate` 메서드는 리소스 그룹을 나타내는 데이터 형식에 대한 포인터를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="600d4-167">이러한 종류의 직접 반환 값은 동기적으로 수행되어야 하는 단기 실행 작업을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="600d4-168">다음 섹션에서는 장기 실행 작업의 예 및 이와 상호 작용하는 방법을 배웁니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="600d4-169">배포 수행</span><span class="sxs-lookup"><span data-stu-id="600d4-169">Performing the deployment</span></span>

<span data-ttu-id="600d4-170">리소스 그룹을 만든 다음에는 배포를 실행해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="600d4-171">이 코드는 여러 논리 부분을 강조하기 위해 더 작은 섹션으로 구분됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

<span data-ttu-id="600d4-172">배포 파일은 `readJSON`에서 로드되며, 이에 대한 세부 사항은 생략되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="600d4-173">이 함수는 리소스 배포 호출을 위한 메타 데이터를 생성하는 데 사용되는 형식인 `*map[string]interface{}`을(를) 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="600d4-174">VM의 암호도 배포 매개 변수에서 수동으로 설정됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-174">The VM's password is also set manually on the deployment parameters.</span></span>

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

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
        return
    }
```

<span data-ttu-id="600d4-175">이 코드는 리소스 그룹을 만들 때와 동일한 패턴을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="600d4-176">새로운 클라이언트가 생성되고 Azure에서 인증을 수행할 수 있는 기능이 제공된 다음, 메서드가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="600d4-177">이 메서드도 리소스 그룹의 해당 메서드와 동일한 이름(`CreateOrUpdate`)을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="600d4-178">이 패턴은 SDK 전체에서 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-178">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="600d4-179">일반적으로 비슷한 작업을 수행하는 메서드는 동일한 이름을 갖습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="600d4-180">가장 큰 차이점은 `deploymentsClient.CreateOrUpdate` 메서드의 반환 값에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="600d4-181">이 값은 [미래 디자인 패턴](https://en.wikipedia.org/wiki/Futures_and_promises)을 따르는 [미래](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="600d4-182">미래는 Azure에서 완료 시 폴링, 취소 또는 차단할 수 있는 장기 실행 작업을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

<span data-ttu-id="600d4-183">이 예에서 수행할 최상의 작업은 이 작업이 완료될 때까지 기다리는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="600d4-184">미래 개체를 기다리기 위해서는 [컨텍스트 개체](https://blog.golang.org/context) 및 `Future`를 생성한 클라이언트가 모두 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="600d4-185">여기에서 가능한 두 가지 오류 소스는 메서드를 호출하려고 시도할 때 클라이언트 측에서 발생한 오류와 서버에서의 오류 응답입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="600d4-186">후자는 `deploymentFuture.Result` 호출 중에 반환됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="600d4-187">배포 정보를 검색한 후 배포 정보가 비어 있는 버그가 발생할 경우 `deploymentsClient.Get` 수동 호출을 통해 데이터를 채울 수 있는 해결 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="600d4-188">할당된 IP 주소 가져오기</span><span class="sxs-lookup"><span data-stu-id="600d4-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="600d4-189">새로 만든 가상 머신에서 어떤 작업을 수행하려면 할당된 IP 주소가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="600d4-190">IP 주소는 NIC(네트워크 인터페이스 컨트롤러) 리소스에 바인딩된 고유의 개별 Azure 리소스입니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

<span data-ttu-id="600d4-191">이 메서드는 매개 변수 파일에 저장된 정보에 의존합니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="600d4-192">이 코드는 가상 머신에 직접 쿼리해서 해당 NIC를 가져오고, NIC에 쿼리해서 해당 IP 주소를 가져온 후, IP 리소스를 직접 쿼리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="600d4-193">이것은 종속성 및 작업이 길게 늘어진 분석 체인이기 때문에 비용이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="600d4-194">JSON 정보는 로컬이기 때문에 대신 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="600d4-195">VM 사용자에 대한 값은 JSON에서도 로드됩니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="600d4-196">이전에는 VM 암호가 인증 파일에서 로드되었습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="600d4-197">다음 단계</span><span class="sxs-lookup"><span data-stu-id="600d4-197">Next steps</span></span>

<span data-ttu-id="600d4-198">이 빠른 시작에서는 기존 템플릿을 사용해서 Go를 통해 배포했습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="600d4-199">그런 다음 SSH를 통해 새롭게 생성된 가상 머신에 연결하여 실행하는지 확인했습니다.</span><span class="sxs-lookup"><span data-stu-id="600d4-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="600d4-200">Go를 사용한 Azure 환경에서 가상 머신을 사용하는 방법에 대해 학습을 계속하려면 [Go용 Azure 컴퓨팅 샘플](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) 또는 [Go용 Azure 리소스 관리 샘플](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="600d4-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="600d4-201">SDK에서 사용 가능한 인증 방법 및 지원되는 인증 유형에 대해 알아보려면 [Azure SDK for Go를 사용한 인증](azure-sdk-go-authorization.md)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="600d4-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
