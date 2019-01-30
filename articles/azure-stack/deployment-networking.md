---
title: Tráfego de rede de implementação do Azure Stack | Documentos da Microsoft
description: Este artigo descreve o que esperar sobre processos de implementação de sistema de rede do Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.lastreviewed: 08/30/2018
ms.openlocfilehash: 3e53d79682772be7eacb649148ceeaec7d9f2865
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247583"
---
# <a name="about-deployment-network-traffic"></a>Sobre o tráfego de rede de implementação
Compreender como fluxos de tráfego de rede do Azure Stack durante a implementação é vital para assegurar uma implantação bem-sucedida. Este artigo orienta-o tráfego de rede esperado durante o processo de implementação para fornecer uma compreensão do que esperar.

Esta ilustração mostra todas as ligações e componentes envolvidos no processo de implantação:

![Topologia de rede de implementação do Azure Stack](media/deployment-networking/figure1.png)

> [!NOTE]
> Este artigo descreve os requisitos para uma implementação ligada, para saber mais sobre outros métodos de implantação, consulte [modelos de ligação de implementação do Azure Stack](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>A implementação da VM
A solução do Azure Stack inclui um grupo de servidores que são utilizados para alojar componentes do Azure Stack e um servidor extra chamado o anfitrião de ciclo de vida do Hardware (HLH). Este servidor é utilizado para implementar e gerir o ciclo de vida da sua solução e aloja a VM de implementação (DVM) durante a implementação.

## <a name="deployment-requirements"></a>Requisitos de implementação
Antes de inicia a implementação, existem alguns requisitos mínimos que podem ser validados pelo seu OEM para garantir a implementação for concluída com êxito. Noções básicas sobre estes requisitos irão ajudá-lo a preparar o ambiente e certifique-se de validação tiver êxito, estes são:

-   [Certificados](azure-stack-pki-certs.md)
-   [Subscrição do Azure](https://azure.microsoft.com/free/?b=17.06)
-   Acesso à Internet
-   DNS
-   NTP

> [!NOTE]
> Este artigo se concentra nos últimos três requisitos. Para obter mais informações sobre os dois primeiros, veja as ligações acima.

## <a name="deployment-network-traffic"></a>Tráfego de rede de implementação
O DVM está configurado com um IP da rede de BMC e requer acesso à rede para a internet. Embora não todos os componentes de rede de BMC necessitem de acesso à Internet ou de encaminhamento externo, alguns componentes específicos de OEM utilizando IPs desta rede também exijam-lo.

Durante a implementação, o DVM autentica relativamente ao Azure Active Directory (Azure AD) com uma conta do Azure da sua subscrição. Para fazer isso, o DVM requer acesso à internet para uma lista de URLs e porta específica. Pode encontrar a lista completa da [publicar pontos de extremidade](azure-stack-integrate-endpoints.md) documentação. O DVM passarão a utilizar um servidor DNS para reencaminhar pedidos DNS efetuados por componentes internos para URLs externos. O DNS interno encaminha estes pedidos para o endereço de reencaminhador DNS por si para OEM antes da implantação. O mesmo é válido para o servidor NTP, um servidor confiável de tempo é necessário para manter a consistência e a hora de sincronização de todos os componentes do Azure Stack.

O acesso à internet necessário para o DVM durante a implementação é apenas saído, chamadas de entrada são efetuadas durante a implementação. Tenha em atenção que ele usa o seu IP como origem e que o Azure Stack não suporta configurações de proxy. Por conseguinte, se necessário, terá de fornecer um proxy transparente ou o NAT para aceder à internet. Durante a implementação, alguns componentes internos começará a aceder à internet através da rede externa VIPs pública a utilizar. Depois de concluída a implementação, todas as comunicações entre o Azure e o Azure Stack é feita através da rede externa, usando o VIP público.

Configurações de rede no Azure Stack comutadores contenham acesso listas de controle (ACLs) que restringem o tráfego entre determinadas origens de rede e destinos. O DVM é o único componente com o acesso sem restrições; até mesmo o HLH é muito restrito. Pode pedir ao OEM sobre as opções de personalização para facilitar a gestão e acesso de suas redes. Devido a estas ACLs, é importante evitar a alteração dos endereços de servidor DNS e NTP no momento da implementação. Se fizer isso, terá de reconfigurar todas as opções para a solução.

Depois de concluída a implementação, os endereços de servidor DNS e NTP fornecidos irão continuar a ser utilizados diretamente pelos componentes do sistema. Por exemplo, se verificar os pedidos DNS após conclusão da implementação, a origem deixará do IP do DVM para um endereço do intervalo de rede externa.

Depois de concluída a implementação, os endereços de servidor DNS e NTP fornecidos irão continuar a ser utilizado pelos componentes do sistema por meio de SDN com a rede externa. Por exemplo, se verificar os pedidos DNS após conclusão da implementação, a origem será alterado de IP do DVM para um VIP público.

## <a name="next-steps"></a>Passos Seguintes
[Validar o registo do Azure](azure-stack-validate-registration.md)
