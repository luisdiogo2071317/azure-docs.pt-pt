---
title: Como se preparar para uma alteração de endereço IP entrada - Azure
description: Se o endereço IP de entrada vai ser alterado, saiba o que fazer para que a sua aplicação continua a funcionar após a alteração.
services: app-service\web
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: tdykstra
ms.openlocfilehash: 31c0d8c43a1a7614ff1965853b8ce0a3997db6f4
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342963"
---
# <a name="how-to-prepare-for-an-inbound-ip-address-change"></a>Como se preparar para uma alteração de endereço IP entrada

Se tiver recebido uma notificação de que está a alterar o endereço IP de entrada da sua aplicação de serviço de aplicações do Azure, siga as instruções neste artigo.

## <a name="determine-if-you-have-to-do-anything"></a>Determinar se tem de fazer nada

* Opção 1: Se a sua aplicação de serviço de aplicações não tiver um domínio personalizado, é necessária nenhuma ação.

* Opção 2: se apenas um registo CNAME (registo DNS que aponte para um URI) é configurado no seu Portal de registo de domínio (terceiros fornecedor DNS ou o DNS do Azure), é necessária nenhuma ação.

* Opção 3: Se um registo (registo DNS que aponte diretamente para o seu endereço IP) estiver configurado no seu Portal de registo de domínio (terceiros fornecedor DNS ou o DNS do Azure), substitua o endereço IP existente pelo novo. Pode encontrar o novo endereço IP ao seguir as instruções na secção seguinte.

* Opção 4: Se o seu aplicativo estiver atrás de um balanceador de carga, filtro de IP ou qualquer outro mecanismo IP que requer o endereço IP da sua aplicação, substitua o endereço IP existente pelo novo. Pode encontrar o novo endereço IP ao seguir as instruções na secção seguinte.

## <a name="find-the-new-inbound-ip-address-in-the-azure-portal"></a>Encontrar o endereço de IP de entrada novo no portal do Azure

O endereço IP de entrada novo que está a ser dado à sua aplicação estiver no portal no **endereço Virtual IP** campo. Este endereço IP novo e antigo estão ligados à sua aplicação agora e, mais tarde antigo será desligado.

1.  Abra o [Portal do Azure](https://portal.azure.com).

2.  No menu de navegação esquerdo, selecione **dos serviços de aplicações**.

3.  Selecione a sua aplicação de serviço de aplicações na lista.

4.  Sob o **configurações** cabeçalho, clique em **propriedades** na navegação à esquerda e localize a seção rotulada **endereço Virtual IP**.

5. Copie o endereço IP e reconfigurar o seu registo de domínio ou o mecanismo IP.

## <a name="next-steps"></a>Passos Seguintes

Este artigo explicou como se preparar para uma alteração de endereço IP que foi iniciada pelo Azure. Para obter mais informações sobre os endereços IP no serviço de aplicações do Azure, consulte [de entrada e endereços IP de saída no serviço de aplicações do Azure](app-service-ip-addresses.md).
