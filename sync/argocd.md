# ArgoCD로 관리되는지 확인

ArgoCD로 관리되는 쿠버네티스 리소스는 meatadata.label필드에 argocd라벨이 존재합니다. helloworld예제 실습 후([Broken link](broken-reference "mention"))에 pod라벨을 확인하면, 아래그림처럼 라벨이 있는 것을 확인할 수 있습니다.

<figure><img src="../.gitbook/assets/image (206).png" alt=""><figcaption></figcaption></figure>
