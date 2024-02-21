---
title: Git
---

# Operação básica

Inicializar um repositório localmente:

    git init

Associar um repositório local a um repositório do GitHub (precisa criar um repositório vazio no GitHub antes do primeiro 'push'):

    git remote add origin https://github.com/USERNAME/REPOSITORY-NAME.git

Inicializar um repositório do GitHub:

    git clone https://github.com/USERNAME/REPOSITORY-NAME.git
              ^^^^^ - Importante: Trocar git:// por https:// se estiver atrás de um firewall
    
Verificar situação dos arquivos do repo:

    git status

Adicionar todos os arquivos modificados no stage:

    git add .

Considera arquivos atualizados e excluídos (mas não adicionados):

    git add . -u

Adiciona arquivos interativamente

    git add -i
    
Adicionar pedaços (hunks) de um arquivo interativamente


    git add -p
    
Remover os arquivos do stage:

    git reset

Commit das alterações:

    git commit -m "Comentário do commit"

Mostra o que será feito:

    git commit --dry-run 
    
Enviar alterações para repositório no GitHub:

    git push -u origin master  (primeira vez, para gravar os parâmetros)

    git push  (próximas vezes)

Obter alterações no repositório do GitHub:

    git pull -u origin master  (primeira vez, para gravar os parâmetros)

    git pull  (próximas vezes)

Listar histórico de commits:

    git log --oneline --all

"Visitar" um branch específico

    git checkout ...
    
Criar um branch e mudar para ele ao mesmo tempo

    git checkout -b novo_branch

Fazer merge do branch novo_branch sem FF (isto é, irá criar um commit merge e manter o "grafo")

    git checkout master

    git merge novo_branch --no-ff
    
Modificar/limpar histórico de commits (interativamente):

    git rebase -i


# Configuração

Configurar proxy:


    git config --global http.proxy http://USUARIO:SENHA@servidor.proxy:8080

    git config --global https.proxy https://USUARIO:SENHA@servidor.proxy:8080

# Bash

## Rename file extension on multiple files

Syntax:

    find PATHTOFILES -name '*.OLDEXTENSION' -exec sh -c 'git mv "$0" "${0%.OLDEXTENSION}.NEWEXTENSION"' {} \;

Example (renaming javascript source files to typescript)

    find src -name '*.js' -exec sh -c 'git mv "$0" "${0%.js}.ts"' {} \;


# Gitlab

Generate SSH key:

`ssh-keygen -t ed25519 -C "user@xxxxx.com.br"`

Copy to clipboard:

`cat ~/.ssh/id_ed25519.pub | clip`

Test connection:

`ssh -T git@gitlab.com`


# Git Flow

Branches

```
develop -> feature
release -> fix
master -> release
```

Example:

`feature/validation`

Start working on a feature:

```
git checkout develop
git pull
git checkout -b feature/<NAME>
```

Development cycle:

```
git add <FILES>
git commit -m "something: what was done"
git push
```

Finish working on feature:

```
git checkout develop
git pull
git checkout feature/<NAME>
git rebase -i --autosquash origin/develop
git push -uf origin feature/<NAME>
```

## NPM registry

```
npm set registry http://npm.xxxxx.com.br
npm adduser --registry http://npm.xxxxx.com.br
```

## Github API

[Search users](https://docs.github.com/en/rest/reference/search#search-users--code-samples)

    https://api.github.com/search/users?q=QUERYSTRING

[Repositories for user](https://docs.github.com/en/rest/reference/repos#list-repositories-for-a-user)

    https://api.github.com/users/USERNAME/repos

## Windows Shortcuts

Ferramenta não-documentada para criação de atalhos do Windows:

    "C:\Program Files\Git\mingw64\bin\create-shortcut.exe" --work-dir "C:\path\to\files" --arguments "--myarg=myval" "C:\path\to\files\file.ext" "C:\path\to\shortcuts\shortcut.lnk"

## Useful snippets

    git log --all --decorate --oneline --graph
    git log --oneline --stat --reverse
    git log -p --stat --reverse
