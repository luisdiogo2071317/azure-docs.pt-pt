---
title: 'Início Rápido: Criar um servidor da Base de Dados do Azure para MySQL - CLI do Azure'
description: Este guia de introdução descreve como utilizar a CLI do Azure para criar uma Base de Dados do Azure para o servidor MySQL num grupo de recursos do Azure.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 04/01/2018
ms.custom: mvc
ms.openlocfilehash: 3ccf2f2e6fbad6c1a1acc4b3e827a3072d8a0cb7
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293820"
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Criar uma Base de Dados do Azure para o servidor MySQL com a CLI do Azure
Este guia de introdução descreve como utilizar a CLI do Azure para criar uma Base de Dados do Azure para o servidor MySQL num grupo de recursos do Azure em cerca de cinco minutos. A CLI do Azure é utilizada para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Se tiver várias subscrições, escolha a subscrição adequada na qual o recurso existe ou é cobrado. Selecione um ID de subscrição específica na sua conta com o comando [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um [grupo de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo.

O exemplo seguinte cria um grupo de recursos com o nome `myresourcegroup` na localização `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Criar uma Base de Dados do Azure para o servidor MySQL
Crie uma Base de Dados do Azure para o servidor MySQL com o comando **[az mysql server create](/cli/azure/mysql/server#az_mysql_server_create)**. Cada servidor pode gerir múltiplas bases de dados. Geralmente, é utilizada uma base de dados em separado para cada projeto ou para cada utilizador.

O exemplo seguinte cria um servidor em EUA Oeste com o nome `mydemoserver` no seu grupo de recursos `myresourcegroup`, com o início de sessão de administrador do servidor `myadmin`. Este é um servidor d**Gen 4** **Fins Gerais** com 2 **vCores**. O nome de um servidor mapeia para o nome DNS e, por conseguinte, é necessário para ser globalmente exclusivo no Azure. Substitua `<server_admin_password>` pelo seu próprio valor.
```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 5.7
```

## <a name="configure-firewall-rule"></a>Configurar a regra de firewall
Crie uma regra de firewall ao nível da Base de Dados do Azure para o servidor MySQL com o comando **[az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_create)**. A regra de firewall ao nível do servidor permite que uma aplicação externa, como a ferramenta de linha de comandos **mysql.exe** ou o MySQL Workbench, se ligue ao seu servidor através da firewall do serviço Azure MySQL. 

O exemplo seguinte cria uma regra de firewall chamada `AllowMyIP` que permite ligações a partir de um endereço IP específico, 192.168.0.1. Substitua o endereço IP ou intervalo de endereços IP que correspondem ao local onde os irá ligar. 

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> As ligações à base de dados do Azure para MySQL comunicam através da porta 3306. Se tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 3306 poderá não ser permitido. Se for este o caso, não pode ligar ao servidor, a menos que o departamento de TI abra a porta 3306.
> 


## <a name="configure-ssl-settings"></a>Configurar as definições de SSL
Por predefinição, são aplicadas ligações SSL entre o servidor e as aplicações cliente. Esta predefinição garante a segurança dos dados "em movimento" ao encriptar o fluxo de dados através da Internet. Para facilitar este início rápido, desative as ligações SSL no seu servidor. A desativação de SSL não é recomendada para servidores de produção. Para obter mais informações, veja [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md) (Configurar a conectividade SSL na sua aplicação para ligar em segurança à Base de Dados do Azure para MySQL).

O exemplo seguinte desativa a aplicação de SSL no seu servidor MySQL.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Obter as informações da ligação

Para ligar ao seu servidor, terá de fornecer credenciais de acesso e informações de anfitrião.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name mydemoserver
```

O resultado está no formato JSON. Aponte o **fullyQualifiedDomainName** e o **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a>Ligar ao servidor com a ferramenta de linha de comandos mysql.exe
Ligue ao servidor com a ferramenta de linha de comandos **mysql.exe**. Pode transferir o MySQL [aqui](https://dev.mysql.com/downloads/) e instalá-lo no seu computador. Em vez disso, também pode clicar no botão **Experimentar** nos exemplos de código ou no botão `>_` na barra de ferramentas superior direita no portal do Azure e iniciar o **Azure Cloud Shell**.

Escreva os seguintes comandos: 

1. Ligar ao servidor com a ferramenta de linha de comandos **mysql**:
```azurecli-interactive
 mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
```

2. Ver o estado do servidor:
```sql
 mysql> status
```
Se tudo correr bem, a ferramenta de linha de comandos deve produzir o seguinte texto:

```dos
C:\Users\>mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             mydemoserver.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Para obter comandos adicionais, veja [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) (Manual de Referência do MySQL 5.7 - Capítulo 4.5.1).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Ligar ao servidor com a ferramenta Workbench GUI do MySQL
1.  Inicie a aplicação MySQL Workbench no computador cliente. Pode transferir e instalar o MySQL Workbench [aqui](https://dev.mysql.com/downloads/workbench/).

2.  Na caixa de diálogo **Configurar Ligação Nova**, introduza as informações seguintes no separador **Parâmetros**:

   ![configurar ligação nova](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Definição** | **Valor Sugerido** | **Descrição** |
|---|---|---|
|   Nome da Ligação | A Minha Ligação | Especifique uma etiqueta para esta ligação (pode ser qualquer nome) |
| Método de Ligação | escolha Standard (TCP/IP) | Utilizar o protocolo TCP/IP para ligar à Base de Dados do Azure para MySQL> |
| Nome de anfitrião | mydemoserver.mysql.database.azure.com | O nome de servidor que apontou anteriormente. |
| Porta | 3306 | É utilizada a porta predefinida para MySQL. |
| Nome de utilizador | myadmin@mydemoserver | O início de sessão de administrador do servidor que apontou anteriormente. |
| Palavra-passe | **** | Utilize a palavra-passe da conta de administrador que configurou anteriormente. |

Clique em **Testar Ligação** para testar se todos os parâmetros estão configurados corretamente.
Agora, pode clicar na ligação para ligar com êxito ao servidor.

## <a name="clean-up-resources"></a>Limpar recursos
Se não precisa destes recursos para outro início rápido/tutorial, pode eliminá-los ao executar o seguinte comando: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Se apenas quiser eliminar o único servidor recém-criado, pode executar o comando **[az mysql server delete](/cli/azure/mysql/server#az_mysql_server_delete)**.
```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Design a MySQL Database with Azure CLI](./tutorial-design-database-using-cli.md) (Conceber uma Base de Dados MySQL com a CLI do Azure)
