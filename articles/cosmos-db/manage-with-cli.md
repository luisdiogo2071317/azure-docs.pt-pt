---
title: Gerir recursos do Azure Cosmos DB com a CLI do Azure
description: Utilize a CLI do Azure para gerir a conta do Azure Cosmos DB, base de dados e contentores.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: d79cae16c109b407840b8565eb80fb87bbed6432
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877552"
---
# <a name="manage-azure-cosmos-db-resources-using-azure-cli"></a>Gerir recursos do Azure Cosmos DB com a CLI do Azure

O guia seguinte descreve os comandos para automatizar a gestão das contas do Azure Cosmos DB, bases de dados e contentores com a CLI do Azure. Ele também inclui comandos para dimensionar o débito de contentores. As páginas de referência para todos os comandos da CLI do Azure Cosmos DB encontram-se disponíveis em [Referência da CLI do Azure](https://docs.microsoft.com/cli/azure/cosmosdb). Também pode encontrar mais exemplos [amostras de CLI do Azure para o Azure Cosmos DB](cli-samples.md), incluindo como criar e gerir contas do Cosmos DB, bases de dados e contentores para o MongoDB, Gremlin, Cassandra e API de tabela.

Este script da CLI de exemplo cria uma conta, base de dados e contentor da API SQL do Azure Cosmos DB.  

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Criar uma conta do Azure Cosmos DB

Para criar uma conta do Azure Cosmos DB com API de SQL, a consistência da sessão, com vários mestres ativada nos E.U.A. leste e E.U.A. oeste, abra o CLI do Azure ou na cloud shell e execute o seguinte comando:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --kind GlobalDocumentDB \
   --default-consistency-level "Session" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="create-a-database"></a>Criar uma base de dados

Para criar uma base de dados do Cosmos DB, abra o CLI do Azure ou na cloud shell e execute o seguinte comando:

```azurecli-interactive
az cosmosdb database create \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup"
```

## <a name="create-a-container"></a>Criar um contentor

Para criar um contentor do Cosmos DB com RU/s de 1000 e uma chave de partição, abra o CLI do Azure ou na cloud shell e execute o seguinte comando:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --partition-key-path = "/myPartitionKey" \
   --throughput 1000
```

## <a name="change-the-throughput-of-a-container"></a>Alterar o débito de um contentor

Para alterar o débito de um contentor do Cosmos DB para RU/s de 400, abra o CLI do Azure ou na cloud shell e execute o seguinte comando:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --throughput 400
```

## <a name="list-account-keys"></a>Listar chaves de conta

Quando cria uma conta do Azure Cosmos DB, o serviço gera duas chaves de acesso mestre que podem ser utilizadas para autenticação quando aceder a conta do Azure Cosmos DB. Ao fornecer duas chaves de acesso, o Azure Cosmos DB permite-lhe voltar a gerar as chaves sem interrupções na sua conta do Azure Cosmos DB. Chaves só de leitura para a autenticação de operações só de leitura também estão disponíveis. Existem duas chaves de leitura / escrita (primárias e secundárias) e duas chaves só de leitura (primário e secundário). Pode obter as chaves para a sua conta ao executar o seguinte comando:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="list-connection-strings"></a>Lista cadeias de ligação

A cadeia de ligação para ligar a sua aplicação à conta do Cosmos DB pode ser obtida com o seguinte comando.

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="regenerate-account-key"></a>Regenerar chave da conta

Deve alterar as chaves de acesso à sua conta do Azure Cosmos DB periodicamente para ajudar a manter as suas ligações mais segura. Duas chaves de acesso são atribuídas para que possa manter as ligações à conta do Azure Cosmos DB através de uma chave de acesso enquanto volta a gerar a chave de acesso.

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup" \
   --key-kind primary
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a CLI do Azure, consulte:

- [Instalar a CLI do Azure](/cli/azure/install-azure-cli)
- [Referência do CLI do Azure](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Exemplos da CLI do Azure adicionais para o Azure Cosmos DB](cli-samples.md)
