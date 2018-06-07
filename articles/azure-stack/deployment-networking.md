---
title: Tráfego de rede de implementação de pilha do Azure | Microsoft Docs
description: Este artigo descreve o que pode esperar sobre processos de rede de implementação de pilha do Azure.
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
ms.date: 05/21/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: b808875e66e867b84e2971c6a5bd031d108d003b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655425"
---
# <a name="about-deployment-network-traffic"></a>Sobre o tráfego de rede de implementação
Implementação de compreender como fluxos de tráfego de rede durante a pilha do Azure é vital para assegurar uma implementação com êxito. Este artigo explica como esperado tráfego de rede durante o processo de implementação para fornecer a compreender o que esperar.

Esta ilustração mostra todas as ligações e componentes envolvidos no processo de implementação:

![Topologia de rede de implementação de pilha do Azure](media/deployment-networking/figure1.png)

> [!NOTE]
> Este artigo descreve os requisitos para uma implementação ligado, para saber mais sobre outros métodos de implementação, consulte [modelos de ligação de implementação do Azure pilha](azure-stack-connection-models.md).

### <a name="the-deployment-vm"></a>A implementação da VM
A solução de pilha do Azure inclui um grupo de servidores que são utilizados para alojar os componentes de pilha do Azure e um servidor adicional chamado o anfitrião de ciclo de vida de Hardware (HLH). Este servidor é utilizado para implementar e gerir o ciclo de vida da sua solução e anfitriões de VM de implementação (DVM) durante a implementação.

## <a name="deployment-requirements"></a>Requisitos de implementação
Antes de começa a implementação, existem alguns requisitos mínimos que podem ser validados pelo seu OEM para garantir a implementação for concluída com êxito. Noções sobre que estes requisitos irão ajudá-lo a preparar o ambiente e certifique-se a validação for bem sucedida, estes são:

-   [Certificados](azure-stack-pki-certs.md)
-   [Subscrição do Azure](https://azure.microsoft.com/free/?b=17.06)
-   Acesso à Internet
-   DNS
-   NTP

> [!NOTE]
> Este artigo foca-se nos últimos três requisitos. Para obter mais informações sobre as duas primeiras, consulte as ligações acima.

## <a name="deployment-network-traffic"></a>Tráfego de rede de implementação
O DVM está configurado com um IP da rede de BMC e necessita de acesso de rede à internet. Apesar de todos os componentes de rede de BMC não necessitam de encaminhamento externo ou o acesso à Internet, alguns componentes específicos OEM utilizar IPs desta rede podem igualmente necessitar de-lo.

Durante a implementação, o DVM efetua a autenticação no Azure Active Directory (Azure AD) através de uma conta do Azure da sua subscrição. Para tal, o DVM requer acesso à internet para uma lista de URLs e portas específicas. Pode encontrar a lista completa de [publicar pontos finais](azure-stack-integrate-endpoints.md) documentação. O DVM irá utilizar um servidor DNS para reencaminhar pedidos DNS efetuados por componentes internos para URLs externos. O DNS interno reencaminha estes pedidos para o endereço de reencaminhador DNS que fornecem ao OEM antes da implementação. O mesmo se aplica para o servidor NTP, um servidor fiável de tempo é necessário para manter a consistência e a hora de sincronização de todos os componentes de pilha do Azure.

O acesso à Internet, necessário para o DVM durante a implementação de saída só está, sem chamadas de entrada são efetuadas durante a implementação. Tenha em atenção que utiliza o IP como origem e essa pilha do Azure não suporta configurações de proxy. Por conseguinte, se necessário, terá de fornecer um proxy transparente ou NAT para aceder à internet. Após a conclusão da implementação, todas as comunicações entre o Azure e Azure pilha que são efetuados através da rede externa através de VIPs público.

Configurações de rede nos comutadores de pilha do Azure contêm acesso as listas de controlo (ACLs) que restringem o tráfego entre determinadas origens de rede e destinos. O DVM é o único componente com o acesso sem restrições; mesmo a HLH é muito restrito. Pode colocar o OEM sobre as opções de personalização para facilitar a gestão e acesso a partir da sua rede. Devido a estas ACLs, é importante evitar a alteração dos endereços de servidor DNS e NTP no momento da implementação. Se o fizer, terá de reconfigurar todos os comutadores, para a solução.

Depois de concluída a implementação, os endereços de servidor DNS e NTP fornecidos irão continuar a ser utilizados diretamente pelos componentes do sistema. Por exemplo, se a verificação de pedidos DNS depois de concluída a implementação, a origem deixará do IP do DVM para um endereço do intervalo de rede externa.

Depois de pilha do Azure é implementada com êxito, o seu parceiro OEM poderá utilizar o DVM para tarefas de pós-implementação adicionais. No entanto, quando todas as tarefas de implementação e configurações de pós-implementação são concluídas, OEM deve remover e eliminar o DVM do HLH.

## <a name="next-steps"></a>Passos Seguintes
[Validar o registo do Azure](azure-stack-validate-registration.md)
