---
title: Publicar oferta de aplicação do Azure - Azure Marketplace | Documentos da Microsoft
description: Descreve o processo e os passos para publicar uma oferta de aplicação do Azure no Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: pbutlerm
ms.openlocfilehash: cfcb33f9586033db9f4197a341227365d43601cc
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232724"
---
# <a name="publish-azure-application-offer"></a>Publicar oferta de aplicação do Azure

Depois de criar uma oferta, fornecendo as informações sobre o **nova oferta** página, pode publicar a oferta. Selecione **publicar** para iniciar o processo de publicação.

O diagrama seguinte mostra os passos principais no processo de publicação de uma oferta para a "ativação online".

![Passos de publicação de oferta](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Descrição detalhada dos passos de publicação

A tabela seguinte apresenta uma lista e descreve cada etapa de publicação e fornece uma estimativa de tempo para concluir cada passo.  Vezes estimativas em "dias" são definidos como dias úteis, que exclui fins de semana e feriados.

|  **Etapa de publicação**           | **tempo**    | **Descrição**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Validar a pré-requisitos         | < 15 min    | Oferecer informações e oferecer definições são validadas.                        |
| Validar as definições de receita influenciada | < 15 min  |      |
| Certificação                  | < 1 dia     | Oferta é analisada pela equipe de certificação do Azure. A oferta é procurada vírus, malware, conformidade de segurança e problemas de segurança. A oferta é verificada para ver de que cumpre todos os critérios de elegibilidade. Para obter mais informações, consulte [pré-requisitos](./cpp-prerequisites.md). Comentários é fornecido se não for encontrado um problema. |
| Validação de unidade de teste          | < a 2 horas   | (Opcional) Se uma versão de teste estiver presente, a Microsoft valida que pode ser implantado e replicada.  |
| Empacotamento e registo de geração de oportunidades potenciais | < 1 hora  | Ativos de técnicos da oferta são empacotados para uso do cliente e os sistemas de oportunidades potenciais são configurados e implementados. |
|  Aprovação do publicador             |  Manual    | Revisão do publicador final e a confirmação antes da oferta entra no ar. A oferta está agora disponível para pré-visualização.  Pode implementar a sua oferta nas subscrições selecionadas (nos passos de informações oferta) para verificar que cumpre todos os seus requisitos.  Depois de verificar a oferta, selecione **Go Live** para que a sua oferta pode mover para o passo seguinte. |
| Revisão da Microsoft                | 7 - 14 dias * | A Microsoft holística analisa a sua aplicação do Azure e envia um e-mail se são descobertos problemas.  * Raramente, este passo pode demorar mais de 14 dias, consoante a complexidade da aplicação e os problemas descobertos.  |
| Em direto                           | < 1 dia | Oferta é lançada, replicada para as regiões especificadas e disponibilizada ao público. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |
 
Pode monitorizar o processo de publicação na **estado** separador para a sua oferta no Portal de parceiros de nuvem.

![Separador de estado para uma oferta de aplicação do Azure](./media/offer-status-tab.png)

Depois de concluir o processo de publicação, a sua oferta será listada na [categoria de aplicação do Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/).


## <a name="errors-and-review-feedback"></a>Erros e os comentários da revisão

Além de exibir o estado de publicação da sua oferta, o **estado** guia também exibe mensagens de erro e comentários a partir do **revisão do Microsoft** passo.  Normalmente, os problemas de revisão são referenciados como pedido pull (PR).  Cada pedido Pull está ligada a um Visual online de Studio Team Services (VSTS, o nome mudado para [do Azure DevOps](https://azure.microsoft.com/services/devops/)) item, que contém detalhes sobre o problema.  A imagem seguinte mostra um exemplo de uma referência de PR de revisão.  Para situações mais complexas, as equipes de suporte e de revisão podem enviar um e-mail. 

![Comentários de revisão de apresentação de separador de status](./media/status-tab-ms-review.png)

Deve resolver cada problema comunicado antes da oferta continua durante o processo de publicação.  O diagrama seguinte ilustra como esse processo de comentários se relaciona com o processo de publicação.

![Publicação passos com comentários do VSTS](./media/pub-flow-vsts-access.png)


### <a name="vsts-access"></a>Acesso do VSTS

Para ver os itens VSTS referenciados em comentários da revisão, os publicadores têm de ser concedidos autorização adequada.  Caso contrário, os novos editores recebem um `401 - Not Authorized` página de resposta.  Para pedir acesso ao sistema VSTS de revisão de oferta, execute os seguintes passos:

1. Recolha as seguintes informações:
    - O nome do publicador e o ID
    - (Aplicação do Azure) do tipo de oferta, nome e ID de SKU de oferta
    - Ligar o pedido pull, por exemplo `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/jfrog/pullrequest/<number>`.  Este URL pode ser obtido a partir da mensagem de notificação ou o endereço da página de 401 resposta.
    - Os endereços de e-mail das pessoas da sua organização de publicação que pretende conceder acesso a.  Estas devem incluir os endereços de proprietário fornecidos ao registrar como um publicador no Portal de parceiros de nuvem.
2. Crie um incidente de suporte.  Na barra de título do Cloud Partner Portal, selecione o **ajudar** botão, em seguida, escolha **suporte** no menu.  Default web browser deve iniciar e navegue para a Microsoft novo incidente página de suporte.  (Poderá ter de entrar primeiro).
3. Especifique a **tipo de problema** como **inclusão de marketplace** e **categoria** como **problemas de acesso ao**, em seguida, selecione **iniciar pedido**.

    ![Categoria de pedido de suporte](./media/support-incident1.png)

4. Na **passo 1 de 2** página, forneça as informações de contacto e selecione **continuar**.
5. Na **passo 2 de 2** página, especifique um título do incidente e fornecer as informações recolhidas no primeiro passo (acima).  Leia e aceite o contrato, em seguida, selecione **submeter**.

Se a criação de incidentes foi concluída com êxito, é apresentada uma página de confirmação.  Guarde as informações de confirmação de referência.  Suporte da Microsoft deve responder ao seu pedido de acesso em poucos dias de negócios.


## <a name="next-steps"></a>Passos Seguintes

Assim que for publicada uma aplicação do Azure, pode [atualizar oferta existente](./cpp-update-existing-offer.md) para refletir a alteração de requisitos comerciais ou técnicos. 
