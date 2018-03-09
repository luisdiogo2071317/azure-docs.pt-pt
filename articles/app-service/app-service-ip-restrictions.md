---
title: "Restrições de IP do App Service do Azure | Microsoft Docs"
description: "Como utilizar as restrições de IP com o App Service do Azure"
author: btardif
manager: stefsch
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/23/2017
ms.author: byvinyal
ms.openlocfilehash: 72416cfcd05767b223cc92ac28bd0e736516ddf6
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="azure-app-service-static-ip-restrictions"></a>Restrições de IP estático do App Service do Azure #

Restrições de IP permite-lhe definir uma lista de endereços IP que estão autorizados a aceder à sua aplicação. Lista de permissões pode incluir os endereços IP individuais ou um intervalo de endereços IP definidos por uma máscara de sub-rede.

Quando um pedido para a aplicação é gerado a partir de um cliente, o endereço IP é avaliado em comparação com a lista de permissões. Se o endereço IP não estiver na lista, a aplicação responde com uma [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) código de estado.

Restrições de IP definidas em Web. config que consome a sua aplicação no tempo de execução (mais exatamente, as restrições são inseridas num conjunto de endereços IP permitidos no ficheiro applicationHost. config, pelo que o se adicionar também um conjunto de endereços IP permitidos no ficheiro Web. config, irá demorar precedência). Em determinadas circunstâncias, alguns módulo poderá ser executado antes da lógica de restrições de IP no HTTP pipeline. Quando isto acontecer, o pedido falha com um código de erro HTTP diferente.

Restrições de IP são avaliadas nas mesmas instâncias de plano de serviço de aplicações atribuídas à sua aplicação.

Para adicionar uma regra de restrição de IP para a sua aplicação, utilize o menu abrir **rede**>**restrições de IP** e clique em **configurar restrições de IP**

![Restrições de IP](media/app-service-ip-restrictions/ip-restrictions.png)  

Aqui, pode rever a lista de regras de restrição de IP definidas para a sua aplicação.

![restrições de IP da lista](media/app-service-ip-restrictions/browse-ip-restrictions.png)

Pode clicar em **[+] adicionar** para adicionar uma nova regra de restrição de IP.

![Adicionar restrições de IP](media/app-service-ip-restrictions/add-ip-restrictions.png)
