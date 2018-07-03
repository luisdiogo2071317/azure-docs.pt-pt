---
title: Como se preparar para uma alteração de endereço IP de SSL - Azure
description: Se o seu endereço SSL IP vai ser alterado, saiba o que fazer para que a sua aplicação continua a funcionar após a alteração.
services: app-service\web
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.topic: article
ms.date: 06/28/2018
ms.author: tdykstra
ms.openlocfilehash: 47626ae4298327d4166705e8797e525331e8a3d1
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342975"
---
# <a name="how-to-prepare-for-an-ssl-ip-address-change"></a>Como se preparar para uma alteração de endereço IP de SSL

Se tiver recebido uma notificação de que está a alterar o endereço IP de SSL da sua aplicação de serviço de aplicações do Azure, siga as instruções neste artigo para liberar o endereço SSL IP existente e atribuir um novo.

## <a name="release-ssl-ip-addresses-and-assign-new-ones"></a>Libertar endereços IP de SSL e atribuir novas etiquetas

1.  Abra o [Portal do Azure](https://portal.azure.com).

2.  No menu de navegação esquerdo, selecione **dos serviços de aplicações**.

3.  Selecione a sua aplicação de serviço de aplicações na lista.

4.  Sob o **configurações** cabeçalho, clique em **definições de SSL** na navegação à esquerda.

5. Na secção de enlaces de SSL, selecione o registo de nome de anfitrião. No editor de que se abre, selecione **SNI SSL** sobre o **tipo de SSL** menu pendente e clique em **Adicionar enlace**. Quando vir a mensagem de êxito da operação, foi lançado o endereço IP existente.

6.  Na **enlaces SSL** secção, selecione novamente o mesmo registo de nome de anfitrião com o certificado. No editor de que se abre, dessa vez escolha **SSL baseados no IP** sobre o **tipo de SSL** menu pendente e clique em **Adicionar enlace de**. Quando vir a mensagem de êxito da operação, adquirir um novo endereço IP.

7.  Se um registo (registo DNS que aponte diretamente para o seu endereço IP) estiver configurado no seu Portal de registo de domínio (terceiros fornecedor DNS ou o DNS do Azure), substitua o endereço IP existente pelo gerados recentemente. Pode encontrar o novo endereço IP ao seguir as instruções na secção seguinte.

## <a name="find-the-new-ssl-ip-address-in-the-azure-portal"></a>Encontrar o novo endereço IP de SSL no Portal do Azure

1.  Aguarde alguns minutos e, em seguida, abra a [portal do Azure](https://portal.azure.com).

2.  No menu de navegação esquerdo, selecione **dos serviços de aplicações**.

3.  Selecione a sua aplicação de serviço de aplicações na lista.

4.  Sob o **configurações** cabeçalho, clique em **propriedades** na navegação à esquerda e localize a seção rotulada **endereço Virtual IP**.

5. Copie o endereço IP e reconfigurar o seu registo de domínio ou o mecanismo IP.

## <a name="next-steps"></a>Passos Seguintes

Este artigo explicou como se preparar para uma alteração de endereço IP que foi iniciada pelo Azure. Para obter mais informações sobre os endereços IP no serviço de aplicações do Azure, consulte [endereços IP de SSL e de SSL no serviço de aplicações do Azure](app-service-ip-addresses.md).
