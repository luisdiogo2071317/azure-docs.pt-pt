---
title: Integrar soluções de segurança no Centro de Segurança do Azure | Microsoft Docs
description: Saiba como é que o Centro de Segurança do Azure se integra com parceiros, para melhorar a segurança geral dos seus recursos do Azure.
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
ms.date: 11/26/2018
ms.author: rkarlin
ms.openlocfilehash: bbdda5012e6132940d00ae23a6d26469b0216fd0
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335446"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integrar soluções de segurança no Centro de Segurança do Azure
Este documento ajuda-o a gerir soluções de segurança já ligadas ao Centro de Segurança do Azure e adicionar novas.

## <a name="integrated-azure-security-solutions"></a>Soluções de segurança do Azure integradas
O Centro de Segurança facilita a ativação de soluções de segurança integradas no Azure. As vantagens incluem:

- **Implementação simplificada**: o Centro de Segurança oferece aprovisionamento integrado de soluções de parceiros integradas. Para soluções como antimalware e avaliação de vulnerabilidades, o Centro de Segurança pode aprovisionar o agente necessário nas suas máquinas virtuais e para aplicações de firewall, o Centro de Segurança pode assegurar grande parte da configuração de rede necessária.
- **Deteções integradas**: os eventos de segurança das soluções de parceiros são automaticamente recolhidos, agregados e apresentados como parte dos alertas e incidentes do Centro de Segurança. Estes eventos também são combinados com deteções de outras origens, para disponibilizarem capacidades avançadas de deteção de ameaças.
- **Gestão e monitorização do estado de funcionamento unificadas**: os clientes podem utilizar eventos de estado de funcionamento integrados para monitorizar todas as soluções de parceiros rapidamente. Está disponível uma gestão básica, com acesso fácil a configuração avançada mediante a utilização da solução do parceiro.

Atualmente, as soluções de segurança integradas incluem:

