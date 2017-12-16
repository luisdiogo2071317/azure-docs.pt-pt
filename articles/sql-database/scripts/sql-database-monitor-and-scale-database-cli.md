---
title: "A base de dados SQL do Azure de exemplo-monitor escala único CLI | Microsoft Docs"
description: Script de exemplo do CLI do Azure para monitorizar e dimensionar uma base de dados SQL do Azure
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 12/14/2017
ms.author: janeng
ms.openlocfilehash: 741c066d62364e34b788883bfc96fba1ea3507c3
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>Utilize o CLI para monitorizar e dimensionar uma base de dados SQL

Neste exemplo de script da CLI do Azure dimensiona uma única base de dados do SQL do Azure para um nível de desempenho diferente depois de consultar as informações de tamanho da base de dados. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

> [!TIP]
> Utilizar [lista do az sql db op](/cli/azure/sql/db/op?#az_sql_db_op_list) para obter uma lista de operações executadas na base de dados e utilize [Cancelar do az sql db op](/cli/azure/sql/db/op#az_sql_db_op_cancel) para cancelar uma operação de atualização na base de dados.

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados à mesma.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [Criar grupo AZ](https://docs.microsoft.com/cli/azure/group#az_group_create) | Cria um grupo de recursos na qual todos os recursos são armazenados. |
| [servidor de sql AZ criar](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Cria um servidor lógico que aloja uma base de dados. |
| [base de dados de sql AZ Mostrar-utilização](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_show_usage) | Mostra as informações de utilização de tamanho de uma base de dados. |
| [atualização de base de dados do sql AZ](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_update) | Atualiza as propriedades de base de dados (por exemplo, o nível de desempenho ou camada de serviço) ou uma base de dados é movido para fora do ou entre conjuntos elásticos. |
| [eliminação do grupo de AZ](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |
|||

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a CLI do Azure, consulte [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/overview).

Exemplos de script CLI de base de dados do SQL adicionais podem ser encontrados no [documentação da SQL Database do Azure](../sql-database-cli-samples.md).
