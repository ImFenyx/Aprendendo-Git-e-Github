# Github Testes

Testando agora git no terminal

---

## Ciclo de vida / Status dos arquivos

**Untracked** -> O arquivo foi removido, ou foi adicionado sem ser reconhecido no git

**Unmodified** -> O arquivo reconhecido no git e não modificado, será Unmodified

**Modified** -> O arquivo reconhecido no git foi modificado

**Staged** -> O arquivo está selecionado para estar no commit e depois se tornar unmodified

---

Para que o git reconheça seu arquivo, digite

`git add <Nome do arquivo>`

---

Para fazer um commit, digite

`git commit -m "<Mensagem do commit>"`

---

Com seus primeiros commits, você pode ver as logs das commits digitando

- `git log`
- `git log --decorate` (Aplica mais detalhes)
- `git log --author="<autor>"` (Filtra pro autores)
- `git shortlog` (Log em ordem alfabética de cada autor)
- `git log --graph` (Gráfico das logs)
**entre outras variantes**

---

Você pode monitorar os commits usando suas hashs

`git show <hash do commit>`

---

Após suas mudanças no código, você pode diferenciar o último commit com o recém modificado digitando

- `git diff`
- `git diff --name-only` (para saber apenas os arquivos modificados)
