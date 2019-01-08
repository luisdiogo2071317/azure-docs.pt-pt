---
title: Escolher o melhor de escalão de preço para o Azure Maps | Documentos da Microsoft
description: Saiba mais sobre oferecidos pelo Azure Maps de escalões de preço
author: walsehgal
ms.author: v-musehg
ms.date: 01/02/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 366637274a8006455f05702f47c02f505a615820
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063238"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Escolher o melhor de escalão de preço no Azure Maps

Mapas do Azure oferece dois escalões de preços. O objetivo deste artigo é para o ajudar a escolher o melhor de preços para as suas necessidades. Para ajudar a escolher o melhor de escalão de preço, pergunte-se a duas perguntas seguintes.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Que capacidades geoespaciais planeja usar?
O escalão de preço S0 é adequado para si se geoespacial as principais APIs cumprir os requisitos de serviço. Se pretender que os recursos mais avançados para a sua aplicação, considere optar para o escalão de preço S1. Recursos de exemplo são um mais imagens híbridas, obtendo o intervalo de rota e geocodificação do batch. O **capacidades de escalão de preço** tabela que se segue dá-lhe uma ideia melhor de necessidades da sua aplicação. Ele também ajuda a escolher um escalão de preço mais adequado para a sua aplicação.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>O número de utilizadores em simultâneo planear oferecer suporte? 
Os escalões de preços S0 e S1 lidar com diferentes quantidades de débito de dados. Antes de escolher um escalão de preço do Azure Maps, pergunte-se algumas perguntas. Um exemplo é "quantos utilizadores simultâneos desejo oferecer suporte?" O escalão de preço S0 lida com até **50 consultas por segundo**. Os identificadores do escalão de preços S1 **mais de 50 consultas por segundo**.

### <a name="pricing-tier-capabilities"></a>Capacidades de escalão de preço

| Capacidade                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Pesquisa                                  |        ✓           |     ✓    |
| Encaminhamento                                 |        ✓           |     ✓    |
| Composição                                  |        ✓           |     ✓    |
| Trânsito                                 |        ✓           |     ✓    |
| Fusos horários                              |        ✓           |     ✓    |
| * Imagens mais imagens de híbrida (pré-visualização)  |        ✓           |     ✓    |
| * Intervalo de rota (pré-visualização)                  |        ✓           |     ✓    |
| * Localização do IP 2 (pré-visualização)                |        ✓           |     ✓    |
| * Os polígonos da pesquisa (pré-visualização)         |        ✓           |     ✓    |
| * A geocodificação de batch (pré-visualização)              |        ✓           |     ✓    |
| * (Pré-visualização) encaminhamento do batch                |        ✓           |     ✓    |
| * Roteamento de matriz (pré-visualização)               |        ✓           |     ✓    |

\* Acesso a estas capacidades do S0 escalão de preço será preterido após 4 de Fevereiro de 2019.

Esses pontos de dados adicionais são a pena considerar:
* Que tipo de enterprise tem?
* Quão crítico é o aplicativo sendo criado?

Consulte a **escalão de preço direcionada a clientes** tabela para obter uma melhor noção dos escalões de preços S0 e S1. Para obter mais informações, consulte [preços do Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Os clientes de destino do escalão de preço

| Escalão de preço  |     Clientes de destinados                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>O escalão de preço S0 é para os clientes que são as empresas de pequenas ou médio porte. É o direito de escalão de preço para se não espera grandes volumes de utilizadores em simultâneo. Também é adequada se o geoespacial principais APIs mostradas na tabela anterior cumprir os requisitos de serviço. Esta camada está disponível em geral. Ele funciona para as aplicações em todas as fases de produção: desde o desenvolvimento de uma prova de conceito e a fase inicial de teste para produção de aplicativo e a implantação.<p>|
| S1            |    <p>O escalão de preço S1 destina-se a clientes que precisem de suporte para enterprise em grande escala, aplicativos de missão crítica ou grandes volumes de utilizadores em simultâneo. Também é para os clientes que precisam de serviços geoespaciais avançadas.</p>|

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como ver e alterar os escalões de preço:

> [!div class="nextstepaction"] 
> [Gerir um escalão de preço](how-to-manage-pricing-tier.md)
