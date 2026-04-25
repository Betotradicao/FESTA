# FESTA — Sistema de Locação de Festas

Sistema customizado em cima do **Odoo 19.0** para gerenciamento de locação de itens para festas.

## Estrutura do projeto

- `odoo/` — clone do Odoo oficial (não versionado neste repo, ver instruções abaixo)
- `custom_addons/` — módulos customizados deste projeto (em desenvolvimento)

## Como rodar localmente

### 1. Clonar o Odoo

```bash
git clone https://github.com/odoo/odoo.git --depth 1 --branch 19.0
```

### 2. Pré-requisitos

- Python 3.10+
- PostgreSQL 14+
- Git

### 3. Instalar dependências do Odoo

```bash
cd odoo
pip install -r requirements.txt
```

### 4. Rodar o Odoo

```bash
python odoo-bin --addons-path=addons,../custom_addons
```

## Segurança

Este projeto segue boas práticas de segurança desde o início. Consulte [SECURITY.md](SECURITY.md) para detalhes. Regras principais:

- Nenhum segredo (senha, token, chave) é commitado — tudo via `.env`
- Variáveis sensíveis listadas em `.env.example` (sem valores reais)
- Dependências sempre atualizadas
- HTTPS obrigatório em produção
- Validação de entrada em todos os endpoints
- Logs sem dados sensíveis (senhas, tokens, dados pessoais)

## Status

🚧 Projeto em desenvolvimento inicial.
