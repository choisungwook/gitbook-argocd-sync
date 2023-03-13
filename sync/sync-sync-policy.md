# Sync와 Sync Policy

## 개념&#x20;

Sync는 git에 있는 의도된 상태를 쿠버네티스 클러스터에 배포하는 작업으로 동기화라고 불립니다. sync의 조건은 git과 쿠버네티스 현재 상태와 차이가 있어야 합니다.

![](<../.gitbook/assets/image (156).png>)



## Sync Policy

Sync Policy는 Auto sync와 Manual sync가 있습니다.  <mark style="color:red;">차이점은 누가 sync을 수행하는</mark>가 입니다.



git과 쿠버네티스 현재 상태를 비교해서 차이가 있다면, **Auto Sync는 argocd가 자동으로 sync 수행**합니다. **manual로 되어 있으면 사용자가 수동으로 sync**를 해줘야 합니다. helloworld예제에서는 manual sync으로 설정했기 때문에 저희가 직접 sync버튼을 클릭했었습니다.

![](<../.gitbook/assets/image (103).png>)



<mark style="color:red;">sync Policy는 상황에 따라 적절히 선택하면 됩니다</mark>. 예를 들어 sync전 검토가 필요하다하면 manual sync를 선택할 수 있습니다. 또 다른 예제는 개발은 auto sync, 운영은 manual sync로 선택할 수 있습니다.



## Sync가 필요한지 확인

Application이 sync가 필요한지 쉽게 확인할 수 있습니다. OutOfSync라는 메세지가 보이면 됩니다. 이 메세지는 git과 쿠버네티스 클러스터 상태와 다르다는 의미이므로 sync가 필요하나는 의미입니다. 자세한 내용은 Sync Status글([sync-status.md](sync-status.md "mention"))에서 다룹니다.

![](<../.gitbook/assets/image (139).png>)





## Sync 결과확인

수동 또는 자동으로 sync을 수행하게 되면 **** health status([health-status.md](health-status.md "mention"))로 sync결과를 확인할 수 있습니다. 결과 확인방법은 각각 글에서 자세히 살펴봅니다.
