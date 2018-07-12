---
title: Acerca das ligações de VPN do Azure ponto a Site | Documentos da Microsoft
description: Este artigo ajuda-o a compreender as ligações ponto a Site e ajuda-o a decidir o tipo de autenticação de gateway de P2S VPN a utilizar.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/06/2018
ms.author: cherylmc
ms.openlocfilehash: 2668d92b5b933f7ccf8ebcccbe7ea77ea6ea1e86
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38721433"
---
# <a name="about-point-to-site-vpn"></a>Acerca da VPN ponto a Site

Uma ligação de gateway de VPN Ponto a Site (P2S) permite-lhe criar uma ligação segura à sua rede virtual a partir de um computador cliente individual. É estabelecida uma ligação P2S ao iniciá-la a partir do computador cliente. Esta solução é útil para as pessoas que trabalham à distância que queiram ligar às VNets do Azure a partir de uma localização remota, como, por exemplo, a partir de casa ou de uma conferência. Uma VPN P2S também é uma solução útil para utilizar em vez de uma VPN S2S, quando são poucos os clientes que precisam de ligar a uma VNet. Este artigo aplica-se ao modelo de implementação clássica e do Resource Manager.

## <a name="protocol"></a>Qual protocolo usar P2S?

VPN Point-to-site pode utilizar um dos seguintes protocolos:

* Secure Socket Tunneling Protocol (SSTP), um protocolo proprietário de VPN baseado em SSL. Uma solução de SSL VPN consegue penetrar firewalls, uma vez que a maioria das firewalls abre a porta TCP 443, que usa SSL. O SSTP só é suportado em dispositivos Windows. O Azure suporta todas as versões do Windows que têm de SSTP (Windows 7 e posterior).

* VPN IKEv2, uma solução de VPN IPsec baseada em normas. A VPN IKEv2 pode ser utilizada para ligar a partir de dispositivos Mac (versões de OSX 10.11 e superiores).

Se tiver um ambiente misto de cliente consiste em dispositivos Windows e Mac, configure o SSTP e IKEv2.

>[!NOTE]
>O IKEv2 para P2S está disponível para apenas o modelo de implementação do Resource Manager. Não está disponível para o modelo de implementação clássica.
>

## <a name="authentication"></a>Como é que os clientes VPN de P2S são são autenticados?

Antes de Azure aceita uma ligação P2S VPN, o utilizador tem de ser autenticada pela primeira vez. Há dois mecanismos que o Azure oferece para autenticar um usuário conectado.

### <a name="authenticate-using-native-azure-certificate-authentication"></a>Autenticar através da autenticação de certificados nativa do Azure

Ao utilizar a autenticação de certificados nativa do Azure, um certificado de cliente que está presente no dispositivo é utilizado para autenticar o usuário conectado. Certificados de cliente são gerados a partir de um certificado de raiz fidedigna e, em seguida, instalados em cada computador cliente. Pode utilizar um certificado de raiz que tenha sido gerado com uma solução empresarial ou pode gerar um certificado autoassinado.

A validação do certificado de cliente é executada pelo gateway de VPN e ocorre durante o estabelecimento da conexão VPN de P2S. O certificado de raiz é necessário para a validação e tem de ser carregado para o Azure.

### <a name="authenticate-using-active-directory-ad-domain-server"></a>Autenticar com o servidor de domínio do Active Directory (AD)

Autenticação de domínio do AD permite aos utilizadores ligar ao Azure através das respetivas credenciais de domínio da organização. Ele requer um servidor RADIUS que se integra com o servidor do AD. As organizações também podem aproveitar a implantação de RADIUS existente.   
  O servidor RADIUS pode ser implementado no local ou na sua VNET do Azure. Durante a autenticação, o Gateway de VPN do Azure funciona como um pass-through e encaminha as mensagens de autenticação e volta entre o servidor RADIUS e o dispositivo de ligação. Portanto, a acessibilidade de Gateway para o servidor RADIUS é importante. Se o servidor RADIUS estiver presente no local, em seguida, uma ligação de VPN S2S do Azure para o site no local é necessária para acessibilidade.  
  O servidor RADIUS também pode integrar com serviços de certificados do AD. Isto permite-lhe utilizar o servidor RADIUS e sua implementação do certificado de empresa para autenticação de certificados P2S como uma alternativa para a autenticação de certificados do Azure. A vantagem é que não é necessário carregar certificados de raiz e certificados revogados para o Azure.

Um servidor RADIUS também pode integrar com outros sistemas de identidade externo. Isso abre muitas opções de autenticação para VPN P2S, incluindo opções de acesso multifatores.

! [ponto a site]] (./media/point-to-site-about/p2s.png "Point-to-Site")

## <a name="what-are-the-client-configuration-requirements"></a>Quais são os requisitos de configuração de cliente?

>[!NOTE]
>Para clientes do Windows, tem de ter direitos de administrador no dispositivo cliente para iniciar a ligação de VPN do dispositivo cliente para o Azure.
>

Os utilizadores utilizam os clientes VPN nativos em dispositivos Windows e Mac para P2S. O Azure disponibiliza um cliente VPN zip de configuração ficheiro que contém as definições necessárias por estes clientes nativos para ligar ao Azure.

* Para dispositivos Windows, a configuração do cliente VPN é composta por um pacote de instalador que os usuários instalam em seus dispositivos.
* Para dispositivos Mac, ele consiste no ficheiro de mobileconfig que os usuários instalam em seus dispositivos.

O ficheiro zip também fornece os valores de algumas das definições importantes no lado do Azure que pode utilizar para criar seu próprio perfil para estes dispositivos. Alguns dos valores incluem o endereço de gateway de VPN, os tipos de túnel configurado, as rotas e o certificado de raiz para validação de gateway.

>[!NOTE]
>[!INCLUDE [TLS version changes](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="gwsku"></a>Quais P2S de suporte de SKUs de Gateway VPN?

[!INCLUDE [p2s-skus](../../includes/vpn-gateway-table-point-to-site-skus-include.md)]

* A Referência de Débito de Agregação baseia-se em medidas de vários túneis agregados através de um único gateway. Não é uma taxa de transferência garantida devido a condições de tráfego de internet e comportamentos da sua aplicação.
* Informações sobre os preços podem ser encontrados na página de preços 
* Informações de SLA (contrato de nível de serviço) podem ser encontradas na página do SLA.

>[!NOTE]
>A SKU Básica não suporta a autenticação IKEv2 ou RADIUS.
>

## <a name="configure"></a>Como posso configurar uma ligação P2S?

Uma configuração P2S requer algumas etapas específicas. Os artigos seguintes contêm as etapas para orientá-lo através da configuração de P2S e links para configurar os dispositivos de cliente VPN:

* [Configurar uma ligação de P2S - autenticação RADIUS](point-to-site-how-to-radius-ps.md)

* [Configurar uma ligação de P2S - autenticação de certificados nativa do Azure](vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="faqcert"></a>FAQ sobre a autenticação de certificados nativa do Azure

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-azurecert-include.md)]

## <a name="faqradius"></a>FAQ sobre a autenticação RADIUS

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-faq-p2s-radius-include.md)]

## <a name="next-steps"></a>Próximos Passos

* [Configurar uma ligação de P2S - autenticação RADIUS](point-to-site-how-to-radius-ps.md)

* [Configurar uma ligação de P2S - autenticação de certificados nativa do Azure](vpn-gateway-howto-point-to-site-rm-ps.md)