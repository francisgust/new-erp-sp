# new-erp-sp
Esta é uma documentação sugerida, formatada em Markdown, para representar o projeto de criação e carga de banco de dados SQL no GitHub.

***

# Projeto de Criação e Carga de Banco de Dados ERP (MySQL)

## 🎯 Visão Geral do Projeto

Este repositório contém o script SQL (`.sql`) completo para criar e popular um banco de dados de sistema de Planejamento de Recursos Empresariais (**ERP**), utilizando o **MySQL**. O banco de dados principal criado é denominado `erp_db`.

O objetivo deste projeto é demonstrar a **criação da estrutura de tabelas, o estabelecimento de relacionamentos (Foreign Keys) e a importação eficiente de grandes volumes de dados** utilizando o comando `LOAD DATA INFILE`.

## 🛠️ Tecnologias e Ferramentas

*   **SGBD:** MySQL
*   **Linguagem:** SQL (DDL e DML)
*   **Importação de Dados:** Arquivos CSV (Comma-Separated Values)

O comando **LOAD DATA INFILE** é a ferramenta mais rápida e eficaz para transferir listas extensas de dados de um arquivo de texto simples, como CSV, para o sistema MySQL, economizando horas de trabalho manual.

## 📂 Estrutura do Banco de Dados (`erp_db`)

O banco de dados é composto por várias tabelas que gerenciam informações centrais de um sistema ERP, incluindo produtos, pedidos, clientes e funcionários.

### Tabelas Criadas:

1.  **Categories**
2.  **Products**
3.  **Suppliers**
4.  **Customers**
5.  **Shippers**
6.  **Orders**
7.  **OrderDetails**
8.  **Employees**

### Relacionamentos (Chaves Estrangeiras):

*   A tabela `Products` possui chaves estrangeiras (`fk_sup` e `fk_cat`) referenciando `Suppliers` e `Categories`, respectivamente.
*   A tabela `Orders` possui chaves estrangeiras referenciando `Customers` (`fk_cust`), `Shippers` (`fk_shi`), e `Employees` (`fk_emp`).
*   A tabela `OrderDetails` possui chaves estrangeiras referenciando `Orders` (`fk_ord`) e `Products` (`fk_det`).

## 🚀 Configuração e Carga de Dados

O script SQL realiza a criação da estrutura e a inserção dos dados logo em seguida.

### 1. Comando LOAD DATA INFILE

A sintaxe básica utilizada para a importação é: `LOAD DATA INFILE 'caminho/para/arquivo.csv' INTO TABLE nome_da_tabela FIELDS TERMINATED BY 'delimitador'`.

**Configurações de Importação:**

*   **Delimitador de Campos:** O delimitador de campo utilizado nos arquivos CSV do projeto é o ponto e vírgula (`;`), conforme demonstrado nos comandos de carga para as tabelas `Categories`, `Suppliers`, `Customers`, `Shippers`, `Orders`, `OrderDetails` e `Employees`. A cláusula `FIELDS TERMINATED BY ';'` é utilizada para ajustar o delimitador, visto que a vírgula é o padrão.
*   **Delimitador de Texto:** Não há delimitador de texto (aspas) (`ENCLOSED BY ''`) na maioria dos carregamentos.
*   **Terminador de Linha:** O terminador de linha utilizado é o `\n`.
*   **Ignorar Cabeçalho:** A cláusula `IGNORE 1 ROWS` é usada em todas as cargas para ignorar a primeira linha do arquivo CSV, que geralmente contém os nomes das colunas.

### 2. Caminhos dos Arquivos CSV

É crucial notar que todos os comandos `LOAD DATA INFILE` estão apontando para um caminho específico no sistema operacional: **`C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/`**.

> **Atenção:** Se você receber um erro de permissão (acessibilidade do arquivo), pode ser necessário ajustar as permissões do arquivo CSV ou movê-lo para um diretório acessível pelo MySQL, ou ainda modificar o caminho no script.

### 3. Tratamento e Transformação de Dados

O script realiza transformações inline durante a importação para garantir a integridade e o formato dos dados:

| Tabela | Campo | Transformação | Comando Utilizado | Referência |
| :--- | :--- | :--- | :--- | :--- |
| **Products** | `Price` | Substitui a vírgula (`,`) por ponto (`.`) para garantir o formato numérico (`DOUBLE(10,2)`). | `set Price = replace(@Price,",",".");` | |
| **Orders** | `OrderDate` | Converte a string da data para o formato `DATE`, usando o padrão `%d/%m/%Y`. | `set OrderDate = str_to_date(@OrderData, "%d/%m/%Y");` | |
| **Employees** | `BirthDate` | Converte a string da data de nascimento para o formato `DATE`, usando o padrão `%d/%m/%Y`. | `set BirthDate = str_to_date(@BirthDate,"%d/%m/%Y");` | |


