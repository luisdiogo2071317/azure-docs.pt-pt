---
title: Configurar ativação pós-falha em vários pontos finais CDN do Azure com o Gestor de tráfego do Azure | Microsoft Docs
description: Saiba mais sobre como definir a segurança do Gestor de tráfego do Azure com pontos finais da CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: v-deasim
ms.custom: ''
ms.openlocfilehash: b52cad1f32cc3d16cf70bb81640dcb1d9f8614bf
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133042"
---
# <a name="set-up-failover-across-multiple-azure-cdn-endpoints-with-azure-traffic-manager"></a>Configurar ativação pós-falha em vários pontos finais CDN do Azure com o Gestor de tráfego do Azure

Quando configurar o Azure rede de entrega de conteúdos (CDN), pode selecionar o fornecedor ideal e o escalão de preço para as suas necessidades. CDN do Azure, com a respetiva infraestrutura global distribuída, por predefinição cria redundância geográfica e local e para melhorar o desempenho e disponibilidade do serviço de balanceamento de carga global. Se uma localização não está disponível para servir conteúdo, automaticamente os pedidos são encaminhados para outra localização e o POP ideal (baseado nessas fatores como a carga de localização e o servidor de pedido) é utilizado para servir de cada pedido de cliente. 
 
Se tiver vários perfis da CDN, pode melhorar ainda mais a disponibilidade e desempenho com o Gestor de tráfego do Azure. Pode utilizar o Gestor de tráfego do Azure com CDN do Azure para equilibrar entre vários pontos finais da CDN para ativação pós-falha, carga georreplicação balanceamento e outros cenários. Num cenário típico de ativação pós-falha, todos os pedidos de cliente primeiro são direcionados para o perfil CDN primário; Se o perfil não estiver disponível, pedidos, em seguida, são transmitidos para o perfil CDN secundário até que o perfil de CDN principal esteja novamente online. Utilizando o Gestor de tráfego do Azure desta forma, garante que a aplicação web está sempre disponível. 

Este artigo fornece orientações e um exemplo de como configurar a ativação pós-falha com **CDN do Azure Standard da Verizon** e **CDN do Azure Standard da Akamai** perfis.

## <a name="set-up-azure-cdn"></a>Configurar a CDN do Azure 
Crie dois ou mais perfis da CDN do Azure e os pontos finais com fornecedores diferentes.

1. Criar um **CDN do Azure Standard da Verizon** e **CDN do Azure Standard da Akamai** perfil seguindo os passos no [criar um novo perfil CDN](cdn-create-new-endpoint.md#create-a-new-cdn-profile).
 
   ![CDN vários perfis](./media/cdn-traffic-manager/cdn-multiple-profiles.png)

2. Em cada um dos perfis de novo, criar, pelo menos, um ponto final, seguindo os passos no [criar um novo ponto final da CDN](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint).

## <a name="set-up-azure-traffic-manager"></a>Definir a segurança do Gestor de tráfego do Azure
Criar um perfil do Traffic Manager do Azure e configurar o balanceamento de carga entre os pontos finais da CDN. 

1. Criar um perfil do Traffic Manager do Azure, seguindo os passos no [criar um perfil do Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile#create-a-traffic-manager-profile-1). 

    Para **método de encaminhamento**, selecione **prioridade**.

2. Adicionar os pontos finais da CDN no perfil do Traffic Manager seguindo os passos no [pontos finais de adicionar o Gestor de tráfego](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-create-profile#add-traffic-manager-endpoints)

    Para **tipo**, selecione **pontos finais externos**. Para **prioridade**, introduza um número.

    Por exemplo, criar *cdndemo101akamai.azureedge.net* com uma prioridade de *1* e *cdndemo101verizon.azureedge.net* com uma prioridade de *2*.

   ![Pontos finais de Gestor de tráfego CDN](./media/cdn-traffic-manager/cdn-traffic-manager-endpoints.png)


## <a name="set-up-custom-domain-on-azure-cdn-and-azure-traffic-manager"></a>Configurar o domínio personalizado na CDN do Azure e o Gestor de tráfego do Azure
Depois de configurar os perfis da CDN e Gestor de tráfego, siga estes passos para adicionar o mapeamento de DNS e registar o domínio personalizado para os pontos finais da CDN. Neste exemplo, o nome de domínio personalizado é *cdndemo101.dustydogpetcare.online*.

1. Visite o web site para o fornecedor de domínio do seu domínio personalizado, como o GoDaddy e crie duas entradas DNS CNAME. 

    a. A entrada de CNAME primeiro, mapear o domínio personalizado, com o subdomínio cdnverify, para o ponto final de CDN. Esta entrada é um passo necessário para registar o domínio personalizado para o ponto final da CDN que adicionou ao Gestor de tráfego no passo 2.

      Por exemplo: 

      `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101akamai.azureedge.net`  

    b. A segunda entrada CNAME, mapear o domínio personalizado, sem o subdomínio cdnverify, para o ponto final de CDN. Esta entrada mapeia o domínio personalizado para o Gestor de tráfego. 

      Por exemplo: 
      
      `cdndemo101.dustydogpetcare.online  CNAME  cdndemo101.trafficmanager.net`   

    > [!NOTE]
    > Se o seu domínio estiver atualmente em direto e não pode ser interrompido, execute este passo pela última vez. Verifique se os pontos finais da CDN e domínios do Gestor de tráfego em direto antes de atualizar o DNS de domínio personalizado para o Gestor de tráfego.
    >


2.  A partir do seu perfil de CDN do Azure, selecione o primeiro ponto final de CDN (Akamai). Selecione **Adicionar domínio personalizado** e entrada *cdndemo101akamai.azureedge.net*. Certifique-se de que a marca de verificação para validar o domínio personalizado é verde. 

    CDN do Azure utiliza o *cdnverify* subdomínio para validar o mapeamento de DNS para concluir este processo de registo. Para obter mais informações, consulte [criar um registo CNAME DNS](cdn-map-content-to-custom-domain.md#create-a-cname-dns-record). Este passo ativa a CDN do Azure reconhecer o domínio personalizado para que pode responder os pedidos.

3.  Voltar para o web site para o fornecedor de domínio do seu domínio personalizado e atualize o mapeamento de DNS primeiro que criou para que o domínio personalizado está mapeado para o segundo ponto final da CDN.
                             
    Por exemplo: 

    `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101verizon.azureedge.net`  

4. A partir do seu perfil de CDN do Azure, selecione o segundo ponto final de CDN (da Verizon) e repita o passo 2. Selecione **Adicionar domínio personalizado**e entrada *cdndemo101akamai.azureedge.net*.
 
Depois de concluir estes passos, o serviço de várias CDN com capacidades de ativação pós-falha é configurado com o Gestor de tráfego do Azure. Poderá aceder o teste de URL do seu domínio personalizado. Para testar a funcionalidade, desative o principal ponto final da CDN e certifique-se de que o pedido é corretamente movido para o ponto final CDN secundário. 

## <a name="next-steps"></a>Passos Seguintes
Pode também configurar outros métodos de encaminhamento, tais como geográfica para equilibrar a carga entre os diferentes pontos finais da CDN. Para obter mais informações, consulte [configurar o método de encaminhamento de tráfego geográfica utilizando o Gestor de tráfego](https://docs.microsoft.com/en-us/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).



