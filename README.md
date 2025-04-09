# Ansible Role: ansible-role-zabbix7-mysql-ubuntu2404

Role para instalação e configuração do **MySQL** no **Ubuntu 24.04**, com foco na preparação do ambiente para o **Zabbix 7**.  
Inclui a definição da senha do usuário `root` com plugin `mysql_native_password` para garantir compatibilidade com o frontend do Zabbix.

[![Ansible Galaxy](https://img.shields.io/badge/Ansible--Galaxy-zabbix7--mysql--ubuntu2404-blue.svg)](https://galaxy.ansible.com/josezipf/zabbix7-mysql-ubuntu2404)

---

## ⚠️ Aviso de Segurança

**ATENÇÃO:** Esta role **define uma senha para o usuário root** do MySQL.  
Ela deve ser usada **somente em instalações novas e limpas** do MySQL, onde ainda **não existe nenhuma configuração ou banco de dados criado**.

Se você executar esta role em um ambiente com MySQL previamente configurado, poderá **sobrescrever senhas existentes e perder o acesso** ao banco.

---

## ✅ Requisitos

- Sistema operacional: **Ubuntu 24.04**
- Ansible **2.12+**
- Python 3 instalado no host alvo
- Coleção **community.mysql** instalada:
  
```bash
ansible-galaxy collection install community.mysql
```

---

## 📦 Variáveis

Estas variáveis estão definidas em:

```text
defaults/main.yml
```

```yaml
# defaults file for install_mysql_zabbix

mysql_root_password: "SenhaForte123"
mysql_zabbix_user: "zabbix"
mysql_zabbix_password: "SenhaZabbix123"
mysql_database: "zabbix"

zabbix_repo_url: "https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0%2Bubuntu24.04_all.deb"
```

Você pode sobrescrevê-las diretamente no seu playbook.

---

## ▶️ Exemplo de Uso

```yaml
- name: Instalação do MySQL para o Zabbix 7
  hosts: localhost
  become: true
  vars:
    mysql_root_password: 'SenhaForteAqui'
    mysql_zabbix_user: 'zabbix'
    mysql_zabbix_password: 'SenhaZabbix'
    mysql_database: 'zabbix'
  roles:
    - josezipf.zabbix7-mysql-ubuntu2404
```

---

## 🧱 O que esta role faz

- Verifica se o sistema é Ubuntu 24.04 (e **aborta** caso não seja)
- Instala pacotes necessários (MySQL, dependências)
- Instala o repositório oficial do Zabbix 7
- Instala e configura o MySQL 8
- Define o plugin de autenticação para `mysql_native_password`
- Define a senha do usuário `root` com **idempotência**
- Cria o banco de dados `zabbix` e o usuário com permissões adequadas
- Tudo com foco em deixar pronto para a instalação do Zabbix Server

---

## 📁 Estrutura da Role

```text
ansible-role-zabbix7-mysql-ubuntu2404/
├── defaults/
│   └── main.yml
├── tasks/
│   ├── check_os.yml
│   ├── install_mysql.yml
│   ├── configure_mysql.yml
│   └── main.yml
├── meta/
│   └── main.yml
├── README.md
```

---

## 🔗 Publicação no Ansible Galaxy

Esta role está publicada e pode ser instalada diretamente com:

```bash
ansible-galaxy install josezipf.zabbix7-mysql-ubuntu2404
```

Ou sincronizada com:

```bash
ansible-galaxy login
ansible-galaxy import josezipf ansible-role-zabbix7-mysql-ubuntu2404
```

---

## ✅ Compatibilidade

| Sistema Operacional | Status     |
|---------------------|------------|
| Ubuntu 24.04        | ✅ Suportado |
| Outras versões      | ❌ Não testado |

---

## 📜 Licença

MIT

---

Criado por [@josezipf](https://github.com/josezipf)
