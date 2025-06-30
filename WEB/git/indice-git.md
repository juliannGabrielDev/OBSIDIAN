---
aliases:
  - 🌳 Índice Git
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
---
![[git.svg]]
# 🧭 Git

```shell
git config --global user.name "juliannGabrielDev"
git config --global user.email "julian.alejandro.gabriel@gmail.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main

echo Windows
git config --global core.autocrlf true

echo Linux - MacOS
git config --global core.autocrlf input
git config --global credential.helper /usr/libexec/git-core/git-credential-libsecret

git config --global color.ui auto

git config --list
```

## Flujo de Control

| Nombre                                                    | Palabras Clave                                                                                                                             |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| [[git-commit\|💾 git commit]]                             | `git commit`, `-m`, `-a`, `--amend`.                                                                                                       |
| [[flujos-de-trabajo-en-git\|🌊 Flujos de Trabajo en Git]] | Git Flow, GitHub Flow, GitLab Flow.                                                                                                        |
| [[git-status\|🔍 git status]]                             | `git status`, `--short`, `--branch`, `-s`, `-b`, `--untracked-files=no`.                                                                   |
| [[git-add\|git add ➕📂]]                                  | `git add <archivo>`, `git add <directorio>/`, `git add .`, `git add -A` o `git add --all`, `git add -u`, `git add -p` o `git add --patch`. |
| [[git-push\|git push ✨]]                                  | `git push`, `origin`, `HEAD`, `-u`, `--set-upstream`, `--force`.                                                                           |

## GitHub

| Nombre | Palabras Clave |
| ------ | -------------- |
|        |                |

## Ramas

| Nombre                                                         | Palabras Clave                                                                                                                          |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| [[hoja-de-trucos-ramas\|Hoja de Trucos para Ramas en Git ✨🌿]] | `git branch`, `git checkout`, `git switch`, `git merge`, `git branch -m`, `git branch -d`, `git branch -D`, `git push origin --delete`. |
| [[head\|HEAD en Git]]                                          | `git show HEAD`, `git reflog`.                                                                                                          |

## Comandos Útiles

| Nombre | Palabras Clave |
| ------ | -------------- |
|        |                |
