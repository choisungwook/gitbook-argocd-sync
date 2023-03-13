# Sync Status

## 개념&#x20;

Sync Status git과 쿠버네티스 현재 상태를 비교한 결과를 보여줍니다. 디폴트로 3분마다 또는 사용자가 Refresh을 수행하면 Sync Status가 업데이트 됩니다.



<mark style="color:red;">상태(status)는 Synced, Out Of Sync 2종류</mark>가 있습니다. git과 클러스터 현재 상태가 같으면 Synced, 다르면 Out Of Sync입니다.

![](<../.gitbook/assets/image (73).png>)



## Sync status 확인방법

메인 화면에서는 왼쪽 \[Sync Status]메뉴에서 볼 수 있습니다. Sync상태는 초록색 막대기가 표시되고 OutOfSync은 주황색 막대기로 표시됩니다.

![](<../.gitbook/assets/image (46).png>)



운영할 때는 OutOfSync 상태로 필터링 설정해서 모니터링을 합니다.

![](<../.gitbook/assets/image (186).png>)



application 상세 페이지에서는 현재 Sync 상태와 가장 최근 Sync Status가 있습니다.

![](<../.gitbook/assets/image (144).png>)



\[More]버튼을 클릭하면 각 sync 상태의 상세정보를 볼 수 있습니다.

![](<../.gitbook/assets/image (79).png>)



Sync이력은 상단메뉴 \[Sync Status]버튼을 클릭하면 볼 수 있습니다.

![](<../.gitbook/assets/image (89).png>)



git에 push된 의도된 상태가 올바르지 않다면(예: 쿠버네티스 문법 오류, namespace 없음) Error메세지가 표시됩니다.

![](<../.gitbook/assets/image (133).png>)

