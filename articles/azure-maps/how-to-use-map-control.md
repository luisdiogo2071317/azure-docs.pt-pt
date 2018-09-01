---
title: Como usar o controle de mapa do Azure Maps | Documentos da Microsoft
description: Saiba como utilizar a biblioteca de Javascript do lado do cliente de controlo de mapas do Azure Maps.
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 619dda44b5ad74ba1dcb62cfb3318687aa6ec4d5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43340140"
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Como usar o controle de mapa do Azure Maps
A biblioteca de Javascript do lado do cliente de controlo de mapas permite-lhe compor mapas e funcionalidade do Azure Maps embedded nas suas aplicações móveis ou web. 

## <a name="create-a-new-map-in-a-web-page"></a>Criar um novo mapa numa página da web

Para incorporar um mapa numa página da web, usando a biblioteca de Javascript do lado do cliente de controlo do mapa.

1. Crie um novo ficheiro e dê-lhe o nome Mapsearch.

2. Adicionar as referências de origem de Azure Maps folha de estilo e script, para o `<head>` elemento do arquivo:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Para processar um novo mapa no seu browser, adicione uma **#map** referência no `<style>` elemento.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Para inicializar o controlo do mapa, definir uma nova seção no corpo do html e criar um script. Utilize a sua própria chave de conta do Azure Maps no script. Se precisar de criar uma conta ou encontrar a sua chave, consulte [como gerir a sua conta do Azure Maps e as chaves](how-to-manage-account-keys.md)

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Abra o ficheiro no seu navegador da web e ver o mapa processado.

## <a name="next-steps"></a>Passos Seguintes

Este artigo mostrou como criar um mapa básico com a sua chave de mapas do Azure. Para obter mais exemplos de código adicionar a seus mapas, veja os artigos seguintes: 

* [Criar um mapa](map-create.md)
* [Escolha um estilo de mapa](choose-map-style.md)
