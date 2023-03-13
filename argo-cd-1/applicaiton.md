# Applicaiton

{% embed url="https://youtu.be/7QD6llN-WPo?t=16" %}

## Application이란?

helloworld 예제 실습과정 기억나신가요? git정보와 쿠버네티스 클러스터를 입력했었습니다. **** <mark style="color:red;">저희가 입력한 정보는 application CRD로 생성되고 argocd가 관리</mark>하게 됩니다.

```shell
 kubectl -n argocd get application
```

![](<../.gitbook/assets/image (55).png>)



argocd는 application이라는 단위로 배포할 리소스를 관리합니다. application은 쿠버네티스 CRD로서 argocd를 설치하면 자동으로 CRD가 생성됩니다.

```shell
$ kubectl get crd | grep application
```

![](<../.gitbook/assets/image (93).png>)

결국, argocd를 관리(또는 사용)한다는 것은 application을 관리하는 것과 같은 의미입니다. application은 어떤 git에 있는 내용을 어떤 쿠버네티스 배포할지를 설정합니다.

![](<../.gitbook/assets/image (40).png>)



## Application 조회

application조회는 kubectl, web UI, argocd CLI로 할 수 있습니다.



kubectl로는 get application으로 application을 조회할 수 있습니다.

```shell
kubectl -n argocd get application
```

![](<../.gitbook/assets/image (64).png>)



WEB UI에서는 왼쪽 첫번째 메뉴입니다. kubectl get application목록과 갯수가 동일합니다.

![](<../.gitbook/assets/image (185).png>)



application에 대한 상세정보는 kubectl describe로 확인할 수 있습니다.

```shell
kubectl -n argocd describe application helloworld | grep Spec -A 10
```

![](<../.gitbook/assets/image (131).png>)



WEB UI에서는 왼쪽 위에 있는 \[APP DEDTAILS]버튼을 클릭하면 됩니다.

![](<../.gitbook/assets/image (192).png>)

## Application 생성

application생성은 kubectl, web UI, argocd CLI로 할 수 있습니다.



WEB UI는 helloworld예제에서 살펴봤듯이 \[NEW APP]버튼으로 할 수 있습니다.

![](<../.gitbook/assets/image (62).png>)



kubectl은 CRD로 application을 정의하고 apply로 생성합니다. CRD가 익숙하지 않으면 WEB UI에 입력칸을 채우고 오른쪽 위에 있는 \[EDIT As YAML]버튼을 클릭하면 CRD를 볼 수 있습니다.

![](<../.gitbook/assets/image (87).png>)

![](<../.gitbook/assets/image (27).png>)



helloworld 예제의 CRD는 아래와 같습니다.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: helloworld
spec:
  destination:
    name: ''
    namespace: ''
    server: 'https://kubernetes.default.svc'
  source:
    path: example-1
    repoURL: 'https://github.com/choisungwook/argocd-practice.git'
    targetRevision: HEAD
  project: default
```

