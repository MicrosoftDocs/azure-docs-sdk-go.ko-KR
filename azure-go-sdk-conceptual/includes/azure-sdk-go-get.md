Azure SDK for Go는 Go 버전 1.8 이상과 호환됩니다. [Azure Stack 프로필](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles)을 사용하는 환경의 경우 최소 요구 사항은 Go 버전 1.9입니다. 시스템에서 Go를 사용할 수 없는 경우 [Go 설치 지침](https://golang.org/doc/install)을 참조하십시오.

Azure SDK for Go 및 해당 종속성은 `go get`을(를) 통해 얻을 수 있습니다.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> URL에서 `Azure`은(는) 대문자로 표시해야 합니다. 그렇지 않으면 SDK를 사용할 때 대소문자 관련 가져오기 문제가 발생할 수 있습니다. 또한 가져오기 구문에서도 `Azure`을(를) 대문자로 표시해야 합니다.

