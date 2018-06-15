---
title: Tráfego vista no Gestor de tráfego do Azure | Microsoft Docs
description: Introdução à vista de tráfego do Gestor de tráfego
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 7ce51017fdee92e5589c06b398c9650930d5436d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30179842"
---
# <a name="traffic-manager-traffic-view"></a>Vista de tráfego do Gestor de tráfego

Gestor de tráfego fornece com o encaminhamento de nível de DNS para que os utilizadores finais são direcionados para os pontos finais bom estado de funcionamento com base no método de encaminhamento especificou quando criou o perfil. Vista de tráfego fornece o Gestor de tráfego com uma vista das suas bases de utilizadores (a um nível de granularidade de resolução DNS) e os respetivos padrão de tráfego. Quando ativa a vista de tráfego, esta informação é processada para lhe fornecer os conhecimentos acionáveis. 

Ao utilizar a vista de tráfego, pode:
- compreenda onde as bases de utilizadores estão localizadas (até uma local DNS resolução nível granularidade).
- ver o volume de tráfego (observado como consultas DNS, processadas pelo Gestor de tráfego do Azure) provenientes destes regiões.
- obter informações sobre o que é a latência representativa reportada por estes utilizadores.
- descrição detalhada para os padrões de tráfego específica de cada uma destas bases de utilizadores a regiões do Azure onde tiver pontos finais. 

Por exemplo, pode utilizar a vista de tráfego para compreender as regiões que têm um grande número de tráfego mas sofrem do latências. Em seguida, pode utilizar estas informações para planear a expansão de requisitos de espaço para o novo regiões do Azure para que estes utilizadores podem ter uma experiência de latência inferior.

## <a name="how-traffic-view-works"></a>Como funciona a vista de tráfego

Vista de tráfego funciona por meio do Traffic Manager, observe as consultas de entrada recebidas nos últimos sete dias em relação a um perfil que tenha esta funcionalidade ativada. De receber informações de consultas, ver o tráfego extrai o IP de origem de resolução DNS que é utilizado como uma representação da localização dos utilizadores. Estes são, em seguida, agrupados numa granularidade de nível de resolução DNS para criar regiões de base do utilizador, utilizando as informações geográficas de endereços IP mantida pelo Gestor de tráfego. Gestor de tráfego, em seguida, procura em regiões do Azure para a qual a consulta foi encaminhada e constrói um mapeamento de fluxo de tráfego para os utilizadores dessas regiões.  
No próximo passo, Gestor de tráfego está correlacionada com a região de base de utilizador para mapeamento de região do Azure com as tabelas de latência de intelligence rede mantém para redes de outro utilizador final compreender a latência média reportada por utilizadores dessas regiões quando ligar a regiões do Azure. Todos os cálculos estas, em seguida, são combinados num por DNS resolução IP nível local antes de é apresentada ao utilizador. Pode consumir as informações de várias formas.

>[!NOTE]
>A latência descrita na vista de tráfego é uma latência representativa entre o utilizador final e regiões do Azure à qual tinha ligada para e não é a latência de pesquisa DNS. Tráfego Vista torna devolveu uma melhor estimativa de esforço de latência entre a resolução DNS local e a região do Azure, que a consulta foi encaminhada para, se existir dados suficientes disponíveis, em seguida, a latência será nulo. 

## <a name="visual-overview"></a>Descrição geral do Visual

Quando navegar para o **tráfego vista** secção na sua página do Traffic Manager, é-lhe apresentado um mapa geográfico com uma sobreposição das informações de vista de tráfego. O mapa fornece informações sobre a base de utilizadores e os pontos finais para o perfil do Traffic Manager.

### <a name="user-base-information"></a>Informações de base do utilizador

Para essas resoluções DNS locais para a localização de que as informações estão disponíveis, são apresentados no mapa. A cor de resolução de DNS indica que a latência média experiente pelos utilizadores finais que utilizou que resolução DNS para as consultas de Gestor de tráfego.

Se paira o rato sobre uma localização de resolução DNS no mapa, mostra:
- o endereço IP de resolução de DNS
- o volume de tráfego de consulta DNS visto pelo Gestor de tráfego a partir do mesmo
- os pontos finais para que o tráfego do DNS resolução foi encaminhada, como uma linha entre o ponto final e a resolução DNS 
- a latência média de que a localização para o ponto final, representado como a cor da linha ligá-las

### <a name="endpoint-information"></a>Informações de ponto final

Regiões do Azure na qual residem os pontos finais são apresentadas como azuis pontos no mapa. Se o ponto final é externo e não tem uma região do Azure mapeada para a mesma, é apresentada na parte superior do mapa. Clique em qualquer ponto final para ver as localizações diferentes (baseadas na resolução DNS utilizada) do onde o tráfego foi direcionado para esse ponto final. As ligações são apresentadas como uma linha entre o ponto final e a localização de resolução DNS e são caixas com linhas tracejadas, de acordo com a latência representativa entre esse par. Além disso, pode ver o nome do ponto final, a região do Azure em que é executado e o volume total de pedidos que foram direcionados para o mesmo por este perfil do Traffic Manager.


## <a name="tabular-listing-and-raw-data-download"></a>Listagem de tabela e a transferência de dados não processados

Pode ver os dados da vista de tráfego em formato tabular no portal do Azure. Há uma entrada para cada IP de resolução DNS / ponto final emparelhe que mostra o endereço IP de resolução DNS, o nome e localização geográfica da região do Azure em que o ponto final está localizado (se disponível), o volume de pedidos associados esse resolução de DNS para esse ponto final e a latência representativa associados a utilizadores finais a utilizar se o DNS (onde disponível). Também pode transferir os dados de tráfego vista como um ficheiro CSV que pode ser utilizado como parte de um fluxo de trabalho de análise da sua preferência.

## <a name="billing"></a>Faturação

Quando utilizar a vista de tráfego, é-lhe cobrada com base no número de pontos de dados utilizado para criar as informações apresentadas. Atualmente, o tipo de ponto de dados apenas utilizado é as consultas recebidas contra o perfil do Traffic Manager. Para obter mais detalhes sobre os preços, visite o [Gestor de tráfego a página de preços](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Passos Seguintes

- Saiba [como funciona o Gestor de tráfego](traffic-manager-overview.md)
- Saiba mais sobre o [métodos de encaminhamento de tráfego](traffic-manager-routing-methods.md) suportada pelo Gestor de tráfego
- Saiba como [criar um perfil de Gestor de tráfego](traffic-manager-create-profile.md)

