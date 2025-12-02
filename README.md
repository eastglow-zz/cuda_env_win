# 🧪 포터블 CUDA Fortran 연구 환경 (Portable CUDA Fortran Lab)

이 리포지토리는 **CUDA Fortran** 고성능 수치해석을 위한 **Docker 기반의 포터블 개발 환경**입니다.
NVIDIA HPC SDK가 내장된 컨테이너를 사용하여, 호스트 컴퓨터의 설정과 무관하게 어디서든 동일한 연구 환경을 즉시 구축할 수 있습니다.

![Docker](https://img.shields.io/badge/Docker-Enabled-blue?logo=docker)
![NVIDIA](https://img.shields.io/badge/NVIDIA-HPC%20SDK-green?logo=nvidia)
![Fortran](https://img.shields.io/badge/Lang-CUDA%20Fortran-purple)
![Python](https://img.shields.io/badge/Tools-Matplotlib-yellow?logo=python)

## 🌟 주요 특징 (Key Features)

1.  **설치 스트레스 제로**: 로컬 컴퓨터에 CUDA Toolkit, 컴파일러, 라이브러리를 설치할 필요가 없습니다. (그래픽 드라이버만 있으면 OK)
2.  **강력한 하이브리드 워크플로우**:
    * **계산 (Compute)**: `nvfortran`을 사용한 GPU 가속 연산.
    * **시각화 (Visualize)**: `Python (Matplotlib)`을 이용한 고품질 그래프 생성.
    * **빠른 확인 (Quick Check)**: 외부 뷰어 없이 확인 가능한 `PPM` 이미지 자동 생성.
3.  **OS 독립성**: Windows(WSL2)와 Linux 환경에서 100% 동일한 작동을 보장합니다.

---

## 🛠 필수 요구사항 (Prerequisites)

이 환경을 실행하기 위해 호스트 컴퓨터에 갖춰져야 할 조건입니다.
*(주의: Apple Silicon 맥북 등 NVIDIA GPU가 없는 환경에서는 작동하지 않습니다.)*

### 🪟 Windows 사용자 (Windows 10/11)
1.  **NVIDIA 그래픽 드라이버**: [공식 홈페이지](https://www.nvidia.com/download/index.aspx)에서 Studio 또는 Game Ready 드라이버 설치. (*CUDA Toolkit 설치 불필요*)
2.  **WSL2 활성화**: PowerShell(관리자)에서 `wsl --install` 실행 후 재부팅.
3.  **Docker Desktop**:
    * 설치 후 Settings > General > **"Use WSL 2 based engine"** 체크.
    * Settings > Resources > WSL Integration > **"Enable integration with my default WSL distro"** 체크.
4.  **VS Code**: 확장 프로그램 탭에서 **"Dev Containers"** (Microsoft) 설치.

### 🐧 Linux 사용자 (Ubuntu 등)
1.  **NVIDIA 드라이버**: 리눅스용 드라이버 설치.
2.  **Docker Engine**: 도커 설치.
3.  **NVIDIA Container Toolkit**: GPU를 컨테이너로 연결하기 위한 필수 패키지.
    * `sudo apt-get install -y nvidia-container-toolkit`
    * [설치 가이드 참고](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

---

## 📂 프로젝트 구조 (Directory Structure)

```text
.
├── .devcontainer/           # 환경 설정 폴더 (수정 불필요)
│   ├── Dockerfile           # 설치할 프로그램 정의 (nvfortran, python, pip 등)
│   └── devcontainer.json    # VS Code 확장 및 GPU 연결 설정
├── src/                     # 소스 코드 폴더
│   ├── simulation.cuf       # [메인] CUDA Fortran 수치해석 코드
│   └── visualize.py         # [시각화] 결과 데이터를 그래프로 그리는 파이썬 코드
├── Makefile                 # 실행 자동화 스크립트
└── README.md                # 설명서
```

---

## 🚀 시작하기 (Quick Start)

### 1. 환경 구축 (Build Environment)
1.  VS Code에서 이 프로젝트 폴더를 엽니다.
2.  우측 하단 알림창의 **"Reopen in Container"** 버튼을 클릭합니다.
    * *또는 `F1` 키를 누르고 `Dev Containers: Reopen in Container` 선택.*
3.  최초 실행 시 이미지를 다운로드하고 환경을 구성하느라 **약 5~10분**이 소요됩니다.
4.  구성이 완료되면 VS Code 좌측 하단에 `Dev Container: CUDA Fortran Lab` 표시가 뜹니다.

### 2. 시뮬레이션 실행 (Run Simulation)
VS Code 터미널(`Ctrl` + `~`)을 열고 아래 명령어를 입력하세요.

```bash
make
```

이 명령어는 자동으로 다음 과정을 수행합니다:
1.  `nvfortran`으로 소스코드 컴파일 & 빌드.
2.  GPU 시뮬레이션 실행.
3.  결과물 생성:
    * `results.bin`: Python 분석용 바이너리 데이터.
    * `results.ppm`: 즉시 확인 가능한 이미지 파일.
4.  Python 스크립트를 실행하여 `result_plot.png` 그래프 생성.

### 3. 결과 확인
파일 탐색기에서 생성된 `results.ppm` 또는 `result_plot.png`를 클릭하여 결과를 확인하세요.

---

## 📝 환경 커스터마이징 (Customization)

연구에 필요한 라이브러리를 추가하고 싶다면 다음 파일을 수정하세요.

* **리눅스/파이썬 라이브러리 추가**: `.devcontainer/Dockerfile`
    ```dockerfile
    # 예: scipy 라이브러리 추가
    RUN pip3 install scipy
    ```
* **VS Code 확장 프로그램 추가**: `.devcontainer/devcontainer.json`
    ```json
    "extensions": [
        "새로운.확장.아이디"
    ]
    ```

> **중요**: 설정 파일을 수정한 뒤에는 반드시 `F1` > **`Dev Containers: Rebuild Container`**를 실행해야 변경 사항이 적용됩니다.

---

## 📜 라이선스 (License)
MIT License