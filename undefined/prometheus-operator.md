# Prometheus Operator 설치

## 준비

helm이 설치되어 있어야 하고 Dynamic Provisioning 개념과 Promtheus Operator 사용방법을 알고 있어야 합니다.



## (옵션) OpenEBS 설치

{% hint style="warning" %}
Dynamic Provisioning이 활성화 되어 있으면 OpenEBS는 설치하지 않아도 됩니다.
{% endhint %}

<mark style="color:red;">Dynamic Provisioning(동적 불륨 프로비저닝)을 사용</mark>하기 위해 OpenEBS를 설치합니다.

```shell
helm repo add openebs https://openebs.github.io/charts
helm repo update
helm install openebs --namespace openebs openebs/openebs --create-namespace
```



sdf정상적으로 설치되면 OpenEBS storageclass가 설치됩니다.

```shell
kubectl get sc | grep openebs
```

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>



## Promtheus Operator 설치

### Dynamic Provisioning 설정

Persistence Volume의 Storage class를 OpenEBS hostpath를 사용하겠습니다. hostpath의 단점은 데이터가 pod가 실행한 node에만 있으므로 nodeSelector 또는 nodeAffinity를 설정해줘야 합니다.  저는 nodeAffinity를 사용했습니다.



nodeAffinity를 사용하기 위해 노드 한개를 선택하고 node=promtheus이름으로 라벨링을 합니다. 저는 node2이름을 갖는 노드를 라벨링했습니다.

```shell
kubectl label node node2 node=prometheus
```

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### Promtheus Operator 설치

helm repo를 추가합니다.

```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```



values.yaml파일을 생성하고 오버라이딩 할 값을 설정합니다. 데이터 저장기간(retention)과 NodeAffinity를 설정했습니다.

```yaml
prometheus:
  prometheusSpec:
    # tsdb보존 기간
    retention: 10d

    # volume nodeaffinity
    storageSpec:
      volumeClaimTemplate:
        spec:
          storageClassName: openebs-hostpath
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 50Gi
```



promtheus-operator 차트를 설치합니다.

```shell
helm upgrade --install \
-n promtheus-stack --create-namespace \
-f values.yaml \
operator \
prometheus-community/kube-prometheus-stack
```

<figure><img src="../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

### 설치 확인

pod가 전부 Running상태인지 확인합니다.

```shell
kubectl --namespace prometheus-stack get pods -l "release=operator"
```

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>



## serviceaccount 생성

promtheus CRD를 생성하기 위해 serviceaccount와 clusterrole, clusterrolebinding을 생성합니다. <mark style="color:red;">argocd가 설치된 argocd namespace에 serviceaccount를 생성</mark>했습니다.

```
kubectl apply -f serviceaccount_with_role.yaml
```

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: prometheus
  namespace: argocd
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: prometheus
rules:
- apiGroups: [""]
  resources:
  - nodes
  - nodes/metrics
  - services
  - endpoints
  - pods
  verbs: ["get", "list", "watch"]
- apiGroups: [""]
  resources:
  - configmaps
  verbs: ["get"]
- apiGroups:
  - networking.k8s.io
  resources:
  - ingresses
  verbs: ["get", "list", "watch"]
- nonResourceURLs: ["/metrics"]
  verbs: ["get"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: prometheus
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: prometheus
subjects:
- kind: ServiceAccount
  name: prometheus
  namespace: argocd
```



## 부록: Grafana 접속방법

Promtheus Operator stack helm차트를 설치하면 Grafana도 같이 설치됩니다.

```
kubectl -n promtheus-stack get deploy,svc
```

<figure><img src="../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

Grafana에 접속하기 위해 svc를 port-forward하거나 타입을 nodePort로 변경하면 됩니다. 저는 port-forward를 사용했습니다.

```shell
 kubectl -n promtheus-stack port-forward service/operator-grafana 8080:80
```

<figure><img src="../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>



grafana 관리자 계정과 비밀번호는 secret으로 저장됩니다.

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>



jsonpath로 계정과 비밀번호를 조회할 수 있습니다.

```
# 관리자 계정
kubectl -n promtheus-stack get secret operator-grafana -o jsonpath='{.data.admin-user}' | base64 -d; echo

# 관리자 비밀번호
kubectl -n promtheus-stack get secret operator-grafana -o jsonpath='{.data.admin-password}' | base64 -d; echo
```

<figure><img src="../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

## 참고자료

* promtheus operator helm charts: [https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics)
* OpenEBS promtheus install example: [https://openebs.io/docs/stateful-applications/prometheus](https://openebs.io/docs/stateful-applications/prometheus)
* promtheus operator getting-started: [https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/user-guides/getting-started.md](https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/user-guides/getting-started.md)
