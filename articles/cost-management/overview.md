---
title: "Descrição geral do Azure Cost Management da Cloudyn | Microsoft Docs"
description: "A Gestão de Custos do Azure, da Cloudyn, é uma solução de gestão de custos multiclouds que o ajuda a utilizar o Azure e outros recursos da cloud."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: overview
ms.service: cost-management
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 969340080bfe2b04704367c2225895728773119e
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2017
---
# <a name="what-is-azure-cost-management"></a>O que é o Azure Cost Management?

O Azure Cost Management da Cloudyn permite-lhe controlar a utilização da cloud e as despesas dos seus recursos do Azure e outros fornecedores de cloud, incluindo AWS e Google. Os relatórios do dashboard fáceis de compreender também ajudam na alocação de custos e showbacks/estornos. O Cost Management ajuda a otimizar os gastos da cloud ao identificar recursos subutilizados que possa gerir e ajustar.

Para ver um vídeo introdutório, veja [Introduction to Azure Cost Management by Cloudyn (Introdução ao Azure Cost Management da Cloudyn)](https://youtu.be/NWIRny6Wpsk).

## <a name="monitor-usage-and-spending"></a>Monitorizar a utilização e os gastos

Monitorizar a utilização e os gastos é extremamente importante para infraestruturas da cloud porque as organizações pagam pelos recursos que consomem ao longo do tempo. Quando a utilização excede os limiares do contrato, podem ocorrer excessos de custos inesperados. Alguns fatores importantes podem dificultar a monitorização do ad-hoc. Em primeiro lugar, a projeção de custos baseada na utilização média assume que o consumo permanece consistente num determinado período de faturação. Em segundo lugar, quando os custos estão próximos ou excedem o seu orçamento, é importante receber notificações de forma pró-ativa para ajustar os gastos. Além disso, os fornecedores de serviços cloud podem não oferecer projeção de custo de acordo com os limiares ou relatórios de comparação de períodos.

Os relatórios ajudam a monitorizar os gastos para analisar e controlar a utilização da cloud, os custos e as tendências. Ao utilizar relatórios ao Longo do Tempo, pode detetar anomalias diferentes das tendências normais. São visíveis ineficiências na sua implementação da cloud nos relatórios de otimização. Também pode reparar em ineficiências nos relatórios de análise de custos.

![Relatório de Custos ao Longo do Tempo](media\overview\cost-over-time-rpt.png)


## <a name="manage-costs"></a>Gerir os custos

Os dados históricos podem ajudá-lo a gerir os custos, ao analisar os custos de utilização e ao longo do tempo para identificar tendências. As tendências são então utilizadas para prever despesas futuras. O Cost Management também inclui relatórios úteis de custo previsto.

A alocação de custos gere os custos ao analisá-los com base na sua política de etiquetagem. Pode utilizar etiquetas nas suas contas personalizadas, recursos e entidades para refinar a alocação de custos. O Gestor de Categoria organiza as suas etiquetas para ajudar a oferecer governação adicional. E utiliza a alocação de custos para análise de custos/estorno para mostrar a utilização de recursos e os custos associados para influenciar os comportamentos de consumo ou cobrar clientes de inquilino.

O controlo de acesso ajuda a gerir os custos, ao garantir que os utilizadores e as equipas acedam apenas aos dados de gestão de custos que precisam. Utilize a estrutura de entidade, a gestão de utilizadores e os relatórios agendados com listas de destinatários para atribuir acesso.

O alerta ajuda a gerir os custos ao notificá-lo automaticamente quando ocorrerem despesas invulgares ou gastar demasiado. Os alertas também podem notificar outros intervenientes automaticamente sobre anomalias de gastos e riscos de grandes gastos. Vários relatórios suportam alertas com base nos limiares de custo e orçamento. No entanto, os alertas não são atualmente suportados para contas ou subscrições de parceiro CSP.

## <a name="improve-efficiency"></a>Melhorar a eficiência

Pode determinar a utilização da VM ideal e identificar as VMs inativas ou remover as mesmas ou os discos desligados com o Cost Management. Através da utilização de informações nos relatórios de Ineficácia e Otimização de Dimensionamento, pode criar um plano para reduzir ou remover VMs inativas. No entanto, os relatórios de otimização não são atualmente suportados para contas ou subscrições de parceiro CSP.

![recomendações de tamanho](.\media\overview\sizing.png)

Se aprovisionou Instâncias Reservadas do AWS, pode melhorar a sua utilização de instâncias reservadas com relatórios de Otimização, onde pode ver recomendações de compra, modificar reservas e planear o aprovisionamento.

## <a name="next-steps"></a>Passos seguintes

Agora que está familiarizado com o Cost Management, o passo seguinte é registar o seu ambiente da cloud e começar a explorar os seus dados.

- [Registe uma subscrição do Azure individual](quick-register-azure-sub.md)
