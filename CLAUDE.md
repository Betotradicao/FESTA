# CLAUDE.md — Regras do Projeto FESTA

> Este arquivo é carregado automaticamente em toda conversa. Define as regras que o Claude DEVE seguir ao trabalhar neste projeto.

## Sobre o projeto

**FESTA** é um sistema de locação de itens para festas, construído em cima do **Odoo 19.0**.

- Repositório: https://github.com/Betotradicao/FESTA
- Owner: Betotradicao (betotradicao76@gmail.com)
- Stack: Odoo 19.0 (Python) + PostgreSQL
- Linguagem das mensagens/commits/comentários: **Português (pt-BR)**

## Estrutura

- `odoo/` — clone do Odoo oficial. **NUNCA commitar.** Já está no .gitignore.
- `custom_addons/` — módulos customizados (criar conforme necessidade)
- `.env` — segredos. **NUNCA commitar.** Já está no .gitignore.
- `.env.example` — template SEM valores reais. Pode commitar.
- `SECURITY.md` — política de segurança completa. **Toda mudança deve respeitar.**

## 🔒 Regras de SEGURANÇA (não-negociáveis)

Todas as decisões de código devem seguir o `SECURITY.md`. Resumo crítico:

1. **Nunca expor `.env`, tokens, senhas ou chaves no Git** — verificar antes de cada commit.
2. **Nunca concatenar SQL** — sempre ORM ou queries parametrizadas.
3. **Nunca confiar em entrada do cliente** — validar tudo no servidor.
4. **Nunca logar dados sensíveis** (senhas, tokens, CPF, dados pessoais).
5. **Nunca usar `eval()` ou execução dinâmica** com input do usuário.
6. **Senhas sempre com bcrypt (cost ≥ 12)** ou argon2.
7. **HTTPS obrigatório** em produção, com headers de segurança (HSTS, CSP, X-Frame-Options).
8. **Rate limiting** em endpoints de login e ações sensíveis.
9. **Cookies sempre com flags** `HttpOnly`, `Secure`, `SameSite`.
10. **Permissão mínima** para usuário do banco — não usar `postgres`/`root` na app.

Antes de cada commit, fazer mentalmente o checklist de SECURITY.md.

## Git workflow

- Branch principal: `main`
- Mensagens de commit: em português, descritivas, focando no **porquê** (não no o que).
- **Nunca fazer `git push --force`** sem confirmação explícita do usuário.
- **Nunca usar `--no-verify`** para pular hooks.
- Antes de commitar, verificar `.gitignore` está pegando: `.env`, `node_modules/`, `__pycache__/`, `odoo/`.

## Estilo de código

- Python: seguir convenções do Odoo (PEP 8, snake_case, docstrings em métodos públicos).
- Comentários: só onde o **porquê** não é óbvio. Não documentar o **o que** (o código já mostra).
- Nomes de variáveis e funções: descritivos, em inglês (padrão Odoo); strings de UI em português.

## Comportamento esperado do Claude

- **Linguagem das respostas:** português (pt-BR), tom direto e amigável.
- **Antes de criar arquivo novo:** confirmar se já não existe um similar pra editar.
- **Antes de instalar dependência:** explicar o porquê e confirmar com o usuário.
- **Antes de operações irreversíveis** (delete, force push, drop table): SEMPRE pedir confirmação.
- **Em segredos/credenciais:** se o usuário colar uma chave/token no chat, alertar imediatamente que precisa ser rotacionada e que NÃO deve ir pro Git.

## Stack técnico (referência rápida)

- **Odoo 19.0** — ERP base
- **Python 3.10+** — linguagem do Odoo
- **PostgreSQL 14+** — banco de dados (com extensão `pgvector` se for usar busca vetorial)
- **Node.js 20+** — só se houver integrações JS auxiliares
- **Git** — versionamento (remote: GitHub `Betotradicao/FESTA`)

## Comandos úteis

```bash
# Rodar Odoo localmente
cd odoo && python odoo-bin --addons-path=addons,../custom_addons

# Verificar se algum segredo escapou pro git stage
git diff --cached | grep -iE "(api[_-]?key|secret|password|token)" 

# Atualizar Odoo (puxar mudanças do upstream)
cd odoo && git pull origin 19.0
```
