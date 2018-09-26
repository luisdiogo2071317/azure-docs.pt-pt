---
title: Dimensionamento automático e o Gateway de aplicação com redundância de zona no Azure (pré-visualização pública) | Documentos da Microsoft
description: Este artigo fornece informações sobre limites de tamanho de pedido de firewall de aplicação web e listas de exclusão no Gateway de aplicação com o portal do Azure.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: victorh
ms.openlocfilehash: ab1c9405042de02183b8742fa940a3a5a482923a
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165234"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Dimensionamento automático e o Gateway de aplicação com redundância de zona (pré-visualização pública)

Gateway de aplicação e de Firewall de aplicações Web (WAF) estão agora disponíveis em pré-visualização pública num novo SKU que oferece aprimoramentos de desempenho e adiciona suporte para críticos novos recursos, como o dimensionamento automático, a redundância de zona e o suporte para VIPs estáticos. Sob o SKU disponível em geral, os recursos existentes continuam a ser suportado no novo SKU, com algumas exceções, listadas na seção de limitações conhecidas. Os novos SKU incluem os seguintes aprimoramentos:

- **Dimensionamento automático**: padrões de carga de Gateway de aplicação ou WAF as implementações ao abrigo o dimensionamento automático do SKU pode aumentar ou reduzir verticalmente com base no tráfego a alteração. Dimensionamento automático também remove o requisito para escolher uma contagem de instâncias ou de tamanho de implementação durante o aprovisionamento. Por conseguinte, o SKU oferece verdadeira elasticidade. No novo SKU, o Gateway de aplicação pode funcionar tanto em capacidade fixa (dimensionamento automático desativada) bem como no modo de dimensionamento automático ativado. Modo de capacidade fixo é útil para cenários com cargas de trabalho consistentes e previsíveis. Modo de dimensionamento automático é benéfico em aplicações que observamos muita variação no tráfego de aplicativo.
   
   > [!NOTE]
   > Dimensionamento automático não está atualmente disponível para o SKU da WAF. Configure a WAF com o modo de capacidade fixo, em vez do modo de dimensionamento automático.
- **Redundância de zona**: implementação de um Gateway de aplicação ou WAF pode abranger várias zonas de disponibilidade, eliminando a necessidade de aprovisionar e crie instâncias separadas do Gateway de aplicação em cada zona com o Gestor de tráfego. Pode escolher uma zona única ou várias zonas de onde as instâncias de Gateway de aplicação estão implementadas, portanto resiliência de falha de zona garantir. Pode ser distribuído, da mesma forma, o conjunto de back-end para aplicações em zonas de disponibilidade.
- **Melhorias de desempenho**: O dimensionamento automático do SKU oferece até 5 X melhor desempenho de descarga SSL em comparação com o SKU em disponibilidade geral.
- **Tempo de implantação e atualização mais rápido** o dimensionamento automático do SKU fornece o tempo de implantação e atualização mais rápido em comparação com o SKU em disponibilidade geral.
- **VIP estático**: O VIP do gateway de aplicação suporta agora o tipo de VIP estático exclusivamente. Isto garante que o VIP associado ao gateway de aplicação não altera, mesmo após um reinício.

> [!IMPORTANT]
> O dimensionamento automático e o gateway de aplicação com redundância de zona SKU está atualmente em pré-visualização pública. Esta pré-visualização é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Algumas funcionalidades poderão não ser suportadas ou podem ter capacidades restringidas. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Regiões suportadas
SKU de dimensionamento automático está disponível nos E.U.A. Leste 2, E.u.a. Central, E.u.a. oeste2, França, Europa Ocidental e Sudeste asiático.

## <a name="pricing"></a>Preços
Durante a pré-visualização, não existe nenhum custo associado. Vai ser cobrada a recursos que não seja o gateway de aplicação, como o Cofre de chaves, máquinas virtuais, etc. 

## <a name="known-issues-and-limitations"></a>Problemas e limitações conhecidos

|Problema|Detalhes|
|--|--|
|Faturação|Não existe atualmente nenhum faturação.|
|Modo FIPS, o WebSocket|Estes não são atualmente suportadas.|
|Modo só de ILB|Isso não é atualmente suportado. Público e o modo ILB é suportado em conjunto.|
|Dimensionamento automático de firewall de aplicação Web|WAF não suporta o modo de dimensionamento automático. Modo de capacidade fixo é suportado.|

## <a name="next-steps"></a>Passos Seguintes
- [Criar um dimensionamento automático, o gateway de aplicação com redundância de zona com um endereço IP virtual reservado com o Azure PowerShell](tutorial-autoscale-ps.md)
- Saiba mais sobre [Gateway de aplicação](overview.md).
- Saiba mais sobre [Firewall do Azure](../firewall/overview.md). 

