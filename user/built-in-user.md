# Built-in user

Argo CD를 설치하면 관리자 권한을 갖는 admin 사용자가 있습니다. admin 사용자는 local user유형입니다.



Argo CD CLI([Broken link](broken-reference "mention"))로 admin 사용자를 확인할 수 있습니다.&#x20;

```shell
argocd account list
```

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>



admin 사용자의 권한은 builtin-policy.csv([https://github.com/argoproj/argo-cd/blob/master/assets/builtin-policy.csv](https://github.com/argoproj/argo-cd/blob/master/assets/builtin-policy.csv))로 관리됩니다.

<figure><img src="../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

## 참고자료

* [https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/#basic-built-in-roles](https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/#basic-built-in-roles)
* [https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/](https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/)
* [https://github.com/argoproj/argo-cd/blob/master/assets/builtin-policy.csv](https://github.com/argoproj/argo-cd/blob/master/assets/builtin-policy.csv)
