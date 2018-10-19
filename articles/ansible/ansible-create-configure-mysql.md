---
title: Criar e Configurar uma Base de Dados do Azure para o servidor MySQL com o Ansible (Pré-visualização)
description: Saiba como utilizar o Ansible para criar e configurar uma Base de Dados do Azure para o servidor MySQL
ms.service: ansible
keywords: ansible, azure, devops, bash, manual de procedimentos, mysql, base de dados
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/23/2018
ms.openlocfilehash: 508274d11a9693d28a9b3a01bd6ebbd7198e8711
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586705"
---
# <a name="create-and-configure-an-azure-database-for-mysql-server-using-ansible-preview"></a>Criar e Configurar uma Base de Dados do Azure para o servidor MySQL com o Ansible (pré-visualização)
A [Base de Dados do Azure para MySQL](https://docs.microsoft.com/azure/mysql/) é um serviço gerido que utiliza para executar, gerir e dimensionar as Bases de Dados MySQL de elevada disponibilidade na cloud. Este Início Rápido mostra-lhe como criar um servidor da Base de Dados do Azure para MySQL em cerca de cinco minutos com o portal do Azure. 

O Ansible permite-lhe automatizar a implementação e a configuração de recursos no seu ambiente. Este artigo mostra como utilizar o Ansible para criar uma Base de Dados do Azure para o servidor MySQL e configurar a regra de firewall em cinco minutos. 

## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure** - se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> O Ansible 2.7 é necessário para executar os manuais de procedimentos de exemplo neste tutorial. Pode instalar a versão RC do Ansible 2.7 ao executar `sudo pip install ansible[azure]==2.7.0rc2`. O Ansible 2.7 será lançado em outubro de 2018. Depois disso, não será necessário especificar a versão aqui porque a versão predefinida será 2.7.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.  

O exemplo seguinte cria um grupo de recursos com o nome **myResourceGroup** na localização **eastus**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Guarde o manual de procedimentos acima como *rg.yml*. Para executar o manual de procedimentos, utilize o comando **ansible playbook** da seguinte forma:
```bash
ansible-playbook rg.yml
```

## <a name="create-mysql-server-and-database"></a>Criar o Servidor MySQL e a Base de Dados
O exemplo seguinte cria um Servidor MySQL com o nome **mysqlserveransible** e uma Base de Dados do Azure para MySQL com o nome **mysqldbansible**. Este é um servidor de Ger 5 para Fins Básicos com 1 vCore. Tenha em atenção que o valor **mysqlserver_name** tem de ser exclusivo, e veja a documentação dos [escalões de preços](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers) para entender os valores válidos por região e por escalão. 

Introduza a sua `<server_admin_password>`:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus
    mysqlserver_name: mysqlserveransible
    mysqldb_name: mysqldbansible
    admin_username: mysqladmin
    admin_password: <server_admin_password> 
  tasks:
    - name: Create MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        sku:
          name: B_Gen5_1
          tier: Basic
        location: "{{ location }}"
        version: 5.6
        enforce_ssl: True
        admin_username: "{{ admin_username }}"
        admin_password: "{{ admin_password }}"
        storage_mb: 51200
    - name: Create instance of MySQL Database
      azure_rm_mysqldatabase:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
        name: "{{ mysqldb_name }}"
```

Guarde o manual de procedimentos acima como *mysql_create.yml*. Para executar o manual de procedimentos, utilize o comando **ansible playbook** da seguinte forma:
```bash
ansible-playbook mysql_create.yml
```

## <a name="configure-firewall-rule"></a>Configurar a regra de firewall
A regra de firewall ao nível do servidor permite que uma aplicação externa, como a ferramenta de linha de comandos **mysql** ou o MySQL Workbench, se ligue ao seu servidor através da firewall do serviço MySQL do Azure. O exemplo seguinte cria uma regra de firewall com o nome **externalaccess** que permite ligações de qualquer endereço IP externo. 

Introduza o seu **startIpAddress** e **endIpAddress** com o intervalo de endereços IP que correspondem ao local onde os irá ligar: 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
  - name: Open firewall to access MySQL Server from outside
    azure_rm_resource:
      api_version: '2017-12-01'
      resource_group: "{{ resource_group }}"
      provider: dbformysql
      resource_type: servers
      resource_name: "{{ mysqlserver_name }}"
      subresource:
        - type: firewallrules
          name: externalaccess
      body:
        properties:
          startIpAddress: "0.0.0.0"
          endIpAddress: "255.255.255.255"
```

> [!NOTE]
> As ligações à base de dados do Azure para MySQL comunicam através da porta 3306. Se tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 3306 poderá não ser permitido. Se for este o caso, não pode ligar ao servidor, a menos que o departamento de TI abra a porta 3306.
> 

Neste caso, o módulo **azure_rm_resource** é utilizado para efetuar esta tarefa, que permite a utilização direta da API REST.

Guarde o manual de procedimentos acima como *mysql_firewall.yml*. Para executar o manual de procedimentos, utilize o comando **ansible playbook** da seguinte forma:
```bash
ansible-playbook mysql_firewall.yml
```

## <a name="connect-to-the-server-using-command-line-tool"></a>Ligar ao servidor com a ferramenta de linha de comandos
Pode transferir o MySQL [aqui](https://dev.mysql.com/downloads/) e instalá-lo no seu computador. Em vez disso, também pode clicar no botão **Experimentar** nos exemplos de código ou no botão `>_` na barra de ferramentas superior direita no portal do Azure e iniciar o **Azure Cloud Shell**.

Escreva os seguintes comandos: 

1. Ligar ao servidor com a ferramenta de linha de comandos **mysql**:
```azurecli-interactive
 mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
```

2. Ver o estado do servidor:
```sql
 mysql> status
```

Se tudo correr bem, a ferramenta de linha de comandos deve produzir o seguinte texto:

```
demo@Azure:~$ mysql -h mysqlserveransible.mysql.database.azure.com -u mysqladmin@mysqlserveransible -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65233
Server version: 5.6.39.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64) using  EditLine wrapper

Connection id:          65233
Current database:
Current user:           mysqladmin@13.76.42.93
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mysqlserveransible.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 36 min 21 sec

Threads: 5  Questions: 559  Slow queries: 0  Opens: 96  Flush tables: 3  Open tables: 10  Queries per second avg: 0.256
--------------
```

## <a name="using-facts-to-query-mysql-servers"></a>Utilizar factos para consultar os Servidores MySQL
O exemplo seguinte consulta Servidores MySQL em **myResourceGroup** e, posteriormente, todas as bases de dados no servidor:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Query MySQL Servers in current resource group
      azure_rm_mysqlserver_facts:
        resource_group: "{{ resource_group }}"
      register: mysqlserverfacts

    - name: Dump MySQL Server facts
      debug:
        var: mysqlserverfacts

    - name: Query MySQL Databases
      azure_rm_mysqldatabase_facts:
        resource_group: "{{ resource_group }}"
        server_name: "{{ mysqlserver_name }}"
      register: mysqldatabasefacts

    - name: Dump MySQL Database Facts
      debug:
        var: mysqldatabasefacts
```

Guarde o manual de procedimentos acima como *mysql_query*.yml. Para executar o manual de procedimentos, utilize o comando **ansible playbook** da seguinte forma:

```bash
ansible-playbook mysql_query.yml
```

Verá então o resultado seguinte do servidor MySQL: 
```json
"servers": [
    {
        "admin_username": "mysqladmin",
        "enforce_ssl": false,
        "fully_qualified_domain_name": "mysqlserveransible.mysql.database.azure.com",
        "id": "/subscriptions/685ba005-af8d-4b04-8f16-a7bf38b2eb5a/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysqlserveransible",
        "location": "eastus",
        "name": "mysqlserveransible",
        "resource_group": "myResourceGroup",
        "sku": {
            "capacity": 1,
            "family": "Gen5",
            "name": "B_Gen5_1",
            "tier": "Basic"
        },
        "storage_mb": 5120,
        "user_visible_state": "Ready",
        "version": "5.6"
    }
]
```

E também verá o resultado seguinte da Base de Dados MySQL:
```json
"databases": [
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "information_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysql",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransibler"
    },
    {
        "charset": "latin1",
        "collation": "latin1_swedish_ci",
        "name": "mysqldbansible",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    },
    {
        "charset": "utf8",
        "collation": "utf8_general_ci",
        "name": "performance_schema",
        "resource_group": "myResourceGroup",
        "server_name": "mysqlserveransible"
    }
]
```

## <a name="clean-up-resources"></a>Limpar recursos

Se não precisar destes recursos, pode eliminá-los ao executar o exemplo abaixo. Elimina um grupo de recursos com o nome **myResourceGroup**. 

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Guarde o manual de procedimentos acima como *rg_delete*.yml. Para executar o manual de procedimentos, utilize o comando **ansible playbook** da seguinte forma:
```bash
ansible-playbook rg_delete.yml
```

Se apenas quiser eliminar o único servidor MySQL recém-criado, pode eliminá-lo ao executar o comando abaixo:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    mysqlserver_name: mysqlserveransible
  tasks:
    - name: Delete MySQL Server
      azure_rm_mysqlserver:
        resource_group: "{{ resource_group }}"
        name: "{{ mysqlserver_name }}"
        state: absent
```

Guarde o manual de procedimentos acima como *mysql_delete.yml*. Para executar o manual de procedimentos, utilize o comando **ansible playbook** da seguinte forma:
```bash
ansible-playbook mysql_delete.yml
```

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Ansible no Azure](https://docs.microsoft.com/azure/ansible/)