# Manual completo — Hermes Agent no Windows 11 para estudar TikTok LIVE

> Guia de instalação segura, configuração, integração com Telegram e criação de um analisador de vendas para TikTok LIVE.
>
> Atualizado em: 29/07/2026

## 1. Objetivo deste manual

Este manual ensina, desde o começo, como:

1. preparar o Windows 11;
2. instalar o VirtualBox;
3. executar o Kali Linux em uma máquina virtual;
4. instalar o Hermes Agent sem usar a conta `root`;
5. configurar o OpenCode Zen e um modelo de IA;
6. integrar o Hermes com um bot do Telegram;
7. entender com segurança a demonstração do vídeo sobre o Hermes;
8. analisar gravações de TikTok LIVE;
9. acompanhar uma LIVE quase em tempo real sem guardar o vídeo completo;
10. analisar produtos, falas, comentários, audiência e métricas de vendas;
11. descobrir quais padrões realmente ajudam uma LIVE a vender;
12. criar a futura skill `tiktok-live-analyst`.

O vídeo estudado foi:

- [Crie seu próprio agente de IA hacker gratuito — Hermes Agent + Kali Linux](https://www.youtube.com/watch?v=IxK_awn5xTs)

O vídeo demonstra o Hermes como agente de testes de segurança. Neste projeto, aproveitaremos a mesma base técnica para uma finalidade diferente: estudar TikTok LIVE e melhorar transmissões de vendas.

---

## 2. O que o vídeo do Hermes ensina

O fluxo demonstrado no vídeo é:

1. instalar o Hermes no Kali Linux pelo WSL2;
2. escolher o `Full Setup`;
3. configurar o OpenCode Zen;
4. selecionar o DeepSeek V4 Flash Free;
5. usar o terminal local;
6. criar um bot no Telegram;
7. parear o usuário autorizado;
8. executar o gateway em segundo plano;
9. criar uma skill de pentest;
10. importar skills de cibersegurança;
11. testar o agente em um laboratório autorizado.

### 2.1 Correções importantes sobre o vídeo

- O DeepSeek V4 Flash Free não é garantidamente gratuito e ilimitado para sempre. A gratuidade é temporária e o provedor pode aplicar limites.
- Modelos gratuitos podem ter políticas diferentes de retenção de dados. Não envie informações confidenciais de clientes, pedidos ou faturamento sem conferir a política vigente.
- O projeto `Anthropic-Cybersecurity-Skills` é comunitário. Ele não é um produto oficial da empresa Anthropic.
- Executar o Hermes como `root` aumenta muito o risco.
- Escrever em um prompt que "todos os alvos estão autorizados" não cria autorização real.
- Se o gateway estiver instalado como serviço, `screen` e `tmux` normalmente não são necessários.
- O bot do Telegram deve ficar protegido por pareamento ou lista de usuários permitidos.
- O DeepSeek usado como modelo principal não "assiste" diretamente a um vídeo longo. Precisamos separar áudio, frames, comentários e métricas.

### 2.2 Referências

- [Instalação oficial do Hermes](https://hermes-agent.nousresearch.com/docs/getting-started/installation)
- [Segurança oficial do Hermes](https://hermes-agent.nousresearch.com/docs/user-guide/security)
- [Documentação do OpenCode Zen](https://opencode.ai/docs/zen/)
- [Projeto comunitário Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills)

---

# Parte I — Preparação do Windows 11

## 3. Por que usar uma máquina virtual

No Windows 11, há três caminhos possíveis:

| Caminho | Vantagem | Desvantagem |
|---|---|---|
| Instalação nativa | Mais simples | O agente acessa diretamente o Windows |
| WSL2 | Bom desempenho e integração | Pode acessar arquivos montados do Windows |
| Máquina virtual | Melhor isolamento | Usa mais memória e processador |

Para este projeto, a máquina virtual é a opção recomendada.

O Kali, o Hermes e os componentes de análise ficam separados do Windows. Se algum comando quebrar o ambiente, é possível restaurar um snapshot.

Uma máquina virtual não elimina todos os riscos. Ela reduz o alcance do agente, principalmente se:

- a rede permanecer em modo `NAT`;
- não houver pastas compartilhadas com permissão de escrita;
- o agente não for executado como `root`;
- as aprovações perigosas continuarem ativadas.

## 4. Requisitos recomendados

### 4.1 Configuração mínima

- Windows 11 64 bits;
- virtualização Intel VT-x ou AMD-V;
- 8 GB de RAM no computador;
- aproximadamente 80 GB livres;
- conexão estável com a internet.

### 4.2 Configuração confortável

- 16 GB ou mais de RAM;
- processador com pelo menos 6 núcleos;
- SSD;
- 100 GB livres;
- placa de vídeo NVIDIA, opcional para transcrição local mais rápida.

### 4.3 Recursos da máquina virtual

| Memória do computador | RAM para a VM | Processadores para a VM |
|---:|---:|---:|
| 8 GB | 3 GB | 2 |
| 16 GB | 6 GB | 4 |
| 32 GB | 8 a 12 GB | 4 a 6 |

Evite entregar toda a memória ou todos os processadores à VM. O Windows também precisa continuar funcionando.

---

## 5. Conferir se a virtualização está habilitada

1. Pressione `Ctrl + Shift + Esc`.
2. Abra o **Gerenciador de Tarefas**.
3. Entre em **Desempenho**.
4. Selecione **CPU**.
5. Procure a informação **Virtualização**.

O resultado esperado é:

```text
Virtualização: Habilitada
```

Se aparecer desabilitada, será necessário habilitar Intel VT-x, Intel Virtualization Technology, SVM Mode ou AMD-V na BIOS/UEFI.

Não altere outras opções da BIOS sem saber a finalidade.

---

## 6. Instalar o VirtualBox

1. Acesse [Downloads do VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Baixe a versão para **Windows hosts**.
3. Execute o instalador.
4. Mantenha as opções padrão.
5. Autorize a instalação dos adaptadores de rede.
6. Reinicie o Windows se o instalador solicitar.

Caso o VirtualBox apresente erro relacionado a Hyper-V, VT-x ou AMD-V, anote a mensagem completa antes de alterar recursos de segurança do Windows.

---

# Parte II — Kali Linux na máquina virtual

## 7. Baixar a imagem oficial do Kali

Use a máquina virtual pronta fornecida pelo projeto Kali:

1. Acesse [Download oficial do Kali Linux](https://www.kali.org/get-kali/).
2. Procure **Pre-built Virtual Machines**.
3. Selecione **VirtualBox**.
4. Baixe a imagem de 64 bits.
5. Baixe e instale o 7-Zip, caso ainda não tenha.
6. Extraia o arquivo `.7z`.

Evite baixar imagens do Kali de sites não oficiais.

## 8. Importar a máquina virtual

1. Abra o VirtualBox.
2. Clique em **Máquina → Adicionar**.
3. Entre na pasta extraída.
4. Selecione o arquivo com extensão `.vbox`.
5. Confirme a adição.

Referência:

- [Importar uma máquina virtual pronta do Kali no VirtualBox](https://www.kali.org/docs/virtualization/import-premade-virtualbox/)

### 8.1 Login inicial

As imagens prontas normalmente usam:

```text
Usuário: kali
Senha: kali
```

Altere a senha imediatamente:

```bash
passwd
```

---

## 9. Configurar a VM com segurança

Com a máquina virtual desligada, abra **Configurações**.

### 9.1 Sistema

- RAM: conforme a tabela deste manual;
- processadores: conforme a tabela;
- mantenha a ordem de inicialização padrão;
- não ultrapasse a faixa verde indicada pelo VirtualBox.

### 9.2 Rede

Use:

```text
Adaptador 1: NAT
```

Não utilize **Bridge/Ponte** inicialmente. O modo Bridge coloca a VM diretamente na mesma rede dos computadores, celulares, câmeras e outros aparelhos.

### 9.3 Integrações com o Windows

Inicialmente, deixe:

```text
Área de transferência compartilhada: Desativada
Arrastar e soltar: Desativado
Pastas compartilhadas: Nenhuma
```

Posteriormente, para analisar gravações próprias, poderá ser criada uma pasta compartilhada somente leitura.

### 9.4 Snapshot inicial

Depois de iniciar a VM, alterar a senha e atualizar o Kali, crie:

```text
Nome do snapshot: 01-Kali-Limpo
```

Caminho:

```text
VirtualBox → Máquina → Tirar snapshot
```

---

## 10. Atualizar e preparar o Kali

Abra o terminal:

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt install -y curl git ca-certificates ffmpeg python3 python3-venv
sudo reboot
```

Depois do reinício, confira:

```bash
python3 --version
git --version
ffmpeg -version
```

---

# Parte III — Instalação segura do Hermes

## 11. Regras antes da instalação

Para este projeto:

- use o usuário normal `kali`;
- não execute `sudo su`;
- não use `hermes --yolo`;
- não configure `approvals.mode: off`;
- não monte o disco inteiro do Windows dentro do Kali;
- não informe tokens ou chaves em prints;
- tire snapshots antes de alterações importantes.

---

## 12. Instalar o Hermes

Com o usuário normal:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
source ~/.bashrc
```

Confira:

```bash
hermes --help
hermes doctor
```

Se aparecer `command not found`, tente:

```bash
source ~/.bashrc
export PATH="$HOME/.local/bin:$PATH"
hermes --help
```

---

## 13. Fazer o Full Setup

Execute:

```bash
hermes setup
```

As telas podem mudar com atualizações, mas o fluxo geral é:

1. selecione `Full Setup`;
2. escolha `OpenCode`;
3. escolha `OpenCode Zen`;
4. informe sua API key;
5. selecione `DeepSeek V4 Flash Free`, se ainda estiver disponível;
6. mantenha a URL base padrão;
7. escolha execução local;
8. mantenha aprovações em `Smart` ou `Manual`;
9. configure Telegram depois que o chat básico funcionar;
10. configure visão, navegador e transcrição apenas conforme a necessidade.

### 13.1 Cuidados com o modelo gratuito

- A disponibilidade gratuita pode terminar.
- Pode haver limite diário ou mensal.
- O contexto da opção gratuita pode ser menor que o limite nativo do modelo.
- Dados enviados a modelos gratuitos podem ser utilizados para melhoria do serviço.
- Não envie planilhas com nomes, endereços, telefones ou outros dados pessoais de clientes.

Para métricas sensíveis, considere:

- remover dados pessoais;
- enviar apenas valores agregados;
- usar um provedor com política apropriada;
- utilizar um modelo local quando possível.

---

## 14. Testar o Hermes

Inicie:

```bash
hermes
```

Primeiro teste:

```text
Mostre o diretório atual e informe as ferramentas disponíveis.
Não altere, crie ou apague nenhum arquivo.
```

Depois:

```text
Crie um plano de análise para uma gravação de TikTok LIVE,
mas não execute nenhum comando ainda.
```

Comandos úteis:

```bash
hermes doctor
hermes update
hermes model
hermes tools
```

---

## 15. Configurar visão e áudio

O modelo de texto não processa sozinho uma LIVE inteira.

O analisador utiliza:

- FFmpeg para separar áudio e imagens;
- transcrição para transformar fala em texto;
- um modelo com visão para interpretar frames;
- OCR para ler nomes, preços e descontos;
- Hermes para relacionar todos os resultados.

Abra:

```bash
hermes tools
```

Configure um backend de visão disponível na sua conta. Confira custos e privacidade antes de utilizar.

Referências:

- [Visão e imagens no Hermes](https://hermes-agent.nousresearch.com/docs/user-guide/features/vision)
- [Ferramentas do Hermes](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools)

---

# Parte IV — Bot do Telegram

## 16. Criar o bot

No Telegram:

1. procure `@BotFather`;
2. confirme que é o bot oficial;
3. envie `/newbot`;
4. escolha o nome de exibição;
5. crie um usuário que termine com `bot`;
6. copie o token recebido;
7. não envie esse token a outras pessoas.

Se o token vazar, use o BotFather para revogá-lo.

---

## 17. Configurar o gateway

No Kali:

```bash
hermes gateway setup
```

Selecione:

1. Telegram;
2. configuração por token;
3. informe o token;
4. mantenha autorização por pareamento;
5. reinicie o gateway quando solicitado.

Depois:

```bash
hermes gateway start
hermes gateway status
```

Envie uma mensagem para o bot. Ele deverá fornecer um código de pareamento.

No Kali:

```bash
hermes pairing approve telegram CODIGO_RECEBIDO
```

Confira usuários:

```bash
hermes pairing list
```

Revogue um usuário, quando necessário:

```bash
hermes pairing revoke telegram ID_DO_USUARIO
```

### 17.1 Regras do bot

- não habilite acesso para todos;
- mantenha apenas seu ID permitido;
- não publique o nome do bot junto com o token;
- não aprove códigos de pessoas desconhecidas;
- não coloque o bot em grupos públicos;
- mantenha a VM atualizada;
- lembre que o bot para quando a VM estiver desligada.

Não é necessário executar o gateway dentro de `screen` ou `tmux` quando ele já estiver instalado como serviço.

---

## 18. Snapshot do Hermes configurado

Quando o chat e o Telegram estiverem funcionando:

```text
Nome do snapshot: 02-Hermes-Configurado
```

Esse snapshot deve ser criado antes da instalação de skills externas ou do analisador de LIVE.

---

# Parte V — Skills externas e segurança

## 19. Skills de cibersegurança mostradas no vídeo

O repositório citado no vídeo é comunitário:

- [mukul975/Anthropic-Cybersecurity-Skills](https://github.com/mukul975/Anthropic-Cybersecurity-Skills)

Não instale centenas de skills cegamente.

Adicione como fonte:

```bash
hermes skills tap add mukul975/Anthropic-Cybersecurity-Skills
```

Pesquise:

```bash
hermes skills search pentest
```

Inspecione uma skill:

```bash
hermes skills inspect IDENTIFICADOR_DA_SKILL
```

Instale somente depois de revisar:

```bash
hermes skills install IDENTIFICADOR_DA_SKILL
```

Audite as skills:

```bash
hermes skills audit
```

### 19.1 Limite de uso

Testes de segurança devem ocorrer somente em:

- aplicações de sua propriedade;
- ambientes com autorização expressa;
- DVWA;
- OWASP Juice Shop;
- Metasploitable;
- laboratórios do Hack The Box ou TryHackMe;
- outros ambientes criados especificamente para treinamento.

A análise de TikTok LIVE não precisa dessas skills de pentest. Para nosso objetivo, será criada uma skill separada e limitada à análise de conteúdo e vendas.

---

# Parte VI — Como o Hermes estudará TikTok LIVE

## 20. O que significa "assistir" a uma LIVE

Uma IA não assiste continuamente da mesma forma que uma pessoa.

O sistema precisa transformar a transmissão em dados menores:

| Fonte | Informação obtida |
|---|---|
| Frames | Produto, cenário, iluminação, preço e texto na tela |
| Áudio transcrito | Argumentos, CTA, dúvidas, explicações e repetições |
| Comentários | Interesse, objeções, perguntas e solicitações |
| Eventos | Likes, entradas, seguidores, compartilhamentos e espectadores |
| Catálogo | Nome, SKU, categoria, preço e comissão |
| LIVE Dashboard | Cliques, pedidos, conversão, GMV e desempenho por SKU |

É importante separar três níveis:

1. **Observado:** apareceu no vídeo ou foi falado.
2. **Inferido:** parece ter causado interesse, mas não há confirmação.
3. **Confirmado:** existe métrica do TikTok comprovando clique, pedido ou venda.

O agente nunca deve apresentar inferência como venda confirmada.

---

## 21. Dois modos de análise

### 21.1 Modo pós-LIVE

É o melhor primeiro passo.

Entradas:

- gravação do OBS;
- catálogo de produtos;
- métricas do LIVE Dashboard;
- prints;
- comentários exportados, quando disponíveis.

Saídas:

- relatório completo;
- linha do tempo;
- ranking dos produtos;
- perguntas frequentes;
- roteiro melhorado;
- pontos fortes e fracos;
- hipóteses para o próximo teste.

### 21.2 Modo quase em tempo real

O sistema acompanha a LIVE em blocos:

1. recebe 30 segundos da transmissão;
2. extrai o áudio;
3. captura frames;
4. recebe comentários e eventos;
5. acumula o contexto;
6. a cada 1 ou 2 minutos consulta o Hermes;
7. envia uma dica pelo Telegram;
8. descarta o vídeo temporário;
9. mantém apenas os dados permitidos.

O atraso dependerá do computador, modelo de transcrição, modelo de visão e conexão.

---

# Parte VII — Análise pós-LIVE com OBS

## 22. Gravar a LIVE

No OBS, prefira gravar suas próprias transmissões.

Configuração sugerida:

- formato de gravação: `MKV`;
- resolução: a resolução utilizada na LIVE;
- FPS: `30`;
- encoder de hardware, quando disponível;
- áudio do apresentador em faixa separada, se possível;
- espaço livre suficiente em disco.

O formato MKV é mais resistente a interrupções inesperadas. Depois:

```text
OBS → Arquivo → Remuxar gravações
```

Converta para MP4 para facilitar o processamento.

---

## 23. Criar uma pasta compartilhada somente leitura

No Windows, crie:

```text
C:\LivesTikTok
```

No VirtualBox:

1. desligue a VM;
2. abra **Configurações → Pastas compartilhadas**;
3. selecione `C:\LivesTikTok`;
4. marque **Somente leitura**;
5. marque montagem automática;
6. ligue a VM.

A pasta somente leitura impede que o Hermes altere ou apague as gravações originais.

Caso a pasta não apareça, confira os Guest Additions e os grupos de usuário do VirtualBox antes de mudar permissões.

---

## 24. Estrutura de cada LIVE

Use uma pasta por transmissão:

```text
LivesTikTok/
└── 2026-07-29_loja-exemplo/
    ├── live.mp4
    ├── produtos.csv
    ├── metricas.csv
    ├── comentarios.jsonl
    └── prints-dashboard/
```

### 24.1 Exemplo de `produtos.csv`

```csv
sku,nome,categoria,preco,desconto,comissao,link
SKU001,Mini aspirador,Casa,79.90,10,12.00,https://exemplo
SKU002,Escova secadora,Beleza,119.90,15,18.00,https://exemplo
```

### 24.2 Exemplo de `metricas.csv`

```csv
data_hora,sku,visualizadores,cliques,pedidos,faturamento
2026-07-29 14:30:00,SKU001,850,40,3,239.70
2026-07-29 14:32:00,SKU001,910,62,6,479.40
```

Utilize os nomes reais das colunas que o TikTok disponibilizar. Não invente dados ausentes.

---

## 25. Extrair áudio e frames manualmente

O Hermes poderá executar este processo, mas é importante entender os comandos.

### 25.1 Extrair áudio

```bash
ffmpeg -i live.mp4 -vn -ac 1 -ar 16000 audio.wav
```

### 25.2 Capturar um frame a cada 10 segundos

```bash
mkdir -p frames
ffmpeg -i live.mp4 -vf "fps=1/10,scale=720:-2" frames/frame_%06d.jpg
```

### 25.3 Por que não capturar todos os frames

Uma LIVE de duas horas a 30 FPS possui aproximadamente 216 mil frames.

Capturar um frame a cada 10 segundos reduz isso para aproximadamente 720 imagens, tornando a análise mais barata e rápida.

Nos momentos importantes, o sistema pode aumentar temporariamente a frequência.

---

## 26. Métricas importantes

Para cada LIVE e produto, tente obter:

- visualizadores únicos;
- pico simultâneo;
- média de espectadores;
- tempo médio assistido;
- novos seguidores;
- compartilhamentos;
- comentários;
- cliques no produto;
- adições ao carrinho;
- pedidos;
- unidades;
- faturamento ou GMV;
- comissão;
- taxa de clique;
- taxa de conversão;
- cancelamentos e devoluções, quando disponíveis.

Referência:

- [LIVE Dashboard do TikTok Shop](https://seller-br.tiktok.com/university/essay?knowledge_id=6821759409309441&lang=en)

### 26.1 Cálculos

Taxa de clique:

```text
CTR = cliques no produto ÷ visualizadores únicos × 100
```

Conversão após o clique:

```text
Conversão = pedidos ÷ cliques no produto × 100
```

Receita por mil visualizadores:

```text
RPM de audiência = faturamento ÷ visualizadores únicos × 1.000
```

Esses cálculos somente são válidos quando as métricas possuem o mesmo intervalo e critério de atribuição.

---

## 27. Relatório esperado

O relatório deve conter:

1. resumo executivo;
2. dados disponíveis e dados ausentes;
3. qualidade da análise;
4. linha do tempo da LIVE;
5. produtos apresentados;
6. tempo de apresentação por produto;
7. argumentos utilizados;
8. chamadas para compra;
9. perguntas e objeções;
10. variações de audiência;
11. produtos com mais interesse;
12. produtos com vendas confirmadas;
13. produtos com interesse sem conversão;
14. faixa de preço;
15. estilo de demonstração;
16. melhorias;
17. roteiro da próxima LIVE;
18. testes recomendados.

Exemplo:

```text
Produto: Mini aspirador
Exibição: 8 minutos
Cliques: 142
Pedidos: 18
Conversão após clique: 12,6%

Observação:
O apresentador demonstrou o produto em uma mesa com sujeira real.

Confirmação:
Os cliques e pedidos aumentaram durante e após a demonstração.

Hipótese:
A comparação antes/depois pode ter contribuído para o aumento.

Recomendação:
Fixar o produto antes da demonstração, mostrar o resultado
nos primeiros 30 segundos e repetir preço e benefício.
```

---

# Parte VIII — Acompanhamento em tempo real

## 28. Projeto usado como referência

Projeto:

- [Michele0303/tiktok-live-recorder](https://github.com/Michele0303/tiktok-live-recorder)

O projeto atual:

1. encontra o usuário ou `room_id`;
2. obtém candidatos de URL da LIVE;
3. recebe o stream em blocos;
4. grava esses blocos em arquivo;
5. converte a gravação para MP4.

Código principal:

- [tiktok_recorder.py](https://github.com/Michele0303/tiktok-live-recorder/blob/main/src/core/tiktok_recorder.py)

No método atual, os blocos recebidos são acumulados e escritos no arquivo. Nossa versão deve trocar essa escrita permanente por um pipeline temporário.

---

## 29. "Assistir" sem salvar a LIVE completa

O sistema ainda precisa receber os bytes da transmissão. Isso é inevitável.

A diferença é:

| Gravador atual | Analisador proposto |
|---|---|
| Salva todo o vídeo | Mantém apenas buffer temporário |
| Converte no final | Processa durante a transmissão |
| Não transcreve | Transcreve blocos de áudio |
| Não interpreta frames | Analisa imagens espaçadas |
| Não captura comentários | Integra eventos da LIVE |
| Entrega gravação | Entrega dicas e relatório |

Fluxo:

```text
TikTok LIVE
    ↓
Resolvedor de room_id e URL
    ↓
Buffer temporário de 30 segundos
    ├── áudio → transcrição
    ├── frames → visão e OCR
    └── horário → sincronização
    ↓
Comentários e eventos em tempo real
    ↓
Agregador de contexto
    ↓
Hermes analisa a cada 1 ou 2 minutos
    ├── dica no Telegram
    └── histórico para relatório final
```

---

## 30. Instalar e testar o gravador original

Faça isso somente no snapshot de desenvolvimento e somente com uma LIVE própria ou autorizada.

Dependências:

```bash
sudo apt update
sudo apt install -y git ffmpeg curl
```

Instalar `uv`:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
```

Clonar:

```bash
git clone https://github.com/Michele0303/tiktok-live-recorder
cd tiktok-live-recorder
uv venv
uv sync
```

Ver ajuda:

```bash
uv run python src/main.py -h
```

Teste curto de 30 segundos:

```bash
mkdir -p output
uv run python src/main.py \
  -user SEU_USUARIO \
  -duration 30 \
  -output ./output
```

Esse teste ainda grava um arquivo. Ele serve apenas para conferir se a resolução da LIVE funciona antes de desenvolver o analisador.

Não utilize proxy para contornar bloqueios regionais ou controles de acesso.

---

## 31. Comentários e eventos

O gravador não coleta toda a interação social.

Uma referência para comentários e eventos é:

- [TikTokLive para Python](https://github.com/isaackogan/TikTokLive)

Ela pode fornecer eventos como:

- comentários;
- likes;
- presentes;
- entradas;
- seguidores;
- compartilhamentos;
- perguntas;
- espectadores atuais;
- sinais relacionados a compras.

Trata-se de uma integração não oficial baseada em engenharia reversa. Ela pode parar de funcionar quando o TikTok mudar seus serviços.

Se esse componente for distribuído ou comercializado, confira também sua licença modificada AGPL.

---

## 32. Arquitetura recomendada do novo projeto

Não altere diretamente o projeto original. Crie um projeto separado:

```text
tiktok-live-analyst/
├── app/
│   ├── capture/
│   │   ├── live_resolver.py
│   │   └── stream_reader.py
│   ├── audio/
│   │   ├── segmenter.py
│   │   └── transcriber.py
│   ├── vision/
│   │   ├── frame_sampler.py
│   │   ├── ocr.py
│   │   └── product_matcher.py
│   ├── events/
│   │   └── tiktok_events.py
│   ├── metrics/
│   │   ├── importer.py
│   │   └── calculator.py
│   ├── hermes/
│   │   ├── context_builder.py
│   │   └── advisor.py
│   ├── reports/
│   │   ├── live_report.py
│   │   └── history_report.py
│   └── main.py
├── config/
├── data/
├── reports/
├── tests/
├── pyproject.toml
└── README.md
```

### 32.1 Responsabilidade dos módulos

| Módulo | Responsabilidade |
|---|---|
| `capture` | Encontrar e consumir a LIVE |
| `audio` | Criar blocos e transcrever |
| `vision` | Capturar frames, OCR e reconhecer produtos |
| `events` | Receber comentários e eventos |
| `metrics` | Importar dados oficiais e calcular indicadores |
| `hermes` | Preparar contexto e solicitar análise |
| `reports` | Gerar relatório e ranking histórico |

---

## 33. Processamento contínuo recomendado

### 33.1 Janela curta

A cada 30 segundos:

- transcrever a fala;
- capturar de 3 a 6 frames;
- agrupar comentários;
- registrar espectadores, likes e compartilhamentos;
- identificar o produto atual.

### 33.2 Janela de recomendação

A cada 2 minutos:

- resumir a janela;
- comparar com a janela anterior;
- detectar alteração de audiência;
- identificar pergunta repetida;
- identificar CTA ausente;
- enviar somente uma recomendação útil.

Evite mensagens demais durante a LIVE.

### 33.3 Encerramento

Ao terminar:

- finalizar a transcrição;
- importar métricas oficiais;
- gerar linha do tempo;
- produzir relatório;
- atualizar histórico;
- apagar buffers temporários.

---

## 34. Modos de retenção

### 34.1 Privacidade máxima

- buffer de 30 segundos;
- apaga áudio e frames após análise;
- mantém somente relatório agregado.

### 34.2 Estudo — recomendado

- mantém transcrição;
- mantém comentários sem dados desnecessários;
- mantém uma imagem por minuto;
- mantém métricas e relatórios;
- não guarda o vídeo completo.

### 34.3 Completo

- mantém vídeo;
- mantém áudio;
- mantém frames;
- mantém comentários;
- mantém métricas.

Use o modo completo apenas quando houver autorização e espaço suficiente.

---

## 35. Mensagem em tempo real

Exemplo:

```text
ANÁLISE AO VIVO — 14:32

Produto atual: Escova secadora
Tempo apresentado: 4min20s
Comentários relacionados: 37
Espectadores: +12% nos últimos 2 minutos
Pergunta mais repetida: "Serve para cabelo cacheado?"

Observado:
O produto foi mostrado, mas ainda não foi demonstrado funcionando.

Oportunidade:
Explique o resultado em cabelo cacheado ou faça uma demonstração.

Próxima ação:
Mostre o produto funcionando, repita o preço e peça para
clicarem no produto fixado.
```

---

# Parte IX — Descoberta dos produtos que mais vendem

## 36. O que é possível descobrir

Com uma única LIVE:

- produtos com mais interesse;
- perguntas mais frequentes;
- apresentação mais envolvente;
- momentos de aumento ou queda de audiência;
- possíveis motivos de clique.

Com várias LIVEs e métricas oficiais:

- categorias que mais vendem;
- faixa de preço com melhor conversão;
- produto com melhor CTR;
- produto com melhor conversão;
- produto com maior faturamento;
- produto com maior comissão;
- demonstrações que mais funcionam;
- horários com melhor resultado;
- duração ideal por produto;
- argumentos que antecedem compras.

Use aproximadamente 10 LIVEs como histórico inicial. Quanto maior e mais consistente o histórico, melhor a comparação.

---

## 37. Ranking correto

Não use somente o número de pedidos.

O relatório deve comparar:

| Indicador | O que mostra |
|---|---|
| Cliques | Interesse no produto |
| CTR | Capacidade de gerar clique |
| Pedidos | Volume vendido |
| Conversão | Capacidade de transformar clique em pedido |
| Faturamento | Receita total |
| Comissão | Retorno do criador |
| Cancelamentos | Qualidade da venda |
| Tempo apresentado | Eficiência da exposição |

Exemplo:

```text
Produto A:
100 pedidos, mas foi apresentado por 90 minutos.

Produto B:
60 pedidos, mas foi apresentado por 20 minutos.

Conclusão:
O Produto A vendeu mais unidades.
O Produto B pode ter sido mais eficiente por minuto.
```

---

## 38. Análise de concorrentes

Em uma LIVE pública de terceiros, normalmente é possível observar:

- apresentação;
- roteiro;
- frequência de CTA;
- preços exibidos;
- perguntas públicas;
- likes e espectadores visíveis;
- ordem dos produtos;
- demonstrações.

Não é possível confirmar, sem dados oficiais:

- pedidos;
- conversão;
- faturamento;
- cancelamentos;
- comissão real.

O relatório deve usar termos como:

```text
"gerou mais comentários"
"parece ter aumentado o interesse"
"houve aumento de espectadores"
```

E nunca:

```text
"foi o produto que mais vendeu"
```

sem métricas que confirmem isso.

Grave e analise somente conteúdo próprio, autorizado ou utilizado de acordo com as regras aplicáveis. Não tente acessar conteúdo privado, contornar bloqueios ou coletar dados pessoais desnecessários.

---

# Parte X — Skill do Hermes

## 39. Prompt completo para criar a skill

Depois que o pipeline estiver funcionando, envie ao Hermes:

```text
Crie uma skill chamada tiktok-live-analyst.

OBJETIVO

Analisar gravações e transmissões autorizadas de TikTok LIVE,
com foco em qualidade da apresentação, interesse do público,
desempenho dos produtos e melhoria de vendas.

FONTES DE DADOS

- transcrição da fala;
- frames extraídos da LIVE;
- OCR dos textos e preços;
- catálogo de produtos;
- comentários;
- likes;
- entradas;
- seguidores;
- compartilhamentos;
- número de espectadores;
- métricas oficiais do TikTok Shop LIVE Dashboard;
- histórico de LIVEs anteriores.

REGRAS

1. Trabalhe somente com conteúdo próprio, autorizado ou público
   utilizado de acordo com as regras aplicáveis.
2. Nunca invente pedidos, conversão, faturamento ou comissão.
3. Diferencie sempre:
   a) observado;
   b) inferido;
   c) confirmado por métricas.
4. Não trate comentários, likes ou aumento de espectadores como
   prova de venda.
5. Não inclua dados pessoais de clientes no relatório.
6. Não execute compras, comentários, curtidas ou interações na LIVE.
7. Não acesse conteúdo privado nem contorne controles de acesso.
8. Preserve as gravações originais.

PROCESSAMENTO

1. Organize a LIVE em uma linha do tempo.
2. Identifique o produto apresentado em cada intervalo.
3. Registre início, fim e duração de cada apresentação.
4. Identifique nome, preço, desconto, benefício e demonstração.
5. Identifique chamadas para clicar, comprar ou adicionar ao carrinho.
6. Identifique gatilhos, provas, comparações e respostas a objeções.
7. Agrupe comentários por produto e assunto.
8. Localize as perguntas mais repetidas.
9. Detecte alterações de espectadores e engajamento.
10. Relacione os momentos com cliques, pedidos e faturamento somente
    quando essas métricas forem fornecidas.
11. Compare produtos, categorias, preços, horários e apresentadores.
12. Gere recomendações específicas e verificáveis.

SAÍDA DURANTE A LIVE

A cada dois minutos, envie no máximo uma recomendação contendo:

- produto atual;
- comportamento observado;
- pergunta ou objeção principal;
- mudança de audiência;
- ação recomendada.

SAÍDA FINAL

Gere:

- resumo executivo;
- fontes analisadas;
- dados ausentes;
- linha do tempo;
- ranking de interesse;
- ranking de vendas confirmadas;
- perguntas e objeções;
- argumentos mais eficientes;
- pontos fracos;
- roteiro melhorado;
- testes para a próxima LIVE;
- nível de confiança de cada conclusão.
```

---

## 40. Prompt de análise pós-LIVE

```text
Analise esta pasta de TikTok LIVE.

Antes de começar:

1. Liste os arquivos encontrados.
2. Informe quais dados estão disponíveis.
3. Informe quais dados estão ausentes.
4. Não execute alterações nos arquivos originais.
5. Separe observações, hipóteses e conclusões confirmadas.

Depois:

1. Extraia a transcrição.
2. Gere frames a cada 10 segundos.
3. Identifique os produtos e períodos de apresentação.
4. Analise fala, CTA, demonstração, preço e objeções.
5. Relacione com comentários e métricas.
6. Calcule indicadores somente com dados compatíveis.
7. Gere um relatório Markdown.
8. Gere um roteiro para a próxima LIVE.
```

---

## 41. Prompt de acompanhamento ao vivo

```text
Acompanhe os blocos desta TikTok LIVE.

A cada janela de dois minutos:

1. Resuma o produto e a demonstração.
2. Identifique a principal pergunta.
3. Informe mudança de espectadores e engajamento.
4. Verifique se houve preço, benefício e CTA.
5. Envie somente uma recomendação prática.
6. Não interrompa com mensagens quando não houver recomendação útil.
7. Não afirme que houve venda sem métrica oficial.

Ao terminar:

1. consolide a linha do tempo;
2. importe as métricas disponíveis;
3. gere relatório;
4. atualize o histórico;
5. exclua somente os buffers temporários autorizados.
```

---

# Parte XI — Plano de implementação

## 42. Fase 1 — Ambiente

- [ ] Virtualização habilitada.
- [ ] VirtualBox instalado.
- [ ] Kali importado.
- [ ] Senha alterada.
- [ ] Rede NAT.
- [ ] Snapshot `01-Kali-Limpo`.

## 43. Fase 2 — Hermes

- [ ] Hermes instalado como usuário normal.
- [ ] OpenCode configurado.
- [ ] Modelo testado.
- [ ] Aprovações ativadas.
- [ ] Visão configurada.
- [ ] Telegram pareado.
- [ ] Snapshot `02-Hermes-Configurado`.

## 44. Fase 3 — Pós-LIVE

- [ ] Gravar uma LIVE própria.
- [ ] Organizar catálogo.
- [ ] Exportar métricas.
- [ ] Extrair áudio.
- [ ] Gerar frames.
- [ ] Criar transcrição.
- [ ] Gerar primeiro relatório.
- [ ] Conferir manualmente as conclusões.

## 45. Fase 4 — MVP em tempo real

- [ ] Detectar quando o usuário entra ao vivo.
- [ ] Obter o stream.
- [ ] Criar buffer temporário.
- [ ] Transcrever blocos.
- [ ] Capturar frames.
- [ ] Receber comentários e eventos.
- [ ] Gerar dica a cada dois minutos.
- [ ] Enviar dica pelo Telegram.
- [ ] Apagar buffers.
- [ ] Gerar relatório final.

## 46. Fase 5 — Inteligência histórica

- [ ] Armazenar resultados de pelo menos 10 LIVEs.
- [ ] Normalizar nomes e categorias.
- [ ] Comparar preços e descontos.
- [ ] Calcular CTR e conversão.
- [ ] Comparar desempenho por minuto.
- [ ] Identificar padrões repetidos.
- [ ] Gerar roteiro recomendado.

---

# Parte XII — Critérios de aceitação

## 47. MVP pós-LIVE aprovado quando

- a gravação original não é alterada;
- o áudio é transcrito;
- os frames são extraídos;
- produtos são relacionados à linha do tempo;
- o relatório diferencia hipótese e confirmação;
- métricas não são inventadas;
- o relatório é compreensível;
- o processamento pode ser repetido.

## 48. MVP em tempo real aprovado quando

- detecta uma LIVE autorizada;
- processa sem guardar o vídeo completo;
- atraso permanece aceitável;
- comentários ficam sincronizados com a fala;
- envia no máximo uma dica por janela;
- não apresenta engajamento como venda;
- finaliza e limpa buffers;
- gera relatório ao encerrar.

---

# Parte XIII — Problemas comuns

## 49. VirtualBox não inicia a VM

Confira:

- virtualização habilitada;
- memória disponível;
- arquitetura 64 bits;
- mensagem exata do VirtualBox;
- atualizações do VirtualBox.

Não desative recursos de segurança do Windows sem confirmar a causa.

## 50. `hermes: command not found`

```bash
source ~/.bashrc
export PATH="$HOME/.local/bin:$PATH"
hermes doctor
```

## 51. Bot não responde

```bash
hermes gateway status
hermes pairing list
hermes doctor
```

Confira:

- VM ligada;
- internet funcionando;
- token correto;
- pareamento aprovado;
- provedor do modelo funcionando.

## 52. Transcrição muito lenta

- use blocos maiores;
- utilize modelo de transcrição menor;
- reduza a frequência;
- habilite GPU compatível;
- processe pós-LIVE antes de tentar tempo real.

## 53. Análise visual cara ou lenta

- use um frame a cada 10 ou 15 segundos;
- reduza a imagem para 720 pixels de largura;
- use OCR local para preços;
- envie ao modelo de visão somente frames relevantes;
- aumente a frequência apenas em momentos de demonstração.

## 54. Gravador não encontra a LIVE

O projeto não é oficial e pode quebrar quando o TikTok muda seus endpoints.

Confira:

- usuário correto;
- LIVE realmente ativa;
- versão do repositório;
- [issues atuais do tiktok-live-recorder](https://github.com/Michele0303/tiktok-live-recorder/issues);
- se o problema também ocorre com outras LIVEs autorizadas.

Não contorne bloqueios, CAPTCHAs, restrições regionais ou controles de acesso.

---

# Parte XIV — Checklist de segurança

Antes de ligar o sistema:

- [ ] Estou usando usuário normal, não `root`.
- [ ] A VM usa rede NAT.
- [ ] Não habilitei YOLO.
- [ ] Aprovações estão em Smart ou Manual.
- [ ] O Telegram está limitado ao meu usuário.
- [ ] Tokens não aparecem em prints.
- [ ] Gravações do Windows estão montadas como somente leitura.
- [ ] Tenho snapshot recente.
- [ ] A LIVE é própria ou autorizada.
- [ ] Não estou coletando dados pessoais desnecessários.
- [ ] Métricas confidenciais estão anonimizadas.
- [ ] Buffers temporários possuem política de exclusão.
- [ ] O relatório diferencia interesse de venda confirmada.

---

# Parte XV — Referências principais

## Hermes

- [Documentação oficial](https://hermes-agent.nousresearch.com/docs/)
- [Instalação](https://hermes-agent.nousresearch.com/docs/getting-started/installation)
- [Segurança](https://hermes-agent.nousresearch.com/docs/user-guide/security)
- [Telegram](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram)
- [Visão](https://hermes-agent.nousresearch.com/docs/user-guide/features/vision)
- [Skills](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills)

## Kali e VirtualBox

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Download do Kali](https://www.kali.org/get-kali/)
- [Kali no VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)
- [Importar VM pronta](https://www.kali.org/docs/virtualization/import-premade-virtualbox/)

## TikTok LIVE

- [LIVE Dashboard do TikTok Shop](https://seller-br.tiktok.com/university/essay?knowledge_id=6821759409309441&lang=en)
- [tiktok-live-recorder](https://github.com/Michele0303/tiktok-live-recorder)
- [TikTokLive para eventos](https://github.com/isaackogan/TikTokLive)

## Modelos e skills

- [OpenCode Zen](https://opencode.ai/docs/zen/)
- [Anthropic-Cybersecurity-Skills comunitário](https://github.com/mukul975/Anthropic-Cybersecurity-Skills)

---

## 55. Próximo passo recomendado

Não comece pela versão em tempo real.

Siga esta ordem:

1. instalar VirtualBox e Kali;
2. configurar Hermes e Telegram;
3. gravar uma LIVE própria curta;
4. criar o primeiro relatório pós-LIVE;
5. conferir manualmente se a análise está correta;
6. repetir com outras LIVEs;
7. somente depois criar o capturador contínuo;
8. por último, adicionar dicas em tempo real e histórico de produtos.

Essa ordem permite validar cada parte separadamente e descobrir erros antes que o sistema tome conclusões incorretas durante uma transmissão real.
