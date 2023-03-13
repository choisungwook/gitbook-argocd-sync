# helm chart

{% embed url="https://youtu.be/o8DC9TcJjiw" %}

Argo cd에서 helm차트를 release방법을 설명합니다. 이 글은 Argo cd WEB UI를 이용합니다.



## releaes 방법

<mark style="color:red;">Argo cd에서 helm차트 release 방법은 2가지</mark> 입니다. helm release설정은 Argo cd application생성 과정 중 source에서 설정합니다.



Argo cd에서 helm차트를 사용하는 방법은 2가지 입니다.

* helm 차트 저장소 주소를 지정
* git주소 지정



### helm 차트 저장소 주소 지정

첫 번째 방법은 helm차트 저장소 주소를 지정하는 방법입니다.



helm차트 저장소를 선택하려면 ① source타입을 “HELM”을 선택하면 됩니다. 그리고 helm차트 주소를 지정하면 helm차트 목록과 helm차트 버전이 자동으로 출력됩니다.



아래 예제는 “[https://build-deploy-pipeline.github.io/helm-charts/](https://build-deploy-pipeline.github.io/helm-charts/)” helm차트 저장소를 사용하고 fastapi 0.1.0버전 helm차트를 사용했습니다.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>



### git 주소 지정

두 번째 방법은 git에 있는 helm차트 경로를 직접 지정하는 방법입니다. “GIT’타입을 선택 한 후 git 주소 helm차트 경로를 지정하면 됩니다.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>





## helm values override

Argo cd에서도 helm차트 values를 override할 수 있습니다. source에서 helm차트를 지정하고 스크롤을 아래로 내리면 helm values를 설정하는 화면이 나옵니다.



아래 그림처럼 helm차트 경로에 다른 values.yaml을 지정해서 helm values를 override할 수 있습니다.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>



또는 필드 1개씩 입력하여 helm values를 override할 수 있습니다.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>



## Helm 차트 release 원리

<mark style="color:red;">Argo cd는 helm install명령어로 차트를 release하지 않습니다. helm template으로 차트를 렌더링하고 deploy</mark>합니다. 아래 명령어와 동작이 비슷합니다. 그래서 helm ls 명령어로 helm release, helm rollback 등 helm 관리 명령어를 적용할 수 없습니다.

```
helm template | kubectl apply -f
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>참고자료: <a href="https://argo-cd.readthedocs.io/en/stable/faq/#after-deploying-my-helm-application-with-argo-cd-i-cannot-see-it-with-helm-ls-and-other-helm-commands">https://argo-cd.readthedocs.io/en/stable/faq/#after-deploying-my-helm-application-with-argo-cd-i-cannot-see-it-with-helm-ls-and-other-helm-commands</a></p></figcaption></figure>
