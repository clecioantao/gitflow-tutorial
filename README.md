# Gitflow

# **Guia Completo de GitFlow**

Este guia apresenta o fluxo GitFlow, comandos principais, boas práticas e erros comuns para equipes que utilizam Git em seus projetos. Ele serve como referência para aplicar o modelo de branching GitFlow no dia a dia.

---

## **Fluxo de Trabalho do GitFlow**

O GitFlow organiza o trabalho em diferentes tipos de branches:

1. **main**:
    - Contém o código de produção.
    - Cada versão lançada é marcada com uma **tag** (ex.: `1.0.0`).
2. **develop**:
    - Branch principal para desenvolvimento.
    - Acumula as mudanças antes de serem liberadas.
3. **feature**:
    - Para novas funcionalidades.
    - Criado a partir de `develop` e integrado de volta ao final.
4. **release**:
    - Para preparar uma nova versão.
    - Criado a partir de `develop` e integrado ao `main` e `develop`.
5. **hotfix**:
    - Para corrigir problemas críticos encontrados em produção.
    - Criado a partir de `main` e integrado ao `main` e `develop`.

---

## **Comandos do GitFlow**

### 1. **Criar uma feature**

Use o comando abaixo para iniciar o desenvolvimento de uma nova funcionalidade:

```bash
git flow feature start nome-da-feature
```

Após terminar a funcionalidade, finalize-a:

```bash
git flow feature finish nome-da-feature
```

---

### 2. **Criar uma release**

Use o comando para iniciar a preparação de uma nova versão:

```bash
git flow release start versão
```

Finalize a release e integre-a ao `main` e `develop`:

```bash
git flow release finish versão
```

---

### 3. **Criar um hotfix**

Para corrigir um problema em produção, crie uma branch de hotfix:

```bash
git flow hotfix start versã
```

Finalize o hotfix e integre-o ao `main` e `develop`:

```bash
git flow hotfix finish versão
```

---

### 4. **Comandos úteis do Git**

- Listar branches ativos:
    
    ```bash
    git branch
    ```
    
- Ver tags existentes:
    
    ```bash
    git ta
    ```
    
- Ver histórico de commits:
    
    ```bash
    git log --oneline
    ```
    

---

## **Boas Práticas no Uso do GitFlow**

1. **Mensagens de commit semânticas**:
    - Use o padrão **Conventional Commits**:
        - `feat`: Nova funcionalidade.
        - `fix`: Correção de bug.
        - `chore`: Alterações auxiliares (documentação, scripts).
2. **Versionamento semântico**:
    - Formato: `X.Y.Z`
        - `X`: Mudança de versão principal.
        - `Y`: Adição de funcionalidades.
        - `Z`: Correções de bugs.
3. **Atualização frequente dos branches**:
    - Suba alterações e tags frequentemente:
        
        ```bash
        git push origin --al
        git push origin --tags
        ```
        
4. **Automatize validações**:
    - Use **Git Hooks** para validar tags ou mensagens de commit.
    - Configure pipelines CI/CD para garantir qualidade e consistência.

---

## **Erros Comuns e Soluções**

### 1. **Tag incorreta**

- Apague localmente:
    
    ```bash
    git tag -d nome-da-tag
    ```
    
- Apague no repositório remoto:
    
    ```bash
    git push origin :refs/tags/nome-da-tag
    ```
    

---

### 2. **Merge manual de branches**

- Sempre use os comandos `git flow finish` para concluir branches e evitar inconsistências.

---

### 3. **Branches desatualizados**

- Certifique-se de sempre atualizar os branches remotos:
    
    ```bash
    git pull origin main
    git pull origin develo
    ```
    

---

## **Checklist do GitFlow**

1. **Configuração inicial**:
    - Instale o GitFlow:
        
        ```bash
        
        sudo apt install git-flo
        ```
        
    - Inicialize o fluxo no repositório:
        
        ```bash
        
        git flow init
        ```
        
2. **Uso diário**:
    - Criar e finalizar features:
        
        ```bash
        git flow feature start nome-da-feature
        git flow feature finish nome-da-feature
        ```
        
    - Preparar e finalizar releases:
        
        ```bash
        git flow release start versão
        git flow release finish versão
        ```
        
    - Resolver hotfixes:
        
        ```bash
        git flow hotfix start versão
        git flow hotfix finish versão
        ```
        

---

## **Automatizando o Fluxo**

### 1. **Validação de mensagens de commit**

Crie um hook `commit-msg` para garantir mensagens semânticas:

```bash
#!/bin/sh
COMMIT_MSG_FILE=$1
if ! grep -qE '^(feat|fix|chore): .+' "$COMMIT_MSG_FILE"; then
  echo "A mensagem de commit deve seguir o padrão 'feat: descrição' ou 'fix: descrição'."
  exit 1
fi
```

---

### 2. **Validação de tags**

Use o hook `pre-push` para garantir que as tags seguem o padrão semântico:

```bash
#!/bin/sh
git tag | grep -E '^[0-9]+\.[0-9]+\.[0-9]+$' || {
  echo "Tag inválida. Use o formato X.Y.Z."
  exit 1
}
```

---

### 3. **Pipelines CI/CD**

Configure ferramentas como **GitHub Actions** para validar automaticamente:

- Mensagens de commit.
- Versionamento correto.
- Releases no `main` ou `develop`.