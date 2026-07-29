# 07 — Instalar em Linux nativo (Debian/Ubuntu/Kali)

Se você já usa Linux no dia a dia, tem dois caminhos: instalar as ferramentas de pentest direto no seu sistema, ou isolar tudo em um container Docker (recomendado se seu Linux principal não é dedicado a testes).

## Caminho 1 — Kali nativo (dual-boot ou já usa Kali)

Se você já tem Kali instalado como sistema principal:

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y kali-linux-large curl git ffmpeg python3 python3-venv
```

Referência oficial: [Metapacotes do Kali](https://www.kali.org/docs/general-use/metapackages/).

## Caminho 2 — Debian/Ubuntu + ferramentas avulsas

Se você usa Ubuntu/Debian/Fedora como principal e não quer trocar de distro, instale só o que precisa:

```bash
sudo apt update
sudo apt install -y curl git ffmpeg python3 python3-venv nmap
```

Ferramentas específicas de pentest (Burp Suite Community, sqlmap, etc.) podem ser instaladas avulsas conforme a necessidade — evite instalar um "metapacote" inteiro de pentest em uma máquina que você usa pra trabalho/pessoal.

## Caminho 3 (recomendado para isolamento) — Kali em container Docker

Mantém tudo isolado do seu sistema de arquivos principal, mesmo em Linux nativo:

```bash
docker pull kalilinux/kali-rolling
docker run -it --name kali-lab kalilinux/kali-rolling /bin/bash
```

Dentro do container:

```bash
apt update && apt install -y kali-linux-large curl git ffmpeg python3 python3-venv
```

Para persistir dados entre reinícios do container, monte um volume:

```bash
docker run -it --name kali-lab -v ~/kali-lab-data:/root/lab kalilinux/kali-rolling /bin/bash
```

Referência oficial: [Kali Linux em Docker](https://www.kali.org/docs/containers/using-kali-docker-images/).

## Instalar o Hermes Agent

Independente do caminho escolhido, a partir daqui siga [`03-instalar-hermes-agent.md`](03-instalar-hermes-agent.md) — os comandos `curl ... | bash` e `hermes setup` são os mesmos em qualquer Linux.

## Regras de isolamento (iguais ao Windows)

- Não rode o agente como `root` dentro do container/VM — crie um usuário normal mesmo lá dentro.
- Se estiver em container Docker, não monte `/` do host nem diretórios sensíveis (`~/.ssh`, `~/.aws`, etc.) como volume.
- Se estiver em dual-boot nativo, considere uma VM (VirtualBox/GNOME Boxes/virt-manager) em vez de instalar ferramentas de pentest direto no seu sistema principal.
