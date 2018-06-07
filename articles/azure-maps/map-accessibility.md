---
title: Efetuar uma aplicação acessível com o Azure Maps | Microsoft Docs
description: Como criar uma aplicação acessível utilizando mapas do Azure
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 05/18/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: 2523287669628b8c58028cae9887743c20b6bc5e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655379"
---
# <a name="building-an-accessible-application"></a>Criar uma aplicação acessível

Este artigo mostra como criar um mapa de aplicação que pode ser utilizada por um leitor de ecrã.

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Se uma aplicação acessível' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a caneta <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>tornar uma aplicação acessível</a> pelo Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) no <a href='https://codepen.io'>CodePen</a>.
</iframe>

O mapa é prebuilt com algumas funcionalidades de acessibilidade. Um utilizador pode navegar o mapa de utilizar o teclado e se estiver a executar um leitor de ecrã, o mapa notificará o utilizador de alterações para o seu estado. Por exemplo, o utilizador será notificado do mapa latitude novo, longitude, zoom e Localidade quando o mapa está panned ou ampliado. Qualquer informação adicional que é colocada no mapa base deve ter informações textual correspondentes para os utilizadores de leitor de ecrã. Utilizar [pop-up](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) é uma forma de alcançar isto. No exemplo acima de pesquisa, é adicionado um pop-up com informações textual para o mapa para cada pin que é colocado no mapa. Utilizando o [do pop-up](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) anexar método permite que o o pop-up para ser visualizadas por um leitor de ecrã sem apresentar visualmente o pop-up no mapa.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a classe de pop-up e respectivos métodos utilizados neste artigo:

* [Pop-up](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [Anexar](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#attach)
    * [Remover](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#remove)
    * [Abrir](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [Fechar](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Consulte a nossa [página de código de exemplo](http://aka.ms/AzureMapsSamples) para cenários de mapeamento mais.
