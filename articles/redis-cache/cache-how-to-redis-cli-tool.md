---
title: Como utilizar a cli de redis com a Cache de Redis do Azure | Microsoft Docs
description: Saiba como utilizar a cli de redis com a Cache de Redis do Azure.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: wesmc
ms.openlocfilehash: facc3e2079c8f5e19266f5379762d71754a6ed84
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-redis-cache"></a>Como utilizar a ferramenta de linha de comandos de Redis com a Cache de Redis do Azure

*redis cli.exe* é uma ferramenta de linha de comandos popular para interagir com uma Cache de Redis como um cliente. Esta ferramenta também está disponível para utilização com a Cache de Redis do Azure.

A ferramenta está disponível para plataformas Windows transferindo o [Redis ferramentas da linha de comandos para o Windows](https://github.com/MSOpenTech/redis/releases/). 

Se pretender executar a ferramenta da linha de comandos noutra plataforma, transfira a Cache de Redis do [ http://redis.io/download ](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Recolher informações de acesso da cache

Pode recolher as informações necessárias para aceder à cache utilizando três métodos:

1. Utilizando a CLI do Azure [az redis chaves de lista](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Azure PowerShell com [Get AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-4.4.1)
3. No portal do Azure.

Nesta secção, irá obter as chaves do portal do Azure.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Ativar o acesso redis cli.exe

Cache de Redis do Azure, apenas SSL da porta (6380) está ativada por predefinição. O `redis-cli.exe` ferramenta da linha de comandos não suporta SSL. Tem duas opções de configuração para utilizá-lo:

1. [Ativar (6379) de porta não SSL](cache-configure.md#access-ports) - **esta configuração não é recomendada** porque esta configuração, as chaves de acesso são enviadas através de TCP em texto não encriptado. Esta alteração pode comprometer a acesso à sua cache. O único cenário apenas onde poderá considerar esta configuração é quando apenas estão a aceder uma cache de teste.

2. Transfira e instale [stunnel](https://www.stunnel.org/downloads.html).

    Executar **stunnel GUI início** para iniciar o servidor.

    Faça duplo clique no ícone na barra de tarefas para o servidor de stunnel e clique em **Mostrar a janela de registo**.

    No menu de janela de registo stunnel, clique em **configuração** > **Editar configuração** para abrir o ficheiro de configuração atual.

    Adicione a seguinte entrada para *redis cli.exe* sob o **definições de serviço** secção. Insira o nome da sua cache real em vez de `yourcachename`. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Guarde e feche o ficheiro de configuração. 
  
    No menu de janela de registo stunnel, clique em **configuração** > **recarregar configuração**.


## <a name="connect-using-the-redis-command-line-tool"></a>Estabelecer ligação utilizando a ferramenta de linha de comandos do Redis.

Quando utilizar stunnel, execute *redis cli.exe*e passar apenas o *porta*, e *chave de acesso* () principal ou secundário para ligar à cache.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![stunnel com a cli de redis](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Se estiver a utilizar uma cache de teste com o **não seguros** porta não SSL, execute `redis-cli.exe` e passar o *nome de anfitrião*, *porta*, e *chave de acesso*() principal ou secundário para ligar à cache de teste.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![stunnel com a cli de redis](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como utilizar o [consola Redis](cache-configure.md#redis-console) para comandos do problema.

