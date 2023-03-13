# git private repo 관리

{% embed url="https://youtu.be/ZKvXEFnWMU8" %}
[https://youtu.be/ZKvXEFnWMU8](https://youtu.be/ZKvXEFnWMU8)
{% endembed %}

{% hint style="danger" %}
private repo관리 기능은 버전이 낮은 Argo CD에 버그가 있습니다. 그러므로 높은 버전 Argo CD를 사용하시길 바랍니다.
{% endhint %}

## 개념

Git private repo는 허용된 사용만 접근이 가능하므로, ArgoCD는 Private repo에 접근하기 위해 인증을 관리하는 기능이 있습니다.



## 설정 방법

WEB UI, kubectl, argo CLI로 생성할 수 있습니다. 프로토콜은 HTTPS, SSH를 지원하고 세부설정은 username/password, key pair, TLS certificates가 있습니다.



### WEB UI 관리

WEB UI에서는 \[왼쪽 메뉴 2번째 → Repository]을 클릭하면 설정화면으로 이동합니다.

![](<../.gitbook/assets/image (184).png>)

![](<../.gitbook/assets/image (32).png>)



### kubectl 관

kubectl로는 private repo를 관리하려면 secret을 생성하면 됩니다. secret을 정의할 때 annotations에 argocd.argoproj.io/secret-type가 필요합니다.

\| 참고자료: [https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#repositories](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#repositories)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: private-repo
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  type: git
  url: git@github.com:argoproj/my-private-repository
  sshPrivateKey: |
    -----BEGIN OPENSSH PRIVATE KEY-----
    ...
    -----END OPENSSH PRIVATE KEY-----
```



## 예제

<mark style="color:red;">이 글에서는 SSH방법을 실습</mark>합니다. 실습을 진행하기 위해 private git repo와 ssh키 쌍이 필요합니다.



### private repo 생성

먼저 private git repo를 생성합니다. 저는 github으로 생성했습니다.



### 키쌍 생성

ssh-keygen명령어로 키쌍(공개키/개인키)을 생성합니다.

```shell
ssh-keygen -f sample
```

![](<../.gitbook/assets/image (153).png>)



공개키는 sample.pub파이고 개인키는 sample파일입니다.

![](<../.gitbook/assets/image (174).png>)

### private repo에 공개키 등록&#x20;

생성한 공개키를 github private repo에 등록하는 과정입니다. github repo페이지에서 \[Settings → Deploy Keys]메뉴에서 키를 등록할 수 있습니다.

![](<../.gitbook/assets/image (182).png>)

<mark style="color:red;"></mark>

<mark style="color:red;">Allow write access를 선택</mark>해야 합니다. 음… 이유는 잘 모르겠지만 내부로직에서 쓰기권한이 필요한 작업이 있나봐요

![](<../.gitbook/assets/image (190).png>)

### Argo CD private repo 생성

github prviate repo를 Argo CD에 등록하는 과정입니다.



WEB UI에서는 \[왼쪽 메뉴 2번째 → Repository]을 클릭하면 설정화면으로 이동합니다.

![](<../.gitbook/assets/image (59).png>)



상단 메뉴에서 \[CONNECT REPO USING SSH] 버튼을 클릭합니다

![](<../.gitbook/assets/image (19).png>)

아래 그림처럽 입력하고 \[Connect]버튼을 클릭합니다.

![](<../.gitbook/assets/image (24).png>)



성공적으로 연결되면 Successful메세지가 뜹니다. <mark style="color:red;">만약 잘 안된다면 argo cd버전을 업데이트 해보세요</mark>. argo cd버그로 연결이 안될 수 있습니다.

![](<../.gitbook/assets/image (17).png>)



### Application 생성

등록한 private git repo를 이용해서 application을 생성해보겠습니다.



private repo에는 nginx pod를 생성하는 yaml파일을 추가했습니다.

![](<../.gitbook/assets/image (132).png>)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: private-repo-test
  labels:
    name: private-repo-test
spec:
  containers:
  - name: private-repo-test
    image: nginx
    resources:
      limits:
        memory: "64Mi"
        cpu: "0.2"
    ports:
      - containerPort: 80
```



repository는 등록했던 private repository를 선택합니다.

![](<../.gitbook/assets/image (200).png>)



Sync버튼을 클릭하면 private repo에 있는 의도된 상태가 쿠버네티스에 배포됩니다.

![](<../.gitbook/assets/image (158).png>)
