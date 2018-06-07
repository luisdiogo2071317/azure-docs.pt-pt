---
title: Como utilizar o controlo de mapa de mapas do Azure | Microsoft Docs
description: Saiba como utilizar a biblioteca de Javascript do lado do cliente de controlo de mapa de mapas do Azure.
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 4ba4b9a9f2357d283ddc03a4723cb08b48d40a9b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599216"
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