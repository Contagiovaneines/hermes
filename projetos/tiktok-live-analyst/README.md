# Projeto: tiktok-live-analyst

Este projeto reaproveita a mesma base técnica do vídeo original (Hermes Agent rodando em Kali/WSL) para um propósito **completamente diferente de hacking**: analisar as próprias transmissões de TikTok LIVE (ou transmissões com autorização) para melhorar apresentação de produto e vendas.

Ele existe justamente para mostrar, na prática, o que `docs/02-agentes-de-ia-tool-calling.md` explica: o "agente hacker" do vídeo é só um framework genérico de IA com tool-calling — o que ele faz depende das ferramentas conectadas a ele. Aqui, as ferramentas são FFmpeg, transcrição de áudio, OCR e leitura de métricas de vendas, não shell ofensivo.

## Conteúdo

- `MANUAL.md` — manual completo, passo a passo, escrito pelo autor deste repositório: preparação do Windows 11, VirtualBox, Kali, instalação segura do Hermes, integração com Telegram, arquitetura do analisador de LIVE, prompts prontos para a skill `tiktok-live-analyst`, plano de implementação em fases e checklist de segurança.
- `exemplos/produtos.csv` — exemplo de catálogo de produtos.
- `exemplos/metricas.csv` — exemplo de métricas de LIVE (visualizadores, cliques, pedidos, faturamento).
- `exemplos/comentarios.jsonl` — exemplo de comentários capturados durante a transmissão.

## Regras de uso (resumo — detalhes no MANUAL.md)

- Analise apenas conteúdo próprio ou explicitamente autorizado.
- Nunca invente pedidos, conversão, faturamento ou comissão — diferencie sempre observado / inferido / confirmado por métrica real.
- Não colete dados pessoais desnecessários de clientes ou espectadores.
- Não tente contornar bloqueios, CAPTCHAs ou controles de acesso do TikTok.
- Preserve sempre as gravações originais (pastas somente leitura).

Leia o `MANUAL.md` completo antes de implementar qualquer parte — ele já cobre ambiente, instalação, arquitetura, prompts e um plano de implementação em 5 fases.
