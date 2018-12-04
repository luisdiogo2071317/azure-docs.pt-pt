---
title: Escolher o melhor de escalão de preço para o Azure Maps | Documentos da Microsoft
description: Saiba mais sobre oferecidos pelo Azure Maps de escalões de preço
author: walsehgal
ms.author: v-musehg
ms.date: 12/03/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 20c59f95dcdb47ef3e457f177d7ad1a300981907
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856273"
---
# <a name="choosing-the-right-pricing-tier-in-azure-maps"></a>Escolher o direito de escalão de preço no Azure Maps

Mapas do Azure oferece dois escalões de preços. O objetivo deste artigo é para o ajudar a escolher o melhor de preços para as suas necessidades. Para ajudar a escolher o melhor de escalão de preço, pergunte-se duas perguntas:

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Que capacidades geoespaciais planeja usar?
Se sentir que os seus requisitos de serviço são satisfeitos pela geoespacial principais APIs, em seguida, o escalão de preço S0 é adequado para si. Se pretender que os recursos mais avançados para a sua aplicação, como imagens de uma + híbrida, obtendo o intervalo de rota, geocodificação, etc. do batch, considere optar para o escalão de preço S1. A tabela abaixo com **capacidades de escalão de preço** irá fornecer-lhe uma ideia melhor de necessidades da sua aplicação e também ajudará escolher o escalão mais adequado para a sua aplicação de um preço.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>O número de utilizadores em simultâneo planear oferecer suporte? 
S0 e escalões de preço S1 podem lidar com diferentes quantidades de débito de dados. Antes de escolher um escalão de preço do Azure Maps, considere se perguntando a perguntas como o número de utilizadores em simultâneo que pretende suportar? O S0 ao escalão de preço pode processar até **50 consultas por segundo** e o S1 pode manipular o escalão de preço **mais de 50 consultas por segundo**.

### <a name="pricing-tier-capabilities"></a>Capacidades de escalão de preço

| Capacidade                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Pesquisa                                  |        ✓           |     ✓    |
| Encaminhamento                                 |        ✓           |     ✓    |
| Composição                                  |        ✓           |     ✓    |
| Trânsito                                 |        ✓           |     ✓    |
| Fusos horários                              |        ✓           |     ✓    |
| Imagens + híbrida imagens (pré-visualização)      |                    |     ✓    |
| Intervalo de rota (pré-visualização)                   |                    |     ✓    |
| Localização do IP 2 (pré-visualização)                 |                    |     ✓    |
| Polígonos da pesquisa (pré-visualização)          |                    |     ✓    |
| Geocodificação de batch (pré-visualização)               |                    |     ✓    |
| Encaminhamento de batch (pré-visualização)                 |                    |     ✓    |
| Roteamento de matriz (pré-visualização)                |                    |     ✓    |


Alguns pontos de dados adicionais que vale a pena considerar, que tipo de enterprise tem ou quão crítico é o aplicativo sendo criados?

Consulte a tabela com **escalão de preço direcionados aos clientes** para ter uma ideia melhor dos escalões de preços S0 e S1. Para obter mais informações sobre os preços do Azure Maps, veja [preços do Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Os clientes de destino do escalão de preço

| Escalão de preço  |        Clientes de destinados                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>O escalão de preço S0 é para os clientes que são as empresas de pequenas ou médio porte. É um direito de escalão de preço para se não espera grandes volumes de utilizadores em simultâneo e os requisitos de serviço são satisfeitos pela geoespacial core APIs conforme indicado pela tabela abaixo. Esta camada está disponível em geral e é aplicável para aplicações em todas as fases de produção desde a prova de desenvolvimento do conceito e a fase inicial de teste para produção de aplicativo e a implantação.<p>|
| S1            |    <p>O escalão de preço S1 é para os clientes que necessitam de suporte em grande escala empresarial, aplicativos de missão crítica, grandes volumes de utilizadores em simultâneo, ou requer serviços geoespaciais avançadas.</p>|


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre ver e alterar o escalão de preço:

> [!div class="nextstepaction"]
> [Gerir o escalão de preço](how-to-manage-pricing-tier.md)