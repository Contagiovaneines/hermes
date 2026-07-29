# Projeto: recon-organizer

Agente que organiza a saída de ferramentas de enumeração (nmap, gobuster, nikto) rodadas **manualmente por você** contra máquinas de laboratório (HTB, TryHackMe, sua própria VM Metasploitable). Ver regras completas em [`docs/05-casos-de-estudo-labs-autorizados.md`](../../docs/05-casos-de-estudo-labs-autorizados.md).

## Objetivo

Você roda a enumeração. O agente:

- organiza portas/serviços em tabela;
- aponta o que parece mais interessante investigar primeiro, com justificativa;
- mantém um histórico por máquina/sala;
- nunca executa comandos de exploração sozinho.

## Estrutura de dados

```text
recon-organizer/
├── README.md
└── exemplos/
    └── nmap-exemplo.txt
```

## Prompt para o Hermes

```text
Aqui está a saída do nmap contra 10.10.10.5 (minha máquina do laboratório
TryHackMe, sala "Basic Pentesting"). Organize portas e serviços em uma
tabela, aponte os 2 ou 3 pontos mais interessantes pra investigar
primeiro e explique por quê. Não sugira exploits específicos — só a
direção de investigação (ex.: "verificar versão do serviço X contra
CVEs conhecidas").
```

## Regras

- Só aponte esse fluxo para IPs de laboratórios autorizados (HTB, THM, sua rede local isolada).
- O agente organiza e explica; a exploração em si você faz manualmente, é isso que ensina.
