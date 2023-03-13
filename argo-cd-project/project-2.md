# Project 설정

{% embed url="https://youtu.be/rx-G89XGObA?t=900" %}

## 설정이란**?**

project 생성은 쉽지만 설정은 매우 많습니다. Project생성은 단순히 그룹이름을 만드는 것이고, Project에 속한 Appliction에 대한 세부내용은 Project 설정에서 합니다.



## Default Project 설정

default project를 설정을 살펴볼게요.  default Project는 모든 설정이 풀려있어 Application을 제한 없이 생성할 수 있습니다.



project목록에서 default를 클릭합니다.

<figure><img src="../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>



핵심 설정만 살펴볼게요. \[Source Repositories]는 applicatoin이 사용한 git repo주소를 제한합니다. 별표(아스타)로 되어 있기 때문에 default는 모든 git repo을 사용할 수 있습니다.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>



\[Destinations]은 sync를 허용할 쿠버네티스 클러스터 목록입니다. default project는 별표(아스타)로 되어 있어서 모든 쿠버네티스 클러스터에 sync할 수 있습니다.

<figure><img src="../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>



\[Cluster Resource Allow List]는 sync할 때 허용할 쿠버네티스 리소스 목록입니다. default project는 별표(아스타)로 되어 있어서 모든 쿠버네티스 리소스를 sync할 수 있습니다.

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>
