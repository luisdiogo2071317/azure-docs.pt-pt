---
title: Como utilizar o controlo de mapa de mapas do Azure | Microsoft Docs
description: Saiba como utilizar a biblioteca de Javascript do lado do cliente de controlo de mapa de mapas do Azure.
services: azure-maps
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: article
ms.service: azure-maps
manager: timlt
ms.openlocfilehash: bbd06aad9052d2a775c35dd08f80462f8ea505a9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Como utilizar o controlo de mapa de mapas do Azure
A biblioteca de Javascript do lado do cliente de controlo de mapa permite-lhe compor maps e funcionalidade embedded Azure mapeia para o seu web ou aplicação móvel. 

## <a name="create-a-new-map-in-a-web-page"></a>Criar um mapa de novo numa página web

Pode incorporar um mapa de uma página web através da biblioteca de Javascript do lado do cliente de controlo de mapa.

1. Criar um novo ficheiro e nome MapSearch.html.

2. Adicione as referências de origem de mapas de Azure folha de estilos e script, para o `<head>` elemento do ficheiro:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Para compor um mapa de novo no seu browser, adicione um **#map** referência no `<style>` elemento.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Para inicializar o controlo de mapa, definir uma nova secção no corpo html e criar um script. Utilize a sua própria chave de conta do Azure Maps no script. Se precisar de criar uma conta ou a encontrar a chave, consulte [como gerir a sua conta de mapas do Azure e as chaves](how-to-manage-account-keys.md)

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Abra o ficheiro no seu browser e visualizar o mapa composto.

## <a name="next-steps"></a>Passos Seguintes

Este artigo mostrou como criar um mapa básico com a sua chave de mapas de Azure. Para obter mais exemplos de código adicionar ao seu maps, consulte os artigos seguintes: 

* [Criar um mapa](map-create.md)
* [Adicionar um pin](map-add-pin.md)