# Self healing

## 개념

sync policy가 auto로 설정된 경우 선택할 수 있는 옵션입니다. argocd로 동기화된 리소스를 항상 git에 있는 의도된 상태로 보장해주는 기능입니다.

![](<../.gitbook/assets/image (181).png>)



## 예제

예를 들어 git의 deployment에 replica가 1이 있다고 가정해봅시다. <mark style="color:red;">argocd로 동기화 한 후에, 긴급 패치로 kubectl로 replica 2로 수정</mark>했습니다. 이 때, Self Healing이 활성화 되어 있으면 <mark style="color:red;">긴급패치 했던 replica의 갯수는 git 설정되어 있는 1로 자동 수정</mark>됩니다.



