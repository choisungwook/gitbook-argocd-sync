# Project 설정 예제

{% embed url="https://youtu.be/rx-G89XGObA?t=900" %}

## Project 생성

test이름을 갖는 Project를 생성합니다.

<figure><img src="../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>



Project를 생성하면 Project설정화면으로 자동으로 이동합니다.

<figure><img src="../.gitbook/assets/image (194).png" alt=""><figcaption></figcaption></figure>



## Destination 설정

Destination필드는 Project에 속한 Appliction이 어디 클러스터에 동기화 될 수 있는지를 설정합니다.



\[Destinations]에 별표(아스타)를 입력하여 모든 쿠버네티스 클러스터에 sync하도록 설정합니다.

<figure><img src="../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

## Cluster resource allow list 설정

Cluster resource allow list는 동기화 할 쿠버네티스 namespace와 리소스를 제한합니다.&#x20;



\[Cluster resource allow list]에 모두 별표(아스타)를 입력하여 모든 쿠버네티스 리소스와 모든 namespace에 sync할 수 있도록 설정합니다.

<figure><img src="../.gitbook/assets/image (178).png" alt=""><figcaption></figcaption></figure>



