# Health Status

## 개념

Health Status는 2가지 의미가 있습니다.

1. git의 의도된 상태가 이상없이 클러스터에 동기화 되었을 때, 쿠버네티스 리소스가 이상없는지 쿠버네티스 상태를 보여줍니다. 예를 들어 pod가 생성되었는데 Crash가 발생하면 Argo CD는 상태가 이상하다고 표시해줍니다.
2. sync status의 상세정보를 보여줍니다.



## Healthy 상태

동기화 한 쿠버네티스 리소스가 이상없는 상태입니다.&#x20;

![](<../.gitbook/assets/image (167).png>)



## Missing 상태

Sync Status가 OutOfSync일 때, 어떤 리소스가 git과 클러스터 상태가 불일치 한지 보여줍니다. 아래 예제에서는 service가 불일치하여 service에만 불일치 아이콘이 보입니다. 다른 아이콘은 Healthy이죠? service가 불일치하여 최종적으로는 Application자체가 불일치로 판단되었습니다.

![](<../.gitbook/assets/image (197).png>)



## Missing 상태 상세정보

Missing 상태를 발견하면 리소스 어떤 부분이 git과 현재 클러스터 상태 차이가 있는지 보고 싶으실겁니다. ArgoCD는 친절하게도 어떻게 차이가 나는지 보여줍니다.



missing 상태인 리소스를 클릭합니다.

![](<../.gitbook/assets/image (91).png>)



상세정보 창에서 \[Diff]탭을 클릭하시면, 현재 클러스터와 git이 어떤 차이가 있는지 볼 수 있습니다. 왼쪽이 현재 클러스터 상태이고 오른쪽이 git에 있는 의도된 상태입니다. 아래 그림 예제에서는 service 리소스가 전혀 없는 상황이고 git에 service리소스가 추가된 상황입니다.

![](<../.gitbook/assets/image (101).png>)



애플리케이션 전체 리소스 차이를 보려면 application을 클릭하고 \[Diff]탭으로 이동하면 됩니다.

![](<../.gitbook/assets/image (106).png>)



![](<../.gitbook/assets/image (126).png>)



## Progressing 상태

Sync작업(git의 의도된 상태를 클러스터로 동기화)이 수행 중이면 Progressing으로 표시됩니다.

![](<../.gitbook/assets/image (129).png>)



## Degraded 상태

Degraded는 Sync작업을 실패했다는 의미입니다. 쿠버네티스 설정 문제(예: serviceaccount 권한 없음, 노드에 스케쥴링 불가 등) 또는 네트워크 등 인프라 문제 등 때문에 동기화를 실패할 수 있습니다. 운영단계에서 가장 주목받는 Health Status입니다.



## Suspended 상태

sync작업이 일시중지된 상태입니다. 다른 작업이 끝나야 수행되거나 외부 이벤트가 필요한 경우 suspended상태로 됩니다.
