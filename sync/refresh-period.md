# Refresh Period

## 개념

argocd는 default 설정으로 <mark style="color:red;">3분(180초)마다 git과 현재상태</mark>를 비교합니다. 이 주기를 Period 시간(time)이라고 합니다.



3분마다 차이를 비교하는 것은 운영단계에서 매우 중요합니다. <mark style="color:red;">git의 업데이트 내용이 바로 반영이 안되는 상황이 발생</mark>합니다. 최악은 3분을 대기해야 합니다.

![](<../.gitbook/assets/image (18).png>)



## period 시간 수정&#x20;

helm으로는 values.yaml을 오버라이딩해서 수정할 수 있고 yaml파일로 배포했다면 Argo Controller deployment(또는 statefulset) command을 수정해야 합니다.

> helm values.yaml 링크: [https://github.com/muwon/argo-helm/blob/master/charts/argo-cd/values.yaml#L38](https://github.com/muwon/argo-helm/blob/master/charts/argo-cd/values.yaml#L38)

![](<../.gitbook/assets/image (74).png>)
