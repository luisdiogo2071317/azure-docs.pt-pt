---
title: Lógica de processamento de regra de Firewall do Azure
description: Saiba mais sobre a lógica de processamento de regra de Firewall do Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/27/2018
ms.author: victorh
ms.openlocfilehash: 542682037a932a2e3b08c71b38b64b2694ad40f3
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228442"
---
# <a name="azure-firewall-rule-processing-logic"></a>Lógica de processamento de regra de Firewall do Azure
Firewall do Azure tem regras NAT, regras de rede e regras de aplicações. As regras são processadas, de acordo com o tipo de regra.


## <a name="network-rules-and-applications-rules"></a>Regras de rede e regras de aplicações 
Regras de rede são regras aplicadas, primeiro, então a aplicação. As regras são de terminação. Portanto, se uma correspondência for encontrada nas regras de rede, em seguida, regras de aplicações não são processadas.  Se não houver nenhuma correspondência de regra de rede, e se o protocolo de pacote é HTTP/HTTPS, o pacote, em seguida, é avaliado pelas regras de aplicação. Se ainda não for encontrada, o pacote é avaliado em relação a coleção de regras de infraestrutura. Se não há nenhuma correspondência, o pacote é negado por predefinição.

## <a name="nat-rules"></a>Regras de NAT
Conectividade de entrada pode ser ativada ao configurar a tradução de endereços de rede de destino (DNAT) conforme descrito em [Tutorial: filtrar o tráfego de entrada com DNAT de Firewall do Azure no portal do Azure](tutorial-firewall-dnat.md). Regras DNAT são aplicadas pela primeira vez. Se for encontrada uma correspondência, é adicionada uma regra implícita de rede correspondente para permitir o tráfego traduzido. Pode substituir esse comportamento adicionando explicitamente uma coleção de regras de rede com regras de negar que correspondem o tráfego traduzido. Não existem regras de aplicações são aplicadas para que estas ligações.


## <a name="next-steps"></a>Passos Seguintes

- Saiba como [implementar e configurar uma Firewall de Azure](tutorial-firewall-deploy-portal.md).