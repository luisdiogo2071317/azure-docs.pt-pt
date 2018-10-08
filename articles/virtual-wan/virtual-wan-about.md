---
title: Descrição geral da WAN Virtual do Azure | Microsoft Docs
description: Saiba mais sobre a conectividade ramo a ramo dimensionável automatizada da WAN Virtual.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 09/25/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 54d177d7542d7501a91a955be20af776b16657a2
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182370"
---
# <a name="what-is-azure-virtual-wan"></a>O que é a WAN Virtual do Azure?

A WAN Virtual do Azure é um serviço de rede que fornece conectividade ramo a ramo otimizada e automatizada através do Azure. A WAN Virtual permite-lhe ligar e configurar dispositivos de ramo para comunicar com o Azure. Isto pode ser feito manualmente ou através de dispositivos do fornecedor preferencial através de um parceiro de WAN Virtual. Veja o artigo [Parceiros preferenciais](https://go.microsoft.com/fwlink/p/?linkid=2019615) para obter detalhes. A utilização de dispositivos do parceiro preferencial permite uma fácil utilização, simplificação da conectividade e gestão da configuração. O dashboard incorporado na WAN do Azure apresenta informações instantâneas de resolução de problemas que podem ajudar a poupar tempo e permitem ver facilmente a conectividade em larga escala.

![Diagrama da WAN Virtual](./media/virtual-wan-about/virtualwan.png)

Este artigo fornece uma vista rápida da conectividade de rede das suas cargas de trabalho pertencentes e não pertencentes ao Azure. A WAN Virtual oferece as seguintes vantagens:

* **Soluções de conectividade integradas no hub-and-spoke:** automatize a configuração Site a Site e a conectividade entre os sites no local e um hub do Azure.
* **Instalação e configuração automatizadas do spoke:** ligue as redes virtuais e as cargas de trabalho diretamente ao hub do Azure.
* **Resolução de problemas intuitiva:** pode ver o fluxo ponto a ponto no Azure e utilizar estas informações para tomar as medidas necessárias.

## <a name="s2s"></a>Ligações Site a site

Para criar uma ligação Site a Site através da WAN Virtual, pode prosseguir através de um [parceiro de WAN Virtual](https://go.microsoft.com/fwlink/p/?linkid=2019615) ou criar a ligação manualmente.

### <a name="s2spartner"></a>Trabalhar com um parceiro de WAN Virtual

Quando trabalha com um parceiro de WAN Virtual, o processo é:

1. O controlador de dispositivos de ramo (VPN/SDWAN) é autenticado para exportar informações centradas nos sites para o Azure com um [Principal de Serviço do Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md).
2. O controlador de dispositivos de ramo (VPN/SDWAN) obtém a configuração de conectividade do Azure e atualiza o dispositivo local. Isto automatiza a transferência de configuração, a edição e a atualização do dispositivo VPN no local.
3. Depois de o dispositivo ter a configuração certa do Azure, é estabelecida uma ligação site a site (dois túneis ativos) à WAN do Azure. O Azure suporta IKEv1 e IKEv2. O BGP é opcional.


Se não quiser utilizar um parceiro preferencial, pode configurar a ligação manualmente, veja [Criar uma ligação de Site a Site com WAN Virtual](virtual-wan-site-to-site-portal.md).

## <a name="p2s"></a>Ligações Ponto a site (Pré-visualização)

Uma ligação Ponto a Site (P2S) permite-lhe criar uma ligação segura ao seu hub virtual a partir de um computador cliente individual. É estabelecida uma ligação P2S ao iniciá-la a partir do computador cliente. Esta solução é útil para as pessoas que trabalham à distância que queiram ligar a partir de uma localização remota, como, por exemplo, a partir de casa ou de uma conferência. Uma VPN P2S também é uma solução útil para utilizar em vez de uma VPN S2S, quando são poucos os clientes que precisam de ligar.

Para criar a ligação manualmente, veja [Criar uma ligação Ponto a Site com a WAN Virtual](https://go.microsoft.com/fwlink/p/?linkid=2020051&clcid).

## <a name="er"></a>Ligações do ExpressRoute (Pré-visualização)

Para criar a ligação manualmente, veja [Criar uma ligação ExpressRoute com a WAN Virtual](https://go.microsoft.com/fwlink/p/?linkid=2020148&clcid).


## <a name="resources"></a>Recursos da WAN virtual

Para configurar uma WAN virtual ponto a ponto, crie os seguintes recursos:

* **virtualWAN:** o recurso virtualWAN representa uma sobreposição virtual da rede do Azure e é uma coleção de vários recursos. Contém ligações a todos os hubs virtuais que quer ter na WAN virtual. Os recursos da WAN virtual estão isolados uns dos outros e não podem conter um hub comum. Os Hubs Virtuais na WAN Virtual não comunicam entre si. A propriedade "Permitir tráfego de ramo para ramo" permite o tráfego entre os sites VPN, bem como Sites com o VPN para ExpressRoute ativado. Lembre-se de que o ExpressRoute na WAN Virtual do Azure está atualmente em Pré-visualização.

* **Site:** o recurso de site conhecido como vpnsite representa o dispositivo VPN no local e as respetivas definições. Ao trabalhar com um parceiro de WAN Virtual, tem uma solução integrada para exportar automaticamente estas informações para o Azure.

* **Hub:** um hub virtual é uma rede virtual gerida pela Microsoft. O hub contém vários pontos finais de serviço para ativar a conectividade da rede no local (vpnsite). O hub é o núcleo da sua rede numa região. Só pode existir um hub por região do Azure. Quando cria um hub com o portal do Azure, este cria uma VNet e um vpngateway do hub virtual.

  Um gateway do hub não é igual a um gateway de rede virtual que utiliza para o ExpressRoute e o Gateway de VPN. Por exemplo, ao utilizar a WAN Virtual, não cria uma ligação Site a Site a partir do site no local diretamente à VNet. Em vez disso, cria uma ligação Site a Site ao hub. O tráfego passa sempre pelo gateway do hub. Isto significa que as VNets não precisam do seu próprio gateway de rede virtual. A WAN Virtual permite às VNets tirar partido do fácil dimensionamento através do hub virtual e do gateway do hub virtual. 

* **Ligação de rede virtual do hub:** o recurso de ligação da rede virtual do hub é utilizado para ligar o hub diretamente à sua rede virtual. Neste momento, só pode ligar a redes virtuais que estejam na mesma região do hub.

* **Tabela de rotas do hub:** pode criar uma rota do hub virtual e aplicar a rota à tabela de rotas do hub virtual. Pode aplicar várias rotas à tabela de rotas do hub virtual.

## <a name="faq"></a>FAQ

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]


## <a name="next-steps"></a>Passos seguintes

* Veja as [localizações e parceiros do WAN Virtual](https://aka.ms/virtualwan)