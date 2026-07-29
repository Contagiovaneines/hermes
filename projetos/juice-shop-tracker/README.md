# Projeto: juice-shop-tracker

Agente que ajuda a acompanhar seu progresso nos desafios do [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) — incluindo os desafios de bypass descritos em [`docs/06-bypass-em-labs-autorizados.md`](../../docs/06-bypass-em-labs-autorizados.md).

## Objetivo

Você resolve os desafios manualmente (é isso que ensina de verdade); o agente organiza suas anotações, cobra revisão espaçada dos conceitos e monta um relatório de progresso.

## Como rodar sua instância do Juice Shop (local, sem risco)

```bash
docker pull bkimminich/juice-shop
docker run -d -p 3000:3000 bkimminich/juice-shop
```

Acesse `http://localhost:3000` — é sua própria instância, isolada, feita pra ser "atacada".

## Estrutura de dados

```text
juice-shop-tracker/
├── README.md
└── exemplos/
    └── progresso.csv
```

## Prompt para o Hermes

```text
Aqui está meu progresso.csv do Juice Shop. Para cada desafio marcado como
"resolvido sozinho" ou "resolvido com dica", peça que eu explique em uma
frase por que a vulnerabilidade existia. Para os marcados como "não
tentado", sugira por qual categoria eu deveria começar, com base nas
categorias que já resolvi bem. Não me dê a solução de nenhum desafio que
eu ainda não tentei.
```

## Regras

- Nunca peça ao agente pra resolver o desafio por você antes de tentar — isso não gera aprendizado.
- Não aponte esse fluxo para nenhuma outra instância além da sua própria (local ou sua conta pessoal em plataformas como TryHackMe, que hospedam o Juice Shop de forma isolada por usuário).
