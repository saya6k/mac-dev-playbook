# Mac Dev Playbook

[![CI][badge-gh-actions]][link-gh-actions]

내 Mac에 필요한 패키지, 앱, dotfiles를 한 번에 설치하는 Ansible playbook.
[geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)을
가져와 `config.yml`로 내 환경에 맞게 덮어썼다.

새 MacBook으로 옮길 때의 전체 절차는 [MIGRATION.md](MIGRATION.md)에 있다.

## 빠른 시작

```bash
xcode-select --install
pip3 install --user ansible
export PATH="$HOME/Library/Python/3.9/bin:$PATH"

git clone https://github.com/saya6k/mac-dev-playbook.git ~/Projects/mac-dev-playbook
cd ~/Projects/mac-dev-playbook
ansible-galaxy install -r requirements.yml
ansible-playbook main.yml --ask-become-pass
```

App Store 앱(카카오톡)을 설치하려면 실행 전에 App Store에 로그인해 둔다.
Homebrew 명령이 실패하면 `brew doctor`로 원인을 확인한다.

## 구성

| 파일 | 내용 |
|---|---|
| `default.config.yml` | upstream 기본값. 직접 고치지 않는다. |
| `config.yml` | 내 설정. Homebrew 패키지·cask·탭, App Store 앱, dotfiles 저장소, git 사용자 정보 |
| `tasks/post/dotfiles.yml` | [saya6k/dotfiles](https://github.com/saya6k/dotfiles)를 클론해 `config/` 파일을 `~`에 심링크로 연결하고 Oh My Zsh를 설치한다 |
| `MIGRATION.md` | 새 Mac 이전 절차, 따로 옮길 파일, 수동 설치 앱 목록 |

upstream 기능 중 아래는 꺼 두었다 (`config.yml`).

- `configure_dotfiles`: upstream 역할은 저장소 루트의 파일만 연결하는데, 내 dotfiles는 `config/` 아래에 있어서 `tasks/post/dotfiles.yml`로 대신한다.
- `configure_osx`, `configure_terminal`: upstream 작성자 개인 설정이다.
- `configure_dock`: Dock은 손으로 정리한다.

### dotfiles 연결

| 저장소 파일 | 연결 위치 |
|---|---|
| `config/.gitconfig` | `~/.gitconfig` |
| `config/.gitignore` | `~/.gitignore` |
| `config/.zshrc` | `~/.zshrc` |
| `config/.zshrc-local.macos` | `~/.zshrc-local` |
| `config/AGENTS.md` | `~/.claude/CLAUDE.md`, `~/.config/opencode/AGENTS.md` |
| `config/opencode.json` | `~/.config/opencode/opencode.json` |

그 자리에 심링크가 아닌 일반 파일이 있으면 `.bak`으로 옮긴 뒤 연결한다.
git 이름과 이메일은 `config.yml`의 값으로 `~/.gitconfig.local`에 쓴다 (이미 있으면 그대로 둔다).

## 패키지 추가·제거

`config.yml`의 목록을 고친 뒤 해당 태그만 다시 실행한다.

```bash
ansible-playbook main.yml -K --tags homebrew   # formula, cask, 탭
ansible-playbook main.yml -K --tags mas        # App Store 앱
ansible-playbook main.yml -K --tags post       # dotfiles
```

App Store 앱 ID는 `mas list` 또는 `mas search <이름>`으로 찾는다.

## upstream 변경 반영

```bash
git fetch upstream
git merge upstream/master
```

## 테스트

`check` 모드로 실제 변경 없이 결과를 미리 볼 수 있다.

```bash
ansible-playbook main.yml -K --check --diff --tags post
```

## 출처

[Jeff Geerling](https://www.jeffgeerling.com/)의
[mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)을 바탕으로 했다.
라이선스는 [LICENSE](LICENSE) (MIT). upstream 작성자의 설치 기록은
[full-mac-setup.md](full-mac-setup.md)에 그대로 두었다.

[badge-gh-actions]: https://github.com/saya6k/mac-dev-playbook/actions/workflows/ci.yml/badge.svg
[link-gh-actions]: https://github.com/saya6k/mac-dev-playbook/actions/workflows/ci.yml
