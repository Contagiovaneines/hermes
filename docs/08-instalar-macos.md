# 08 — Instalar no macOS (Intel e Apple Silicon)

O Kali não roda nativamente no macOS. Os dois caminhos recomendados são UTM (máquina virtual gráfica) ou Docker (mais leve, sem GUI).

## Caminho 1 — UTM (VM completa, recomendado para iniciantes)

1. Instale o [UTM](https://mac.getutm.app/) (gratuito, ou pela Mac App Store).
2. Baixe a imagem oficial do Kali para o seu processador:
   - Apple Silicon (M1/M2/M3/M4): [Kali para Apple Silicon](https://www.kali.org/get-kali/#kali-virtual-machines) — imagem ARM64.
   - Intel: use a imagem VirtualBox/VMware de 64 bits normal.
3. Abra o UTM, crie uma VM a partir da imagem baixada (ou use o instalador `.utm` pronto, se disponível para Apple Silicon).
4. Configure rede como **Shared Network** (equivalente ao NAT do VirtualBox) — evite modo Bridged inicialmente.
5. Ligue a VM, troque a senha padrão (`kali`/`kali`) com `passwd`.

Referência oficial: [Kali em UTM (Apple Silicon)](https://www.kali.org/docs/virtualization/install-utm-guest-vm/).

## Caminho 2 — Docker (mais leve, sem GUI)

Requer o [Docker Desktop para Mac](https://www.docker.com/products/docker-desktop/) instalado (funciona em Intel e Apple Silicon):

```bash
docker pull kalilinux/kali-rolling
docker run -it --name kali-lab kalilinux/kali-rolling /bin/bash
apt update && apt install -y kali-linux-large curl git ffmpeg python3 python3-venv
```

Em Apple Silicon, o Docker roda a imagem ARM64 automaticamente; se precisar forçar x86_64 (algumas ferramentas antigas), use `docker run --platform linux/amd64 ...` (mais lento, via emulação Rosetta/QEMU).

## Instalar o Hermes Agent

A partir da VM (UTM) ou do container (Docker) já rodando Linux, siga [`03-instalar-hermes-agent.md`](03-instalar-hermes-agent.md) normalmente — os comandos são idênticos.

## Observações específicas do macOS

- O Homebrew (`brew install --cask utm` ou `brew install --cask docker`) facilita instalar o UTM/Docker Desktop, se você já usa Homebrew.
- Guest Additions/SPICE tools do UTM melhoram desempenho gráfico e clipboard — instale-os dentro da VM depois do primeiro boot.
- Como no Windows/Linux: usuário normal (não `root`), rede isolada, snapshot antes de mudanças, aprovações do Hermes em `Smart`/`Manual`.
