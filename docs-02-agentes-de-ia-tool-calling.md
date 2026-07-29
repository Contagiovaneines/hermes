# 02 — Agentes de IA com tool-calling: o conceito

## O que é um "agente"

Um agente de IA é um LLM que, além de responder texto, pode chamar ferramentas externas (comandos de shell, APIs, buscas na web, leitura/escrita de arquivo) para executar ações e usar o resultado pra decidir o próximo passo, em loop, até concluir uma tarefa.

## Como funciona tecnicamente

1. O modelo recebe uma lista de *tools* disponíveis: nome, descrição e schema de parâmetros.
2. Quando decide usar uma, o modelo retorna uma chamada estruturada, por exemplo:

   ```json
   {"tool": "run_command", "args": {"cmd": "nmap -sV 127.0.0.1"}}
   ```
3. Um orquestrador (o "agente" propriamente dito) executa essa chamada de verdade e devolve o resultado ao modelo.
4. O modelo continua o raciocínio com esse novo dado, decide se chama outra ferramenta ou se já pode responder, e repete até terminar.

É a mesma arquitetura usada em assistentes de código (Claude Code, opencode), agentes de pesquisa e no Hermes Agent. O que muda entre um "agente de programação", um "agente de pentest" e um "agente de análise de vendas" é só **quais ferramentas você conecta a ele e quais permissões ele tem** — o loop de raciocínio é idêntico.

```text
Usuário → Prompt
   ↓
LLM decide: preciso de uma ferramenta?
   ↓ sim
Chama a ferramenta (shell, API, arquivo...)
   ↓
Resultado real volta pro LLM
   ↓
LLM decide: mais uma chamada, ou já respondo?
   ↓ (repete até concluir)
Resposta final
```

## Por que isso é sensível quando a ferramenta é "shell no seu Kali"

Um agente autônomo com acesso a shell dentro de uma distro de pentest pode, em teoria, ser instruído — por você ou por texto malicioso que ele processe (ex.: conteúdo de uma página web, um arquivo, uma mensagem) — a rodar comandos ofensivos contra qualquer alvo acessível pela rede. Por isso o Hermes (como qualquer agente sério) tem controles de aprovação (`Smart`/`Manual`, nunca `off`/`--yolo`) e por isso este repositório insiste em rodar tudo isolado (NAT, snapshot, usuário não-root) e só contra alvos autorizados.

## Nível de permissão como o verdadeiro "produto"

Pensando em termos de projeto, o valor de um agente não está no modelo em si (o mesmo DeepSeek/Claude/GPT por trás pode ser usado pra qualquer coisa), mas em:

- **quais ferramentas** ele pode chamar (shell? navegador? banco de dados? API de terceiros?);
- **com qual escopo** (uma pasta específica? a máquina inteira? a rede?);
- **com qual aprovação** (autônomo total, confirmação manual por ação, só leitura?).

É por isso que o mesmo framework (Hermes Agent) vira, neste repositório, tanto uma base pra estudar pentest em laboratório (`docs/05-casos-de-estudo-labs-autorizados.md`) quanto um analisador de TikTok LIVE (`projetos/tiktok-live-analyst/`) — o "agente hacker" do vídeo original é só esse mesmo framework com skills de shell/rede conectadas e pouca fricção de aprovação.
