---
title: Criar um cluster de Explorador de dados do Azure e uma base de dados com a CLI
description: Este artigo descreve como criar um cluster do Explorador de dados do Azure e a base de dados com a CLI do Azure
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: howto
ms.date: 1/31/2019
ms.openlocfilehash: 8c035524adebcb131872c700280201aaac07c52b
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55747957"
---
# <a name="create-an-azure-data-explorer-cluster-and-a-database-using-cli"></a>Criar um cluster de Explorador de dados do Azure e uma base de dados com a CLI

Este artigo descreve como criar um cluster do Explorador de dados do Azure e a base de dados com a CLI do Azure.

## <a name="whats-the-difference-between-the-management-plane-and-data-plane-apis"></a>O que é a diferença entre o plano de gestão e as APIs de plano de dados

Existem duas bibliotecas de API diferentes, as APIs do plano de gestão e os dados.
As APIs de gestão são utilizadas para gerir os recursos, por exemplo criar um cluster, criar uma base de dados, eliminar uma ligação de dados, alterar o número de contagem de instâncias etc. O plano de dados, as APIs são usadas para interagir com os dados, executados consultas, ingerir dados etc.

## <a name="configure-the-cli-parameters"></a>Configure os parâmetros CLI

Iniciar sessão na sua conta

```Bash
az login
```

Defina a subscrição onde pretende colocar em cluster seja criado.

```Bash
az account set --subscription "your_subscription"
```

## <a name="create-the-azure-data-explorer-cluster"></a>Criar o cluster do Explorador de dados do Azure

Crie o seu cluster com o seguinte comando.

```Bash
az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
```

Forneça os valores seguintes

    **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | nome | *azureclitest* | O nome do cluster pretendido.|
    | sku | *D13_v2* | O SKU que irá ser utilizado para o seu cluster. |
    | resource-group | *testrg* | O nome do grupo de recursos onde seria possível criar o cluster. |
    | | |

Se desejar, existem mais parâmetros opcionais que pode utilizar, como a capacidade do cluster etc.

Para verificar se o cluster foi criado com êxito, pode executar

```Bash
az kusto cluster show --name azureclitest --resource-group testrg
```

Se o resultado contém "provisioningState" com o valor de "Êxito", significa que o cluster foi criado com êxito.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Criar a base de dados do cluster do Explorador de dados do Azure

Crie a sua base de dados com o seguinte comando.

```Bash
az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
```

Forneça os valores seguintes

    **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | cluster-name | *azureclitest* | O nome do cluster onde o devem ser criados.|
    | nome | *clidatabase* | O nome pretendido da base de dados.|
    | resource-group | *testrg* | O nome do grupo de recursos onde seria possível criar o cluster. |
    | soft-delete-period | *3650:00:00:00* | Período de tempo que dados devem ser mantidos para que fique disponível para consulta. |
    | armazenamento frequente-cache-período | *3650:00:00:00* | Período de tempo que dados devem ser mantidos na cache. |
    | | |

Pode ver a base de dados que criou ao executar

```Bash
az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
```

Isso é tudo tem agora um cluster e uma base de dados.

## <a name="clean-up-resources"></a>Limpar recursos

Se tenciona seguir os nossos inícios rápidos e tutoriais, mantenha os recursos que criou.

Quando eliminar um cluster, também elimina todos os bancos de dados no mesmo. Utilize o comando elimina o cluster abaixo.

```Bash
az kusto cluster delete --name azureclitest --resource-group testrg
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Quickstart: Ingerir dados de Hub de eventos no Explorador de dados do Azure](ingest-data-event-hub.md)
