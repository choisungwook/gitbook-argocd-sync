# Prune

## 개념&#x20;

prune은 동기화된 리소스 삭제옵션입니다. argocd로 쿠버네티스 리소스를 동기화하고 <mark style="color:red;">git에서 리소스를 삭제할 때, 해당 리소스를 쿠버네티스에서 삭제할지 유지할지 결정하는 옵션</mark>입니다.



prune이 비활성화되어 있으면 argocd로 동기화한 쿠버네티스 리소스는, git에 리소스가 삭제되더라도 쿠버네티스에 삭제되지 않고 유지됩니다. 반대로 prune이 활성화되면 git에 삭제되면 쿠버네티스 리소스도 삭제됩니다.



## Prune 활성화

prune옵션은 default로 비활성화 되어 있습니다. applicatino생성 때 Prune체크박스를 선택해야 활성화 됩니다.

![](<../.gitbook/assets/image (198).png>)



Sync Policy가 Auto이면 \[Prune Resources]로 설정할 수 있습니다.

![](<../.gitbook/assets/image (68).png>)



Prune이 비활성화 되어 있으면 리소스는 삭제되지 않지만 X표시 아이콘이 보입니다.

![](<../.gitbook/assets/image (13).png>)

![](<../.gitbook/assets/image (204).png>)



## 운영 Tip

prune이 비활성화 되어 있으면 OutOfSync Health Status상태가 늘어납니다. git에 삭제되었지만 쿠버네티스 클러스터에는 있기 때문이죠.

![](<../.gitbook/assets/image (193).png>)
