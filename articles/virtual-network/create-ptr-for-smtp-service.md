---
title: Configurar zonas de pesquisa inversa para uma verificação de faixa de SMTP no Azure | Documentos da Microsoft
description: Descreve como configurar zonas de pesquisa inversa para uma verificação de faixa de SMTP no Azure
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: WillChen
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.custom: ''
ms.openlocfilehash: 815e3c711850eab11aef63e04a1c512c4510a910
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684299"
---
#  <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Configurar zonas de pesquisa inversa para uma verificação de faixa de SMTP

Este artigo descreve como utilizar uma zona inversa no DNS do Azure e criar um registo de DNS inverso (PTR) para a verificação de faixa de SMTP. 

## <a name="symptom"></a>Sintoma

Se alojar um servidor de SMTP no Microsoft Azure, poderá receber a seguinte mensagem de erro quando enviar ou receber uma mensagem de servidores de correio remoto:

**554: nenhum registo PTR** 

## <a name="solution"></a>Solução

Para um endereço IP virtual no Azure, os inversos registos são criados no Microsoft pertencentes à empresa zonas de domínio, zonas de domínio personalizado não.

Para configurar os registos PTR no Microsoft pertencentes à empresa zonas, utilize a propriedade - ReverseFqdn no recurso PublicIpAddress. Para obter mais informações, consulte [configurar DNS inverso para os serviços alojados no Azure](../dns/dns-reverse-dns-for-azure-services.md). 

Ao configurar os registros PTR, certifique-se de que o endereço IP e o FQDN inverso são propriedade da subscrição. Se tentar definir um FQDN inverso que não pertence à subscrição, receberá a seguinte mensagem de erro:

    Set-AzureRmPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership: 
                        
    1) ReverseFqdn corresponde ao fqdn de qualquer recurso de ip público na subscrição; 
    2) ReverseFqdn é resolvido para o fqdn (através de cadeia de registos CName) de qualquer recurso de ip público na subscrição; 
    3) Ele é resolvido para o endereço de ip (através de CName e A cadeia de registos) de um recurso de ip público estático na subscrição. 

Se alterar manualmente a faixa de SMTP para corresponder ao nosso padrão inverter FQDN, o servidor de correio remoto ainda pode falhar porque ele poderá esperar o anfitrião de faixa de SMTP de acordo com o registro MX do domínio.