- Proteção de ponto final ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html),[Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://www.microsoft.com/windows/comprehensive-security) e [System Center Endpoint Protection](https://docs.microsoft.com/sccm/protect/deploy-use/endpoint-protection))
- Firewall de aplicações Web ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/products.html) e [Gateway de Aplicação do Azure](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/))
- Firewalls de próxima geração ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2), [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html) e [Palo Alto Networks](https://www.paloaltonetworks.com/products))
- Avaliação de vulnerabilidades ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/) e [Rapid7](https://www.rapid7.com/products/insightvm/))

> [!NOTE]
> O Centro de Segurança não instala o Microsoft Monitoring Agent em aplicações virtuais de parceiro, porque a maioria dos fornecedores de segurança proíbe agentes externos em execução nas suas aplicações.
>
>


| Endpoint Protection               | Plataformas                             | Instalação do Centro de Segurança | Deteção do Centro de Segurança |
|-----------------------------------|---------------------------------------|------------------------------|---------------------------|
| Windows Defender (Microsoft Antimalware)                  | Windows Server 2016                   | Não, Incorporado no SO           | Sim                       |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (ver nota abaixo) | Através de Extensão                | Sim                       |
| Trend Micro – Todas as versões         | Família Windows Server                 | Não                           | Sim                       |
| Symantec v12.1.1100+              | Família Windows Server                 | Não                           | Sim                       |
| McAfee v10+                       | Família Windows Server                 | Não                           | Sim                       |
| Kaspersky                         | Família Windows Server                 | Não                           | Não                        |
| Sophos                            | Família Windows Server                 | Não                           | Não                        |

> [!NOTE]
> A deteção do System Center Endpoint Protection (SCEP) numa máquina virtual do Windows Server 2008 R2 requer SCEP ser instaladas após PowerShell 3.0 (ou uma versão superior).
>
>

## <a name="how-security-solutions-are-integrated"></a>Como são integradas as soluções de segurança
As soluções de segurança do Azure implementadas a partir do Centro de Segurança são ligadas automaticamente. Também pode ligar outras origens de dados de segurança, incluindo:

- Azure AD Identity Protection
- Computadores em execução no local ou noutras clouds
- Solução de segurança que suporte o Common Event Format (CEF)
- Microsoft Advanced Threat Analytics

![Integração de soluções de parceiros](./media/security-center-partner-integration/security-center-partner-integration-fig8.png)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Gerir soluções de segurança do Azure integradas e outras origens de dados

1. Inicie sessão no [portal do Azure](https://azure.microsoft.com/features/azure-portal/).

2. No menu **Microsoft Azure**, selecione **Centro de Segurança**. **Centro de Segurança - Descrição Geral** é aberto.

3. No menu Centro de Segurança, selecione **Soluções de segurança**.

  ![Descrição geral do Centro de Segurança](./media/security-center-partner-integration/overview.png)

Em **Soluções de segurança**, pode ver informações sobre o estado de funcionamento da solução de segurança do Azure integrada e efetuar tarefas de gestão básicas. Também pode ligar outros tipos de origens de dados de segurança, tais como alertas do Azure Active Directory Identity Protection e registos de firewall no Common Event Format (CEF).

### <a name="connected-solutions"></a>Soluções ligadas

A secção **Soluções ligadas** inclui soluções de segurança que estão ligadas atualmente ao Centro de Segurança e informações sobre o estado de funcionamento de cada solução.  

![Soluções ligadas](./media/security-center-partner-integration/security-center-partner-integration-fig4.png)

Veja [Gerir soluções de parceiros ligados](security-center-partner-solutions.md) para obter mais informações.

### <a name="discovered-solutions"></a>Soluções detetadas

O Centro de Segurança deteta automaticamente as soluções de segurança em execução no Azure, mas não ligadas ao Centro de Segurança, e apresenta as soluções na secção **Soluções detetadas**. Isto inclui soluções do Azure, como o [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection), bem como soluções de parceiros.

> [!NOTE]
> É necessário o escalão Standard do Centro de Segurança ao nível da subscrição para a funcionalidade de soluções detetadas. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preço de Segurança.
>
>

Selecione **LIGAR** numa solução para se integrar com o Centro de Segurança e ser notificado sobre alertas de segurança.

![Soluções detetadas](./media/security-center-partner-integration/security-center-partner-integration-fig5.png)

O Centro de Segurança também deteta as soluções implementadas na subscrição que podem reencaminhar registos do Formato de Evento Comum (CEF). Saiba como [ligar uma solução de segurança](quick-security-solutions.md) que utilize registos CEF ao Centro de Segurança.

### <a name="add-data-sources"></a>Adicionar origens de dados

A secção **Adicionar origens de dados** inclui outras origens de dados disponíveis que podem ser ligadas. Para obter instruções sobre como adicionar dados a partir de qualquer uma destas origens, clique em **ADICIONAR**.

![Origens de dados](./media/security-center-partner-integration/security-center-partner-integration-fig7.png)


## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a integrar soluções de parceiros no Centro de Segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Ligar o Microsoft Advanced Threat Analytics ao Centro de Segurança do Azure](security-center-ata-integration.md)
* [Ligar o Azure Active Directory Identity Protection ao Centro de Segurança do Azure](security-center-aadip-integration.md)
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança](security-center-monitoring.md). Saiba como monitorizar o estado de funcionamento dos recursos do Azure.
* [Monitor partner solutions with Security Center](security-center-partner-solutions.md) (Monitorizar soluções de parceiros com o Centro de Segurança). Saiba como monitorizar o estado de funcionamento das soluções dos seus parceiros.
* [Azure Security Center FAQs](security-center-faq.md) (FAQ do Centro de Segurança do Azure). Encontre respostas para as perguntas mais frequentes sobre o Centro de Segurança.
* [Blogue Azure Security](https://blogs.msdn.com/b/azuresecurity/). Encontre mensagens do blogue acerca da segurança e conformidade do Azure.
