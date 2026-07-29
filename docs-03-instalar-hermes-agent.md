# 03 — Instalação segura do Hermes Agent

Antes de instalar, revise o `checklist-seguranca.md`. Faça tudo dentro do WSL/VM isolada preparada em `01-wsl-e-kali.md`, nunca na sua máquina Windows principal.

## Regras antes de instalar

- Use um usuário Linux normal — **nunca** `root`/`sudo su` pra rodar o agente.
- Não use flags de execução sem confirmação (`--yolo` ou equivalente).
- Não configure `approvals.mode: off`. Mantenha `Smart` ou `Manual`.
- Não monte o disco inteiro do Windows dentro da VM/WSL.
- Não coloque tokens ou chaves de API em prints/logs.
- Tire um snapshot da VM antes de qualquer alteração importante (se estiver usando VirtualBox).

## Instalar

Com o usuário normal, dentro do Kali/WSL:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
source ~/.bashrc
```

Confira a instalação:

```bash
hermes --help
hermes doctor
```

Se aparecer `command not found`:

```bash
source ~/.bashrc
export PATH="$HOME/.local/bin:$PATH"
hermes --help
```

## Setup completo

```bash
hermes setup
```

Fluxo geral (as telas podem mudar com atualizações do Hermes):

1. selecione `Full Setup`;
2. escolha um provedor de modelo (ex.: OpenCode Zen, OpenRouter, ou sua própria API key);
3. selecione um modelo disponível na sua conta/plano;
4. mantenha a URL base padrão;
5. escolha execução local;
6. mantenha aprovações em `Smart` ou `Manual`;
7. configure integrações (Telegram/Discord/etc.) só depois que o chat básico já estiver funcionando.

### Cuidados com modelos gratuitos

- Disponibilidade e limites gratuitos podem mudar sem aviso.
- Políticas de retenção de dados variam por provedor — não envie dados sensíveis (senhas, dados pessoais de terceiros, informações de clientes) sem checar a política vigente.

## Testar o agente com segurança

```bash
hermes
```

Primeiro teste, só leitura:

```text
Mostre o diretório atual e informe as ferramentas disponíveis.
Não altere, crie ou apague nenhum arquivo.
```

Comandos úteis de diagnóstico:

```bash
hermes doctor
hermes update
hermes model
hermes tools
```

## Integração opcional com Telegram (controle remoto do agente)

1. No Telegram, fale com `@BotFather` (confirme que é o bot oficial), envie `/newbot` e guarde o token — não compartilhe com ninguém.
2. No Kali/WSL:

   ```bash
   hermes gateway setup
   ```

   Selecione Telegram, informe o token, mantenha autorização por pareamento.
3. Inicie o gateway:

   ```bash
   hermes gateway start
   hermes gateway status
   ```
4. Envie uma mensagem pro bot; ele vai gerar um código de pareamento. Aprove:

   ```bash
   hermes pairing approve telegram CODIGO_RECEBIDO
   ```
5. Audite quem tem acesso:

   ```bash
   hermes pairing list
   hermes pairing revoke telegram ID_DO_USUARIO   # se precisar remover alguém
   ```

Regras: nunca aprove pareamento de gente desconhecida, nunca coloque o bot em grupo público, sempre mantenha só o seu próprio ID autorizado.

## Skills externas

Não instale skills de terceiros "no escuro". Antes de instalar:

```bash
hermes skills tap add USUARIO/REPOSITORIO
hermes skills search PALAVRA-CHAVE
hermes skills inspect IDENTIFICADOR_DA_SKILL     # leia o que ela faz antes de instalar
hermes skills install IDENTIFICADOR_DA_SKILL
hermes skills audit                              # revisão periódica do que está instalado
```

## Referências oficiais

- [Documentação do Hermes Agent](https://hermes-agent.nousresearch.com/docs/)
- [Instalação](https://hermes-agent.nousresearch.com/docs/getting-started/installation)
- [Segurança](https://hermes-agent.nousresearch.com/docs/user-guide/security)
- [Skills](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills)
- [Repositório no GitHub](https://github.com/NousResearch/hermes-agent)
