---
title: 'Início Rápido: criar uma Azure Database for MariaDB Server – CLI do Azure'
description: Este início rápido descreve como pode utilizar a CLI do Azure para criar uma Azure Database for MariaDB Server num grupo de recursos do Azure.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 9e87dacb80aa7fc5f5b073e631fc06ae74b3ad00
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996609"
---
# <a name="create-an-azure-database-for-mariadb-server-using-azure-cli"></a>Criar uma Azure Database for MariaDB Server com a CLI do Azure
Este início rápido descreve como pode utilizar a CLI do Azure para criar uma Azure Database for MariaDB Server num grupo de recursos do Azure em cerca de cinco minutos. A CLI do Azure é utilizada para criar e gerir recursos do Azure a partir da linha de comandos ou em scripts.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Se tiver várias subscrições, escolha a subscrição adequada na qual o recurso existe ou é cobrado. Selecione um ID de subscrição específica na sua conta com o comando [az account set](/cli/azure/account#az-account-set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um [grupo de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) com o comando [az group create](/cli/azure/group#az-group-create). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo.

O exemplo seguinte cria um grupo de recursos com o nome `myresourcegroup` na localização `westus`.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mariadb-server"></a>Criar uma Azure Database for MariaDB server
Crie uma Azure Database for MariaDB server com o comando **[az mariadb server create](/cli/azure/mariadb/server#az-mariadb-server-create)**. Cada servidor pode gerir múltiplas bases de dados. Geralmente, é utilizada uma base de dados em separado para cada projeto ou para cada utilizador.

**Definição** | **Valor de exemplo** | **Descrição**
---|---|---
nome | mydemoserver | Escolha um nome exclusivo que identifique a sua Azure Database for MariaDB Server. O nome do servidor pode conter apenas letras minúsculas, números e o caráter de hífen (-). Tem de conter entre 3 e 63 carateres.
resource-group | myResourceGroup | Indique o nome do grupo de recursos do Azure.
nome de SKU | GP_Gen5_2 | O nome de SKU. Segue a convenção {escalão de preço}_{geração de computação}_{vCores} em estenografia. Consulte por baixo desta tabela para obter mais informações sobre o parâmetro de nome de SKU.
retenção de cópia de segurança | 7 | Quando tempo deve se deve reter uma cópia de segurança. A unidade é dias. O intervalo é de 7-35. 
cópia de segurança georredundante | Desativado | Se as cópias de segurança georredundantes devem estar ativadas para este servidor ou não. Valores permitidos: Ativado, Desativado.
localização | euaoeste | A localização do Azure para o servidor.
imposição de SSL | Ativado | Se SSL deve ser ativado ou não para este servidor. Valores permitidos: Ativado, Desativado.
tamanho de armazenamento | 51200 | A capacidade de armazenamento do servidor (a unidade é megabytes). O tamanho de armazenamento válido é 5120 MB no mínimo e aumenta em incrementos de 1024 MB. Consulte o documento que contém os [escalões de preço](./concepts-pricing-tiers.md) para obter mais informações sobre limites de tamanho de armazenamento. 
versão | 10.2 | A versão do motor principal de MariaDB.
utilizador administrador | myadmin | O nome de utilizador para o início de sessão do administrador. Não pode ser **azure_superuser**, **admin**, **administrador**, **raiz**, **convidado** nem **público**.
palavra-passe de administrador | Password123 | A palavra-passe do utilizador administrador. Tem de conter entre 8 e 128 carateres. A sua palavra-passe tem de conter carateres das três categorias seguintes: letras em maiúsculas inglesas, letras em minúsculas inglesas, números e carateres não alfanuméricos.

O valor do parâmetro sku-name segue a convenção {escalão de preço} \_ {geração de computação} \_ {vCores}, conforme os exemplos abaixo:
+ `--sku-name B_Gen5_4` mapeia para Básico, Ger 5 e 4 vCores.
+ `--sku-name GP_Gen5_32` mapeia para Fins Gerais, Ger 5 e 32 vCores.
+ `--sku-name MO_Gen5_2` mapeia para Otimizada para Memória, Ger 5 e 2 vCores.

Leia a documentação dos [escalões de preços](./concepts-pricing-tiers.md) para entender os valores válidos por região e por escalão.

O exemplo seguinte cria um servidor em EUA Oeste com o nome `mydemoserver` no seu grupo de recursos `myresourcegroup`, com o início de sessão de administrador do servidor `myadmin`. Este é um servidor **Ger 5** **Fins Gerais** com 2 **vCores**. O nome de um servidor mapeia para o nome DNS e, por conseguinte, é necessário para ser globalmente exclusivo no Azure. Substitua `<server_admin_password>` pelo seu próprio valor.
```azurecli-interactive
az mariadb server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 10.2
```

## <a name="configure-firewall-rule"></a>Configurar a regra de firewall
Crie uma regra de firewall ao nível da Azure Database for MariaDB server com o comando **[az mariadb server firewall-rule create](/cli/azure/mariadb/server/firewall-rule#az-mariadb-server-firewall-rule-create)**. A regra de firewall ao nível do servidor permite que uma aplicação externa, como a ferramenta de linha de comandos **mysql** ou o MySQL Workbench, se ligue ao seu servidor através da firewall do serviço Azure MariaDB. 

O exemplo seguinte cria uma regra de firewall chamada `AllowMyIP` que permite ligações a partir de um endereço IP específico, 192.168.0.1. Substitua o endereço IP ou intervalo de endereços IP que correspondem ao local onde os irá ligar. 

```azurecli-interactive
az mariadb server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> As ligações à Azure Database for MariaDB comunicam através da porta 3306. Se tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 3306 poderá não ser permitido. Se for este o caso, não pode ligar ao servidor, a menos que o departamento de TI abra a porta 3306.
> 

## <a name="configure-ssl-settings"></a>Configurar as definições de SSL
Por predefinição, são aplicadas ligações SSL entre o servidor e as aplicações cliente. Esta predefinição garante a segurança dos dados "em movimento" ao encriptar o fluxo de dados através da Internet. Para facilitar este início rápido, desative as ligações SSL no seu servidor. A desativação de SSL não é recomendada para servidores de produção. Para obter mais informações, consulte [Configure SSL connectivity in your application to securely connect to Azure Database for MariaDB](./howto-configure-ssl.md) (Configurar a conectividade SSL na sua aplicação para ligar em segurança à Azure Database for MariaDB).

O exemplo seguinte desativa a imposição de SSL no seu servidor MariaDB.
 
 ```azurecli-interactive
 az mariadb server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a>Obter as informações da ligação

Para ligar ao seu servidor, terá de fornecer credenciais de acesso e informações de anfitrião.

```azurecli-interactive
az mariadb server show --resource-group myresourcegroup --name mydemoserver
```

O resultado está no formato JSON. Aponte o **fullyQualifiedDomainName** e o **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.mariadb.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMariaDB/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
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
  "type": "Microsoft.DBforMariaDB/servers",
  "userVisibleState": "Ready",
  "version": "10.2"
}
```

## <a name="connect-to-server-using-mysql-command-line"></a>Ligar ao servidor através da linha de comandos mysql
Ligue ao servidor através da ferramenta de linha de comandos **mysql**. Pode transferir a ferramenta da linha de comandos [aqui](https://dev.mysql.com/downloads/) e instalá-lo no seu computador. Em vez disso, também pode clicar no botão **Experimentar** nos exemplos de código ou no botão `>_` na barra de ferramentas superior direita no portal do Azure e iniciar o **Azure Cloud Shell**.

Escreva os seguintes comandos: 

1. Ligar ao servidor com a ferramenta de linha de comandos **mysql**:
```azurecli-interactive
 mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
```

2. Ver o estado do servidor na linha de comandos mysql > :
```sql
status
```
Se tudo correr bem, a ferramenta de linha de comandos deve produzir o seguinte texto:

```bash
C:\Users\>mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.39.0 MariaDB Server

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.23, for Linux (x86_64)

Connection id:          64681
Current database:
Current user:           myadmin@40.118.201.21
SSL:                    Cipher in use is AES256-SHA
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.6.39.0 MariaDB Server
Protocol version:       10
Connection:             mydemoserver.mariadb.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    utf8
Conn.  characterset:    utf8
TCP port:               3306
Uptime:                 1 day 3 hours 28 min 50 sec

Threads: 10  Questions: 29002  Slow queries: 0  Opens: 33  Flush tables: 3  Open tables: 1  Queries per second avg: 0.293
--------------

mysql>
```

> [!TIP]
> Para obter comandos adicionais, veja [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) (Manual de Referência do MySQL 5.7 - Capítulo 4.5.1).

## <a name="connect-to-server-using-mysql-workbench"></a>Ligar ao servidor com o MySQL Workbench
1.  Inicie a aplicação MySQL Workbench no computador cliente. Pode transferir e instalar o MySQL Workbench [aqui](https://dev.mysql.com/downloads/workbench/).

2.  Na caixa de diálogo **Configurar Ligação Nova**, introduza as informações seguintes no separador **Parâmetros**:

   ![configurar ligação nova](./media/quickstart-create-mariadb-server-database-using-azure-cli/setup-new-connection.png)

| **Definição** | **Valor Sugerido** | **Descrição** |
|---|---|---|
|   Nome da Ligação | A Minha Ligação | Especifique uma etiqueta para esta ligação (pode ser qualquer nome) |
| Método de Ligação | escolha Standard (TCP/IP) | Utilizar o protocolo TCP/IP para ligar à Azure Database for MariaDB |
| Nome de anfitrião | mydemoserver.mariadb.database.azure.com | O nome de servidor que apontou anteriormente. |
| Porta | 3306 | É utilizada a porta predefinida para MariaDB. |
| Nome de utilizador | myadmin@mydemoserver | O início de sessão de administrador do servidor que apontou anteriormente. |
| Palavra-passe | **** | Utilize a palavra-passe da conta de administrador que configurou anteriormente. |

Clique em **Testar Ligação** para testar se todos os parâmetros estão configurados corretamente.
Agora, pode clicar na ligação para ligar com êxito ao servidor.

## <a name="clean-up-resources"></a>Limpar recursos
Se não precisa destes recursos para outro início rápido/tutorial, pode eliminá-los ao executar o seguinte comando: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Se desejar eliminar apenas o único servidor recém-criado, pode executar o comando **[az mariadb server delete](/cli/azure/mariadb/server#az-mariadb-server-delete)**.
```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

<!--
## Next steps

> [!div class="nextstepaction"]
> [Design a MariaDB Database with Azure CLI](./tutorial-design-database-using-cli.md)
-->