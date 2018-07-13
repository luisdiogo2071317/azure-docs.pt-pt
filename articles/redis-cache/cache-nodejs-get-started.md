---
title: Início Rápido para aprender a utilizar a Cache de Redis do Azure com o Node.js| Microsoft Docs
description: Neste início rápido, vai aprender a utilizar a Cache de Redis do Azure com o Node.js e o node_redis.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: quickstart
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/21/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 8f71feb610884af29bdfbf170cfc411f32c50233
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38700787"
---
# <a name="quickstart-how-to-use-azure-redis-cache-with-nodejs"></a>Início Rápido: Como utilizar a Cache de Redis do Azure com o Node.js



A Cache de Redis do Azure permite-lhe aceder a uma cache de Redis segura, dedicada e gerida pela Microsoft. Pode aceder a uma cache a partir de qualquer aplicação dentro do Microsoft Azure.

Este tópico mostra-lhe como começar com a Cache de Redis do Azure com o Node.js. 

Pode utilizar qualquer editor de código para concluir os passos deste início rápido. No entanto, o [Visual Studio Code](https://code.visualstudio.com/) é uma excelente opção, disponível nas plataformas Windows, macOS e Linux.

![Aplicação de cache concluída](./media/cache-nodejs-get-started/cache-app-complete.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Pré-requisitos
Instale o [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Este tutorial utiliza o [node_redis](https://github.com/mranney/node_redis). Para obter exemplos de utilização de outros clientes Node.js, consulte a documentação individual para os clientes Node.js listados em [Clientes Node.js Redis](http://redis.io/clients#nodejs).


## <a name="create-a-cache"></a>Criar uma cache
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](../../includes/redis-cache-access-keys.md)]


Adicione variáveis de ambiente ao **NOME DO ANFITRIÃO** e à chave de acesso **Primária**. Vai utilizar estas variáveis do seu código em vez de incluir informações confidenciais diretamente no código.

```
set REDISCACHEHOSTNAME=contosoCache.redis.cache.windows.net
set REDISCACHEKEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```


## <a name="connect-to-the-cache"></a>Ligar à cache

As versões mais recentes do [node_redis](https://github.com/mranney/node_redis) fornecem suporte para ligar a Cache de Redis do Azure através de SSL. O seguinte exemplo mostra como ligar a Cache de Redis do Azure com o ponto final de SSL de 6380. 

```js
var redis = require("redis");

// Add your cache name and access key.
var client = redis.createClient(6380, process.env.REDISCACHEHOSTNAME,
    {auth_pass: process.env.REDISCACHEKEY, tls: {servername: process.env.REDISCACHEHOSTNAME}});
```

Não crie uma ligação nova para cada operação no código. Em vez disso, reutilize ligações tanto quanto possível. 

## <a name="create-a-new-nodejs-app"></a>Criar uma aplicação nova em Node.js

Crie um novo ficheiro de script com o nome *redistest.js*.

Adicione o seguinte exemplo de JavaScript ao ficheiro. Este código mostra como ligar a uma instância da Cache de Redis do Azure através do nome do anfitrião da cache e de variáveis de ambiente da chave. O código também armazena e obtém um valor de cadeia na cache. Os comandos `PING` e `CLIENT LIST` também são executados. Para obter mais exemplos de utilização de Redis com o cliente [node_redis](https://github.com/mranney/node_redis), veja [http://redis.js.org/](http://redis.js.org/).

```js
var redis = require("redis");
var bluebird = require("bluebird");

bluebird.promisifyAll(redis.RedisClient.prototype);
bluebird.promisifyAll(redis.Multi.prototype);

async function testCache() {

    // Connect to the Redis cache over the SSL port using the key.
    var cacheConnection = redis.createClient(6380, process.env.REDISCACHEHOSTNAME, 
        {auth_pass: process.env.REDISCACHEKEY, tls: {servername: process.env.REDISCACHEHOSTNAME}});
        
    // Perform cache operations using the cache connection object...

    // Simple PING command
    console.log("\nCache command: PING");
    console.log("Cache response : " + await cacheConnection.pingAsync());

    // Simple get and put of integral data types into the cache
    console.log("\nCache command: GET Message");
    console.log("Cache response : " + await cacheConnection.getAsync("Message"));    

    console.log("\nCache command: SET Message");
    console.log("Cache response : " + await cacheConnection.setAsync("Message",
        "Hello! The cache is working from Node.js!"));    

    // Demostrate "SET Message" executed as expected...
    console.log("\nCache command: GET Message");
    console.log("Cache response : " + await cacheConnection.getAsync("Message"));    

    // Get the client list, useful to see if connection list is growing...
    console.log("\nCache command: CLIENT LIST");
    console.log("Cache response : " + await cacheConnection.clientAsync("LIST"));    
}

testCache();
```

Execute o script com o Node.js.

```
node redistest.js
```

No exemplo abaixo, pode ver que a chave `Message` tinha anteriormente um valor em cache, o qual foi definido com a Consola Redis no portal do Azure. A aplicação atualizou esse valor em cache. A aplicação também executou os comandos `PING` e `CLIENT LIST`.

![Aplicação de cache concluída](./media/cache-nodejs-get-started/cache-app-complete.png)


## <a name="clean-up-resources"></a>Limpar recursos

Se avançar para o próximo tutorial, pode manter os recursos que criou neste início rápido e reutilizá-los.

Caso contrário, se tiver concluído a aplicação de exemplo do início rápido, poderá eliminar os recursos do Azure criados neste início rápido, para evitar encargos. 

> [!IMPORTANT]
> A eliminação de um grupo de recursos é irreversível e o grupo de recursos e todos os recursos nele contidos serão permanentemente eliminados. Confirme que não elimina acidentalmente o grupo de recursos ou recursos errados. Se tiver criado os recursos para alojar este exemplo num grupo de recursos existente que contém os recursos que pretende manter, poderá eliminar cada recurso individualmente nos respetivos painéis em vez de eliminar o grupo de recursos.
>

Inicie sessão no [Portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.

Na caixa de texto **Filtrar por nome...**, escreva o nome do grupo de recursos. As instruções neste artigo utilizaram um grupo de recursos denominado *TestResources*. No grupo de recursos na lista de resultados, clique em **...** e em **Eliminar grupo de recursos**.

![Eliminar](./media/cache-nodejs-get-started/cache-delete-resource-group.png)

É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos para confirmar e clique em **Eliminar**.

Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.



## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a utilizar a Cache de Redis do Azure a partir de uma aplicação de Node.js. Avance para o início rápido seguinte para utilizar a Cache de Redis com uma aplicação Web ASP.NET.

> [!div class="nextstepaction"]
> [Criar uma aplicação Web ASP.NET que utiliza uma Cache de Redis do Azure](./cache-web-app-howto.md)



