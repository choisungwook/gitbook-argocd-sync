# login

Argo CD CLI 명령어를 사용하려면 login이 필요합니다. 저는 self-singed인증서를 사용하므로 --insecure인자를 사용했습니다.

```
argocd login <argocd server 쿠버네티스 서비스 Cluster IP>:<port> --insecure
```

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>



admin default 비밀번호는 쿠버네티스 secret에 저장되어 있습니다.

```shell
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
