---
title: Início Rápido para criar uma aplicação Python que utiliza a Cache de Redis do Azure | Microsoft Docs
description: Neste início rápido, vai aprender a criar uma Aplicação Python que utiliza a Cache de Redis
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/11/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: b66df55043e4fd29f352c6e9ec3b8674800bc4be
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
ms.locfileid: "34160500"
---
# <a name="quickstart-use-azure-redis-cache-with-python"></a>Início Rápido: Utilizar a Cache de Redis do Azure com Python
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

## <a name="introduction"></a>Introdução

Este início rápido mostra como ligar a uma Cache de Redis do Azure com Python para ler e escrever numa cache. 

![Teste de Python concluído](./media/cache-python-get-started/cache-python-completed.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* [Ambiente Python 2 ou Python 3](https://www.python.org/downloads/) instalado com [pip](https://pypi.org/project/pip/). 

## <a name="create-a-redis-cache-on-azure"></a>Criar uma cache de Redis no Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="install-redis-py"></a>Instalar redis-py

[Redis-py](https://github.com/andymccurdy/redis-py) é uma interface de Python para a Cache de Redis. Utilize a ferramenta de pacotes Python, *pip*, para instalar o pacote redis-py. 

O exemplo seguinte utiliza *pip3* para Python3 para instalar o pacote redis-py no Windows 10 com uma linha de Comandos de Programador do Visual Studio 2017 em execução com privilégios de Administrador elevados.

    pip3 install redis

![Instalar redis-py](./media/cache-python-get-started/cache-python-install-redis-py.png)


## <a name="read-and-write-to-the-cache"></a>Ler e escrever na cache

Execute o Python e teste-o com a cache a partir da linha de comandos. Substitua `<Your Host Name>` e `<Your Access Key>` pelos valores para a Cache de Redis. 

```python
>>> import redis
>>> r = redis.StrictRedis(host='<Your Host Name>.redis.cache.windows.net',
        port=6380, db=0, password='<Your Access Key>', ssl=True)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
b'bar'
```

## <a name="create-a-python-script"></a>Criar um script Python

Crie um novo ficheiro de texto de script com o nome *PythonApplication1.py*.

Adicione o seguinte script a *PythonApplication1.py* e guarde o ficheiro. Este script irá testar o acesso da cache. Substitua `<Your Host Name>` e `<Your Access Key>` pelos valores para a Cache de Redis. 

```python
import redis

myHostname = "<Your Host Name>.redis.cache.windows.net"
myPassword = "<Your Access Key>"

r = redis.StrictRedis(host=myHostname, port=6380,password=myPassword,ssl=True)

result = r.ping()
print("Ping returned : " + str(result))

result = r.set("Message", "Hello!, The cache is working with Python!")
print("SET Message returned : " + str(result))

result = r.get("Message")
print("GET Message returned : " + result.decode("utf-8"))

result = r.client_list()
print("CLIENT LIST returned : ") 
for c in result:
    print("id : " + c['id'] + ", addr : " + c['addr'])
```

Execute o script com o Python.

![Teste de Python concluído](./media/cache-python-get-started/cache-python-completed.png)


## <a name="clean-up-resources"></a>Limpar recursos

Se avançar para outro tutorial, pode manter os recursos que criou neste início rápido e reutilizá-los.

Caso contrário, se tiver concluído a aplicação de exemplo do início rápido, pode eliminar os recursos do Azure criados neste início rápido, para evitar encargos. 

> [!IMPORTANT]
> A eliminação de um grupo de recursos é irreversível e o grupo de recursos e todos os recursos contidos no mesmo serão permanentemente eliminados. Confirme que não elimina acidentalmente o grupo de recursos ou recursos errados. Se tiver criado os recursos para alojar este exemplo num grupo de recursos existente que contém os recursos que pretende manter, pode eliminar cada recurso individualmente nos respetivos painéis em vez de eliminar o grupo de recursos.
>

Inicie sessão no [Portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.

Na caixa de texto **Filtrar por nome...**, escreva o nome do grupo de recursos. As instruções neste artigo utilizaram um grupo de recursos denominado *TestResources*. No seu grupo de recursos na lista de resultados, clique em **...** e em **Eliminar grupo de recursos**.

![Eliminar](./media/cache-web-app-howto/cache-delete-resource-group.png)

É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos para confirmar e clique em **Eliminar**.

Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.


## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Criar uma aplicação Web ASP.NET simples que utiliza uma Cache de Redis do Azure.](./cache-web-app-howto.md)



<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
