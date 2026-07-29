# Projeto: ctf-writeup-assistant

Agente que transforma suas anotações soltas de laboratório (HTB, THM, CTFs) em writeups estruturados — o formato que compõe portfólio e ajuda a fixar o que a OSCP cobra no relatório final.

## Objetivo

Você registra o que fez enquanto resolve a máquina (comandos, achados, raciocínio). O agente organiza isso num writeup com seções padronizadas, sem inventar passos que você não anotou.

## Estrutura de dados

```text
ctf-writeup-assistant/
├── README.md
└── exemplos/
    ├── notas-brutas.md
    └── writeup-template.md
```

## Prompt para o Hermes

```text
Aqui estão minhas notas brutas da máquina "Blue" do HackTheBox
(notas-brutas.md). Gere um writeup seguindo exatamente o
writeup-template.md, usando só o que está nas minhas notas. Se faltar
alguma informação pra preencher uma seção (ex. tempo total gasto),
marque como "[completar]" em vez de inventar.
```

## Regras

- O agente não deve preencher passos de exploração que não estejam nas suas notas — isso vira ficção, não documentação.
- Writeups de máquinas ativas (não "retired") do HackTheBox não podem ser publicados publicamente, por regra da própria plataforma — mantenha privado até a máquina virar "retired".
