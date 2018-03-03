---
title: "Preferência de leitura do MongoDB a utilizar com o MongoDB Cosmos BD do Azure API | Microsoft Docs"
description: "Saiba como utilizar a preferência de leitura do MongoDB o API do MongoDB Cosmos BD do Azure"
services: cosmos-db
documentationcenter: 
author: vidhoonv
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: 
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 02/26/2018
ms.author: viviswan
ms.openlocfilehash: b28285695f52d6dbcc33d9fb0efe06b43cdf1cab
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-globally-distribute-reads-using-read-preference-with-the-azure-cosmos-db-mongodb-api"></a>Como distribuir globalmente lê a preferência de leitura, utilizando o API do MongoDB Cosmos BD do Azure 

Este artigo mostra como distribuir global de operações de leitura utilizando [preferência de leitura do MongoDB](https://docs.mongodb.com/manual/core/read-preference/) definições da BD do Azure Cosmos MongoDB API. 

## <a name="prerequisites"></a>Pré-requisitos 
Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Consulte este [início rápido](tutorial-global-distribution-mongodb.md) artigo para obter instruções sobre como utilizar o portal do Azure para configurar a conta de base de dados do Azure Cosmos com distribuição global e, em seguida, ligar utilizando a API do MongoDB.

## <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Abra uma janela de terminal do git, tal como git bash, e `cd` para um diretório de trabalho.  

Execute os seguintes comandos para clonar o repositório de exemplo. Com base na sua plataforma de interesse, utilize um dos repositórios do exemplo seguinte:

1. [Aplicação de exemplo do .NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference)
2. [Aplicação de exemplo do NodeJS]( https://github.com/Azure-Samples/azure-cosmos-db-mongodb-node-geo-readpreference)
3. [Aplicação de exemplo o mongoose](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-mongoose-geo-readpreference)
4. [Aplicação de exemplo de Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)
5. [Aplicação de exemplo SpringBoot](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-spring)


```bash
git clone <sample repo url>
```

## <a name="run-the-application"></a>Executar a aplicação

Consoante a plataforma utilizada, instale os pacotes necessários e iniciar a aplicação. Para instalar dependências, siga o ficheiro Leia-me incluído no repositório de aplicação de exemplo. Por exemplo, a aplicação de exemplo NodeJS, utilize os seguintes comandos para instalar os pacotes necessários e iniciar a aplicação.

```bash
cd mean
npm install
node index.js
```
A aplicação tenta ligar a uma origem de MongoDB e falhará, porque a cadeia de ligação é inválida. Siga os passos a Leia-me para atualizar a cadeia de ligação `url`. Além disso, atualize o `readFromRegion` para uma região de leitura na sua conta de base de dados do Azure Cosmos. As instruções que se seguem são do exemplo NodeJS:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos DB account's values. 
```

Depois de seguir estes passos, a aplicação de exemplo executa e produz a seguinte saída:

```
connected!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromSecondaryfunc query completed!
```

## <a name="read-using-read-preference-mode"></a>Utilizar o modo de leitura preferência de leitura

MongoDB fornece os seguintes modos de preferência de leitura para os clientes utilizarem:

1. PRIMÁRIO
2. PRIMARY_PREFERRED
3. SECUNDÁRIO
4. SECONDARY_PREFERRED
5. MAIS PRÓXIMO

Consulte a secção detalhada [comportamento de preferência de leitura do MongoDB](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) modos de preferência de leitura de documentação para obter detalhes sobre o comportamento de cada um deles. Na base de dados do Azure Cosmos primário mapeia para a região de escrita e mapas secundários a região de leitura.

Com base nos cenários comuns, recomendamos que utilize as seguintes definições:

1. Se **lê de latência baixa** são necessários, utilize o **NEAREST** preferência modo de leitura. Esta definição indica as operações de leitura para o mais próximo região disponível. Tenha em atenção que se a região mais próximo é a região de escrita, em seguida, estas operações são direcionadas para essa região.
2. Se **distribuição de elevada disponibilidade e a georreplicação de leituras** são necessários (latência não é uma restrição), em seguida, utilize o **preferencial SECUNDÁRIO** preferência modo de leitura. Esta definição indica as operações de leitura para uma região de leitura disponível. Não se estiver disponível nenhuma região de leitura, pedidos são direcionados para a região de escrita.

O fragmento seguinte da aplicação de exemplo mostra como configurar mais próximo de leitura de preferência no NodeJS:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.NEAREST});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromNearestfunc query completed!");
  });
```

Da mesma forma, o fragmento abaixo mostra como configurar a preferência de leitura SECONDARY_PREFERRED no NodeJS:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.SECONDARY_PREFERRED});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromSecondaryPreferredfunc query completed!");
  });
```

Consulte os correspondente repos de aplicação de exemplo para outras plataformas, tais como [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) e [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

## <a name="read-using-tags"></a>Utilizando as etiquetas de leitura

Para além de modo a preferência de leitura, MongoDB permite a utilização de etiquetas para direcionar as operações de leitura. Na base de dados do Azure Cosmos para a API do MongoDB, o `region` tag é incluída por predefinição como parte do `isMaster` resposta:

```json
"tags": {
         "region": "West US"
      }
```

Por conseguinte, pode utilizar MongoClient o `region` tag juntamente com o nome de região para direcionar as operações de leitura para regiões específicas. Para contas de base de dados do Azure Cosmos, nomes de região podem ser encontrados no portal do Azure no lado esquerdo em **definições -> dados de réplica globalmente**. Esta definição é útil para alcançar **ler isolamento** -pretendem casos em que aplicação de cliente direcionar as operações de leitura apenas numa região específica. Esta definição é ideal para não-produção/análise de tipo de cenários, o que executam em segundo plano e não são serviços críticos de produção.

O fragmento seguinte da aplicação de exemplo mostra como configurar a preferência de leitura com etiquetas no NodeJS:

```javascript
 var query = {};
  var readcoll = client.db('regionDB').collection('regionTest',{readPreference: new ReadPreference(ReadPreference.SECONDARY_PREFERRED, {"region": "West US"})});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromRegionfunc query completed!");
  });
```

Consulte os correspondente repos de aplicação de exemplo para outras plataformas, tais como [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) e [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

Neste artigo, aprendeu como distribuir global de operações de leitura com o MongoDB API do Azure Cosmos base de dados preferência de leitura.

## <a name="clean-up-resources"></a>Limpar recursos

Se não continuar a utilizar esta aplicação, elimine todos os recursos criados por este artigo no portal do Azure com os seguintes passos:

1. No menu do lado esquerdo do portal do Azure, clique em **Grupos de recursos** e, em seguida, clique no nome de recurso que criou. 
2. Na página do grupo de recursos, clique em **Eliminar**, escreva o nome do recurso a eliminar na caixa de texto e, em seguida, clique em **Eliminar**.

## <a name="next-steps"></a>Passos Seguintes

* [Importar dados da MongoDB para o Azure Cosmos DB](mongodb-migrate.md)
* [Configurar uma conta de base de dados do Azure Cosmos global replicada e utilizá-la com a API do MongoDB](tutorial-global-distribution-mongodb.md)
* [Desenvolver localmente com o emulador](local-emulator.md)
