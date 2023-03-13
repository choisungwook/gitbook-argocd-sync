# Project 권한(Role)과 예제

## 개념

Project Role은 특정 Project에만 유효한 Role입니다. 예를 들어 test1 Project에 생성한 Role은 test1에만 유효하고 다른 Project에는 사용할 수 없습니다. 그리고 web UI로그인은 못하고 JWT방식만 지원합니다.&#x20;

## 사용 목적

Argo CD전체 관리 권한보다는 Project만 관리할 수 있는 제한된 권한을 설정할 때 적합합니다.



## 생성 방법

web UI, Argo CD CLI, appproject CRD 모두 가능합니다.&#x20;



web UI에서는 Project화면 왼쪽 위에 생성버튼이 있습니다.

<figure><img src="../.gitbook/assets/image (164).png" alt=""><figcaption></figcaption></figure>



Role이름과 설명을 입력하면 Role을 생성할 수 있습니다.

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>



생성된 Role은 Project Role탭에서 확인할 수 있습니다.

<figure><img src="../.gitbook/assets/image (189).png" alt=""><figcaption></figcaption></figure>



## Policy 설정

지금 생성한 Role은 권한설정이 안되어 있어 권한이 아무것도 없습니다. 권한 설정은 Project를 설정 후에 Global Role([role.md](../user/role.md "mention"))처럼 설정하면 됩니다.

<figure><img src="../.gitbook/assets/image (173).png" alt=""><figcaption></figcaption></figure>



## JWT 생성

Project Role은 곧바로 Project에 적용되어 사용되지 않고 JWT를 발급받아 사용할 수 있습니다. JWT는 Project Role설정 화면에서 생성할 수 있습니다. 성성된 JWT는 설정화면이 닫힌 후에 알 수 없으므로 잘 보관해야 합니다.

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>



## JWT 사용 예

JWT는 WEB UI에 사용할 수 없고 Argo CLI 또는 REST API, SDK에서 사용할 수 있습니다. 저는 Argo CD CLI 사용 예를 소개드릴게요.



먼저 생성한 JWT를 환경변수에 설정합니다.&#x20;

```shell
TOKEN=<TOKEN>
```

<figure><img src="../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>



argocd account can-i로 test1 Project의 Application 조회(get)이 가능한지 확인할 수 있습니다. Argo CD login을 하지 않아도 TOKEN으로 인증과 인가가 가능해졌습니다!.

```shell
argocd account can-i get applications 'test1/*' --auth-token $TOKENl
```

<figure><img src="../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>



test1 Project에 Application 생성 권한이 있는지 확인해볼까요? 당연히 create application Policy를 추가하지 않았으므로 No 메세지가 나옵니다.

```shell
argocd account can-i create applications 'test1/*' --auth-token $TOKEN
```

<figure><img src="../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure>

WEB UI에서 test1 Project의 Appliction을 생성할 수 있도록 create Policy를 추가합니다.

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>



그리고 다시 Application 생성 권한을 검사하면 yes가 나옵니다.

```
argocd account can-i create  applications 'test1/*' --auth-token $TOKEN
```

<figure><img src="../.gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

