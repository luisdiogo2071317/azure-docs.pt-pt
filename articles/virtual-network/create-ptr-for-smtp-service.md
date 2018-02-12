---
title: "Configurar zonas de pesquisa inversa para uma verificação de faixa SMTP no Azure | Microsoft Docs"
description: "Descreve como configurar as zonas de pesquisa inversa para uma verificação de faixa SMTP no Azure"
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: WillChen
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 02/06/2018
ms.author: genli
ms.custom: 
ms.openlocfilehash: 1e95b00ea08105238a860265e46275c24ed7bfbd
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2018
---
#  <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Configurar zonas de pesquisa inversa para uma verificação de faixa SMTP

Este artigo descreve como utilizar uma zona inversa no DNS do Azure e criar um registo DNS inverso (PTR) para verificar a faixa de SMTP. 

## <a name="symptom"></a>Sintoma

Se alojar um servidor SMTP no Microsoft Azure, poderá receber a seguinte mensagem de erro ao enviar ou receber uma mensagem de servidores de correio remoto:

**554: No PTR Record** 

## <a name="solution"></a>Solução

Para um endereço IP virtual no Azure, são criados os registos inversas pertencentes à empresa zonas de domínio, zonas de domínio personalizado não a Microsoft.

Para configurar os registos PTR no Microsoft pertencentes à empresa zonas, utilize a propriedade de - ReverseFqdn no recurso PublicIpAddress. Para obter mais informações, consulte [configurar inversa de DNS para serviços alojados no Azure](../dns/dns-reverse-dns-for-azure-services.md). 

Quando configurar os registos PTR, certifique-se de que o endereço IP e o FQDN inverso são proprietário da subscrição. Se tentar definir um FQDN inverso que não pertence à subscrição, receberá a mensagem de erro seguinte:

    Set-AzureRmPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership: 
                        
    1) ReverseFqdn corresponde ao fqdn de qualquer recurso de ip público sob a subscrição; 
    2) ReverseFqdn é resolvido para o fqdn (através de cadeia de registos CName) de qualquer recurso de ip público em pertence à subscrição; 
    3) Resolve para o endereço ip (através de cadeia de registos CName e A) de um recurso de ip público estático sob a subscrição. 

Se alterar manualmente a faixa de SMTP para corresponder à nossa predefinido inversamente FQDN, o servidor de correio remoto ainda pode falhar porque poderá esperar o anfitrião de faixa de SMTP para fazer corresponder o registo MX para o domínio.