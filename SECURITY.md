# Política de Segurança — FESTA

Este documento define as regras de segurança que TODOS os arquivos do projeto devem seguir desde o início. O objetivo é evitar vulnerabilidades comuns que levam a invasões.

## 🚫 NUNCA fazer

1. **Nunca commitar segredos** — tokens, senhas, chaves de API, certificados nunca entram no Git. Use `.env` (que já está no `.gitignore`).
2. **Nunca usar `eval()` ou execução dinâmica** com entrada do usuário.
3. **Nunca concatenar strings em SQL** — sempre usar queries parametrizadas (ORM ou prepared statements).
4. **Nunca confiar em dados do cliente** — validar TUDO no servidor (mesmo se já validou no front).
5. **Nunca logar dados sensíveis** — senhas, tokens, CPF, dados de cartão NUNCA aparecem em log.
6. **Nunca expor stack traces em produção** — só em ambiente de desenvolvimento.
7. **Nunca usar HTTP em produção** — sempre HTTPS com certificado válido.
8. **Nunca dar permissão `0777`** em arquivos/pastas — usar o mínimo necessário.

## ✅ SEMPRE fazer

### Autenticação e sessão
- Senhas sempre com **hash forte** (`bcrypt` com cost ≥ 12, ou `argon2`)
- JWT/sessões com **expiração curta** (15-60 min) e refresh token separado
- **Rate limiting** em endpoints de login (máx 5 tentativas / 15 min)
- **2FA** disponível para contas administrativas
- Cookies com flags: `HttpOnly`, `Secure`, `SameSite=Lax` ou `Strict`

### Banco de dados
- Usar **ORM** (do Odoo / Sequelize / Prisma) — evita SQL injection automaticamente
- **Backups automatizados** diários, com teste de restore mensal
- Usuário do banco com **permissão mínima** (não usar `postgres`/`root` na app)
- Conexão via SSL/TLS quando o banco está em outro host

### Entrada de dados
- **Validar** tipo, tamanho, formato (allowlist > blocklist)
- **Sanitizar** HTML que vai ser exibido (evita XSS) — usar lib específica, nunca regex
- **Limite de upload** (tamanho máx, tipos permitidos por extensão E magic number)
- **CSRF tokens** em todas as ações de mudança de estado (POST/PUT/DELETE)

### Headers HTTP de segurança
- `Strict-Transport-Security` (HSTS)
- `X-Frame-Options: DENY` (anti-clickjacking)
- `X-Content-Type-Options: nosniff`
- `Content-Security-Policy` (CSP) restritiva
- `Referrer-Policy: strict-origin-when-cross-origin`

### Dependências
- Rodar `npm audit` / `pip-audit` antes de cada deploy
- Atualizar pacotes com vulnerabilidades críticas em **48h**
- Usar `package-lock.json` / `requirements.txt` com versões fixadas

### Logs e auditoria
- Logar: tentativas de login, mudanças de permissão, acesso a dados sensíveis
- Logs com **timestamp UTC**, IP, user-agent, ID do usuário
- Reter logs por **mínimo 90 dias** em local separado da app

### Infraestrutura
- Firewall: liberar **só** as portas necessárias (80, 443, SSH com chave)
- SSH **sem senha** (só chave) e em porta não-padrão
- Atualizações de SO automáticas para patches críticos
- Servidor sem GUI (menos superfície de ataque)

## 📋 Checklist antes de subir código

- [ ] Não tem nenhum `console.log()` / `print()` com dado sensível?
- [ ] Não tem nenhuma chave/token/senha no código?
- [ ] Toda entrada do usuário está validada?
- [ ] As queries usam ORM ou parameter binding?
- [ ] Os endpoints exigem autenticação onde precisa?
- [ ] As permissões dos arquivos novos estão corretas?
- [ ] O `.env.example` foi atualizado se adicionou variável nova?

## 🆘 Em caso de incidente

1. **Trocar imediatamente** todas as chaves/tokens potencialmente expostos
2. **Revogar sessões ativas** dos usuários afetados
3. **Verificar logs** de acesso suspeito
4. **Documentar** o incidente (data, causa, ação tomada)
5. **Notificar** usuários se houve vazamento de dados (LGPD)

## 📚 Referências

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Odoo Security](https://www.odoo.com/security)
- [LGPD](https://www.gov.br/lgpd)
