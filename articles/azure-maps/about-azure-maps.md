---
title: Descrição Geral do Azure Maps | Microsoft Docs
description: Uma introdução ao Azure Maps
services: azure-maps
keywords: ''
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: overview
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: e84580c4023be87ebfc1988c631af0b76e213987
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="an-introduction-to-azure-maps"></a>Uma introdução ao Azure Maps
O Azure Maps é um portefólio de serviços geoespaciais que incluem APIs de serviço para Mapas, Pesquisa, Encaminhamento, Trânsito e Fusos Horários. O portfólio de serviços em conformidade com o Azure OneAPI permite-lhe utilizar ferramentas familiares para desenvolver e dimensionar rapidamente soluções que integram informações de localização nas suas soluções do Azure. O Azure Maps fornece aos programadores de todos os setores da indústria poderosas capacidades geoespaciais repletas de dados de mapeamento recentes essenciais à disponibilização de contexto geográfico a aplicações móveis e Web. O Azure Maps é um conjunto de APIs REST que integra um controlo JavaScript baseado na Web destinado a tornar o desenvolvimento mais fácil, flexível e portátil nos vários suportes de dados. 

O vídeo que se segue oferece uma apresentação do Azure Maps:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

O Azure Maps é constituído por cinco serviços principais que têm por objetivo reforçar as aplicações do Azure que precisam de contexto geográfico. Cada um dos serviços é explicado em detalhe.

O **Render Service** foi concebido para os programadores criarem aplicações Web e móveis baseadas em mapeamento. O serviço utiliza imagens gráficas em padrão de alta qualidade, disponíveis em 19 níveis de zoom ou imagens de mapa de formato de vetor totalmente personalizáveis.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

O **Route Service** contém cálculos de geometria consistentes de infraestruturas do mundo real e múltiplas direções baseadas nos meios de transporte. O serviço permite que os programadores calculem as direções tendo em conta os diversos meios de transporte, como carros ligeiros, pesados, bicicletas ou a pé. O serviço também consegue levar em linha conta dados como condições de tráfego, restrições de peso ou o transporte de substâncias perigosas.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

O **Search Service** foi concebido para os programadores procurarem endereços, locais, listas de empresas por nome ou categoria e outras informações geográficas. O Search Service também permite aplicar um [geocódigo inverso](https://en.wikipedia.org/wiki/Reverse_geocoding) aos endereços e cruzamentos com base na latitude/longitude. 

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

O **Time Zone Service** permite-lhe consultar informações de fuso horário atuais, históricas e futuras através de pares de latitude/longitude ou de um [ID IANA](http://www.iana.org/). O serviço Time Zone Service também permite converter IDs de fuso horário do Microsoft Windows para fusos horários IANA, obter um desvio de fuso horário UTC e obter a hora atual num fuso horário específico. Segue-se o exemplo de uma resposta de JSON típica para uma consulta ao Time Zone Service:

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

O **Traffic Service** é um conjunto de serviços Web concebido para os programadores criarem aplicações Web e móveis que precisem de dados de tráfego. O serviço fornece dois tipos de dados:
* Fluxo de tráfego – velocidades observadas em tempo real e durações das viagens para todas as estradas principais da rede. 
* Incidentes de tráfego – perceção exata dos engarrafamentos e incidentes na rede rodoviária.

![Tráfego no Azure Maps](media/about-azure-maps/Introduction_Traffic.png)

O Azure Maps foi criado a pensar na mobilidade e pode alimentar aplicações de várias plataformas visto que o modelo de programação é agnóstico e suporta a saída de JSON através de APIs REST. Além disso, o Azure Maps disponibiliza um prático controlo de mapas em JavaScript com um modelo de programação simples tendo em vista um desenvolvimento fácil e rápido de aplicações Web e móveis. 

O Azure Maps utiliza um esquema de autenticação baseado em chaves, pelo que, para aceder aos serviços, basta navegar para o [portal do Azure](http://portal.azure.com) e criar uma conta do Azure Maps. A conta inclui duas chaves pré-geradas para si. Comece a integrar estas capacidades de localização diretamente nas suas aplicações ao utilizar as suas chaves nos pedidos feitos ao serviço Azure Maps.

## <a name="unsupported-regions"></a>Regiões não suportadas
Atualmente, a API do Azure Maps não se encontra disponível em alguns países. Consulte o seu endereço IP atual e certifique-se de que a localização do mesmo não está num dos países não suportados abaixo:

* Argentina
* China
* Índia
* Marrocos
* Paquistão
* Coreia do Sul

## <a name="relationship-with-bing-maps"></a>Relação com o Bing Maps
Os mapas apresentados neste documento são diferentes dos fornecidos pelo serviço Mapas Bing. Embora eles partilhem muitas funcionalidades iguais, os serviços são diferentes e não estão relacionados. Este serviço do Azure não afeta o plano nem a oferta de produtos do serviço Mapas Bing.

O objetivo da Microsoft é disponibilizar uma escolha à comunidade de programadores em termos de ofertas de serviços de localização. A tabela seguinte contém orientações para os programadores que estejam a ponderar qual dos serviços irão utilizar: 

| Cenário | Utilize o Azure Maps quando… | Utilize o Bing Maps quando... |
| ------------- | ------------- | ------------- |
| Ambiente de programação | Criar ou coordenar com outros serviços do Azure | Utilizar uma cloud de terceiros ou outro ambiente de programação |
| Fase de desenvolvimento  | Como o Azure Maps está atualmente na fase de Pré-visualização Pública, está otimizado para a fase inicial de testes e desenvolvimento de uma Prova de Conceito | É preciso um SLA de nível empresarial para um ambiente de produção |
| Opções de preços | As opções preliminares de preços para programadores são suficientes | São precisos preços de nível empresarial personalizados |
| Ambiente de cenário de utilização | É precisa a utilização no veículo | Não é precisa a utilização no veículo |
| Cobertura geográfica | A Índia, a China, o Japão e a Coreia do Sul não são necessários | A cobertura dos mapas da Índia, da China, do Japão e da Coreia do Sul é necessária |
| Conteúdo do mapa | Os mapas de superfície standard são suficientes | As imagens de satélite, com vista aérea e da rua são necessárias |
| Origem do mapa subjacente | Os dados de mapeamento do TomTom são preferenciais | Os dados de mapeamento do HERE são preferenciais |

Inscreva-se numa [conta do Azure Maps hoje mesmo](http://aka.ms/azurelbsportal).

## <a name="next-steps"></a>Passos seguintes

Agora, já tem uma perceção geral do Azure Maps. O passo seguinte é experimentar uma aplicação de exemplo que faça a demonstração do serviço.

> [!div class="nextstepaction"]
> [Iniciar um mapa de pesquisa interativo de demonstração](quick-demo-map-app.md)

