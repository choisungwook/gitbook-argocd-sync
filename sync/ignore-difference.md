# Ignore Difference

## 개념&#x20;

Ignore Difference는 특정 필드나 리소스를 비교대상에 제외시키는 기능입니다. 에를 들어 replica를 git에 수정해도 argocd에서 replica수정을 무시하게 합니다.



## 사용방법

안타깝게도 아직은 WEB UI에서 Ignore Difference를 제공하지 않습니다. Application CRD 또는 argocd configmap에서 설정할 수 있습니다. 자세한 내용은 argocd diffing문서([https://argo-cd.readthedocs.io/en/stable/user-guide/diffing/](https://argo-cd.readthedocs.io/en/stable/user-guide/diffing/))를 참고하시길 바랍니다.



아래 예제는 Applicatoin CRD정의 입니다.

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: external-secrets-operator
  ...
spec:
  project: default
  source:
    ...
  destination:
    ...
  syncPolicy:
    ...
  ignoreDifferences: <--
    ...
```
