---
title: Descrição Geral do Azure Maps | Microsoft Docs
description: Uma introdução ao Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 09/12/2018
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 4db494de87993a6434eaf5dbf441094578d72ea4
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165969"
---
# <a name="what-is-azure-maps"></a>O que é o Azure Maps?

O Azure Maps é uma coleção de serviços geoespaciais, apoiado por dados de mapeamento atualizados, para que possa dar contexto geográfico preciso para as suas aplicações Web e móveis. Contém as APIs REST para processamento de mapas e pesquisa de pontos de interesse. As APIs também podem localizar rotas para pontos de interesse, condições de tráfego, fusos horários e uma localização de um endereço IP. As APIs funcionam com ferramentas familiares para desenvolver e dimensionar rapidamente soluções que integram informações de localização nas suas soluções do Azure. Para além das APIs REST, o [controlo de mapas em JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control) baseado na Web é disponibilizado para tornar o desenvolvimento fácil, flexível e portátil em vários suportes.

O vídeo seguinte explica o Azure Maps detalhadamente:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Controlos de Mapas

### <a name="web-control"></a>Controlo Web

O Controlo Web do Azure Maps permite-lhe personalizar mapas interativos com o seu próprio conteúdo e imagens para apresentar nas suas aplicações móveis ou Web. Este controlo utiliza o WebGL, permitindo-lhe compor grandes conjuntos de dados com elevado desempenho. Desenvolva com o controlo através do JavaScript ou do TypeScript.

![Controlo Web do Azure Maps](media/about-azure-maps/Introduction_WebMapControl.png)

## <a name="services-in-azure-maps"></a>Serviços no Azure Maps

O Azure Maps é composto pelos seguintes seis serviços que podem conceder contexto geográfico às aplicações do Azure.

### <a name="render-service"></a>Serviço de composição

O serviço de Composição foi concebido para os programadores criarem aplicações Web e móveis baseadas em mapeamento. O serviço utiliza imagens gráficas em padrão de alta qualidade, disponíveis em 19 níveis de zoom ou imagens de mapa de formato de vetor totalmente personalizáveis.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

