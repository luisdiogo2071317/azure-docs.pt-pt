---
title: Ligar o Microsoft Advanced Threat Analytics ao centro de segurança do Azure | Documentos da Microsoft
description: Saiba como o Centro de segurança do Azure se integra com o Microsoft Advanced Threat Analytics.
services: security-center
documentationcenter: na
author: terrylan
manager: MBaldwin
editor: ''
ms.assetid: 5d80bf91-16c3-40b3-82fc-e0805e6708db
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: yurid
ms.openlocfilehash: 6129c83e33d046c2fc9ce1724a27efce5efdfa0e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258804"
---
# <a name="connecting-microsoft-advanced-threat-analytics-to-azure-security-center"></a>Ligar o Microsoft Advanced Threat Analytics ao centro de segurança do Azure
Este documento ajuda-o a configurar a integração entre o Microsoft Advanced Threat Analytics e o Centro de segurança do Azure.

## <a name="why-add-advanced-threat-analytics-data"></a>Por que adicionar dados do Advanced Threat Analytics?
[Advanced Threat Analytics (ATA)](https://docs.microsoft.com/advanced-threat-analytics/what-is-ata) é uma plataforma no local que ajuda a detetar comportamentos de utilizador suspeita. Quando estiver ligado, é possível ver as ações suspeitas detetadas pelo ATA no Centro de segurança. Esta integração permite-lhe ver, correlacionar e investigar todos os alertas de segurança relacionados com as suas cargas de trabalho de cloud híbrida no Centro de segurança. 

## <a name="how-do-i-configure-this-integration"></a>Como posso configurar esta integração?
Supondo que já tenha instalado o ATA, e a funcionar corretamente no local, siga estes passos para configurar esta integração:

1. Inicie sessão para o ATA Center e aceder ao portal do ATA.
2. Clique em **servidor Syslog** no painel esquerdo.

    ![Servidor syslog](./media/security-center-ata-integration/security-center-ata-integration-fig1.png)

3. Na **ponto final do servidor Syslog** campo, escreva 127.0.0.7 (tem de ser este endereço) e escreva 5114 na porta (recomendada). Embora o número da porta seja uma recomendação, qualquer porta exclusiva deverá funcionar. Deixe todas as outras opções como está e clique em **guardar**.
4. Clique em **notificações** no painel esquerdo e ative todas as notificações de Syslog (recomendadas), conforme mostrado na imagem seguinte:

    ![Notificações](./media/security-center-ata-integration/security-center-ata-integration-fig2.png)

5. Clique em **Guardar**.
6. Abra o dashboard do **Centro de Segurança**.
7. No painel esquerdo, clique em **soluções de segurança**.
8. Sob **Advanced Threat Analytics**, clique em **adicionar**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig3.png)
    
9. Ir para a última etapa e clique em **Download agente**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig4.png)

10. Na **adicionar novo computador de não pertencente ao Azure** , selecione a área de trabalho.

    ![Não Pertencente ao Azure](./media/security-center-ata-integration/security-center-ata-integration-fig5.png)

11. Na **agente direto** página, transferir o agente de Windows adequado e anota o **ID de área de trabalho** e **chave primária**.

    ![Agente direto](./media/security-center-ata-integration/security-center-ata-integration-fig6.png)

12. Instale este agente no ATA Center. Durante a instalação, certifique-se de selecionar a opção **ligar o agente ao Azure Log Analytics**e forneça o *ID de área de trabalho*, e *chave primária* quando solicitado.


Depois de concluir a instalação, a integração está concluída e será capaz de ver os novos alertas enviadas do ATA para o Centro de segurança no **pesquisa** resultado. A solução é apresentado na **soluções de segurança** página, em **soluções ligadas**. 

## <a name="next-steps"></a>Passos Seguintes
Neste documento, aprendeu a ligar ao Microsoft ATA ao centro de segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Ligar o Azure Active Directory Identity Protection ao Centro de Segurança do Azure](security-center-aadip-integration.md)
* [Definir políticas de segurança no Centro de segurança do Azure](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições do Azure e grupos de recursos.
* [Gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) – Saiba como o recomendações o ajudam a proteger os seus recursos do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros com o Centro de Segurança do Azure](security-center-partner-solutions.md) – Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
- [Segurança de dados do Centro de segurança do Azure](security-center-data-security.md) -Saiba como os dados são geridos e salvaguardados no Centro de segurança.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de segurança do Azure](https://blogs.msdn.com/b/azuresecurity/) — obtenha os mais recentes notícias de segurança do Azure e as informações.


