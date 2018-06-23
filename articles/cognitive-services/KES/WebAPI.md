---
title: Interface Web API na API do serviço de exploração de dados de conhecimento | Microsoft Docs
description: Utilize a interface de API web para criar uma experiência de pesquisa avançada, semântica no conhecimento exploração de serviço (KES) API nos serviços cognitivos.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 16c5680eb4f249a5d37e6b90eea92cfff7090eef
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351547"
---
# <a name="web-api-interface"></a>Interface de API Web
Os ficheiros de modelo criados pelo serviço de exploração de dados de conhecimento podem ser alojados e acedidos através de um conjunto de web APIs.  As APIs poderá ser alojadas do computador local utilizando o [ `host_service` ](CommandLine.md#host_service-command) comando ou pode ser implementada para um serviço em nuvem do Azure a utilizar o [ `deploy_service` ](CommandLine.md#deploy_service-command) comando.  Ambas as técnicas expõem pontos finais da API do seguintes:
* [*interpretar* ](interpretMethod.md) – interpreta uma cadeia de consulta de linguagem natural. Devolve interpretações anotadas para permitir experiências de conclusão automática de caixa de pesquisa que antecipam o que o utilizador está a escrever.
* [*avaliar* ](evaluateMethod.md) – Evaluates e devolve o resultado de uma expressão de consulta structured.
* [*calchistogram* ](calchistogramMethod.md) – calcula um histograma de valores de atributo para objetos devolvidos por uma expressão de consulta structured.

Utilizado em conjunto, estes métodos API permitem a criação de uma experiência de pesquisa avançada de semântica.  Fornecida uma cadeia de consulta de linguagem natural a *interpretar* método fornece anotadas versões da consulta de entrada com expressões de consulta structured, com base nos dados subjacentes gramática e no índice.  O *avaliar* método avalia a expressão de consulta estruturada e devolve os objetos de índice correspondente para apresentação.  O *calchistogram* método calcula as distribuições de valor de atributo para ativar a filtragem e refinement.

**Exemplo**

Num domínio académico publicações, se um utilizador tipos a cadeia "latentes s", o *interpretar* método pode fornecer um conjunto de interpretações classificados, sugerindo que o utilizador pode procurar a palavra-chave "latentes semântica análise", o título "estrutura latentes Analysis Services", ou outras expressões começados por "latentes s".  Estas informações podem ser utilizadas para rapidamente Guia do utilizador para os resultados da pesquisa pretendido.

Para este domínio, o *avaliar* método pode ser utilizado para obter um conjunto de correspondência de publicações desde o índice académico e o *calchistogram* método pode ser utilizado para calcular a distribuição do atributo valores para as publicações correspondentes, que podem ser utilizados ao filtro adicional e refinar os resultados da pesquisa.

Tenha em atenção que, para melhorar a legibilidade dos exemplos, as chamadas de REST API contém carateres (tais como espaços) que não foram codificados de URL. O código irá precisar de aplicar as codificações de URL adequadas.
