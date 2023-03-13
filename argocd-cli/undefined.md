# 설치

{% embed url="https://youtu.be/baq7t72JFxM" %}

이 글은 AroGD CD CLI설치 방법을 소개합니다.



## 개념

kubectl처럼 Argo CD를 CLI로 관리할 수 있습니다. WEB UI보다 더 많은 기능을 조작할 수 있습니다.



## 설치

Argo CD CLI를 설치하는 방법은 매우 다양합니다.&#x20;



### WEB UI설치 방법

가장 간단한 방법은 Web UI에서 바이너리를 다운로드 받는 것입니다.

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>



다운로드 받은 파일은 실행권한을 추가해줘야합니다. 그리고 환경변수 PATH로 설정된 디렉터리로 옮겨주면, 아무 경로에서 Argo CD 명령어를 실행할 수 있습니다.

```shell
mv ./argocd... ./argocd
chmod +x ./argocd
sudo mv ./argocd /usr/local/bin
```

### Homebrew 설치 방법(MAC OS only)

MAC OS를 사용하시는 분은 HomeBrew로단하게 설치할 수 있습니다. M1계열 CPU도 지원합니다.

```shell
brew install argocd
```



## 설치 확인

설치가 잘 되었으면, argocd CLI 버전을 확인 명령어가 잘 실행됩니다.

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>
