# 🛍️ HEFESTOOLS - Sistema de Mapeamento de Shoppings

## 📋 Sobre o Projeto
Sistema mobile para mapeamento de shoppings em Salvador, com direcionamento interno, controle de estacionamento e promoções.

## 🗃️ Modelagem do Banco de Dados

### DER - Diagrama Entidade-Relacionamento
<img width="2374" height="1231" alt="DiagramaER_Shopping" src="https://github.com/user-attachments/assets/23e96c58-007e-49d6-ae01-c240d11bdbb7" />

### Principais Tabelas
| Tabela | Descrição |
|--------|-----------|
| `usuario` | Cadastro de usuários (comum, lojista, admin) |
| `loja` | Lojas do shopping com localização |
| `produto` | Produtos em promoção |
| `vaga` | Controle de vagas de estacionamento |
| `rota` | Rotas de navegação entre lojas |

### Scripts Disponíveis
- -- Banco de dados: hefestools_shopping
CREATE DATABASE IF NOT EXISTS hefestools_shopping;
USE hefestools_shopping;

-- Tabela de usuários
CREATE TABLE usuario (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    senha VARCHAR(255) NOT NULL,
    tipo ENUM('comum', 'lojista', 'admin') NOT NULL DEFAULT 'comum',
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    ativo BOOLEAN DEFAULT TRUE
);

-- Tabela de lojas
CREATE TABLE loja (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    andar VARCHAR(50),
    coordenadas VARCHAR(100), -- formato: "x,y" ou JSON
    ativa BOOLEAN DEFAULT TRUE,
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabela de lojistas (relação usuário-loja)
CREATE TABLE lojista (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_usuario INT NOT NULL,
    id_loja INT NOT NULL,
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_usuario) REFERENCES usuario(id),
    FOREIGN KEY (id_loja) REFERENCES loja(id),
    UNIQUE KEY unique_lojista_loja (id_usuario, id_loja)
);

-- Tabela de produtos
CREATE TABLE produto (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    preco_original DECIMAL(10,2),
    preco_promocao DECIMAL(10,2),
    valido_ate DATE NOT NULL,
    id_loja INT NOT NULL,
    ativo BOOLEAN DEFAULT TRUE,
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_loja) REFERENCES loja(id)
);

-- Tabela de promoções (opcional, pode ser integrada com produtos)
CREATE TABLE promocao (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_produto INT NOT NULL,
    id_loja INT NOT NULL,
    data_inicio DATE NOT NULL,
    data_fim DATE NOT NULL,
    ativa BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (id_produto) REFERENCES produto(id),
    FOREIGN KEY (id_loja) REFERENCES loja(id)
);

-- Tabela de vagas de estacionamento
CREATE TABLE vaga (
    id INT PRIMARY KEY AUTO_INCREMENT,
    numero VARCHAR(10) NOT NULL UNIQUE,
    status ENUM('livre', 'ocupada') DEFAULT 'livre',
    atualizado_em DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    ativa BOOLEAN DEFAULT TRUE
);

-- Tabela de rotas (para navegação entre lojas)
CREATE TABLE rota (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_origem INT NOT NULL,
    id_destino INT NOT NULL,
    acessivel BOOLEAN DEFAULT FALSE,
    coordenadas TEXT, -- JSON com pontos da rota
    FOREIGN KEY (id_origem) REFERENCES loja(id),
    FOREIGN KEY (id_destino) REFERENCES loja(id)
);

-- Tabela de logs de atualização de vagas (para auditoria)
CREATE TABLE log_vaga (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_vaga INT NOT NULL,
    status_anterior ENUM('livre', 'ocupada'),
    status_novo ENUM('livre', 'ocupada'),
    atualizado_por INT, -- id do admin se foi manual
    atualizado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    tipo_atualizacao ENUM('automatica', 'manual'),
    FOREIGN KEY (id_vaga) REFERENCES vaga(id),
    FOREIGN KEY (atualizado_por) REFERENCES usuario(id)
);[script sql.sql](https://github.com/user-attachments/files/23667019/script.sql.sql)
 - Criação do banco e tabelas
- -- Banco de dados: hefestools_shopping
CREATE DATABASE IF NOT EXISTS hefestools_shopping;
USE hefestools_shopping;

-- Tabela de usuários
CREATE TABLE usuario (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    senha VARCHAR(255) NOT NULL,
    tipo ENUM('comum', 'lojista', 'admin') NOT NULL DEFAULT 'comum',
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    ativo BOOLEAN DEFAULT TRUE
);

-- Tabela de lojas
CREATE TABLE loja (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    andar VARCHAR(50),
    coordenadas VARCHAR(100), -- formato: "x,y" ou JSON
    ativa BOOLEAN DEFAULT TRUE,
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabela de lojistas (relação usuário-loja)
CREATE TABLE lojista (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_usuario INT NOT NULL,
    id_loja INT NOT NULL,
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_usuario) REFERENCES usuario(id),
    FOREIGN KEY (id_loja) REFERENCES loja(id),
    UNIQUE KEY unique_lojista_loja (id_usuario, id_loja)
);

-- Tabela de produtos
CREATE TABLE produto (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT,
    preco_original DECIMAL(10,2),
    preco_promocao DECIMAL(10,2),
    valido_ate DATE NOT NULL,
    id_loja INT NOT NULL,
    ativo BOOLEAN DEFAULT TRUE,
    criado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_loja) REFERENCES loja(id)
);

-- Tabela de promoções (opcional, pode ser integrada com produtos)
CREATE TABLE promocao (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_produto INT NOT NULL,
    id_loja INT NOT NULL,
    data_inicio DATE NOT NULL,
    data_fim DATE NOT NULL,
    ativa BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (id_produto) REFERENCES produto(id),
    FOREIGN KEY (id_loja) REFERENCES loja(id)
);

-- Tabela de vagas de estacionamento
CREATE TABLE vaga (
    id INT PRIMARY KEY AUTO_INCREMENT,
    numero VARCHAR(10) NOT NULL UNIQUE,
    status ENUM('livre', 'ocupada') DEFAULT 'livre',
    atualizado_em DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    ativa BOOLEAN DEFAULT TRUE
);

-- Tabela de rotas (para navegação entre lojas)
CREATE TABLE rota (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_origem INT NOT NULL,
    id_destino INT NOT NULL,
    acessivel BOOLEAN DEFAULT FALSE,
    coordenadas TEXT, -- JSON com pontos da rota
    FOREIGN KEY (id_origem) REFERENCES loja(id),
    FOREIGN KEY (id_destino) REFERENCES loja(id)
);

-- Tabela de logs de atualização de vagas (para auditoria)
CREATE TABLE log_vaga (
    id INT PRIMARY KEY AUTO_INCREMENT,
    id_vaga INT NOT NULL,
    status_anterior ENUM('livre', 'ocupada'),
    status_novo ENUM('livre', 'ocupada'),
    atualizado_por INT, -- id do admin se foi manual
    atualizado_em DATETIME DEFAULT CURRENT_TIMESTAMP,
    tipo_atualizacao ENUM('automatica', 'manual'),
    FOREIGN KEY (id_vaga) REFERENCES vaga(id),
    FOREIGN KEY (atualizado_por) REFERENCES usuario(id)
);[script sql.sql](https://github.com/user-attachments/files/23667028/script.sql.sql)
 - Dados para teste

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
