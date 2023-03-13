# Argo CD user

Argo CD 사용자는 local user와 SSO user로 분리됩니다. <mark style="color:red;">Argo CD 사용자의 목적은 사용자별로 Argo CD 권한을 제한</mark>할 수 있습니다. 관리자가 아닌 일반 사용자를 위한 계정을 생성하고 일부 권한만 제한시킬 수 있습니다.



공식문서에 소개된 것처럼, local user는 인증/인가를 제외한 다른 기능을 제공하지 않습니다. 그래서인지 Argo CD문서에서는 사용자의 기능을 강화하고 싶으면,  <mark style="color:red;">SSO를 사용하는 것을 권장</mark>하고 있습니다.

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>



## 참고자료

* [https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management](https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/#overview)
