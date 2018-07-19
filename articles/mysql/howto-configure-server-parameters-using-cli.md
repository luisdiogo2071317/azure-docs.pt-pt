---
title: Configure os parâmetros de serviço na base de dados do Azure para MySQL
description: Este artigo descreve como configurar os parâmetros de serviço na base de dados do Azure para MySQL com o utilitário de linha de comandos da CLI do Azure.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 07/18/2018
ms.openlocfilehash: 637e2d27e92c1a2618fcf8b524e475a4d2f88f12
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136377"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Personalizar parâmetros de configuração do servidor com a CLI do Azure
Pode listar, mostrar e atualizar os parâmetros de configuração para uma base de dados do Azure para o servidor MySQL com a CLI do Azure, o utilitário de linha de comandos do Azure. Um subconjunto de configurações de mecanismos é exposto ao nível do servidor e pode ser modificado. 

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, terá de:
- [Uma base de dados do Azure para o servidor MySQL](quickstart-create-mysql-server-database-using-azure-cli.md)
- [CLI 2.0 do Azure](/cli/azure/install-azure-cli) utilitário da linha de comandos ou utilize o Azure Cloud Shell no browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Lista de parâmetros de configuração do servidor da base de dados do Azure para o servidor MySQL
Para listar todos os parâmetros modificáveis num servidor e os respetivos valores, execute o [lista de configuração do az mysql server](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) comando.

Pode listar os parâmetros de configuração de servidor para o servidor **mydemoserver.mysql.database.azure.com** no grupo de recursos **myresourcegroup**.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
A definição de cada um dos parâmetros listados, consulte a secção de referência do MySQL sobre [variáveis de sistema do servidor](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Mostrar detalhes de parâmetro de configuração do servidor
Para mostrar os detalhes sobre um parâmetro de configuração específica para um servidor, execute o [show do az mysql server configuration](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_show) comando.

Este exemplo mostra detalhes sobre o **lenta\_consulta\_log** parâmetro de configuração do servidor do servidor **mydemoserver.mysql.database.azure.com** no grupo de recursos **myresourcegroup.**
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>Modificar um valor de parâmetro de configuração do servidor
Também pode modificar o valor de um determinado servidor parâmetro de configuração, que atualiza o valor de configuração subjacente para o motor do MySQL server. Para atualizar a configuração, utilize o [az mysql server configuration set](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) comando. 

Para atualizar o **lenta\_consulta\_log** parâmetro de configuração do servidor do server **mydemoserver.mysql.database.azure.com** no grupo de recursos  **myresourcegroup.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Se pretender repor o valor de um parâmetro de configuração, omita o opcional `--value` parâmetro e o serviço aplica-se o valor predefinido. No exemplo anterior, teria o seguinte aspeto:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Este código redefinirá o **lenta\_consulta\_log** configuração para o valor predefinido **OFF**. 

## <a name="working-with-the-time-zone-parameter"></a>Trabalhar com o parâmetro de fuso horário

### <a name="populating-the-time-zone-tables"></a>Preencher as tabelas de fuso horário

As tabelas de fuso horário no seu servidor podem ser preenchidas chamando o `az_load_timezone` procedimento armazenado a partir de uma ferramenta como a linha de comandos MySQL ou o MySQL Workbench.

> [!NOTE]
> Se estiver a executar o `az_load_timezone` comando a partir do MySQL Workbench, poderá ter de desativar o modo de atualização segura primeiro usando `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Para ver os valores de fuso horário disponível, execute o seguinte comando:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Definir o fuso de horário de nível global

O fuso de horário de nível global pode ser definido utilizando o [az mysql server configuration set](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) comando.

O seguinte comando atualizações a **tempo\_zona** parâmetro de configuração do servidor do servidor de **mydemoserver.mysql.database.azure.com** no grupo de recursos  **myresourcegroup** para **E.U.A. / Pacífico**.

```azurecli-interactive
az mysql server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Definir o fuso de horário de nível de sessão

A sessão de nível de tempo de zona pode ser definida ao executar o `SET time_zone` comando a partir de uma ferramenta como a linha de comandos MySQL ou o MySQL Workbench. O exemplo abaixo define o fuso horário o **E.U.A. / Pacífico** fuso horário.  

```sql
SET time_zone = 'US/Pacific';
```

Consulte a documentação do MySQL para [funções de tempo de data e](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz).


## <a name="next-steps"></a>Passos Seguintes

- Como configurar [parâmetros do servidor no portal do Azure](howto-server-parameters.md)