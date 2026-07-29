Maquina: Blue (HackTheBox, Windows, Easy)
Data: 2026-07-25

- nmap -sV -p- 10.10.10.40 -> porta 445 aberta, SMB
- smbclient -L //10.10.10.40 -> sem credenciais, listou shares
- versao SMB aparenta vulneravel a MS17-010 (EternalBlue)
- confirmei com script auxiliary/scanner/smb/smb_ms17_010 no msfconsole
- usei exploit ms17_010_eternalblue no msfconsole (laboratorio HTB, maquina propria da conta)
- ganhei shell SYSTEM direto, sem precisar de escalonamento de privilegio
- capturei flag user.txt e root.txt
- tempo total: cerca de 40 minutos
