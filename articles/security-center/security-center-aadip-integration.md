---
title: Ligação do Azure Active Directory Identity Protection ao centro de segurança do Azure | Documentos da Microsoft
description: Saiba como o Centro de segurança do Azure se integra com o Azure Active Directory Identity Protection.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 0d4b77c2-dba4-4e46-8f55-ab04ddd92496
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2018
ms.author: rkarlin
ms.openlocfilehash: 8c0e90436cf030f1280b3c50f3fdc91c7429d542
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340305"
---
# <a name="connecting-azure-active-directory-identity-protection-to-azure-security-center"></a>Ligação do Azure Active Directory Identity Protection ao centro de segurança do Azure
Este documento ajuda-o a configurar a integração entre o Azure Active Directory (AD) Identity Protection e o Centro de segurança do Azure.

## <a name="why-connect-azure-ad-identity-protection"></a>Por que motivo ligar o Azure AD Identity Protection?
[O Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) ajuda a detetar potenciais vulnerabilidades que afetam as identidades da sua organização. Quando estiver ligado, é possível ver alertas do Azure AD Identity Protection no Centro de segurança. Esta integração permite-lhe ver, correlacionar e investigar todos os alertas de segurança relacionados com as suas cargas de trabalho de cloud híbrida no Centro de segurança.

## <a name="how-do-i-configure-this-integration"></a>Como posso configurar esta integração?
Se sua organização já estiver a utilizar o Azure AD Identity Protection, siga os passos abaixo para configurar a integração:

1. Abra o dashboard do **Centro de Segurança**.
2. No painel esquerdo, clique em **soluções de segurança**. Centro de segurança Deteta automaticamente se o Azure AD Identity Protection está ativado para a sua organização.

    ![AADIP](./media/security-center-aadip-integration/security-center-aadip-integration-fig1.png)

3. Clique em **CONNECT**.
4. Na **integrar o Azure AD Identity Protection** página, desloque para baixo e selecione a área de trabalho apropriada:

    ![área de trabalho](./media/security-center-aadip-integration/security-center-aadip-integration-fig2.png)

5. Clique em **Ligar**.

Depois de concluir esta configuração, a solução do Azure AD Identity Protection é apresentado na **soluções de segurança** página, em **soluções ligadas**.

## <a name="next-steps"></a>Passos Seguintes
Neste documento, aprendeu a ligar o Azure AD Identity Protection ao centro de segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Ligar o Microsoft Advanced Threat Analytics ao Centro de Segurança do Azure](security-center-ata-integration.md)
* [Definir políticas de segurança no Centro de segurança do Azure](tutorial-security-policy.md) – Saiba como configurar políticas de segurança para as suas subscrições do Azure e grupos de recursos.
* [Gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) – Saiba como o recomendações o ajudam a proteger os seus recursos do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros com o Centro de Segurança do Azure](security-center-partner-solutions.md) – Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
- [Segurança de dados do Centro de segurança do Azure](security-center-data-security.md) -Saiba como os dados são geridos e salvaguardados no Centro de segurança.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de segurança do Azure](https://blogs.msdn.com/b/azuresecurity/) — obtenha os mais recentes notícias de segurança do Azure e as informações.
