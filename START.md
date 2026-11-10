# 시험 시작 절차 (작업 AI, 검토 AI, 반증 AI)

폴더 구조 (과제 폴더 이름은 예시)
```
상위 폴더/
├── workspace/   과제 폴더: 작업 AI, 검토 AI
├── orig/        처음 상태 복사본: 읽기만
└── adv/         반증 AI 폴더: 이 안에 work/ 실험 복사본이 생김
```

## 1번 터미널: 작업 AI (과제 폴더)
```bash
cp -r . ../orig                  # 처음 상태 저장. 아무것도 고치기 전에!
git clone --depth 1 https://github.com/snappytak-ship-it/tak /tmp/tak
cp /tmp/tak/AGENTS.md ./AGENTS.md
wc -l AGENTS.md                  # 60이면 정상
pwd                              # 2, 3번 터미널에 알려 줄 경로
codex
```
첫 메시지: `시작`

## 2번 터미널: 검토 AI (같은 과제 폴더, 읽기 전용)
```bash
cd <과제 폴더>
codex --sandbox read-only
```
첫 메시지: `/tmp/tak/AGENTS_review.md를 읽고 지금부터 그 지침대로만 해. 이 폴더의 AGENTS.md는 작업 AI용이니 따르지 마. 처음 상태 복사본은 ../orig야. 읽었으면 역할을 한 줄로 말해 줘.`

## 3번 터미널: 반증 AI (과제 폴더 옆의 adv 폴더)
```bash
cd <과제 폴더>/..
mkdir -p adv
cp /tmp/tak/AGENTS_adversary.md adv/AGENTS.md
cd adv
codex
```
첫 메시지: `원본 작업 폴더는 <1번 터미널의 pwd 경로>, 처음 상태 복사본은 <상위 폴더>/orig야. 지침을 읽었으면 역할을 한 줄로 말해 줘.`

## 주의
1. 상위 폴더 자체에서 codex를 실행하지 않는다. 과제 폴더가 반증 AI의 쓰기 범위에 들어간다.
2. 상위 폴더에 쓰기 권한이 없으면 orig와 adv를 /tmp 아래에 만든다 (`cp -r . /tmp/orig`, `mkdir -p /tmp/adv`).
3. git이 없으면 2순위 `curl -fsSL https://raw.githubusercontent.com/snappytak-ship-it/tak/main/AGENTS.md -o AGENTS.md`
4. PowerShell이면 `/tmp/tak` 대신 `$env:TEMP\tak`, `cp` 대신 `Copy-Item`, `curl` 대신 `curl.exe`를 쓴다.
