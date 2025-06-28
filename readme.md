# Github Testes

Testando agora git no terminal

---

## Ciclo de vida / Status dos arquivos

**Untracked** -> O arquivo foi removido, ou foi adicionado sem ser reconhecido no git

**Unmodified** -> O arquivo reconhecido no git e não modificado, será Unmodified

**Modified** -> O arquivo reconhecido no git foi modificado

**Staged** -> O arquivo está selecionado para estar no commit e depois se tornar unmodified

---

## Adicionar Arquivo

Para que o git reconheça seu arquivo, digite

`git add <Nome do arquivo>`

---

## Commit

Para fazer um commit, digite

`git commit -m "<Mensagem do commit>"`

---

## Logs

Com seus primeiros commits, você pode ver as logs das commits digitando

- `git log`
- `git log --decorate` (Aplica mais detalhes)
- `git log --author="<autor>"` (Filtra pro autores)
- `git shortlog` (Log em ordem alfabética de cada autor)
- `git log --graph` (Gráfico das logs)
**entre outras variantes**

---

## Monitorar commits

Você pode monitorar os commits usando suas hashs

`git show <hash do commit>`

---

## Diferenciar commits

Após suas mudanças no código, você pode diferenciar o último commit com o recém modificado digitando

- `git diff`
- `git diff --name-only` (para saber apenas os arquivos modificados)

---

## Desfazer alterações

Para qualquer imprevisto e reverter uma alteração de um arquivo, digite

`git checkout <nome do arquivo>`

Esse comando vai voltar o estado do seu arquivo para o último commit

Caso você tenha dado um **`git add`**, você pode reverter digitando

`git reset HEAD <nome do arquivo>`

Caso já tenha comittado, você também pode reverter digitando

- `git reset --soft` -> Vai reverter o commit, mas com os arquivos em staged
- `git reset --mixed` -> Vai reverter, voltando os arquivos antes do staged
- `git reset --hard` -> Vai ignorar completamente o commit e destruir todas as alterações

---

## Branches

### 1. O que é um Branch?

Um **branch** (ramo, em português) é uma linha de desenvolvimento independente. É essencialmente um **ponteiro leve e móvel para um commit**.

Quando você inicia um projeto, o Git cria um branch padrão chamado `main` (ou `master` em repositórios mais antigos). Cada vez que você faz um commit, o branch `main` avança, apontando para o seu último trabalho.

Criar um novo branch significa criar um novo ponteiro, que começa apontando para o mesmo commit que o branch atual. A partir daí, os commits feitos nesse novo branch não afetarão o branch original, permitindo que eles "divirjam" e sigam 

---

### 2. Como Funciona na Prática?

O Git não duplica todos os seus arquivos ao criar um branch. Ele é muito mais inteligente e eficiente.

Vamos ver o passo a passo:

1.  **Estado Inicial:** Você tem um branch `main` com alguns commits. O `HEAD` é um ponteiro especial que indica em qual branch você está trabalhando no momento.

    ```
    (HEAD)
      ↓
    [main] → [Commit A] → [Commit B] → [Commit C]
    ```

2.  **Criando um Novo Branch:** Você decide trabalhar em uma nova funcionalidade, como um sistema de login. Você cria um novo branch chamado `feature/login`.

    Comando: `git branch feature/login`

    Agora, você tem um novo ponteiro (`feature/login`) apontando para o mesmo commit que o `main`. O `HEAD` ainda está no `main`.

    ```
      (HEAD)
        ↓
      [main] --------→ [Commit C]
                         ↑
    [feature/login] ---┘
    ```

3.  **Trocando para o Novo Branch:** Para começar a trabalhar na nova funcionalidade, você precisa "mudar" para o novo branch.

    Comando: `git checkout feature/login` (ou o mais moderno `git switch feature/login`)

    O `HEAD` agora aponta para `feature/login`. Seu diretório de trabalho agora reflete o estado deste branch.

    ```
      [main] --------→ [Commit C]
                         ↑
    [feature/login] ---┘
        ↑
      (HEAD)
    ```

4.  **Fazendo Novos Commits:** Você trabalha na funcionalidade de login e faz um novo commit.

    Comando: `git commit -m "Adiciona formulário de login"`

    Apenas o branch `feature/login` avança. O branch `main` permanece intocado, seguro e estável.

    ```
      [main] → [Commit C]
    
                      ↘
                        [feature/login] → [Commit D]
                                            ↑
                                          (HEAD)
    ```

