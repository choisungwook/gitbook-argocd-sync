# 기존 리소스 연결

## 리소스가 이미 존재한다면?

ArgoCD <mark style="color:red;">Application을 생성 또는 Sync할 때, 이미 쿠버네티스 리소스가 존재하면 어떻게 될까요</mark>? 정답은 기존 리소스가 ArgoCD에 연결됩니다.



## 실습

### pod배포

helloworld([Broken link](broken-reference "mention"))예제에 있는 pod를 수동으로 배포해보겠습니다.



pod.yaml파일을 생성합니다.

```shell
cat <<EOF > pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: argocd-helloworld
  labels:
    name: argocd-helloworld
spec:
  containers:
  - name: argocd-helloworld
    image: nginx
    resources:
      limits:
        memory: "64Mi"
        cpu: "0.2"
    ports:
      - containerPort: 80
EOF
```



그리고 kubectl apply로 배포합니다.

```shell
kubectl apply -f pod.yaml
```



### 라벨 확인

배포된 pod라벨을 확인해볼게요.

```shell
kubectl -n default describe po argocd-helloworld
```



yaml파일에 정의한 것처럼 label이 name필드밖에 없습니다.

<figure><img src="../.gitbook/assets/image (146).png" alt=""><figcaption></figcaption></figure>

### Argocd Application 생성

ArgoCD Application을 생성합니다([Broken link](broken-reference "mention") 참고). 생성할 때 Sync Policy는 Manual로 설정합니다.



argocd-helloworld pod는 이미 생성되어 있기 때문에, ArgoCD Application이 pod를 생성하지 않습니다. 대신 <mark style="color:red;">ArgoCD에서 pod를 관리하기 위해 라벨을 추가</mark>합니다. pod를 선택하고 Diff로 리소스를 비교하면 ArgoCD가 라벨을 추가하려는 것을 알 수 있습니다.

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>



Sync버튼을 눌러 동기화작업을 실행합니다.

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>



<mark style="color:red;">pod라벨을 다시 확인하면 ArgoCD 라벨이 새로 생겼습니다</mark>. 라벨이 있다는 의미는 ArgoCD가 pod를 관리하고 있다는 것입니다.

```shell
kubectl -n default describe po argocd-helloworld
```

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

## 삭제 시 주의사항

ArgoCD Application을 삭제하면 어떻게 될까요? pod는 ArgoCD로 생성하지 않았습니다. 하지만, Application 관리범위에 있으므로<mark style="color:red;">, Application삭제 시 같이 삭제</mark>됩니다.



WEB UI에서 Delete버튼을 클릭하여 ArgoCD Application을 삭제합니다.

<figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>



pod목록을 확인하면 pod가 삭제된 것을 확인할 수 있습니다.

```shell
kubectl -n default get po argocd-helloworld
```

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>
