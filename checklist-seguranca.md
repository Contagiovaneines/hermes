# Checklist de segurança — antes de ligar qualquer agente

Use esta lista antes de rodar o Hermes Agent (ou qualquer agente autônomo com acesso a shell), seja no projeto de pentest em labs, seja no analisador de TikTok LIVE.

## Ambiente

- [ ] Estou usando usuário normal, **não** `root`.
- [ ] A VM/WSL usa rede NAT (não Bridge).
- [ ] Não habilitei modo YOLO / `--yolo`.
- [ ] Aprovações do agente estão em `Smart` ou `Manual` (nunca `off`).
- [ ] Tenho snapshot recente da VM antes de mudanças importantes.
- [ ] Não montei o disco inteiro do Windows dentro da VM/WSL.

## Credenciais e integrações

- [ ] Tokens (Telegram, API keys) não aparecem em prints ou logs.
- [ ] O bot/gateway está limitado ao meu usuário (pareamento, não acesso livre).
- [ ] Não aprovei pareamento de pessoas desconhecidas.
- [ ] O bot não está em grupos públicos.

## Escopo de uso

- [ ] O alvo (site, sistema ou transmissão) é meu ou tenho autorização explícita e por escrito.
- [ ] Não estou testando/coletando dados de terceiros sem consentimento.
- [ ] Não estou tentando contornar CAPTCHA, bloqueio regional ou controle de acesso.
- [ ] Dados pessoais coletados são anonimizados ou não são coletados.
- [ ] Sei diferenciar, no relatório final, o que foi **observado**, **inferido** e **confirmado por métrica real** — nunca apresento suposição como fato.

## Encerramento

- [ ] Buffers temporários (áudio, frames, capturas) têm política de exclusão definida.
- [ ] Skills/plugins de terceiros foram revisados antes de instalar (não instale "cegamente").
