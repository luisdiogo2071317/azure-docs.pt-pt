---
title: Gerir ligada a soluções no Centro de segurança do Azure de parceiros | Documentos da Microsoft
description: Este documento vai ajudá-lo a compreender como o Centro de Segurança do Azure lhe permite monitorizar rapidamente o estado de funcionamento das suas soluções de parceiros integradas na sua subscrição do Azure.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/20/2018
ms.author: terrylan
ms.openlocfilehash: 3174f2d6562030702b14ef1fa3708a1a4e8e373b
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301954"
---
# <a name="managing-connected-partner-solutions-with-azure-security-center"></a>Gerir soluções de parceiros ligadas com o Centro de segurança do Azure
Este artigo explica como gerir e monitorizar soluções de segurança ligada no Centro de segurança do Azure.

## <a name="monitoring-partner-solutions"></a>Monitorizar soluções de parceiros
Para monitorizar o estado de funcionamento das soluções de segurança ligadas e realizar o gerenciamento básico:

1. Sob **Centro de segurança - descrição geral**, selecione **soluções de segurança**.

  ![Selecione soluções de segurança][1]

  O **soluções ligadas** secção inclui soluções de segurança que estão ligadas ao centro de segurança e informações sobre o estado de funcionamento de cada solução.

  ![Soluções de parceiros][2]

   O estado de uma solução de parceiro pode ser:

   * Bom estado de funcionamento (verde) – não há nenhum problema de estado de funcionamento.
   * Mau estado de funcionamento (vermelho) – existe um problema de estado de funcionamento que exige atenção imediata.
   * Problemas de estado de funcionamento (cor de laranja), a solução deixou seu estado de funcionamento de relatórios.
   * Não reportada (cinzento) – a solução não comunicou qualquer coisa, mas o estado de uma solução pode ser não reportado se ele recentemente foi ligado e ainda está a ser implementado, ou não existem dados de estado de funcionamento estão disponíveis.

   > [!NOTE]
   > Se os dados de estado de funcionamento não estiverem disponíveis, o Centro de segurança mostra a data e hora do último evento recebido para indicar se a solução está a comunicar ou não. Se não existem dados de estado de funcionamento estão disponíveis e não existem alertas são recebidas dentro dos últimos 14 dias, o Centro de segurança indica que a solução está danificado ou não de relatórios.
   >
   >

2. Selecione **vista** para obter informações adicionais e opções, que inclui:

  - **Consola de soluções**. Abre a experiência de gestão para esta solução.
  - **Associar VM**. Abre o painel de ligar aplicações. Aqui pode ligar recursos à solução de parceiros.
  - **Eliminar solução**.
  - **Configurar**.

   ![Detalhe de solução de parceiros][3]

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a gerir e monitorizar soluções de segurança ligada no Centro de segurança. Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Descrição geral das soluções de segurança](security-center-partner-integration.md) – Saiba como ligar e gerir soluções de segurança.
* [Ligar o Microsoft Advanced Threat Analytics (ATA)](security-center-ata-integration.md) – Saiba como ligar os alertas do ATA.
* [Ligação do Azure Active Directory (AD) Identity Protection ](security-center-aadip-integration.md) – Saiba como ligar os alertas do Azure AD Identity Protection.
* [Integração de parceiros e soluções](security-center-partner-integration.md) -obtenha uma visão geral da integração de outras soluções de segurança.
* [Gerir e responder a alertas de segurança](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/) – Encontre mensagens do blogue acerca da segurança e conformidade do Azure.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/partner-solutions.png
[3]: ./media/security-center-partner-solutions/partner-solutions-detail.png
