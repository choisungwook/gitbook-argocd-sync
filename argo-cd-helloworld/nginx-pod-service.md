# nginx pod, service 배포

{% embed url="https://youtu.be/efBlfbCMRfs?t=1" %}

## 생성

왼쪽 위에 있는 \[new APP]버튼을 클릭합니다.

![](<../.gitbook/assets/image (34).png>)



아래 그림과 같이 ①, ②, ③필드를 설정합니다. ①, ②, ③번에 대한 자세한 내용은 다른 글에서 다룹니다.

![](<../.gitbook/assets/image (161).png>)



ArgoCD가 동기화 할 git정보를 선택합니다. 제 github repo의 main브랜치를 사용했습니다. helloworld 예제에 사용할 폴더는 example-1입니다.

* git url:[https://github.com/choisungwook/argocd-practice.git](https://github.com/choisungwook/argocd-practice.git)

![](<../.gitbook/assets/image (165).png>)

![](<../.gitbook/assets/image (177).png>)



그 다음, 어디 쿠버네티스 클러스터에 동기화 할지 클러스터 정보를 입력합니다.

![](<../.gitbook/assets/image (43).png>)

다 입력 후에 오른쪽 위에 있는 \[Create]버튼을 클릭합니다.

![](<../.gitbook/assets/image (57).png>)



## 동기화

helloworld application이 생성되었습니다. 아직 동기화 작업을 하지 않아 노란색으로 표시됩니다. 생성 단계 ③과정에서 수동(Manual)을 선택했기 때문에 저희가 직접 동기화를 해야합니다.

![](<../.gitbook/assets/image (135).png>)



helloworld를 클릭하면 상세페이지로 이동됩니다.

![](<../.gitbook/assets/image (122).png>)



\[sync]버튼을 눌러서 동기화를 진행합니다.

![](<../.gitbook/assets/image (203).png>)



## 동기화 확인

동기화가 완료되면 왼쪽 위에 Healthy와 Synced글씨가 보입니다. 그리고 대시보드에 pod, service리소스가 보입니다.

![](<../.gitbook/assets/image (149).png>)



동기화된 리소스는 kubectl로 확인할 수 있습니다.

```
kubectl get po,svc | grep helloworld
```

![](<../.gitbook/assets/image (99).png>)



## 삭제

동기화 한 리소스를 삭제해보겠습니다.

&#x20;

WEB UI 상단 메뉴에서 \[Delete]버튼을 클릭합니다.

![](<../.gitbook/assets/image (100).png>)



그리고 생성 단계 ①에서 입력했던 application이름을 입력하고 \[OK]버튼을 클릭합니다.

![](<../.gitbook/assets/image (25).png>)
