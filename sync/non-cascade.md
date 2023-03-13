# Non Cascade 삭제

{% embed url="https://youtu.be/PxLJMM_8hxs" %}

## 개념

ArgoCD Application을 삭제할 때, <mark style="color:red;">쿠버네티스 리소스는 그대로 두고 Application만 삭제하는 옵션</mark>입니다. 디폴트는 ArgoCD Application을 삭제하면 Application에 연결된 쿠버네티스 리소스 전부 삭제됩니다.

<figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

## 예제

HelloWorld예제([Broken link](broken-reference "mention"))를 보고 ArgoCD Application을 생성합니다.

```shell
kubectl get po,svc | grep argocd
```

<figure><img src="../.gitbook/assets/image (172).png" alt=""><figcaption></figcaption></figure>



Application을 Non Cascade옵션으로 삭제해보겠습니다. Delete버튼을 클릭하면 삭제옵션이 있습니다. 세번째  옵션인 Non Cascade를 선택합니다.

<figure><img src="../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>



Application은 삭제되었지만 쿠버네티스 리소스는 그대로 존재합니다.

<figure><img src="../.gitbook/assets/image (205).png" alt=""><figcaption></figcaption></figure>
