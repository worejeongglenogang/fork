

# 🌟✨ **[FORK] Git & GitHub 협업 마스터 노트: 익숙함과 헷갈림 사이** ✨🌟

> ✍️ **2개월간의 경험 정리:** Git을 활용한 개인 작업에는 익숙해졌으나, **협업 시 헷갈리는 부분**을 중심으로 원리 이해와 필수 명령어를 복습합니다. 이 문서는 계속 **`UPDATE`** 될 예정입니다\!

-----

## 🛑 1. Git과 GitHub는 다른 개념이다\! (핵심 구분)

| 🔑 **핵심 키워드** | **Git** (시스템) | **GitHub** (플랫폼) |
| :---: | :---: | :---: |
| **정의** | 소스 코드 관리를 위한 **분산 버전 관리 시스템 (DVCS)** | Git을 바탕으로 하는 **웹 기반 코드 호스팅 플랫폼** |
| **장점** | 💻 오프라인 개발 지원, ⚔️ **Branch & Merge**로 충돌 최소화, 📜 **히스토리 관리** 용이. | 🌐 원격 저장소 역할, 🤝 **Pull Request (PR)** 및 **Issue** 등 협업 도구 제공. |

-----

## 🛠️ 2. Git 기본 개념 및 명령어 (기초 다지기)

### **💾 저장소 (Repository) 및 핵심 개념**

| 용어 | 설명 | 💡 필수 Tip |
| :---: | :--- | :--- |
| **Repository** | 소스 코드를 저장하고 히스토리를 관리하는 공간 (**원격** vs **로컬**) | `$ git remote add [이름] [URL]` (원격 연결) |
| **Staging Area** | `commit`을 위해 변경된 파일을 **준비하는 중간 단계** | `$ git add .` (변경사항 전체 추가) |
| **Commit** | Staging Area의 확정된 변경을 **로컬 저장소에 저장**하는 행위 | `$ git commit -m "feat: 로그인 기능 구현"` |
| **Branch/Head** | 작업물 복사본을 만들어 **새로운 분기점** 생성 (`Head`는 현재 작업 위치) | `$ git checkout -b [브랜치명]` (생성 및 이동) |
| **Push / Pull** | 로컬 $\leftrightarrow$ 원격 간 작업물 **업로드/다운로드** | `$ git push origin [브랜치명]` |

<br>

### **✨ [옵션] 브랜치 작업 상태 한눈에 보기**

```bash
# 현재 생성된 브랜치와 위치한 브랜치 표시
$ git branch

# + 브랜치의 최종 커밋 상태(코드)를 함께 보여줌 (매우 유용!)
$ git branch -v
```

-----

## 🎯 3. 협업의 핵심: `origin`과 `upstream` 저장소

협업 시 헷갈리기 쉬운 원격 저장소를 명확히 구분해야 합니다.

> 🌟 **규칙:** 내 Fork Repository = **`origin`** / 팀의 메인 Repository = **`upstream`**

### 🔗 **원격 저장소 설정 확인**

```bash
# 내 로컬 저장소에 연결된 원격 저장소 목록 확인
$ git remote -v 

# 메인 리포지토리(upstream) 수동 연결
$ git remote add upstream [팀 리포지토리 URL]
```

-----

## 🚀 4. 실전\! 협업을 위한 Git 워크플로우

### **📝 A. 작업 흐름 요약 (7단계)**

1.  팀 Repo $\rightarrow$ **내 Repo (fork)**
2.  내 Repo $\rightarrow$ **로컬 (clone)**
3.  **가상환경/패키지** 설정 및 설치
4.  **새 브랜치 생성** 후 작업
5.  **Add $\rightarrow$ Commit $\rightarrow$ Push** (내 저장소에 저장)
6.  GitHub 페이지에서 **Pull Request (PR)** 요청 (**메인 브랜치 직PR 금지\!**)
7.  Merge 후 **최신 작업물 가져오기 (Sync)**

### **🔧 B. 명령어 필수 세트 (가장 자주 쓰는 것)**

| 단계 | 목적 | 명령어 예시 |
| :---: | :--- | :--- |
| **복제/연결** | 팀 작업물을 로컬로 가져오고 `upstream` 설정 | `$ git clone [URL]` <br> `$ git remote add upstream [URL]` |
| **환경 설정** | 가상환경 설정 및 패키지 설치 | `$ python3 -m venv venv` <br> `$ source ./venv/bin/activate` <br> `$ pip3 install -r requirements.txt` |
| **작업 시작** | 새 브랜치 생성 및 이동 | `$ git checkout -b feature/login` |
| **반영** | 로컬 작업물을 Staging $\rightarrow$ Commit $\rightarrow$ Push | `$ git add .` <br> `$ git commit -m "feat: login UI"` <br> `$ git push origin feature/login` |

### **⭐ C. 핵심\! 최신 작업물 Sync 방법 (`main` 브랜치)**

**Sync는 항상 `main` 브랜치에서 진행하는 것이 원칙입니다\!**

\<details\>
\<summary\> **🔥 1번 방법: Upstream $\rightarrow$ 로컬 $\rightarrow$ Origin으로 작업 (Rebase 선호)** \</summary\>
<br>

1.  **Upstream에서 원본 소스 가져오기 (로컬에만 반영)**
    ```bash
    $ git fetch upstream
    ```
2.  **로컬 main 브랜치 업데이트 (Merge)**
    ```bash
    $ git checkout main 
    $ git merge upstream/main  # Upstream 내용을 로컬 main에 병합
    ```
3.  **내 작업 브랜치 정리 (Rebase)**
    ```bash
    # (선택) Rebase를 통해 log를 깔끔하게 유지 (main 브랜치 최신화)
    $ git rebase main [내_브랜치명] 
    ```
4.  **내 원격 저장소(origin) main도 업데이트**
    ```bash
    $ git push origin main 
    ```

\</details\>

\<details\>
\<summary\> **✨ 2번 방법: Origin에서 가져와 로컬에 적용** \</summary\>
<br>

1.  **내 원격 저장소(origin)가 최신임을 확인하고 로컬로 가져오기**
    ```bash
    $ git pull origin main
    ```
2.  **내 작업 브랜치를 최신 main 브랜치에 Rebase하여 정리**
    ```bash
    $ git rebase main [내_브랜치명]
    ```

\</details\>

### **📢 D. `push` 시 `-u` 옵션의 의미 (Upstream 설정)**

```bash
# -u (== --set-upstream) 옵션: 위계 질서를 정립!
$ git push -u origin main
```

> **🔥 왜 사용해야 할까?**
>
> 이 명령은 `origin`을 `main` 브랜치의 **Upstream으로 설정**합니다. 한 번 설정해두면 다음부터는 간단하게 `$ git push`만 입력해도 해당 브랜치로 푸시할 수 있어 편리합니다. (익숙해지기 전까지는 명시적으로 사용하는 것을 추천\!)
