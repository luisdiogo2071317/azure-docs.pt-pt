---
title: O Azure e do Dynamics 365 consultoria oferta de serviço - definir definições da oferta | Documentos da Microsoft
description: Guia para a definição de definições numa Azure ou do Dynamics 365 consultoria serviço oferta no Portal de parceiros de nuvem da oferta.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: 6ce51dc9bbcdc7b46593a4800c4fce07a42f3171
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346224"
---
# <a name="offer-settings-tab"></a>Separador de definições da oferta

Sobre o **nova oferta** ecrã, a primeira etapa é criar a identidade de oferta. A identidade de oferta consiste em três partes: **ID da oferta**, **ID de publicador**, e **nome**. Cada uma dessas partes é descrita nas seções a seguir.

![Criar uma nova oferta de serviço de consultoria - separador de definições da oferta](media/consultingoffer-settings-tab.png)

*ID de oferta*

Este identificador é um nome exclusivo criado por primeiro, submeta a oferta. Tem de consistir apenas carateres alfanuméricos em minúsculas, traços ou carateres de sublinhado. O **ID da oferta** estarão visíveis no URL e os impactos de resultados do motor de pesquisa. Por exemplo, *yourcompanyname_exampleservice*

Conforme mostrado no exemplo, o **ID da oferta** obtém anexado ao seu ID de publicador para criar um identificador exclusivo. Isso é exposto como uma ligação permanente que pode ser reservada e é indexada por motores de busca.

*Depois de uma oferta em direto, não é possível atualizar seu identificador*

*ID de publicador*

Este identificador está relacionado com a sua conta. Quando estiver conectado com a sua conta organizacional, sua **publicador** ID aparecerá no menu pendente.

*Nome*

Esta cadeia é o que será apresentado como o nome da oferta no AppSource ou no Azure Marketplace. O *nome* campo está limitado a 50 carateres.  O revisor poderá ter de editar o título para permitir a acrescentar o nome da oferta com a duração e o tipo de oferta.

>[!Note]
>Importante: Introduza apenas o nome do serviço real aqui. Não inclua a duração e o tipo de serviço.

O exemplo seguinte pelo Edgewater Fullscope mostra como o nome da oferta é reunido. O nome da oferta é apresentado como:

![Criar uma nova oferta de serviço de consultoria](media/cppsampleconsultingoffer.png)

O nome da oferta é composto por quatro partes:

-   **Duração:** – definidos no **Vitrine detalhes** separador do editor. Duração pode ser expresso em horas, dias ou semanas.
-   **Tipo de serviço:** – definidos no **Vitrine detalhes** separador do editor. Tipos de serviços são `Assessment`, `Briefing`, `Implementation`, `Proof of concept`, e `Workshop`.
-   **Preposition:** - inserida pelo revisor
-   **Nome:** – definidos no **oferecer definições** página.

>[!Note]
>O campo de nome é limitado a 50 carateres. O nome que submeter poderá ter de ser editado pelo revisor para permitir durante o período e tipo a ser acrescentado ao nome da oferta.

A lista seguinte fornece vários nomes de oferta bem com nome:

-   Fundamentos de serviços profissionais: Resumo de 1 hora
-   Migração de plataforma: Resumo de 1 hora
-   PowerApps e Microsoft Flow: Workshop de 1 dia
-   O Azure Machine Learning Services: Prova de conceito de 3 semanas
-   Solução de revenda de módulo e clique em: Resumo de 1 hora
-   Traga os seus dados: Workshop de semanas de 1
-   Análise da cloud: Workshop de 3 dias
-   Treinamento do Power BI: Workshop de 3 dias
-   Solução de gerenciamento de vendas: Implementação de 1 semana
-   Guia de introdução do CRM: Workshop de 1 dia
-   Dynamics 365 para vendas: Avaliação de 2 dias

Depois de concluir o **oferecer definições** guia, pode salvar sua submissão. O nome de oferta agora vai aparecer acima do editor e pode encontrá-lo em todas as ofertas.

**Passos seguintes?**

Agora, pode introduzir [Vitrine detalhes e determinar se pretende publicar no Azure Marketplace ou no AppSource](./cpp-consulting-service-storefront-details.md).