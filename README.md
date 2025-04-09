# Ansible Role: ansible-role-zabbix7-mysql-ubuntu2404

Role para instalação e configuração do **MySQL** no **Ubuntu 24.04** com foco na preparação do ambiente para o **Zabbix 7**. Inclui a definição da senha do usuário `root` com plugin `mysql_native_password` para garantir compatibilidade com o frontend do Zabbix.

---

## ⚠️ Aviso de Segurança

**ATENÇÃO:** Esta role **define uma senha para o usuário root** do MySQL.  
Ela deve ser usada **somente em instalações novas e limpas** do MySQL, onde ainda **não existe nenhuma configuração ou banco de dados criado**.

Caso rode essa role em um ambiente com MySQL já configurado, **pode sobrescrever senhas existentes e causar perda de acesso.**

---

## Requisitos

- Sistema operacional: Ubuntu 24.04
- Ansible 2.12+
- Python 3
- Role `community.mysql` instalada (instalar via `ansible-galaxy collection install community.mysql`)
- Instância com MySQL ainda não configurado (fresh install)

---

## Variáveis

Estas variáveis estão definidas no arquivo:

```yaml
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

Você pode sobrescrever esses valores ao utilizar a role em seu playbook.

---

## Exemplo de uso

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
    - ansible-role-zabbix7-mysql-ubuntu2404
```

---

## Estrutura da Role

- Instala pacotes necessários
- Garante que o MySQL está presente
- Configura o MySQL para usar `mysql_native_password`
- Define senha do usuário `root` com idempotência
- Cria banco de dados e usuário do Zabbix
- Verifica se o sistema é Ubuntu 24.04 (senão, aborta)

---

## Compatibilidade

- ✅ Ubuntu 24.04
- ❌ Não testado em outras versões

---

## Publicação no Galaxy

Este repositório segue a estrutura oficial de roles para publicação no Ansible Galaxy.  
Nome sugerido no Galaxy:

```
josezipf.zabbix7_mysql_ubuntu2404
```

Para sincronizar:

```bash
ansible-galaxy login
ansible-galaxy import josezipf ansible-role-zabbix7-mysql-ubuntu2404
```

---

[![Ansible Galaxy](https://img.shields.io/badge/Ansible--Galaxy-zabbix7--mysql--ubuntu2404-blue.svg)](https://galaxy.ansible.com/josezipf/zabbix7_mysql_ubuntu2404)


## License

MIT

---



## Autor

- José Zipf – [nototi.com.br](https://nototi.com.br)