5.  **Unindo o Trabalho (Merge):** Quando a funcionalidade de login estiver pronta e testada, você pode uni-la de volta ao branch `main`.

    Primeiro, volte para o branch `main`: `git switch main`
    Depois, execute o merge: `git merge feature/login`

    O Git criará um "merge commit" que combina as mudanças do `feature/login` com o `main`. Agora, o `main` contém todo o seu novo trabalho.

    ```
      [main] → [Commit C] -----------------→ [Commit E (Merge)]
          \                                    /
           ↘                                  /
            [feature/login] → [Commit D] ---┘
    ```

---

### 3. Por que Usar Branches? (Vantagens)

Usar branches não é apenas uma "boa prática", é a base para um fluxo de trabalho de desenvolvimento moderno e eficiente.

**1. Isolamento e Segurança:**
*   Você pode desenvolver novas funcionalidades, mesmo que grandes e complexas, sem qualquer risco de quebrar o código principal (`main`). A `main` deve sempre representar uma versão estável e, idealmente, "implantável" (deployable) do seu projeto.

**2. Experimentação Sem Medo:**
*   Tem uma ideia maluca ou quer testar uma nova biblioteca? Crie um branch! Se não der certo, você pode simplesmente deletá-lo (`git branch -d nome-do-branch`) sem deixar rastros no projeto principal.

**3. Trabalho em Equipe (Desenvolvimento Paralelo):**
*   Esta é a maior vantagem para equipes. Vários desenvolvedores podem trabalhar em funcionalidades diferentes ao mesmo tempo, cada um em seu próprio branch. Isso evita que um desenvolvedor precise esperar o outro terminar para poder começar seu trabalho.

**4. Organização e Clareza:**
*   O histórico do seu projeto fica muito mais limpo. Em vez de uma única linha de commits com mensagens como "comecei a funcionalidade X" e "corrigi bug na funcionalidade Y", você tem branches dedicados: `feature/user-profile`, `hotfix/login-bug`, `release/v1.1.0`. Fica fácil entender o que aconteceu e quando.

**5. Facilita o Code Review (Revisão de Código):**
*   Branches são a base para **Pull Requests** (ou Merge Requests) em plataformas como GitHub, GitLab e Bitbucket. O fluxo é:
    1.  Você cria um branch para sua tarefa.
    2.  Termina o trabalho e envia (`push`) seu branch para o repositório remoto.
    3.  Abre um Pull Request, que é um pedido formal para "unir meu branch ao branch `main`".
    4.  Outros desenvolvedores podem revisar seu código, deixar comentários e sugerir melhorias *antes* que ele seja integrado à base principal.

**6. Gerenciamento de Versões e Hotfixes:**
*   Imagine que você lançou a versão 1.0 do seu software (que está no branch `main`). Enquanto a equipe já trabalha em novas funcionalidades para a versão 2.0 em seus próprios branches, um bug crítico é descoberto na versão 1.0.
*   Sem branches, seria um pesadelo. Com branches, é simples:
    1.  Crie um branch chamado `hotfix/bug-critico` a partir do `main`.
    2.  Corrija o bug nesse branch.
    3.  Faça o merge do `hotfix` de volta para o `main` e lance a versão 1.0.1.
    4.  Tudo isso sem interferir no desenvolvimento da versão 2.0.

---

### 4. Exemplos de Comandos Essenciais

```bash
# Lista todos os branches locais
git branch

# Cria um novo branch chamado "nova-feature"
git branch nova-feature

# Muda para o branch "nova-feature"
git checkout nova-feature

# Atalho: cria e já muda para um novo branch
git checkout -b outra-feature

# Use `switch` (mais moderno e claro para navegar)
git switch minha-feature

# Atalho com switch: cria e já muda
git switch -c minha-outra-feature

# Depois de terminar o trabalho no branch, volte para o main
git switch main

# Une as mudanças de "outra-feature" no branch atual (main)
git merge outra-feature

# Deleta o branch localmente (após o merge)
git branch -d outra-feature

# Força a deleção de um branch (cuidado!)
git branch -D outra-feature
```
