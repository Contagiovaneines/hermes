# 06 — Bypass em laboratórios autorizados

"Bypass" (contornar uma proteção) é um tópico central de pentest — mas só é ensinado com responsabilidade quando acontece dentro de **aplicações feitas de propósito pra isso**. Este documento cobre categorias de bypass exatamente como aparecem em plataformas oficiais de treino (OWASP Juice Shop, PortSwigger Web Security Academy), sem payloads prontos pra usar contra sites reais.

> Tudo aqui pressupõe: você rodando a própria instância local do Juice Shop/DVWA, ou um laboratório seu na PortSwigger Academy/TryHackMe/HackTheBox. Fora disso, não é estudo, é crime.

## Categorias de bypass que o OWASP Juice Shop ensina oficialmente

O [Juice Shop](https://owasp.org/www-project-juice-shop/) tem desafios categorizados oficialmente. As categorias que envolvem "contornar" alguma proteção incluem:

| Categoria oficial | O que você aprende a contornar |
|---|---|
| Broken Access Control | Restrições de acesso a rotas/recursos que dependiam só do front-end |
| Improper Input Validation | Validação de formulário feita apenas no cliente (client-side) |
| Broken Authentication | Fluxos de login, recuperação de senha e tokens mal implementados |
| Security Misconfiguration | Configurações padrão ou headers de segurança ausentes |
| Cryptographic Issues | Hashes fracos ou previsíveis usados pra "proteger" dados |

Cada um desses tem uma solução oficial documentada no [Companion Guide do Juice Shop](https://pwning.owasp-juice.shop/) — use-o como gabarito, não como atalho: tente resolver sozinho antes de olhar.

## Categorias de bypass no PortSwigger Web Security Academy

A [PortSwigger Academy](https://portswigger.net/web-security) tem trilhas inteiras dedicadas a contornar defesas, todas em laboratórios isolados (uma instância só sua, criada ao clicar em "Access the lab"):

- **Bypassing SQL injection filters** (dentro da trilha de SQL Injection).
- **Authentication bypass** (trilha de Authentication — 2FA bypass, password reset poisoning).
- **Access control vulnerabilities** (IDOR, bypass de controle baseado em URL).
- **WAF bypass** (dentro da trilha de SQL Injection avançada — contornar filtros de Web Application Firewall no laboratório deles).
- **Rate limiting bypass** (trilha de Authentication — contornar limite de tentativas de login).

## Como estudar isso de forma que realmente forma profissional

1. Leia a teoria da vulnerabilidade antes de tentar o bypass (a Academy explica cada uma).
2. Tente resolver o laboratório sozinho, sem olhar a solução, por pelo menos 20-30 minutos.
3. Anote **por que** a proteção original falhou (ex.: "validação só no JavaScript, nunca no back-end") — isso é o que fica na cabeça depois.
4. Só então compare com a solução oficial.
5. Use o [`projetos/juice-shop-tracker/`](../projetos/juice-shop-tracker/) deste repositório pra registrar isso de forma organizada.

## O que este documento não cobre (de propósito)

- Bypass de CAPTCHA em produção, sistemas antifraude reais, ou rate-limit de APIs de terceiros.
- Bypass de DRM, licenciamento de software ou proteções antipirataria (isso é regulado por leis específicas de anticircumvention, ex. DMCA nos EUA, e não é "pentest").
- Bypass de autenticação/2FA de contas ou serviços reais que não sejam seus.
- Evasão de antivírus/EDR — fora do escopo educacional deste repositório.

Se seu interesse é algum desses tópicos especificamente pra carreira (ex. malware analysis, red team profissional), isso se estuda em cursos formais e certificações avançadas (ex. OSED, OSEP da OffSec), com contrato, escopo e supervisão — não em um repositório de estudo pessoal.
