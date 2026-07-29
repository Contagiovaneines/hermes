# 01 — WSL e Kali Linux: conceitos

## O que é o WSL (Windows Subsystem for Linux)

Camada de compatibilidade da Microsoft que roda um kernel Linux real dentro do Windows, sem precisar de máquina virtual tradicional completa nem dual-boot. Na versão atual (WSL2), cada distribuição roda numa VM leve com kernel Linux próprio, com boa integração de arquivos, rede e GPU com o Windows. É o jeito padrão de rodar ferramentas Linux em máquinas Windows hoje em dia.

## O que o Kali Linux normalmente oferece

Kali é uma distro baseada em Debian, mantida pela Offensive Security (OffSec), voltada a testes de segurança. Ela não tem "poderes" especiais — é uma coleção curada de mais de 600 ferramentas de código aberto já instaladas e configuradas, organizadas por categoria:

- coleta de informação (nmap, recon-ng);
- análise de vulnerabilidades (OpenVAS, Nikto);
- ataques a senha (John the Ripper, Hashcat);
- testes de aplicação web (Burp Suite, sqlmap);
- análise forense e engenharia reversa.

Tudo isso existe separadamente para qualquer Linux; o Kali só empacota e mantém atualizado.

## WSL2 vs. máquina virtual completa (VirtualBox)

| Caminho | Vantagem | Desvantagem |
|---|---|---|
| WSL2 | Instalação rápida, boa integração e desempenho | Pode acessar arquivos montados do Windows por padrão |
| Máquina virtual (VirtualBox) | Melhor isolamento, snapshots fáceis de restaurar | Usa mais memória e processador, mais lenta pra configurar |

Para estudo com um agente autônomo (que executa comandos sozinho), isolamento extra vale a pena. Se optar por VM completa, use rede NAT (não Bridge) e desative pastas compartilhadas com escrita — assim o agente fica isolado da sua rede e dos seus arquivos reais.

## Instalar Kali via WSL no Windows 11

1. Abra o PowerShell **como administrador**.
2. Rode:

   ```powershell
   wsl --install -d kali-linux
   ```

   Isso ativa os componentes do Windows necessários (WSL2 + Virtual Machine Platform) e baixa o Kali direto do repositório oficial da Microsoft Store.
3. Reinicie o computador se for pedido.
4. Na primeira execução, crie um usuário e senha Linux (separados da conta Windows).
5. Atualize os pacotes:

   ```bash
   sudo apt update && sudo apt full-upgrade -y
   ```
6. A instalação mínima não traz todas as ferramentas de pentest. Para o metapacote completo:

   ```bash
   sudo apt install -y kali-linux-large
   ```

Documentação oficial: [Kali Linux — WSL Metapackages](https://www.kali.org/docs/wsl/wsl-metapackages/) e [Microsoft Learn — Instalar WSL](https://learn.microsoft.com/pt-br/windows/wsl/install).

## Alternativa: Kali em VirtualBox (mais isolado)

1. Instale o [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (versão Windows hosts).
2. Baixe a [VM pronta do Kali](https://www.kali.org/get-kali/) (seção "Pre-built Virtual Machines" → VirtualBox, 64 bits).
3. Extraia o `.7z` e importe no VirtualBox (`Máquina → Adicionar`, selecione o `.vbox`).
4. Login inicial: usuário `kali`, senha `kali`. Troque a senha imediatamente com `passwd`.
5. Configure rede como **NAT**, desative área de transferência compartilhada e arrastar-e-soltar.
6. Depois de atualizar o sistema, tire um snapshot limpo (`VirtualBox → Máquina → Tirar snapshot`) antes de instalar qualquer agente.

Referência: [Importar VM pronta do Kali no VirtualBox](https://www.kali.org/docs/virtualization/import-premade-virtualbox/).
