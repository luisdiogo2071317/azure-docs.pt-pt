---
title: Conceber a sua primeira Base de Dados do Azure para a base de dados MySQL - CLI do Azure | Microsoft Docs
description: "Este tutorial explica como criar e gerir a Base de Dados do Azure para o servidor MySQL e a base de dados através da CLI 2.0 do Azure a partir da linha de comandos."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 11/28/2017
ms.custom: mvc
ms.openlocfilehash: 5f323086ce66a504188c1834d20873a52a990311
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Conceber a sua primeira Base de Dados do Azure para a base de dados MySQL

A Base de Dados do Azure para MySQL é um serviço de base de dados relacional na cloud da Microsoft baseado no motor de base de dados da Edição de Comunidade do MySQL. Neste tutorial, utiliza a CLI do Azure (interface de linha de comandos) e outros utilitários para saber como:

> [!div class="checklist"]
> * Criar uma Base de Dados do Azure para MySQL
> * Configurar a firewall do servidor
> * Utilize a [ferramenta da linha de comandos mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) para criar uma base de dados
> * Carregar os dados de exemplo
> * Consultar dados
> * Atualizar dados
> * Restaurar dados

Pode utilizar o Azure Cloud Shell no browser ou [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli) no seu computador para executar os blocos de código neste tutorial.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Se tiver várias subscrições, escolha a subscrição adequada na qual o recurso existe ou é cobrado. Selecione um ID de subscrição específica na sua conta com o comando [az account set](/cli/azure/account#az_account_set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um [grupo de recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) com o comando [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos como um grupo.

O exemplo seguinte cria um grupo de recursos com o nome `mycliresource` na localização `westus`.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Criar uma Base de Dados do Azure para o servidor MySQL
Crie uma Base de Dados do Azure para o servidor MySQL com o comando az mysql server create. Cada servidor pode gerir múltiplas bases de dados. Geralmente, é utilizada uma base de dados em separado para cada projeto ou para cada utilizador.

O exemplo seguinte cria uma Base de Dados do Azure para o servidor MySQL localizada em `westus` no grupo de recursos `mycliresource` com o nome `mycliserver`. O servidor tem um início de sessão de administrador com o nome `myadmin` e a palavra-passe `Password01!`. É criado com o escalão de desempenho **Básico** e com **50** unidades de computação partilhadas entre todas as bases de dados do servidor. Pode aumentar ou reduzir verticalmente a computação e o armazenamento, dependendo das necessidades da aplicação.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Configurar a regra de firewall
Crie uma regra de firewall ao nível da Base de Dados do Azure para o servidor MySQL com o comando az mysql server firewall-rule create. A regra de firewall ao nível do servidor permite que uma aplicação externa, como a ferramenta de linha de comandos **mysql** ou o MySQL Workbench, se ligue ao seu servidor através da firewall do serviço Azure MySQL. 

O exemplo seguinte cria uma regra de firewall para um intervalo de endereços predefinidos. Este exemplo mostra o intervalo completo possível de endereços IP.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a>Obter as informações da ligação

Para ligar ao seu servidor, terá de fornecer credenciais de acesso e informações de anfitrião.
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

O resultado está no formato JSON. Aponte o **fullyQualifiedDomainName** e o **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-mysql"></a>Ligar ao servidor com o mysql
Utilize a [ferramenta de linha de comandos do mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) para ligar à Base de Dados do Azure para o servidor MySQL. Neste exemplo, o comando é:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Criar uma base de dados vazia
Assim que estiver ligado ao servidor, crie uma base de dados vazia.
```sql
mysql> CREATE DATABASE mysampledb;
```

Na linha de comandos, execute o comando seguinte para mudar a ligação para a base de dados recentemente criada:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Criar tabelas na base de dados
Agora que sabe como ligar à Base de Dados do Azure para MySQL, conclua algumas tarefas básicas:

Em primeiro lugar, crie uma tabela e carregue-a com alguns dados. Vamos criar uma tabela que armazena informações de inventário.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Carregar dados para as tabelas
Agora que tem uma tabela, insira alguns dados na mesma. Na janela da linha de comandos aberta, execute a seguinte consulta para inserir algumas linhas de dados.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Tem agora duas linhas de dados de exemplo nas tabelas que criou anteriormente.

## <a name="query-and-update-the-data-in-the-tables"></a>Consultar e atualizar os dados nas tabelas
Execute a seguinte consulta para obter informações da tabela de base de dados.
```sql
SELECT * FROM inventory;
```

Também pode atualizar os dados nas tabelas.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

A linha é atualizada em conformidade ao obter dados.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Restaurar uma base de dados para um ponto anterior no tempo
Imagine que eliminou acidentalmente esta tabela. É algo de que não é fácil recuperar. A Base de Dados do Azure para MySQL permite-lhe voltar atrás para qualquer ponto anterior no tempo até aos últimos 35 dias e restaurar este ponto anterior no tempo para um novo servidor. Pode utilizar este servidor novo para recuperar os dados eliminados. Os passos seguintes restauram o servidor de exemplo para um ponto antes da tabela ter sido adicionada.

Para o restauro, precisa das seguintes informações:

- Ponto de restauro: selecione uma hora antes do servidor ter sido alterado. Tem de ser maior ou igual ao valor da Cópia de segurança mais antiga da base de dados de origem.
- Servidor de destino: indique um novo nome de servidor para o qual pretende restaurar
- Servidor de origem: indique o nome do servidor do qual pretende restaurar
- Localização: não pode selecionar a região, por predefinição é igual ao servidor de origem

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

Para restaurar o servidor e [restaurar para um momento específico](./howto-restore-server-portal.md) antes da tabela ter sido eliminada. Restaurar um servidor para outro ponto anterior no tempo cria um novo servidor duplicado como o servidor original a partir do ponto anterior no tempo que especificar, desde que esteja dentro do período de retenção da sua [camada de serviço](./concepts-service-tiers.md).

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a:
> [!div class="checklist"]
> * Criar uma Base de Dados do Azure para MySQL
> * Configurar a firewall do servidor
> * Utilize a [ferramenta da linha de comandos mysql](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) para criar uma base de dados
> * Carregar os dados de exemplo
> * Consultar dados
> * Atualizar dados
> * Restaurar dados

> [!div class="nextstepaction"]
> [Base de Dados do Azure para MySQL - Amostras da CLI do Azure](./sample-scripts-azure-cli.md)
