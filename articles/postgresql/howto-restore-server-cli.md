---
title: Como criar cópias de segurança e restaurar um servidor na base de dados do Azure para PostgreSQL | Microsoft Docs
description: Saiba como criar cópias de segurança e restaurar um servidor na base de dados do Azure para PostgreSQL utilizando a CLI do Azure.
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 04/01/2018
ms.openlocfilehash: 5c5cc1fdbe48fb93eea204e4619038052e685f1f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31411503"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-cli"></a>Como criar cópias de segurança e restaurar um servidor na base de dados do Azure para PostgreSQL utilizando a CLI do Azure

## <a name="backup-happens-automatically"></a>Cópia de segurança ocorre automaticamente
Base de dados do Azure para servidores de PostgreSQL são cópias de segurança periodicamente para ativar funcionalidades de restauro. Utilizar esta funcionalidade pode restaurar o servidor e todas as suas bases de dados para um anterior ponto no tempo, num servidor novo.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de procedimentos, tem de:
- Um [base de dados do Azure para PostgreSQL servidor e base de dados](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Este guia de procedimentos é necessário utilizar a CLI do Azure versão 2.0 ou posterior. Para confirmar a versão, a linha de comandos da CLI do Azure, introduza `az --version`. Para instalar ou atualizar, consulte o artigo [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="add-the-extension"></a>Adicionar a extensão
Adicione a extensão de gestão atualizada da Base de Dados do Azure para PostgreSQL, com o comando seguinte:
```azurecli-interactive
az extension add --name rdbms
``` 

Certifique-se de que tem a versão de extensão correta instalada. 
```azurecli-interactive
az extension list
```

O JSON de retorno deve incluir o seguinte: 
```json
{
    "extensionType": "whl",
    "name": "rdbms",
    "version": "0.0.5"
}
```

Se não for devolvida a versão 0.0.5, execute o seguinte para atualizar a extensão: 
```azurecli-interactive
az extension update --name rdbms
```


## <a name="set-backup-configuration"></a>Configuração do conjunto de cópia de segurança

Efetue a escolha entre configurar o servidor para cópias de segurança localmente redundantes ou cópias de segurança georredundante durante a criação do servidor. 

> [!NOTE]
> Depois de criar um servidor, o tipo de redundância de que possui, não pode ser mudado vs georredundante localmente redundantes.
>

Durante a criação de um servidor através de `az postgres server create` comando, o `--geo-redundant-backup` parâmetro decide a opção de redundância de cópia de segurança. Se `Enabled`, são efetuadas cópias de segurança de redundante de georreplicação. Ou, se `Disabled` são efetuadas cópias de segurança localmente redundantes. 

O período de retenção de cópias de segurança está definido pelo parâmetro `--backup-retention-days`. 

Para obter mais informações sobre como definir estes valores durante a criação, consulte o [base de dados do Azure para o servidor de PostgreSQL início rápido da CLI](quickstart-create-server-database-azure-cli.md).

O período de retenção de cópias de segurança de um servidor pode ser alterado da seguinte forma:

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --backup-retention-days 10
```

O exemplo anterior é alterado o período de retenção de cópias de segurança de mydemoserver para 10 dias.

O período de retenção de cópias de segurança é regida pelas até que ponto anterior no tempo que pode ser obtido um restauro de ponto no tempo, uma vez que o se basear em cópias de segurança disponíveis. Restauro de ponto no tempo é ainda mais descrito na secção seguinte.

## <a name="server-point-in-time-restore"></a>Restauro do ponto no tempo de servidor
Pode restaurar o servidor para um ponto anterior no tempo. Os dados restaurados são copiados para um novo servidor e o servidor existente for deixado como está. Por exemplo, se uma tabela acidentalmente for removida, ao meio-dia hoje em dia, pode restaurar a hora imediatamente antes ao meio-dia. Em seguida, pode obter a tabela em falta e os dados da cópia de restaurada do servidor. 

Para restaurar o servidor, utilizar a CLI do Azure [restauro do servidor az postgres](/cli/azure/postgres/server#az_postgres_server_restore) comando.

### <a name="run-the-restore-command"></a>Execute o comando de restauro

Para restaurar o servidor, na linha de comandos da CLI do Azure, introduza o seguinte comando:

```azurecli-interactive
az postgres server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

O `az postgres server restore` comando requer os seguintes parâmetros:
| Definição | Valor sugerido | Descrição  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  O grupo de recursos onde o servidor de origem existe.  |
| nome | mydemoserver-restored | O nome do novo servidor que é criado pelo comando restore. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Selecione um ponto no tempo para restaurar. Esta data e hora têm de estar dentro do período de retenção de cópias de segurança do servidor de origem. Utilize o formato ISO8601 de data e hora. Por exemplo, pode utilizar o seus próprios fuso horário local, como `2018-03-13T05:59:00-08:00`. Também pode utilizar o formato UTC Zulu, por exemplo, `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | O nome ou ID do servidor de origem do qual pretende restaurar. |

Quando restaurar um servidor para um ponto anterior no tempo, é criado um novo servidor. O servidor original e respetivas bases de dados a partir do ponto no tempo especificado são copiados para o novo servidor.

A localização e o preço de camada de valores para o servidor restaurado permanecerem o mesmo que o servidor original. 

Depois de concluir o processo de restauro, localize o novo servidor e certifique-se de que a serem restaurados conforme esperado.

## <a name="geo-restore"></a>Georreplicação restauro
Se tiver configurado o servidor para cópias de segurança georredundante, um novo servidor pode ser criado da cópia de segurança de que o servidor existente. Este novo servidor pode ser criado em qualquer região que a base de dados do Azure para PostgreSQL está disponível.  

Para criar um servidor utilizando uma cópia de segurança redundante georreplicação, utilizar a CLI do Azure `az postgres server georestore` comando.

> [!NOTE]
> Quando um servidor é criado pela primeira vez pode não ser imediatamente disponível para o restauro de georreplicação. Poderá demorar algumas horas para os metadados necessários ser preenchido.
>

A georreplicação restaurar o servidor, na linha de comandos da CLI do Azure, introduza o seguinte comando:

```azurecli-interactive
az postgres server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen4_8 
```
Este comando cria um novo servidor chamado *mydemoserver georestored* nos EUA leste, que irão pertencer a *myresourcegroup*. É um objetivo geral, o servidor Gen 4 com 8 vCores. O servidor é criado a partir da cópia de segurança georredundante de *mydemoserver*, que também se encontra no grupo de recursos *myresourcegroup*

Se pretender criar o novo servidor num grupo de recursos diferente do servidor existente, em seguida, no `--source-server` parâmetro seria qualificar o nome do servidor como no exemplo seguinte:

```azurecli-interactive
az postgres server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver" --location eastus --sku-name GP_Gen4_8

```

O `az postgres server georestore` requies os seguintes parâmetros de comando:
| Definição | Valor sugerido | Descrição  |
| --- | --- | --- |
|resource-group| myResourceGroup | O nome do grupo de recursos novo servidor irão pertencer a.|
|nome | mydemoserver-georestored | O nome do novo servidor. |
|source-server | mydemoserver | O nome do servidor existente cujas cópias de segurança redundante georreplicação são utilizadas. |
|localização | eualeste | A localização do novo servidor. |
|nome do SKU| GP_Gen4_8 | Este parâmetro define o escalão de preço, a geração de computação e o número de vCores do novo servidor. GP_Gen4_8 mapeia para um objetivo geral, o servidor Gen 4 com 8 vCores.|


>[!Important]
>Ao criar um novo servidor por um restauro georreplicação, herda o mesmo tamanho de armazenamento e o escalão de preço que o servidor de origem. Não não possível alterar estes valores durante a criação. Depois de criar o novo servidor, o tamanho de armazenamento pode ser escalado para cima.

Depois de concluir o processo de restauro, localize o novo servidor e certifique-se de que a serem restaurados conforme esperado.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre o serviço [cópias de segurança](concepts-backup.md).
- Saiba mais sobre [continuidade do negócio](concepts-business-continuity.md) opções.
