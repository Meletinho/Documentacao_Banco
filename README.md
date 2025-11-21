# 🛍️ HEFESTOOLS - Sistema de Mapeamento de Shoppings

## 📋 Sobre o Projeto
Sistema mobile para mapeamento de shoppings em Salvador, com direcionamento interno, controle de estacionamento e promoções.

## 🗃️ Modelagem do Banco de Dados

### DER - Diagrama Entidade-Relacionamento
![DER](https://via.placeholder.com/800x400/0088cc/ffffff?text=DER+HEFESTOOLS)

### Principais Tabelas
| Tabela | Descrição |
|--------|-----------|
| `usuario` | Cadastro de usuários (comum, lojista, admin) |
| `loja` | Lojas do shopping com localização |
| `produto` | Produtos em promoção |
| `vaga` | Controle de vagas de estacionamento |
| `rota` | Rotas de navegação entre lojas |

### Scripts Disponíveis
- [`script_criacao.sql`](script_criacao.sql) - Criação do banco e tabelas
- [`script_dados_exemplo.sql`](script_dados_exemplo.sql) - Dados para teste

## 👥 Equipe
- **Antonio Carlos** - Product Owner
- **Arthur Jesus** - UX/UI Design  
- **João Paulo** - Scrum Master
- **Pedro Henrique** - Tech Lead
- **Vinícius Sena** - Desenvolvedor

## 🛠️ Tecnologias
- **Front-End**: HTML e CSS
- **Backend**: JS
- **Banco**: MySQL
- **Mobile**: Android (React Native)
- **Versionamento**: GitHub

---

## 🔗 Como Executar

### 1. Banco de Dados
```bash
# Importar estrutura
mysql -u usuario -p < script_criacao.sql

# Importar dados exemplo  
mysql -u usuario -p < script_dados_exemplo.sql
