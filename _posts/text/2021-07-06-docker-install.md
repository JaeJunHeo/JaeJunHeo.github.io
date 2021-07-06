---

title:  "docker 설치 및 오류 해결"
excerpt: "docker 환경 구축"

categories:
  - 일반
tags:
  - Text
last_modified_at: 2021-07-06

---

## 1. docker 설치

[`docker`](https://www.docker.com/products/docker-desktop) 사이트에 접속해서 자신의 운영체제에 맞는 버전을 설치해줍니다.   
저는 윈도우 버전을 설치했고 WSL2 사용을 체크하고 설치했습니다.

## 2. docker 오류 해결

설치 후 재부팅을 하고 도커를 실행해봤지만 오류로 인해 정상적으로 작동이 되지 않습니다.   
`WslRegisterDistribution failed with error: 0x800701bc ...` 오류가 뜹니다.   
구글링을 통해 해답을 찾았습니다. `Microsoft-Windows-Subsystem-Linux` 옵션, `VirtualMachinePlatform` 옵션과 리눅스 커널 업데이트 패키지를 설치해줍시다.

<div class="highlighter-rouge">
  <div class="highlight">
<pre class="highlight"><code class="plaintext">윈도우 터미널(powershell, cmd 등)에서 다음 명령어를 입력합니다.
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
</code></pre>
  </div>
</div>

이후 다음 링크 [`Linux 커널 업데이트 패키지`](http://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)에서 패키지를 다운받아 설치합니다.

## 3. Windows Terminal 앱 설치 및 윈도우용 Ubuntu 앱 설치

저는 편의성을 위해 Windows Terminal 앱을 설치하고 윈도우 Linux 커널을 사용하기 위해 윈도우용 Ubuntu를 마이크로소프트 스토어에서 설치했습니다.

![docker](https://user-images.githubusercontent.com/76190341/124614965-c2932b80-deaf-11eb-9e2d-2dbd0f4f23d8.PNG)

![docker2](https://user-images.githubusercontent.com/76190341/124618520-d3916c00-deb2-11eb-89c1-3514e4dd6866.png)

![docker3](https://user-images.githubusercontent.com/76190341/124618113-785f7980-deb2-11eb-9fd7-e2edf5e72ea6.PNG)

이후 설치한 Ubuntu 앱을 실행시키면 인스톨이 진행되고 이후 계정을 설정해주면 됩니다.   
해당 과정에서 설정한 계정은 윈도우 계정과 완전히 무관합니다.

![docker4](https://user-images.githubusercontent.com/76190341/124614982-c58e1c00-deaf-11eb-9bdd-36e67941ed31.png)

계정 설정까지 완료되면 이제 Windows Terminal 앱에서 곧바로 우분투에 접근 할 수 있습니다.

![ubuntu](https://user-images.githubusercontent.com/76190341/124619109-51ee0e00-deb3-11eb-9784-17dcb504fb64.png)

## 4. 설치 완료

성공적으로 도커가 실행됐음을 확인 할 수 있습니다.

![docker_install](https://user-images.githubusercontent.com/76190341/124619380-924d8c00-deb3-11eb-883c-e55b0e22a7d6.png)