# Argo CD Metrics 추가

## Metrics 사용용도

Argo CD Metrics는 Argo CD컴퍼넌트 CPU, Memory사용량과 Application/Project갯수, Sync Status, Health Status 등 운영에 필요한 정보들이 있습니다. Promtheus로 Argo CD Metrics을 수집하고 Grafanad로 시각화합니다.

<mark style="color:red;">운영에서는 Argo CD 컴퍼넌트 사용량 모니터링이 중요</mark>합니다. Cpu, Memory 등 사용량을 잘 체크하여 실시간으로 잘 응대해야 ArgoCD 장애가 발생하지 않습니다.

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption><p>출처: <a href="https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/">https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/</a></p></figcaption></figure>



## Argo CD Metrics 추가

{% hint style="warning" %}
실습을 진행하기 위해 Promtheus Operator설치가 필요합니다. 설치문서([prometheus-operator.md](prometheus-operator.md "mention"))를 참고하시길 바랍니다.
{% endhint %}

공식문서를 참하여 ServiceMonitor를 사용하여 Argo CD Metrics를 생성합니다. ServiceMonitor은 Promtheus Operator CRD입니다. <mark style="color:red;">release를 promtheus-argocd로 수정했고 namespace도 argocd가 설치된 argocd로 설정했습니다.</mark>

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: argocd-metrics
  namespace: argocd
  labels:
    release: prometheus-argocd
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: argocd-metrics
  endpoints:
  - port: metrics
```



```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: argocd-server-metrics
  namespace: argocd
  labels:
    release: prometheus-argocd
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: argocd-server-metrics
  endpoints:
  - port: metrics
```



```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: argocd-repo-server-metrics
  namespace: argocd
  labels:
    release: prometheus-argocd
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: argocd-repo-server
  endpoints:
  - port: metrics
```



```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: argocd-applicationset-controller-metrics
  namespace: argocd
  labels:
    release: prometheus-argocd
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: argocd-applicationset-controller
  endpoints:
  - port: metrics
```

## Promtheus CRD 생성

{% hint style="info" %}
serviceaccount promtheus와 clusterrole, clusterrolebinding생성이 필요합니다. Promtheus Operator 설치 문서([#serviceaccount](prometheus-operator.md#serviceaccount "mention"))를 참고하시길 바랍니다.&#x20;
{% endhint %}

Promtheus CRD를 생성합니다. serviceMontiorNamespaceSelector필드를 <mark style="color:red;">promtheus-argocd라벨로 설정해야 합니다.</mark>

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: prometheus-argocd
  namespace: argocd
spec:
  serviceAccountName: prometheus
  serviceMonitorNamespaceSelector: {}
  serviceMonitorSelector: 
    matchLabels:
      release: prometheus-argocd
  podMonitorSelector: {}
  resources:
    requests:
      memory: 400Mi

```



정상적으로 생성되면 promtheus pod가 실행됩니다. pod가 없다면, serviceaccount가 없을 가능성이 높습니다. 그리고 promtheus에 접속하기 위한 service도 생성됩니다.

```shell
 kubectl -n argocd get Prometheus,pod,svc
```

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption><p>promtheus CRD생성 확인</p></figcaption></figure>

## Promtheus CRD Server 접속방법

port-forward를 이용하여 Promtheus Server로 접속할 수 있습니다.&#x20;

<figure><img src="../.gitbook/assets/image (52).png" alt=""><figcaption><p>port-foward</p></figcaption></figure>



웹 브라우저에서 127.0.0.1:9090으로 접속하면 Promtheus CRD Server 대시보드가 보입니다.

<figure><img src="../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>



## Metrics 확인

상단메뉴 \[Status]를 클릭하고 Targets 메뉴로 이동합니다.

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>



Targets에서는 ServiceMonitor로 설정한 대상이 보입니다. 정상적으로 ServiceMontor가 인식되면 아래 그림과 같이 argocd target이 보입니다.

<figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

metrics은 Argo CD공식문서([https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics](https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/))를 참조하시면 됩니다. Promtheus server에서는 자동완성 기능으로 metrics를 조회할 수 있습니다.

<figure><img src="../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

## Grafana 연동

먼저, 설치문서([#grafana](prometheus-operator.md#grafana "mention"))를 참고하여 Grafana에 접속합니다. 그리고 왼쪽메뉴에서 설정화면으로 이동합니다.

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>



\[Add data source]버튼을 클릭합니다.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>



Promtheus를 선택합니다.

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

Promtheus CRD의 쿠버네티스 Service 주소를 입력합니다.

<figure><img src="../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

그리고 \[Save & test]버튼을 클릭해서 Promtheus와 연결이 되는지 확인합니다. 아래 그림처럼 초록색 메세지가 떠야 연결이 된겁니다.

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

## Grafana 대시보드 생성

대시보드 템플릿은 Argo CD공식문서에서 제공하는 템플릿([https://github.com/argoproj/argo-cd/blob/master/examples/dashboard.json](https://github.com/argoproj/argo-cd/blob/master/examples/dashboard.json))를 사용하겠습니다.



\[Dashboards -> Import]메뉴를 클릭합니다.

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>



Argo CD공식문서에서 제공하는 템플릿([https://github.com/argoproj/argo-cd/blob/master/examples/dashboard.json](https://github.com/argoproj/argo-cd/blob/master/examples/dashboard.json))을 복사한 후 붙여넣기 합니다.

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

대시보드 이름을 입력하고 \[Import]버튼을 클릭합니다.

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

datasource를 Argocd로 선택하면, ArgoCD Metrics데이터가 그라파나에 시각화됩니다.

<figure><img src="../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

## 참고자료

* argocd metrics 공식문서: [https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/](https://argo-cd.readthedocs.io/en/stable/operator-manual/metrics/)
* promtheus operator 튜토리얼 공식문서: [https://blog.container-solutions.com/prometheus-operator-beginners-guide](https://blog.container-solutions.com/prometheus-operator-beginners-guide)
* promtheus operator 사용메뉴얼: [https://jerryljh.medium.com/prometheus-servicemonitor-98ccca35a13e](https://jerryljh.medium.com/prometheus-servicemonitor-98ccca35a13e)
