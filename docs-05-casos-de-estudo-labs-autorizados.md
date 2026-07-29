# 05 — Casos de estudo: agente de IA em laboratórios autorizados

Tudo abaixo assume alvo autorizado: DVWA, Juice Shop e Metasploitable rodando localmente, ou máquinas do seu próprio laboratório em TryHackMe/HackTheBox. **Nunca** aponte esses fluxos pra um site real de terceiros sem autorização por escrito.

## Ideia 1 — Assistente de enumeração

Você roda a enumeração manualmente (é a parte que ensina de verdade), e usa o agente pra organizar o resultado:

```text
Aqui está a saída do nmap contra 10.10.10.5 (minha máquina do laboratório
TryHackMe, sala X). Organize as portas e serviços encontrados em uma tabela,
aponte quais parecem mais interessantes pra investigar primeiro e explique
por quê, sem executar nenhum comando novo.
```

O agente não substitui rodar `nmap`/`gobuster`/`nikto` você mesmo — ele ajuda a não se perder em outputs longos.

## Ideia 2 — Explicar classes de vulnerabilidade encontradas

Depois de identificar algo no Juice Shop ou DVWA:

```text
Encontrei um campo de busca no Juice Shop que reflete o input sem
escapar HTML. Explique qual classe de vulnerabilidade (OWASP Top 10)
isso provavelmente é, como confirmar com segurança nesse ambiente
de laboratório, e como se corrige no código.
```

Isso transforma o agente num tutor que liga o que você encontrou à teoria (OWASP Top 10, CWE), em vez de só cuspir um payload.

## Ideia 3 — Diário de laboratório / rascunho de relatório

Pentest profissional depende de relatório bem escrito. Use o agente pra manter isso organizado desde o início:

```text
Anote no meu diário de laboratório: hoje testei a máquina "Blue" do
HackTheBox. Encontrei SMB exposto na porta 445, versão vulnerável a
MS17-010. Ainda não explorei. Gere uma entrada de diário com data,
alvo, ferramentas usadas e próximos passos.
```

No fim do laboratório, peça um rascunho de relatório estruturado (resumo executivo, achados, severidade, recomendação) — pratique escrever isso é literalmente parte do que a OSCP cobra.

## Ideia 4 — Checklist de metodologia

```text
Monte um checklist de metodologia de pentest web (baseado no OWASP
Testing Guide) que eu possa seguir contra o Juice Shop, com uma
caixa de seleção por item, sem incluir comandos de exploração —
só a lista de verificação.
```

## Linhas vermelhas (não faça, mesmo "só testando")

- Não peça ao agente pra gerar exploits/payloads prontos pra usar contra um alvo real fora de laboratório.
- Não peça pra ele tentar acessar, escanear ou atacar qualquer domínio que não seja seu ambiente de laboratório.
- Não peça pra ele contornar CAPTCHA, rate limit, WAF ou autenticação de um serviço real.
- Não use credenciais reais de terceiros, mesmo "encontradas" durante o teste.
- Se quiser testar um site que não é seu, isso só é legítimo dentro de um programa de bug bounty com escopo explícito (ex.: HackerOne, Bugcrowd) — e mesmo assim, dentro do escopo definido pelo programa.
