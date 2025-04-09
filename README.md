# Role Ansible: install_mysql_zabbix

Esta role instala e configura o banco de dados MySQL para o Zabbix, incluindo:

- Instalação do repositório oficial do Zabbix para Ubuntu 24.04
- Instalação do MySQL 8
- Criação de banco de dados e usuário para o Zabbix
- Importação do schema do Zabbix (somente se ainda não foi importado)
- Configurações necessárias para garantir a compatibilidade com o Zabbix

---

## 📦 Pacotes instalados

- `zabbix-sql-scripts`: Contém o schema SQL do Zabbix
- `python3-pymysql`: Biblioteca Python para conexão com MySQL
- `mysql-server`: Servidor MySQL 8

---

## 📂 Variáveis disponíveis

As variáveis estão localizadas no arquivo `defaults/main.yml`:

```yaml
mysql_root_password: "SenhaForte123"       # Senha do usuário root do MySQL
mysql_zabbix_user: "zabbix"                # Nome do usuário MySQL para o Zabbix
mysql_zabbix_password: "SenhaZabbix123"    # Senha do usuário Zabbix
mysql_database: "zabbix"                   # Nome do banco de dados

zabbix_repo_url: "https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0%2Bubuntu24.04_all.deb"
```

---

## ⚙️ O que esta role faz?

1. Instala o repositório oficial do Zabbix
2. Atualiza o cache de pacotes
3. Instala pacotes necessários (`zabbix-sql-scripts`, `python3-pymysql`)
4. Instala o MySQL Server 8
5. Habilita e inicia o serviço MySQL
6. Configura o usuário `root` com `mysql_native_password`
7. Cria o banco de dados `zabbix`
8. Cria o usuário `zabbix` com acesso total ao banco
9. Verifica se a base já foi criada (checando `users.ibd`)
10. Se ainda não importado:
    - Habilita `log_bin_trust_function_creators`
    - Importa o schema do Zabbix
    - Restaura `log_bin_trust_function_creators`

---

## 📥 Pré-requisitos

- Ansible 2.14+
- Coleção `community.mysql`
- Servidor Ubuntu 24.04 com acesso root

---

## 🚀 Como usar

```yaml
- hosts: zabbix_db
  become: true
  roles:
    - role: install_mysql_zabbix
```

Se necessário, sobrescreva as variáveis no playbook ou via `extra_vars`.

---

## 🔒 Segurança

- As senhas são protegidas com `no_log: true` nas tasks sensíveis
- A role verifica se o banco já foi importado, mantendo a **idempotência**

---

## 🧪 Testado em

- Ubuntu 24.04 LTS
- MySQL 8
- Zabbix 7.0 (repositório oficial)

---

## 📁 Estrutura do diretório

```
install_mysql_zabbix/
├── defaults/
│   └── main.yml
├── tasks/
│   └── main.yml
├── README.md
```

---

## 📌 Observações

- O diretório do banco é verificado diretamente (`/var/lib/mysql/zabbix`)
- A tabela `users.ibd` é usada como indicador de importação prévia
- O import do schema é feito via shell usando `zcat` e redirecionamento

