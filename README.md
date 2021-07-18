# Criando um banco de dados no PostgreSQL

## Curso da Digital Innovation One

### Banco Carrefour Data Engineer



* **Criando o banco**

  CREATE DATABASE Financeiro;

* **Criando as tabelas "DLL"**

  1. **Criando tabela de bancos**

     CREATE TABLE IF NOT EXISTS banco (
     	numero INTEGER NOT NULL,
     	nome VARCHAR(50) NOT NULL,
     	ativo BOOLEAN NOT NULL DEFAULT TRUE,
     	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
     	PRIMARY KEY (numero)
     );

  2. **Criando tabela de agências**

     CREATE TABLE IF NOT EXISTS agencia (
     	banco_numero INTEGER NOT NULL,
     	numero INTEGER NOT NULL,
     	nome VARCHAR(80) NOT NULL,
     	ativo BOOLEAN NOT NULL DEFAULT TRUE,
     	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
     	PRIMARY KEY (banco_numero,numero),
     	FOREIGN KEY (banco_numero) REFERENCES banco (numero)
     );

  3. **Criando tabela de clientes**

     CREATE TABLE IF NOT EXISTS cliente (
     	numero BIGSERIAL PRIMARY KEY,
     	nome VARCHAR(120) NOT NULL,
     	email VARCHAR(250) NOT NULL,
     	ativo BOOLEAN NOT NULL DEFAULT TRUE,
     	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
     );

  4. **Criando tabela de contas correntes**

     CREATE TABLE IF NOT EXISTS conta_corrente (
     	banco_numero INTEGER NOT NULL,
     	agencia_numero INTEGER NOT NULL,
     	numero BIGINT NOT NULL,
     	digito SMALLINT NOT NULL,
     	cliente_numero BIGINT NOT NULL,
     	ativo BOOLEAN NOT NULL DEFAULT TRUE,
     	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
     	PRIMARY KEY (banco_numero,agencia_numero,numero,digito,cliente_numero),
     	FOREIGN KEY (banco_numero,agencia_numero) REFERENCES agencia (banco_numero,numero),
     	FOREIGN KEY (cliente_numero) REFERENCES cliente (numero)
     );

  5. **Criando tabela de tipos de transações**

     CREATE TABLE IF NOT EXISTS tipo_transacao (
     	id SMALLSERIAL PRIMARY KEY,
     	nome VARCHAR(50) NOT NULL,
     	ativo BOOLEAN NOT NULL DEFAULT TRUE,
     	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
     );

  6. **Criando tabela de clientes transações**

     CREATE TABLE IF NOT EXISTS cliente_transacoes (
     	id BIGSERIAL PRIMARY KEY,
     	banco_numero INTEGER NOT NULL,
     	agencia_numero INTEGER NOT NULL,
     	conta_corrente_numero BIGINT NOT NULL,
     	conta_corrente_digito SMALLINT NOT NULL,
     	cliente_numero BIGINT NOT NULL,
     	tipo_transacao_id SMALLINT NOT NULL,
     	valor NUMERIC(15,2) NOT NULL,
     	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
     	FOREIGN KEY (banco_numero,agencia_numero,conta_corrente_numero,conta_corrente_digito,cliente_numero) REFERENCES conta_corrente(banco_numero,agencia_numero,numero,digito,cliente_numero)
     );

     

     

     