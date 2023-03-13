# Directory Recurse

## 개념&#x20;

argocd는 path로 선택한 리소스 파일만 배포합니다. 지정한 경로에 폴더가 있다면, 폴더의 리소스는 배포되지 않습니다.



폴더를 포함한 모든 리소스를 배포하고 싶다면 \[DIRECTORY RECURSE] 옵션을 선택해야 합니다.

![](<../.gitbook/assets/image (136).png>)



## 예제

{% hint style="info" %}
예제는 [undefined.md](../argo-cd/undefined.md "mention")를 참고해주세요
{% endhint %}

application을 생성하고 sync버튼을 클릭하여 동기화 합니다.&#x20;

* application 이름: example-5
* git
  * repo: fork한 github 주소
  * branch: main
  * path: example-5
* kubernetes
  * namespace: default



sync결과를 보면 deployment는 생성이 되었지만, service폴더에 있는 service리소스는 배포되지 않았습니다.

![](<../.gitbook/assets/image (196).png>)



\[DIRECTORY RECURSE] 옵션을 활성화 해서 service폴더에 있는 리소스를 동기화해보겠습니다. 왼쪽 위에 있는 \[App Details]버튼을 클릭합니다. 그리고 \[Parameters]탭으로 이동하고 \[DIRECTORY RECURSE]체크박스를 선택합니다.

![](<../.gitbook/assets/image (108).png>)



\[Refresh]버튼을 클릭하여 service폴더에 있는 리소스를 인식하는지 확인합니다.

![](<../.gitbook/assets/image (69).png>)



Refresh결과를 보면 service폴더에 있는 리소스를 argocd가 인식했습니다.

![](<../.gitbook/assets/image (116).png>)



Sync버튼을 클릭하면 service리소스가 생성됩니다.

![](<../.gitbook/assets/image (187).png>)
