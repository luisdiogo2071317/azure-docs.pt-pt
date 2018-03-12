---
title: "Ligar o Gateway de aplicação do Microsoft Azure ao centro de segurança do Azure | Microsoft Docs"
description: "Saiba como integrar o Gateway de aplicação e o Centro de segurança do Azure para melhorar a segurança global dos seus recursos."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/07/2018
ms.author: terrylan
ms.openlocfilehash: 7c15e5a86df7ff2a374aa9b62d2775b1eb035fc6
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="connecting-microsoft-azure-application-gateway-to-azure-security-center"></a>Ligar o Gateway de aplicação do Microsoft Azure ao centro de segurança do Azure
Este documento ajuda-o a configurar a integração com o Centro de segurança e de firewall de aplicações do Gateway de aplicação web (WAF).

## <a name="why-connect-application-gateway"></a>Por que motivo ligar o Gateway de aplicação?
WAF no Gateway de aplicação protege as aplicações web de ataques baseados na web comuns, como a injeção de SQL, ataques de scripts entre sites e hijacks de sessão. Centro de segurança integrado no Gateway de aplicação para impedir e detetar ameaças para aplicações web não protegido no seu ambiente.

## <a name="how-do-i-configure-this-integration"></a>Como configurar a esta integração?
Centro de segurança Deteta instâncias de WAF anteriormente implementadas na subscrição. Ligar-se nestas soluções ao centro de segurança para permitir a integração de alertas.

> [!NOTE]
> O Gateway de aplicação WAF também pode ser aprovisionado do Centro de segurança **recomendações** conforme descrito em [adicionar uma firewall de aplicação web](security-center-add-web-application-firewall.md).
>
>

1. Inicie sessão no [Portal do Azure](https://azure.microsoft.com/features/azure-portal/).

2. No **menu do Microsoft Azure**, selecione **Centro de segurança**. **Centro de Segurança - Descrição Geral** é aberto.

3. Em **descrição geral**, selecione **soluções de segurança**.

  ![Descrição geral do Centro de segurança](./media/security-center-connect-application-gateway/overview.png)

4. Em **detetados soluções** localizar Microsoft Firewall de aplicação Web baseada em SaaS e selecione **CONNECT**.

  ![Soluções detetadas](./media/security-center-connect-application-gateway/connect.png)

5. **Ligar a solução de WAF** abre.  Selecione **Connect** para integrar o WAF e o Centro de segurança.

  ![Ligar a solução WAF](./media/security-center-connect-application-gateway/waf-solution.png)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a integrar o Gateway de aplicação WAF no Centro de segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Integrar soluções de segurança no Centro de segurança](security-center-partner-integration.md)
* [Ligação Microsoft Advanced Threat Analytics ao centro de segurança](security-center-ata-integration.md)
* [Proteção de identidade do Azure Active Directory ao ligar ao centro de segurança](security-center-aadip-integration.md)
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança](security-center-monitoring.md).
* [Monitor partner solutions with Security Center](security-center-partner-solutions.md) (Monitorizar soluções de parceiros com o Centro de Segurança).
* [Azure Security Center FAQs](security-center-faq.md) (FAQ do Centro de Segurança do Azure).
* [Blogue Azure Security](http://blogs.msdn.com/b/azuresecurity/).
