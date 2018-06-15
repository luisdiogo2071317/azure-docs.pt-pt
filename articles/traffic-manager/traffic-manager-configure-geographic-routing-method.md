---
title: Configurar o método de encaminhamento de tráfego geográfica utilizando o Gestor de tráfego do Azure | Microsoft Docs
description: Este artigo explica como configurar o método de encaminhamento de tráfego geográfica utilizando o Gestor de tráfego do Azure
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 7b49e2a4eef5a966f1ef2aa283a3089bb5b73734
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
ms.locfileid: "29397808"
---
# <a name="configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Configurar o método de encaminhamento de tráfego geográfica utilizando o Gestor de tráfego

O método de encaminhamento de tráfego geográfica permite-lhe direcionar o tráfego para pontos finais específicos com base na localização geográfica onde os pedidos têm origem. Este tutorial mostra como criar um perfil de Gestor de tráfego com este método de encaminhamento e configurar os pontos finais para receber o tráfego a partir de localizações geográficas específicas.

## <a name="create-a-traffic-manager-profile"></a>Criar um perfil de Gestor de tráfego

1. Num browser, inicie sessão no [portal do Azure](http://portal.azure.com). Se ainda não tiver uma conta, pode inscrever-se para obter uma [avaliação gratuita durante um mês](https://azure.microsoft.com/free/).
2. Clique em **crie um recurso** > **redes** > **perfil do Traffic Manager** > **criar**.
4. No **perfil do Gestor de tráfego criar**:
    1. Forneça um nome para o seu perfil. Este nome tem de ser exclusivos dentro da zona de trafficmanager.net. Para aceder ao perfil do Traffic Manager, utilizar o nome DNS <profilename>. trafficmanager.net.
    2. Selecione o **Geographic** método de encaminhamento.
    3. Selecione a subscrição que pretende criar este perfil em.
    4. Utilizar um grupo de recursos existente ou crie um novo grupo de recursos para colocar a este perfil em. Se optar por criar um novo grupo de recursos, utilize o **localização do grupo de recursos** pendente para especificar a localização do grupo de recursos. Esta definição refere-se para a localização do grupo de recursos e não tem impacto sobre o perfil do Gestor de tráfego que é implementado global.
    5. Depois de clicar em **criar**, perfil do Traffic Manager é criado e implementado global.

![Criar um perfil do Gestor de Tráfego](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Adicionar pontos finais

1. Procure o nome de perfil do Gestor de tráfego que criou na barra de pesquisa do portal e clique no resultado quando é mostrada.
2. Navegue para **definições** -> **pontos finais** no Traffic Manager.
3. Clique em **adicionar** para mostrar o **adicionar ponto final**.
3. Clique em **adicionar** e no **adicionar ponto final** que é apresentado, conclua os seguintes:
4. Selecione **tipo** consoante o tipo de ponto final que está a adicionar. Para perfis de encaminhamento geográficas utilizadas na produção, recomendamos vivamente que tipos de ponto final aninhada que contenha um perfil de subordinados com mais do que um ponto final a utilizar. Para obter mais detalhes, consulte [perguntas mais frequentes sobre os métodos de encaminhamento de tráfego geográfica](traffic-manager-FAQs.md).
5. Indique um **Nome** pelo qual pretende reconhecer este ponto final.
6. Determinados campos nesta página dependem do tipo de ponto final que está a adicionar:
    1. Se estiver a adicionar um ponto final do Azure, selecione o **tipo de recurso de destino** e **destino** com base nos recursos que pretende direcionar o tráfego para
    2. Se estiver a adicionar um **externo** ponto final, forneça o **nome de domínio completamente qualificado (FQDN)** para o ponto final.
    3. Se estiver a adicionar um **aninhados endpoint**, selecione o **recurso de destino** que corresponde ao perfil subordinado que pretende utilizar e especificar o **número mínimo de pontos finais de subordinados**.
7. Na secção de mapeamento de Georreplicação, utilize o pendente para adicionar as regiões a partir de onde pretende que o tráfego a enviar para este ponto final. Tem de adicionar pelo menos uma região e pode ter várias regiões mapeadas.
8. Repita esta para todos os pontos finais que pretende adicionar sob este perfil

![Adicionar um ponto final do Gestor de tráfego](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Utilizar o perfil do Gestor de tráfego
1.  Na barra de pesquisa do portal, procure o **perfil do Traffic Manager** nome que criou na secção anterior e clique no perfil do Gestor de tráfego nos resultados de que o apresentados.
2. Clique em **descrição geral**.
3. O **perfil do Traffic Manager** apresenta o nome DNS do perfil do Traffic Manager recentemente criado. Isto pode ser utilizado por quaisquer clientes (por exemplo, navegando para a mesma utilizando um browser) obter encaminhados para o ponto final à direita como determinado pelo tipo de encaminhamento.  No caso de encaminhamento geográfica, o Gestor de tráfego analisa o IP de origem do pedido a receber e determina a região a partir do qual é a com origem. Se nessa região é mapeada para um ponto final, o tráfego é encaminhado para o mesmo. Se esta região não está mapeada para um ponto final, o Gestor de tráfego devolve uma resposta de consulta NODATA.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [método de encaminhamento de tráfego Geographic](traffic-manager-routing-methods.md#geographic).
- Saiba como [testar as definições do Gestor de tráfego](traffic-manager-testing-settings.md).
