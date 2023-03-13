# App of Apps 패턴

## App of Apps패턴이란

<mark style="color:red;">ArgoCD application을 모아서 관리하는 패턴을 app of apps패턴</mark>이라고 합니다. app of app패턴으로 구성된 application을 sync하면 여러 argoCD application을 생성합니다. 생성된 application은 바라보는 git에 저장된 쿠버네티스 리소스를 배포합니다.

<figure><img src="../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>



## 언제 사용할까?

<mark style="color:red;">여러 application을 관리할 때 사용</mark>합니다. 대표적인 예가 공식문서에서 소개하는 cluster bootstrapping입니다. 새롭게 구축한 쿠버네티스 클러스터에 필요한 쿠버네티스 리소스를 빠르게 배포하는 것을 cluster bootstrapping이라고 부릅니다.



필요한 쿠버네티스 리소스를 application으로 정의하고 여러 application을 app of apps패턴으로 관리하면, 쉽고 빠르게 쿠버네티스 리소스를 새로운 쿠버네티스 배포할 수 있습니다. 또 다른 장점은 동일한 app of apps패턴을 여러 클러스터에 배포하면 동일한 쿠버네티스 인프라가 구축됩니다.

<figure><img src="../.gitbook/assets/image (176).png" alt=""><figcaption></figcaption></figure>



## 실습

예제로 argoCD 공식예제로 app of app패턴을 실습해보겠습니다. 예제는 argoCD repo([https://github.com/argoproj/argocd-example-apps/tree/master/apps](https://github.com/argoproj/argocd-example-apps/tree/master/apps))에 공개되어 있습니다.



예제는 helm차트로 구성되어 있습니다.

```shell
├── Chart.yaml
├── templates
│   ├── guestbook.yaml
│   ├── helm-dependency.yaml
│   ├── helm-guestbook.yaml
│   └── kustomize-guestbook.yaml
└── values.yaml
```



helm차트 템플릿은 argoCD application으로 되어 있습니다.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: guestbook
  namespace: argocd
  finalizers:
  - resources-finalizer.argocd.argoproj.io
spec:
  destination:
    namespace: argocd
    server: {{ .Values.spec.destination.server }}
  project: default
  source:
    path: guestbook
    repoURL: https://github.com/argoproj/argocd-example-apps
    targetRevision: HEAD
```



helm차트를 argoCD application으로 생성해봅시다. sync policy는 Manual로 설정합니다.

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>



source는 argoCD 공식 예제 repo를 입력하고 path는 apps를 설정합니다.

<figure><img src="../.gitbook/assets/image (145).png" alt=""><figcaption></figcaption></figure>



destination에서는 <mark style="color:red;">namespace를 꼭 argocd로 설정</mark>합니다.

<figure><img src="../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>



application을 생성하고 sync버튼을 클릭합니다.

<figure><img src="../.gitbook/assets/image (175).png" alt=""><figcaption></figcaption></figure>



helm차트 템플릿에 정의된 application이 생성됩니다. 각 application의 sync 옵션이 manual로되어 있기 때문에 out of sync로 되어 있습니다.

<figure><img src="../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>



각 application마다 sync버튼을 눌러보세요. application이 sync될 때마다 쿠버네티스 리소스가 배포됩니다. 쿠버네티스 리소스가 모두 배포되면 모두 healthy상태가 됩니다.

<figure><img src="../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>



application이 배포한 쿠버네티스 리소스를 조회해볼까요? guestbook application은 helm-guestbook에 deployment를 배포합니다.

{% hint style="info" %}
gusetbook git repo: [https://github.com/argoproj/argocd-example-apps/blob/master/guestbook/guestbook-ui-deployment.yaml](https://github.com/argoproj/argocd-example-apps/blob/master/guestbook/guestbook-ui-deployment.yaml)
{% endhint %}

```
kubectl -n helm-guestbook get deployment,pod
```

<figure><img src="../.gitbook/assets/image (168).png" alt=""><figcaption></figcaption></figure>



## 삭제 옵션 cascade

<mark style="color:red;">삭제 시 2가지 옵션</mark>이 있습니다. non cascade와 cascade입니다. 두 개의 차이점은 이전 글([non-cascade.md](../sync/non-cascade.md "mention"))을 참고하시길 바랍니다.



디폴트 옵션인 cascade를 선택하면 app of app application과 모든 application이 삭제됩니다. 하지만, non cascade를 선택하면 app of app application만 삭제되고 다른 application은 유지됩니다.

<figure><img src="../.gitbook/assets/image (180).png" alt=""><figcaption></figcaption></figure>



