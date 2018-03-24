---
title: Integrar o seu ambiente de serviço de aplicações do ILB com um gateway de aplicação
description: Obter instruções sobre como integrar uma aplicação no seu ambiente de serviço de aplicações do ILB com um gateway de aplicação
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a6a74f17-bb57-40dd-8113-a20b50ba3050
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: ccompy
ms.openlocfilehash: c64b686d7a9016b3834096ebc88179db8972098f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="integrate-your-ilb-app-service-environment-with-an-application-gateway"></a>Integrar o seu ambiente de serviço de aplicações do ILB com um gateway de aplicação #

O [ambiente de serviço de aplicações](./intro.md) é uma implementação do App Service do Azure na sub-rede da rede virtual do Azure de um cliente. Pode ser implementado com um ponto de final público ou privado para acesso à aplicação. A implementação do ambiente de serviço de aplicações com um ponto de final privado (ou seja, um balanceador de carga interno) é chamada um ambiente de serviço de aplicações do ILB.  

Gateway de aplicação do Azure é uma aplicação virtual que fornece balanceamento de carga de camada 7, a descarga de SSL e a proteção de firewall (WAF) de aplicação web. Pode escutar um IP endereços e encaminhar tráfego público para o ponto final da aplicação. 

As informações seguintes descrevem como integrar um gateway de aplicação WAF configurado com uma aplicação num ambiente de serviço de aplicações de ILB.  

A integração do gateway de aplicação com o ambiente de serviço de aplicações do ILB é um nível de aplicação. Quando configurar o gateway de aplicação com o ambiente de serviço de aplicações do ILB, que está a fazê-lo para aplicações específicas no seu ambiente de serviço de aplicações do ILB. Esta técnica permite alojar aplicações multi-inquilino seguras num ambiente de serviço de aplicação único do ILB.  

![Gateway de aplicação que aponta para a aplicação num ambiente de serviço de aplicações do ILB][1]

Nestas instruções, irá:

* Crie um gateway de aplicação.
* Configure o gateway de aplicação para apontar para uma aplicação no seu ambiente de serviço de aplicações do ILB.
* Configure a sua aplicação que respeite o nome de domínio personalizado.
* Edite o nome de anfitrião DNS público que aponta para o gateway de aplicação.

## <a name="prerequisites"></a>Pré-requisitos

Para integrar o gateway de aplicação no seu ambiente de serviço de aplicações do ILB, tem de:

* Um ambiente de serviço de aplicações do ILB.
* Uma aplicação em execução no ambiente de serviço de aplicações do ILB.
* Um nome de domínio encaminhável de internet para ser utilizado com a sua aplicação no ambiente de serviço de aplicações do ILB.
* O endereço do ILB que utiliza o seu ambiente de serviço de aplicações do ILB. Esta informação é o portal de ambiente de serviço de aplicações em **definições** > **endereços IP**:

    ![Lista de exemplo de endereços IP utilizados pelo ambiente de serviço de aplicações do ILB][9]
    
* Um nome DNS público que é utilizado mais tarde para apontar para o gateway de aplicação. 

Para obter mais informações sobre como criar um ambiente de serviço de aplicações do ILB, consulte [criar e utilizar um ambiente de serviço de aplicações do ILB][ilbase].

Este artigo parte do princípio de que pretende que um gateway de aplicação na mesma rede virtual do Azure em que o ambiente de serviço de aplicação é implementado. Antes de começar a criar o gateway de aplicação, escolha ou crie uma sub-rede que irá utilizar para alojar o gateway. 

Deve utilizar a sub-rede não um com o nome GatewaySubnet. Se o put o gateway de aplicação no GatewaySubnet, irá ser não é possível criar um gateway de rede virtual mais tarde. 

Não é possível colocar o gateway na sub-rede que utiliza o seu ambiente de serviço de aplicações do ILB. O ambiente de serviço de aplicações é a única coisa que pode ser nesta sub-rede.

## <a name="configuration-steps"></a>Passos de configuração ##

1. No portal do Azure, aceda a **novo** > **rede** > **Gateway de aplicação**.

2. No **Noções básicas** área:

   a. Para **nome**, introduza o nome do gateway de aplicação.

   b. Para **camada**, selecione **WAF**.

   c. Para **subscrição**, selecione a mesma subscrição que utiliza a rede virtual do ambiente de serviço de aplicações.

   d. Para **grupo de recursos**, crie ou selecione o grupo de recursos.

   e. Para **localização**, selecione a localização da rede virtual do ambiente de serviço de aplicações.

   ![Novo Noções básicas do criação de gateway aplicação][2]

