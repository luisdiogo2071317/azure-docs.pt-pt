---
title: Serviços de domínio do Azure AD - Verifique o estado de funcionamento do seu domínio gerido | Microsoft Docs
description: Verifique o estado de funcionamento do seu domínio gerido utilizando a página de estado de funcionamento no portal do Azure.
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2018
ms.author: ergreenl
ms.openlocfilehash: ffb00ce85a06812dfc42dfbbba74f712c1877615
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212394"
---
# <a name="check-the-health-of-an-azure-ad-domain-services-managed-domain"></a>Verifique o estado de funcionamento de um domínio gerido dos serviços de domínio do Azure AD

## <a name="overview-of-the-health-page"></a>Descrição geral da página de estado de funcionamento
Utilizando a página de estado de funcionamento no portal do Azure, é capazes de manter atualizado em que está a acontecer no seu domínio gerido. Este artigo explica os elementos da página de estado de funcionamento.

### <a name="how-to-view-the-health-of-your-managed-domain"></a>Como ver o estado de funcionamento do seu domínio gerido
1. Navegue para o [página dos serviços de domínio do Azure AD](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) no portal do Azure.
2. Clique no domínio que pretende ver o estado de funcionamento.
3. No painel de navegação esquerdo, clique em **estado de funcionamento**.

A imagem seguinte ilustra uma página de estado de funcionamento de exemplo: ![página de estado de funcionamento de exemplo](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Estado de funcionamento do seu domínio gerido é avaliado a cada hora. Depois de efetuar alterações ao seu domínio gerido, aguarde até que o próximo ciclo de avaliação para ver o estado de funcionamento atualizada do seu domínio gerido. Timestamp "Avaliado pela última vez" no canto superior direito mostra quando o estado de funcionamento do seu domínio gerido foi avaliado pela última vez.
>

### <a name="status-of-your-managed-domain"></a>Estado do seu domínio gerido
O estado na parte superior direita do Estado de funcionamento da sua página indica o estado de funcionamento global do seu domínio gerido. O estado fatores todos os alertas existentes no seu domínio. Também pode ver o estado do seu domínio na página de descrição geral dos serviços de domínio do Azure AD.

| Estado | Ícone | Explicação |
| --- | :----: | --- |
| A executar | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | O domínio gerido está a funcionar corretamente e não tem quaisquer alertas críticas ou de aviso. Este domínio poderão ter alertas informativos. |
| Necessita de atenção (aviso) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Existem não existem alertas críticos no seu domínio gerido, mas existem um ou mais alertas de aviso que precisam de ser corrigidos. |
| Necessita de atenção (crítico) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Existem um ou mais alertas críticos no seu domínio gerido. Também poderá ter alertas de aviso e/ou informativas. |
| A implementar | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | O domínio está a ser implementado. |

## <a name="monitors"></a>Monitores
Monitores são aspetos do seu domínio gerido que serviços de domínio do Azure AD monitoriza regularmente. É a melhor forma de manter os monitores em bom estado de funcionamento para resolver quaisquer alertas ativos para o seu domínio gerido.

Serviços de domínio do Azure AD monitoriza o seguinte:
 - Cópia de segurança
 - Sincronização com o Azure AD

### <a name="the-backup-monitor"></a>O monitor 'cópia de segurança'
Isto monitoriza se estão a ser efetuadas cópias de segurança regulares do seu domínio gerido. A tabela seguinte explica o que pode esperar na coluna detalhes de monitor de cópia de segurança:

| Valor de detalhe | Explicação |
| --- | --- |
|"Nunca cópias de segurança" | Este estado é normal para um domínio gerido criado recentemente. Geralmente, a primeira cópia de segurança é criada a 24 horas depois do seu domínio gerido está aprovisionado. Se o seu domínio gerido não é criado recentemente ou consulte neste estado durante um período anormal de tempo, [contacte o suporte](active-directory-ds-contact-us.md). |
| Última cópia de segurança foi feita há a 1 para 14 dias | Em geral, este valor é esperado para o monitor de cópia de segurança. |
| Última cópia de segurança foi feita há mais de 14 dias. | Qualquer altura mais de duas semanas é um período de tempo anormalmente longo desde a última cópia de segurança. Alertas críticos ativos podem impedir que os seu domínio gerido a cópia de segurança regulares. Primeiro, resolva qualquer ativo alertas para o seu domínio gerido e, em seguida, se ainda o problema persistir, [contacte o suporte](active-directory-ds-contact-us.md). |


### <a name="the-synchronization-with-azure-ad-monitor"></a>O monitor de 'sincronização com o Azure AD'
Microsoft monitoriza com que frequência o seu domínio gerido está sincronizado com o Azure Active Directory. O número de objetos (utilizadores e grupos) e o número de alterações no diretório do Azure AD, uma vez que a última sincronização pode ambos afetar quanto tempo um período de sincronização pode demorar. Se o seu domínio gerido última sincronização há, mais de três dias [contacte o suporte](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Alertas
São gerados alertas para problemas no seu domínio gerido que precisa de ser resolvidos para que os serviços de domínio do Azure AD para serem executados. Cada alerta explica o problema e fornece um URL de resolução que descreve os passos específicos para resolver o problema. Para ver todos os alertas e os respetivos resoluções, visite o [resolver problemas com alertas](active-directory-ds-troubleshoot-alerts.md) artigo.

### <a name="alert-severity"></a>Gravidade do alerta
Alertas são categorizados em três diferentes níveis de gravidade: crítico, aviso e informativo.

 * **Alertas críticos** problemas que afetam gravemente o seu domínio gerido. Estes alertas devem ser resolvidos imediatamente, como a Microsoft não é possível monitorizar, gerir, patches e sincronizar o seu domínio gerido. 
 * **Alertas de aviso** notificá-lo de problemas que possam afetar o seu domínio gerido no futuro. Estes alertas oferecem recomendações para proteger o seu domínio gerido.
 * **Alertas informativos** notificações que são não afetar negativamente o domínio. Alertas informativos foram concebidas para mantê-lo knowledgeable sobre o que acontece no seu domínio e o domínio do Azure AD serviços.

## <a name="next-steps"></a>Passos Seguintes
- [Resolver alertas no seu domínio gerido](active-directory-ds-troubleshoot-alerts.md)
- [Leia mais sobre os serviços de domínio do Azure AD](active-directory-ds-overview.md)
- [Contacte a equipa de produto](active-directory-ds-contact-us.md)
