---
title: Configure os parâmetros de serviço na base de dados do Azure para MySQL
description: Este artigo descreve como configurar os parâmetros de serviço na base de dados do Azure para MySQL utilizando o utilitário de linha de comandos da CLI do Azure.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 4c04cb77513ec070edce739aa0a49447dc915a1b
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265219"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Personalizar parâmetros de configuração de servidor utilizando a CLI do Azure
Pode listar, mostrar e atualizar os parâmetros de configuração para uma base de dados do Azure para o servidor de MySQL, utilizando a CLI do Azure, o utilitário da linha de comandos do Azure. Um subconjunto das configurações de motor está exposto ao nível do servidor e pode ser modificado. 

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, tem de:
- [Uma base de dados do Azure para o servidor de MySQL](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) utilitário da linha de comandos ou utilize a Shell de nuvem do Azure no browser.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Lista os parâmetros de configuração do servidor da base de dados do Azure para o servidor de MySQL
Para listar todos os parâmetros modificável no servidor e os respetivos valores, execute o [lista de configuração de servidor de mysql az](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) comando.

Pode listar os parâmetros de configuração de servidor para o servidor **mydemoserver.mysql.database.azure.com** no grupo de recursos **myresourcegroup**.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
Para a definição de cada um dos parâmetros listados, consulte a secção de referência de MySQL no [variáveis de servidor do sistema](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Mostrar detalhes de parâmetro de configuração do servidor
Para mostrar os detalhes sobre um parâmetro de configuração específica para um servidor, execute o [mostrar de configuração de servidor de mysql az](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_show) comando.

Este exemplo mostra detalhes sobre o **lenta\_consulta\_registo** parâmetro de configuração de servidor para servidor **mydemoserver.mysql.database.azure.com** sob o grupo de recursos **myresourcegroup.**
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>Modificar um valor de parâmetro de configuração do servidor
Também pode modificar o valor de um determinado servidor parâmetro de configuração, que atualiza o valor de configuração subjacente para o motor do MySQL. Para atualizar a configuração, utilize o [az mysql server configuration set](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) comando. 

Para atualizar o **lenta\_consulta\_registo** parâmetro de configuração do servidor do server **mydemoserver.mysql.database.azure.com** no grupo de recursos  **myresourcegroup.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Se pretender repor o valor de um parâmetro de configuração, omita o opcional `--value` parâmetro e o serviço aplica-se o valor predefinido. Por exemplo acima, este seria aspeto:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Este código repõe o **lenta\_consulta\_registo** configuração para o valor predefinido **OFF**. 

## <a name="next-steps"></a>Passos Seguintes
- Como configurar [parâmetros de servidor no portal do Azure](howto-server-parameters.md)