---
title: "Smart deteção - pacote de deteção de segurança com o Azure Application Insights | Microsoft Docs"
description: "Monitorizar a aplicação com o Azure Application Insights para potenciais problemas de segurança."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: 837525d70f28c0710b8a8b86d411a7854402207f
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="application-security-detection-pack-preview"></a>Pacote de deteção de segurança de aplicações (pré-visualização)

Application Insights automaticamente analisa a telemetria gerada pela sua aplicação e Deteta potenciais problemas de segurança. Esta capacidade permite-lhe identificar potenciais problemas de segurança e lidar com os mesmos corrigindo a aplicação ou efetuando as medidas de segurança necessário.

Esta funcionalidade não necessita de nenhuma configuração especial, diferente de [configurar a sua aplicação para enviar telemetria](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Quando receber este tipo de notificação de deteção inteligente?
Existem três tipos de problemas de segurança que são detetados:
1. Acesso de URL inseguras: um URL na aplicação estiver a ser acedido através de HTTP e HTTPS. Normalmente, um URL que aceite pedidos HTTPS não deve aceitar pedidos de HTTP. Isto pode indicar um problema de segurança ou de erros na sua aplicação.
2. Formulário inseguras: um formulário (ou outro pedido de "POST") na aplicação que utiliza HTTP em vez de HTTPS. Utilizando o HTTP pode comprometer os dados de utilizador que são enviados pelo formulário.
3. Atividade suspeita de utilizador: a aplicação está a ser acedida a partir de vários países pelo mesmo utilizador aproximadamente à mesma hora. Por exemplo, o mesmo utilizador acedeu a aplicação de Espanha e dos Estados Unidos dentro da mesma hora. Esta deteção indica uma tentativa de acesso potencialmente malicioso à sua aplicação.

## <a name="does-my-app-definitely-have-a-security-issue"></a>A minha aplicação sem dúvida tem um problema de segurança?
Não, uma notificação não significa que a aplicação sem dúvida tiver um problema de segurança. Em muitos casos, uma deteção de qualquer um dos cenários acima pode indicar um problema de segurança. No entanto, a deteção poderá ter uma justificação de negócio natural e pode ser ignorada.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>Como posso corrigir a deteção de "Acesso URL inseguras"?
1. **Triagem.** A notificação fornece o número de utilizadores que são acedidos URLs inseguras e o URL que estava a maioria das afetados pelo inseguras acesso. Isto pode ajudar a atribuir uma prioridade para o problema.
2. **Âmbito.** Os utilizadores que percentagem acedidos URLs inseguras? URLs quantas foram afetados? Estas informações podem ser obtidas a partir da notificação.
3. **Diagnosticar.** A deteção fornece a lista de pedidos inseguras e as listas de URLs e os utilizadores que foram afetados, para o ajudar a diagnosticar melhor o problema.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Como posso corrigir a deteção de "Formulário inseguras"?
1. **Triagem.** A notificação fornece o número de formulários inseguras e número de utilizadores cujos dados potencialmente ficou comprometidos. Isto pode ajudar a atribuir uma prioridade para o problema.
2. **Âmbito.** A forma foi envolvida na maior número de transmissões inseguras e o que é a distribuição de transmissões inseguras ao longo do tempo? Estas informações podem ser obtidas a partir da notificação.
3. **Diagnosticar.** A deteção fornece a lista de formulários inseguras e uma divisão do número de inseguras transmissões para cada formulário, para o ajudar a diagnosticar melhor o problema.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>Como posso corrigir a deteção de "atividade do utilizador suspeita"?
1. **Triagem.** A notificação fornece o número de utilizadores diferentes que baixos apresentados o comportamento suspeito. Isto pode ajudar a atribuir uma prioridade para o problema.
2. **Âmbito.** De que países teve os pedidos suspeitos origem? O utilizador foi mais suspeita? Estas informações podem ser obtidas a partir da notificação.
3. **Diagnosticar.** A deteção fornece a lista de utilizadores suspeitos e a lista de países para cada utilizador, para o ajudar a diagnosticar melhor o problema.