3. No **definições** área:

   a. Para **rede Virtual**, selecione a rede virtual do ambiente de serviço de aplicações.

   b. Para **sub-rede**, selecione a sub-rede onde o gateway de aplicação tem de ser implementado. Não utilize GatewaySubnet, pois impedirá a criação de gateways de VPN.

   c. Para **tipo de endereço IP**, selecione **pública**.

   d. Para **endereço IP público**, selecione um endereço IP público. Se não tiver uma, crie uma agora.

   e. Para **protocolo**, selecione **HTTP** ou **HTTPS**. Se estiver a configurar para HTTPS, terá de fornecer um certificado PFX.

   f. Para **firewall de aplicações Web**, pode ativar a firewall e também defini-lo para um **deteção** ou **prevenção** como julgar.

   ![Novas definições de criação do gateway de aplicação][3]
    
4. No **resumo** secção, reveja as definições e selecione **OK**. O gateway de aplicação pode demorar um pouco mais de 30 minutos para concluir a configuração.  

5. Após a conclusão da configuração do gateway de aplicação, aceda ao seu portal de gateway de aplicação. Selecione **conjunto back-end**. Adicione o endereço do ILB para o ambiente de serviço de aplicações do ILB.

   ![Configurar o conjunto de back-end][4]

6. Após a conclusão do processo de configuração do seu conjunto de back-end, selecione **sondas de estado de funcionamento**. Crie uma sonda do Estado de funcionamento para o nome de domínio que pretende utilizar para a sua aplicação. 

   ![Configurar sondas do estado de funcionamento][5]
    
7. Após a conclusão do processo de configuração da sua sondas de estado de funcionamento, selecione **definições HTTP**. Editar as definições existentes, selecione **sonda de utilização personalizada**e escolha a sonda que configurou.

   ![Configurar definições de HTTP][6]
    
8. Vá para o gateway de aplicação **descrição geral** secção e copie o endereço IP público que utiliza o gateway de aplicação. Definir esse endereço IP como um registo a para o seu nome de domínio de aplicação ou utilize o nome DNS para esse endereço de um registo CNAME. É mais fácil selecionar o endereço IP público e copiá-lo a partir da IU o endereço IP público em vez de copiá-lo a partir da ligação no gateway de aplicação **descrição geral** secção. 

   ![Portal de gateway de aplicação][7]

9. Defina o nome de domínio personalizado para a sua aplicação no seu ambiente de serviço de aplicações do ILB. Aceda à sua aplicação no portal e, em **definições**, selecione **domínios personalizados**.

   ![Definir o nome de domínio personalizado da aplicação][8]

Não há informações sobre a configuração de nomes de domínio personalizados para as suas aplicações web no artigo [definir nomes de domínio personalizado da sua aplicação web][custom-domain]. Mas para uma aplicação num ambiente de serviço de aplicações de ILB, não existe qualquer validação no nome de domínio. Porque o proprietário de DNS que gere os pontos finais de aplicação, pode colocar que pretende no mesmo. O nome de domínio personalizado que adicionar neste caso, não precisa de estar no seu DNS, mas ainda precisa de ser configurada com a sua aplicação. 

Após a conclusão da configuração e que tenham permitido um curto período de tempo para as alterações DNS se propague, pode aceder a sua aplicação, utilizando o nome de domínio personalizado que criou. 


<!--IMAGES-->
[1]: ./media/integrate-with-application-gateway/appgw-highlevel.png
[2]: ./media/integrate-with-application-gateway/appgw-createbasics.png
[3]: ./media/integrate-with-application-gateway/appgw-createsettings.png
[4]: ./media/integrate-with-application-gateway/appgw-backendpool.png
[5]: ./media/integrate-with-application-gateway/appgw-healthprobe.png
[6]: ./media/integrate-with-application-gateway/appgw-httpsettings.png
[7]: ./media/integrate-with-application-gateway/appgw-publicip.png
[8]: ./media/integrate-with-application-gateway/appgw-customdomainname.png
[9]: ./media/integrate-with-application-gateway/appgw-iplist.png

<!--LINKS-->
[appgw]: http://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
