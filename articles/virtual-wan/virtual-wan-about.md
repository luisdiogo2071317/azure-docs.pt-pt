---
title: Descrição geral da WAN Virtual do Azure | Microsoft Docs
description: Saiba mais sobre a conectividade ramo a ramo dimensionável automatizada da WAN Virtual.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 07/18/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 67dd6ba9b94ed9d58d91fb644ce9ee9e44ae9e45
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159174"
---
# <a name="what-is-azure-virtual-wan-preview"></a>O que é a WAN Virtual do Azure? (Pré-visualização)

A WAN Virtual do Azure é um serviço de rede que fornece conectividade ramo a ramo otimizada e automatizada através do Azure. A WAN Virtual permite-lhe ligar e configurar dispositivos de ramo para comunicar com o Azure. Isto pode ser feito manualmente ou através de dispositivos do fornecedor preferencial através de um parceiro de WAN Virtual. A utilização de dispositivos do fornecedor preferencial permite uma fácil utilização, simplificação da conectividade e gestão da configuração. O dashboard incorporado na WAN do Azure fornece informações instantâneas de resolução de problemas que podem ajudar a poupar tempo e permitem ver facilmente a conectividade Site a Site em larga escala.

> [!IMPORTANT]
> A WAN Virtual do Azure está atualmente em Pré-visualização Pública gerida. Para utilizar a WAN Virtual, tem de [Inscrever-se na Pré-visualização](#enroll).
>
> Esta Pré-visualização Pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

Este artigo fornece uma vista rápida da conectividade de rede das suas cargas de trabalho pertencentes e não pertencentes ao Azure. A WAN Virtual oferece as seguintes vantagens:

* **Soluções de conectividade integradas no hub-and-spoke:** automatize a configuração Site a Site e a conectividade entre os sites no local e um hub do Azure a partir de várias origens, incluindo soluções de parceiros de WAN Virtual.
* **Instalação e configuração automatizadas do spoke:** ligue as redes virtuais e as cargas de trabalho diretamente ao hub do Azure.
* **Resolução de problemas intuitiva:** pode ver o fluxo ponto a ponto no Azure e utilizar estas informações para tomar as medidas necessárias.

![Diagrama da WAN Virtual](./media/virtual-wan-about/virtualwan.png)

## <a name="vendor"></a>Trabalhar com um parceiro de WAN Virtual

1. O controlador de dispositivos de ramo (VPN/SDWAN) é autenticado para exportar informações centradas nos Sites para o Azure com um Principal de Serviço do Azure.
2. O controlador de dispositivos de ramo (VPN/SDWAN) obtém a configuração de conectividade do Azure e atualiza o dispositivo local. Isto automatiza a transferência de configuração, a edição e a atualização do dispositivo VPN no local.
3. Depois de o dispositivo ter a configuração certa do Azure, é estabelecida uma ligação Site a Site (dois túneis ativos) à WAN do Azure. O Azure requer que o controlador de ramo (VPN/SDWAN) suporte o IKEv2. O BGP é opcional.

## <a name="resources"></a>Recursos da WAN virtual

Para configurar uma WAN virtual ponto a ponto, crie os seguintes recursos:

* **virtualWAN:** o recurso virtualWAN representa uma sobreposição virtual da rede do Azure e é uma coleção de vários recursos. Contém ligações a todos os hubs virtuais que quer ter na WAN virtual. Os recursos da WAN virtual estão isolados uns dos outros e não podem conter um hub comum. Os Hubs Virtuais na WAN Virtual não comunicam entre si.

* **Site:** o recurso de site conhecido como vpnsite representa o dispositivo VPN no local e as respetivas definições. Ao trabalhar com um parceiro de WAN Virtual, tem uma solução integrada para exportar automaticamente estas informações para o Azure.

* **Hub:** um hub virtual é uma rede virtual gerida pela Microsoft. O hub contém vários pontos finais de serviço para ativar a conectividade da rede no local (vpnsite). O hub é o núcleo da sua rede numa região. Só pode existir um hub por região do Azure. Quando cria um hub com o portal do Azure, este cria automaticamente uma VNet e um vpngateway do hub virtual.

  Um gateway do hub não é igual a um gateway de rede virtual que utiliza para o ExpressRoute e o Gateway de VPN. Por exemplo, ao utilizar a WAN Virtual, não cria uma ligação Site a Site a partir do site no local diretamente à VNet. Em vez disso, cria uma ligação Site a Site ao hub. O tráfego passa sempre pelo gateway do hub. Isto significa que as VNets não precisam do seu próprio gateway de rede virtual. A WAN Virtual permite às VNets tirar partido do fácil dimensionamento através do hub virtual e do gateway do hub virtual. 

* **Ligação de rede virtual do hub:** o recurso de ligação da rede virtual do hub é utilizado para ligar o hub diretamente à sua rede virtual. Neste momento, só pode ligar a redes virtuais que estejam na mesma região do hub.

##<a name="enroll"></a>Inscrever-se na Pré-visualização

Antes de poder configurar uma WAN Virtual, tem de inscrever primeiro a sua subscrição na Pré-visualização. Caso contrário, não poderá trabalhar com a WAN Virtual no portal. Para se inscrever, envie um e-mail para <azurevirtualwan@microsoft.com> com o ID da subscrição. Receberá um e-mail assim que a sua subscrição tiver sido inscrita.

## <a name="faq"></a>FAQ

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="feedback"></a>Comentários sobre a pré-visualização

Agradecemos os seus comentários. Envie um e-mail para <azurevirtualwan@microsoft.com> para reportar quaisquer problemas ou para faça comentários (positivos ou negativos) sobre a WAN Virtual. Inclua o nome da sua empresa em "[ ]" na linha de assunto. Inclua também o ID da subscrição se estiver a reportar um problema.

## <a name="next-steps"></a>Passos seguintes

Para criar uma ligação Site a Site através da WAN Virtual, pode prosseguir através de um [parceiro de WAN Virtual](https://aka.ms/virtualwan) ou criar a ligação manualmente. Para criar a ligação manualmente, veja [Criar uma ligação Site a Site com a WAN Virtual](virtual-wan-site-to-site-portal.md).
