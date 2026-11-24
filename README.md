# dotfiles

Cross-platform personal dotfiles and terminal configurations for **macOS** and **Ubuntu Linux**, managed with [chezmoi](https://www.chezmoi.io/).

## Managed Configurations

| Tool | Source Path | Target Path | Notes |
| :--- | :--- | :--- | :--- |
| **Ghostty** | `dot_config/ghostty/config.tmpl` | `~/.config/ghostty/config` | Templated for macOS & Ubuntu |
| **Git** | `dot_gitconfig.tmpl`<br>`dot_gitignore_global` | `~/.gitconfig`<br>`~/.gitignore_global` | Global config and ignore rules |
| **SSH** | `private_dot_ssh/private_config.tmpl` | `~/.ssh/config` | Enforces `0700` dir / `0600` file permissions |
| **act** | `dot_config/act/actrc` | `~/.config/act/actrc` | Local GitHub Actions runner flags |

## Prerequisites

Install `chezmoi` on your target machine:

* **macOS (Homebrew):**
  ```bash
  brew install chezmoi
  ```
* **Ubuntu / Linux:**
  ```bash
  sh -c "$(curl -fsLS get.chezmoi.io)" -- -b ~/.local/bin
  ```

## Quick Setup (New Machine)

1. **Clone this repository:**
   ```bash
   git clone https://github.com/<your-username>/dotfiles.git ~/GitHub/dotfiles
   ```

2. **Point `chezmoi` to the repository:**
   Create `~/.config/chezmoi/chezmoi.toml` with your local `sourceDir`:
   ```bash
   mkdir -p ~/.config/chezmoi
   cat <<EOF> ~/.config/chezmoi/chezmoi.toml
   sourceDir = "$HOME/GitHub/dotfiles"
   EOF
   ```

3. **Preview and apply:**
   ```bash
   chezmoi diff
   chezmoi apply -v
   ```

## Daily Workflow

* **Pull latest changes and apply:**
  ```bash
  git -C "$(chezmoi source-path)" pull && chezmoi apply -v
  ```
* **Edit a managed config:**
  ```bash
  chezmoi edit ~/.config/ghostty/config
  ```
* **Preview template rendering (without writing files):**
  ```bash
  chezmoi execute-template < "$(chezmoi source-path)/dot_config/ghostty/config.tmpl"
  ```
* **Add a new dotfile to the repo:**
  ```bash
  chezmoi add ~/.zshrc
  ```
  *(Use `chezmoi add --template <file>` if the file needs OS-specific conditionals.)*