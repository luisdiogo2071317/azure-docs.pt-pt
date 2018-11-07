---
title: Ligar o Gateway de aplicação do Microsoft Azure ao centro de segurança do Azure | Documentos da Microsoft
description: Saiba como integrar o Gateway de aplicação e o Centro de segurança do Azure para melhorar a segurança geral dos seus recursos.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: acb010b0169fd876bf540f7a4115dbabfeda923a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227200"
---
# <a name="connecting-microsoft-azure-application-gateway-to-azure-security-center"></a>Ligar o Gateway de aplicação do Microsoft Azure ao centro de segurança do Azure
Este documento ajuda-o a configurar a integração com o Centro de segurança e de firewall de aplicações do Gateway de aplicação web (WAF).

## <a name="why-connect-application-gateway"></a>Por que motivo ligar o Gateway de aplicação?
O WAF no Gateway de aplicação protege aplicações web contra ataques baseados na web comuns, como injeção SQL, ataques de script entre sites e controlo de sessão sem autorização. Centro de segurança integra-se com o Gateway de aplicação para impedir e detetar ameaças a aplicações web desprotegidas no seu ambiente.

## <a name="how-do-i-configure-this-integration"></a>Como posso configurar esta integração?
Centro de segurança Deteta instâncias de WAF anteriormente implementadas na subscrição. Ligar estas soluções ao centro de segurança para permitir a integração de alertas.

> [!NOTE]
> A WAF do Gateway de aplicação também pode ser aprovisionado do Centro de segurança **recomendações** conforme descrito na [adicionar uma firewall de aplicações web](security-center-add-web-application-firewall.md).
>
>

1. Inicie sessão no [Portal do Azure](https://azure.microsoft.com/features/azure-portal/).

2. No menu **Microsoft Azure**, selecione **Centro de Segurança**.

3. Sob **higiene de troca de segurança de recursos**, selecione **soluções de segurança**.

  ![Descrição geral do Centro de Segurança](./media/security-center-connect-application-gateway/overview.png)

4. Sob **soluções detetadas** localize Microsoft baseados em SaaS Firewall de aplicações Web e selecione **CONNECT**.

  ![Soluções detetadas](./media/security-center-connect-application-gateway/connect.png)

5. **Ligar solução WAF** abre.  Selecione **Connect** para integrar o Centro de segurança e de WAF.

  ![Ligar a solução WAF](./media/security-center-connect-application-gateway/waf-solution.png)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a integrar o WAF do Gateway de aplicação no Centro de segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Integrar soluções de segurança no Centro de segurança](security-center-partner-integration.md)
* [Ligar o Microsoft Advanced Threat Analytics ao centro de segurança](security-center-ata-integration.md)
* [Ligação do Azure Active Directory Identity Protection ao centro de segurança](security-center-aadip-integration.md)
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança](security-center-monitoring.md).
* [Monitor partner solutions with Security Center](security-center-partner-solutions.md) (Monitorizar soluções de parceiros com o Centro de Segurança).
* [Azure Security Center FAQs](security-center-faq.md) (FAQ do Centro de Segurança do Azure).
* [Blogue Azure Security](https://blogs.msdn.com/b/azuresecurity/).
