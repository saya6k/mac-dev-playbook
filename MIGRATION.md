# 새 MacBook 이전 절차

## 1. 이전 Mac에서 따로 옮길 것 (저장소에 없음)

- `~/.env`: API 키. `.zshrc`가 있으면 불러온다.
- `~/.ssh/`: 키는 AirDrop 등으로 직접 옮긴다.
- 그 밖에 필요한 데이터 (`~/Projects` 등)

## 2. 새 Mac에서 실행

```bash
xcode-select --install
pip3 install --user ansible   # 또는: curl -LsSf https://astral.sh/uv/install.sh | sh && uv tool install ansible-core
export PATH="$HOME/Library/Python/3.9/bin:$PATH"

git clone https://github.com/saya6k/mac-dev-playbook.git ~/Projects/mac-dev-playbook
cd ~/Projects/mac-dev-playbook
ansible-galaxy install -r requirements.yml
```

App Store에 먼저 로그인해야 카카오톡이 설치된다. 그 다음 실행:

```bash
ansible-playbook main.yml --ask-become-pass
```

일부만 다시 돌릴 때: `--tags homebrew`, `--tags mas`, `--tags post`

## 3. 수동 설치 앱

교보eBook, 교보도서관, 서울도서관 전자책 뷰어, Series Comic Viewer, CrossEX,
MACSOOPPackage, REW, SEGGER, DownloadHelper CoApp

## 4. 제외한 것

- npm 전역 패키지: `dkmaker-mcp-rest-api`, `mcp-grocy-api`
- 쓰지 않는 Homebrew 탭 9개
- dotfiles 저장소의 Brewfile, uv 설치 (`curl -LsSf https://astral.sh/uv/install.sh | sh`로 직접 설치)
