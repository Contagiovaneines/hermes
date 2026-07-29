# hermes-agent-lab

Repositório de estudos sobre **agentes de IA com tool-calling** (Hermes Agent, da Nous Research), rodando em **WSL2/Kali Linux no Windows 11**, cobrindo dois eixos:

1. **Conceitos e trilhas legítimas de cibersegurança** — como um agente de IA se encaixa em pentest, e como estudar isso de verdade (labs autorizados, certificações).
2. **Um projeto aplicado fora de hacking** — um analisador de TikTok LIVE (vendas/apresentação), mostrando que a mesma base técnica (agente + tool-calling) serve pra qualquer domínio, não só ataque a sistemas.

Ponto de partida: o vídeo [*"Crie seu próprio hacker com IA grátis e ilimitado"*](https://www.youtube.com/watch?v=IxK_awn5xTs), que demonstra o Hermes Agent + Kali Linux como agente de pentest. Este repositório separa o que é **conceito reaproveitável** do que é **uso restrito a ambientes autorizados**.

## Aviso importante

Nada aqui deve ser usado contra sistemas, sites, contas ou transmissões que não sejam suas ou que você não tenha autorização explícita e por escrito para testar. Isso inclui sites de terceiros, redes de terceiros, contas de outras pessoas e qualquer serviço em produção sem escopo formal de teste. Agentes autônomos com acesso a shell são poderosos, e usar isso sem autorização é crime (no Brasil, invasão de dispositivo informático, art. 154-A do Código Penal). Use apenas em laboratórios próprios ou plataformas feitas pra isso: DVWA, OWASP Juice Shop, Metasploitable, HackTheBox, TryHackMe.

## Estrutura pretendida do repositório

Os arquivos abaixo foram entregues **soltos nesta pasta** (limitação da ferramenta usada pra gerar), com o nome já indicando em qual pasta cada um deve entrar. Ao subir pro GitHub, organize assim:

```text
hermes-agent-lab/
├── README.md
├── LICENSE
├── checklist-seguranca.md
├── docs/
│   ├── 01-wsl-e-kali.md
│   ├── 02-agentes-de-ia-tool-calling.md
│   ├── 03-instalar-hermes-agent.md
│   ├── 04-trilhas-legitimas-pentest.md
│   └── 05-casos-de-estudo-labs-autorizados.md
└── projetos/
    └── tiktok-live-analyst/
        ├── README.md
        ├── MANUAL.md
        └── exemplos/
            ├── produtos.csv
            ├── metricas.csv
            └── comentarios.jsonl
```

Tabela de mapeamento (nome do arquivo entregue → destino final):

| Arquivo entregue | Mover para |
|---|---|
| `docs-01-wsl-e-kali.md` | `docs/01-wsl-e-kali.md` |
| `docs-02-agentes-de-ia-tool-calling.md` | `docs/02-agentes-de-ia-tool-calling.md` |
| `docs-03-instalar-hermes-agent.md` | `docs/03-instalar-hermes-agent.md` |
| `docs-04-trilhas-legitimas-pentest.md` | `docs/04-trilhas-legitimas-pentest.md` |
| `docs-05-casos-de-estudo-labs-autorizados.md` | `docs/05-casos-de-estudo-labs-autorizados.md` |
| `projetos-tiktok-live-analyst-README.md` | `projetos/tiktok-live-analyst/README.md` |
| `projetos-tiktok-live-analyst-MANUAL.md` | `projetos/tiktok-live-analyst/MANUAL.md` |
| `projetos-tiktok-live-analyst-exemplos-produtos.csv` | `projetos/tiktok-live-analyst/exemplos/produtos.csv` |
| `projetos-tiktok-live-analyst-exemplos-metricas.csv` | `projetos/tiktok-live-analyst/exemplos/metricas.csv` |
| `projetos-tiktok-live-analyst-exemplos-comentarios.jsonl` | `projetos/tiktok-live-analyst/exemplos/comentarios.jsonl` |

## O que é o Hermes Agent

[Hermes Agent](https://hermes-agent.nousresearch.com/) é um agente de IA open-source da [Nous Research](https://github.com/NousResearch/hermes-agent), com terminal próprio, memória entre sessões e integração com Telegram/Discord/Slack/WhatsApp/Email. Ele não é "um hacker" — é um framework de agente genérico (comparável ao Claude Code ou ao opencode). O que ele faz depende inteiramente de quais *skills* e ferramentas você conecta a ele. É esse ponto que separa os dois usos deste repositório: mesma base técnica, propósitos completamente diferentes.

## Por onde começar

1. Leia `docs/02-agentes-de-ia-tool-calling.md` pra entender o conceito antes de instalar qualquer coisa.
2. Leia `docs/01-wsl-e-kali.md` e prepare o ambiente.
3. Siga `docs/03-instalar-hermes-agent.md` com o `checklist-seguranca.md` ao lado.
4. Escolha um caminho:
   - Cibersegurança → `docs/04-trilhas-legitimas-pentest.md` e `docs/05-casos-de-estudo-labs-autorizados.md`.
   - Análise de conteúdo/vendas → `projetos/tiktok-live-analyst/README.md`.

## Créditos

- Vídeo original: [O hacker cego — CRIE SEU PRÓPRIO HACKER COM IA GRÁTIS E ILIMITADO!](https://www.youtube.com/watch?v=IxK_awn5xTs)
- Manual do projeto TikTok LIVE: escrito pelo autor deste repositório.
- README e docs em `docs/` organizados com apoio do Claude (Anthropic).

## Licença

MIT — veja `LICENSE`. Ajuste conforme preferir antes de publicar.
