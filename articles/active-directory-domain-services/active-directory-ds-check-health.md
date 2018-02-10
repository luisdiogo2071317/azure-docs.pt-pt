---
title: "Serviços de domínio do Azure AD - Verifique o estado de funcionamento do seu domínio gerido | Microsoft Docs"
description: "Verifique o estado de funcionamento do seu domínio gerido utilizando a página de estado de funcionamento no portal do Azure."
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: ergreenl
ms.openlocfilehash: 4adbce0305bdc1a9b261f5cf644fad876d101bc6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---check-the-health-of-your-managed-domain"></a>Serviços de domínio do Azure AD - Verifique o estado de funcionamento do seu domínio gerido

## <a name="your-domains-health"></a>Estado de funcionamento do seu domínio

Utilizando a página de estado de funcionamento no portal do Azure, é capazes de manter atualizado em que está a acontecer no seu domínio gerido. Este artigo passos através de todos os elementos do Estado de funcionamento e da página lhe ensinar como certificar-se de que o domínio é na forma tip-top.

### <a name="view-your-health-page"></a>Ver a página de estado de funcionamento

1. Navegue para a página de serviços de domínio do Azure AD no [portal do Azure](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices)
2. Clique no domínio que pretende ver o estado de funcionamento.
3. No painel de navegação esquerdo, clique em "Estado de funcionamento".

A imagem seguinte é uma página de estado de funcionamento de exemplo.

![Página de estado de funcionamento de exemplo](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Estado de funcionamento do seu domínio é avaliado relativamente a cada hora. Depois de efetuar alterações ao seu domínio gerido, tem de aguardar até o próximo ciclo de avaliação para ver o seu domínio Atualizar estado de funcionamento. Pode verificar quando o domínio foi avaliado pela última vez utilizando o timestamp "Avaliado pela última vez", localizada no canto superior direito.
>

### <a name="status-of-your-managed-domain"></a>Estado do seu domínio gerido

O estado na parte superior direita do Estado de funcionamento da sua página indica o estado de funcionamento global do seu domínio. O estado fatores todos os alertas existentes no seu domínio. Também pode ver o estado do seu domínio na página de descrição geral dos serviços de domínio do Azure AD.

Estados de um domínio gerido:

| Estado | Ícone | Explicação |
| --- | :----: | --- |
| Em Execução | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | O domínio gerido está a funcionar corretamente e não tem quaisquer alertas críticas ou de aviso. Este domínio poderão ter alertas informativos. |
| Necessita de atenção (aviso) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Existem não existem alertas críticos no seu domínio gerido, mas existem um ou mais alertas de aviso que precisam de ser corrigidos. |
| Necessita de atenção (crítico) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Existem um ou mais alertas críticos no seu domínio gerido. Também poderá ter alertas de aviso e/ou informativas. |
| A implementar | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | O domínio está a ser implementado. |

## <a name="monitors"></a>Monitores

Monitores de detalhe determinados aspetos sobre o seu domínio gerido que supervises de serviços de domínio do Azure AD. É a melhor forma de manter os monitores em bom estado de funcionamento para resolver todos os alertas no seu domínio gerido.

Os monitores disponíveis são:
 - Cópia de segurança
 - Sincronização no AAD

### <a name="backup"></a>Cópia de segurança

Isto monitoriza frequência podemos fazer cópias de segurança do seu domínio gerido. Segue-se uma tabela que explica a coluna de detalhes de monitor de cópia de segurança e que valores devem ser esperada.

| Valor de detalhe | Explicação |
| --- | --- |
|"Nunca cópias de segurança" | Este estado é normal para um domínio recentemente criado. A primeira cópia de segurança é criada geralmente após 24 horas. Se o seu domínio gerido não é criado recentemente ou são neste estado durante um período anormal de tempo, [contacte o suporte](active-directory-ds-contact-us.md). |
| Última cópia de segurança foi feita há a 1 para 14 dias | Em geral, este é o valor esperado do monitor de cópia de segurança. |
| Última cópia de segurança foi feita há mais de 14 dias. | Qualquer altura mais de duas semanas é um período de tempo anormalmente longo desde a última cópia de segurança. Primeiro, resolva todos os alertas que são apresentados no seu domínio gerido e, em seguida, se o problema persistir, [contacte o suporte](active-directory-ds-contact-us.md). |


### <a name="synchronization-with-azure-ad"></a>Sincronização com o Azure AD

Microsoft mantém um registo com que frequência o seu domínio gerido está sincronizado com o Azure Active Directory. A quantidade de utilizadores no seu domínio gerido, bem como a quantidade de alterações efetuadas desde a última sincronização pode ambos afetar quanto tempo um período de sincronização pode demorar. Em geral, se tiver sido mais de três dias desde a última sincronização, é recomendado [contacte o suporte](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Alertas

Os alertas são problemas no seu domínio gerido que precisa de ser resolvidos para que os serviços de domínio do Azure AD para serem executados. Cada alerta explica o problema e fornece um URL que descreve os passos específicos para resolver o problema. Para ver todos os alertas e os respetivos resoluções, visite o [resolver problemas com alertas](active-directory-ds-troubleshoot-alerts.md) artigo.

### <a name="severity"></a>Gravidade
Alertas são categorizados em três diferentes níveis de gravidade: crítico, aviso e informativo.

 * **Alertas críticos** problemas que afetam gravemente o seu domínio gerido. Estes alertas devem ser resolvidos imediatamente, como a Microsoft não é possível monitorizar, gerir, patches e sincronizar o seu domínio gerido.
 * **Alertas de aviso** podem ser problemas que podem afetar o seu domínio no futuro, mas são não necessariamente "interrompendo" o serviço. Estes alertas descrevem as melhores práticas e fornecer sugestões para proteger o seu domínio gerido.
 * **Alertas informativos** notificações que são não afetar negativamente o domínio. Alertas informativos foram concebidas para mantê-lo knowledgeable sobre o que acontece no seu domínio e o domínio do Azure AD serviços.

## <a name="next-steps"></a>Passos Seguintes
- [Resolver alertas no seu domínio gerido](active-directory-ds-troubleshoot-alerts.md)
- [Leia mais sobre os serviços de domínio do Azure AD](active-directory-ds-features.md)
- [Contacte-nos](active-directory-ds-contact-us.md)