Agora, o serviço de Composição oferece APIs de pré-visualização para permitir que os programadores trabalhem com imagens de satélite. Para obter mais detalhes, leia as [APIs de Composição do Azure Maps](https://docs.microsoft.com/rest/api/maps/render).

### <a name="route-service"></a>Serviço de encaminhamento

O serviço de Composição contém cálculos de geometria consistentes de infraestruturas do mundo real e múltiplas direções baseadas nos meios de transporte. O serviço permite que os programadores calculem as direções tendo em conta os diversos meios de transporte, como carros ligeiros, pesados, bicicletas ou a pé. O serviço também consegue levar em linha conta dados como condições de tráfego, restrições de peso ou o transporte de substâncias perigosas.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

O serviço de Composição oferece agora uma pré-visualização das funcionalidades avançadas, como o processamento em lotes de vários pedidos de encaminhamento, matrizes de tempo de deslocação e distância entre um conjunto de origens e destinos, e encontrar rotas ou distâncias em que pode viajar com base nos requisitos de tempo ou combustível. Para obter detalhes sobre as capacidades de encaminhamento, leia as [APIs de Rota do Azure Maps](https://docs.microsoft.com/rest/api/maps/route).

### <a name="search-service"></a>Serviço de pesquisa

O serviço de Pesquisa foi concebido para os programadores procurarem endereços, locais, listas de empresas por nome ou categoria e outras informações geográficas. O Serviço de Pesquisa também permite aplicar um [geocódigo inverso](https://en.wikipedia.org/wiki/Reverse_geocoding) aos endereços e cruzamentos com base nas latitudes e longitudes.

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

O serviço de Pesquisa também oferece funcionalidades avançadas, como a pesquisa ao longo de uma rota, a pesquisa numa área mais ampla, colocação de um grupo de pedidos de pesquisa em lote, bem como pesquisa por área maior em vez de um ponto de localização. As APIs de pesquisa de área e batch estão atualmente em pré-visualização. Para obter mais detalhes sobre as capacidades de pesquisa, leia a página [APIs de Pesquisa do Azure Maps](https://docs.microsoft.com/rest/api/maps/search).

### <a name="time-zone-service"></a>Serviço de Fuso Horário

O serviço de Fuso Horário permite-lhe consultar informações de fuso horário atuais, históricas e futuras através de pares de latitude/longitude ou de um [ID IANA](http://www.iana.org/). O serviço Time Zone Service também permite converter IDs de fuso horário do Microsoft Windows para fusos horários IANA, obter um desvio de fuso horário UTC e obter a hora atual num fuso horário específico. Segue-se o exemplo de uma resposta de JSON típica para uma consulta ao Time Zone Service:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Para obter detalhes sobre este serviço, visite a página [APIs de Fuso Horário do Azure Maps](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Serviço de tráfego

O serviço de Tráfego é um conjunto de serviços Web concebido para os programadores criarem aplicações Web e móveis que precisem de dados de tráfego. O serviço fornece dois tipos de dados:

* Fluxo de tráfego – velocidades observadas em tempo real e durações das viagens para todas as estradas principais da rede.
* Incidentes de tráfego – perceção exata dos engarrafamentos e incidentes na rede rodoviária.

![Tráfego no Azure Maps](media/about-azure-maps/Introduction_Traffic.png)

Visite a página [APIs de Tráfego do Azure Maps](https://docs.microsoft.com/rest/api/maps/traffic) para obter mais detalhes.

### <a name="ip-to-location"></a>IP para Localização

O serviço IP para Localização permite-lhe pré-visualizar o código de país de duas letras obtido para um determinado endereço IP. Este serviço pode ajudar a personalizar e melhorar a experiência do utilizador ao capacitar conteúdo da aplicação personalizado com base na localização geográfica.

Para obter informações sobre as APIs REST para o IP do serviço de localização, visite a página [APIs de Geolocalização do Azure Maps](https://docs.microsoft.com/rest/api/maps/geolocation).

## <a name="programming-model"></a>Modelo de programação

O Azure Maps foi criado para oferecer mobilidade e pode potenciar aplicações de várias plataformas. Ele utiliza um modelo de programação que é agnóstico quanto ao idioma e oferece suporte à saída JSON através de [APIs REST](https://docs.microsoft.com/rest/api/maps/).

Além disso, o Azure Maps disponibiliza um prático [controlo de mapas em JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control) com um modelo de programação simples tendo em vista um desenvolvimento fácil e rápido de aplicações Web e móveis.

## <a name="usage"></a>Utilização

Para aceder aos serviços do Maps, navegue para o [portal do Azure](http://portal.azure.com) e crie uma conta do Azure Maps.

O Azure Maps utiliza um esquema de autenticação baseado em chave. A conta inclui duas chaves pré-geradas para si. Comece a integrar estas capacidades de localização na sua aplicação através de qualquer uma das chaves e a fazer um pedido para o serviço do Azure Maps.

## <a name="supported-regions"></a>Regiões suportadas

A API do Azure Maps está atualmente disponível em todos os países, exceto nos seguintes:

* Argentina
* China
* Índia
* Marrocos
* Paquistão
* Coreia do Sul

Verifique se a localização do endereço IP atual não está num dos países não suportados abaixo.

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre as novas funcionalidades do Azure Maps:

> [!div class="nextstepaction"]
> [Route Matrix, Isochrones, IP lookup, and more](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/) (Matriz de Rota, Isocrones, pesquisa de IP e mais)

Continue para experimentar uma aplicação de exemplo que demonstra o serviço:

> [!div class="nextstepaction"]
> [Iniciar um mapa de pesquisa interativo de demonstração](quick-demo-map-app.md)
