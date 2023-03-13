# kustomize

## 개요

이 문서는 Argo CD에서 Kustomize를 사용하는 방법을 설명합니다.



## 사용 방법

<mark style="color:red;">Argo CD는 kustomize 설정 파일이 있으면 kustomize build를 실행</mark>합니다. 그리고 이 결과를 쿠버네티스에에 배포합니다.  정리하면 Argo CD가 kustomize를 실행하는 방법은 아래 명령어와 비슷합니다.

```sh
kustomize build | kubectl apply -f
```



Argo CD 공식 예제 ([https://github.com/argoproj/argocd-example-apps/blob/master/kustomize-guestbook/kustomization.yaml](https://github.com/argoproj/argocd-example-apps/blob/master/kustomize-guestbook/kustomization.yaml))를 사용했습니다. kustomization.yaml파일이 있어 Argo CD는 kustomize build명령어 실행합니다.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



## 플러그인 사용 제약

<mark style="color:red;">kustomize의 기본 기능 이외에 플러그인을 사용하는 경우, 정상적으로 동작하지 않을 수 있습니다.</mark>&#x20;



다음 예제는 helm 플러그인을 사용할 경우 kustomize build 오류가 발생합니다.

{% hint style="info" %}
예제코드: [https://github.com/choisungwook/argocd-practice/blob/main/kustomize-helm/kustomization.yaml](https://github.com/choisungwook/argocd-practice/blob/main/kustomize-helm/kustomization.yaml)
{% endhint %}

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



<mark style="color:red;">플러그인이 올바르게 작동하려면 공식 문서를 참고하여 kustomize build 옵션을 수정</mark>해야 합니다. kustomize build 옵션은 argocd-cm configmap에서 설정할 수 있습니다. 아래 예제는 helm 플러그인을 사용하기 위해 --enable-helm 인자를 build 옵션에 추가한 것입니다.

{% hint style="info" %}
공식문서링크: [https://argo-cd.readthedocs.io/en/stable/user-guide/kustomize/#kustomizing-helm-charts](https://argo-cd.readthedocs.io/en/stable/user-guide/kustomize/#kustomizing-helm-charts)
{% endhint %}

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
data:
  kustomize.buildOptions: --enable-helm
```



configmap을 수정한 경우 argo-server pod를 재부팅해야 합니다.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
