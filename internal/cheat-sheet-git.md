# Cheat Sheet Git: O Fluxo de Contribuição Open Source

## 0. Mapa Mental dos Remotes
- **`upstream`**: O repositório oficial do SuperTuxKart (de onde você baixa novidades).
- **`origin`**: O seu Fork no seu GitHub pessoal (para onde você envia o seu código).
- **`master`**: O trunk oficial estável para onde todos os PRs convergem.

---

## 1. Ritual de Início - Sincronizando com o Oficial (`upstream`)

Antes de começar a codar no dia, traga as novidades que outros devs comitaram na `master` oficial:

```bash
# Baixa as novidades do repositório oficial
git fetch upstream

# Reaplica os seus commits locais em cima da master oficial mais recente
git rebase upstream/master
```

---

## 2. Checkpoint - Salvando seu Progresso Localmente

```bash
# Vê o que foi alterado e em qual branch você está
git status

# Adiciona os arquivos modificados
git add src/config/player_profile.cpp

# Salva na sua máquina com uma mensagem clara em inglês
git commit -m "feat: add support for custom profile icon"
```

---

## 3. Upload - Enviando para o seu GitHub (`origin`)

```bash
# No primeiro envio de uma branch nova (ou se o lazygit acusar divergência de tracking):
# O '-u' vincula sua branch local diretamente ao seu fork no GitHub:
git push -u origin nome-da-sua-branch

# Nos envios normais do dia a dia:
git push origin nome-da-sua-branch

# ⚠️ SE VOCÊ FEZ REBASE (Passo 1):
# Como o histórico local foi reescrito sobre a master oficial, o Git recusa push normal.
# Nesse caso, force a atualização no SEU fork:
git push -f origin nome-da-sua-branch
```

---

## 4. Panic Button - Resolvendo Conflitos do Rebase

Se no Passo 1 o Git avisar de conflito (alguém mexeu na mesma linha que você), o processo pausa temporariamente:

```bash
# 1. Abra o arquivo no VSCode, escolha as linhas certas (aceitar atual/incoming) e salve.

# 2. Avise o Git que o conflito daquele arquivo foi resolvido:
git add arquivo-resolvido.cpp

# 3. Mande o Git continuar o processo (NÃO use 'git commit' aqui):
git rebase --continue
```

*(Se der ruim total e você quiser cancelar o rebase e voltar exatamente a como estava antes: `git rebase --abort`)*
