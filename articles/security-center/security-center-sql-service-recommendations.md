---
title: Proteger o serviço SQL do Azure e os dados no Centro de segurança do Azure | Documentos da Microsoft
description: Este documento trata recomendações no Centro de segurança do Azure que o ajudam a proteger os seus dados e o serviço SQL do Azure e mantenha-se em conformidade com as políticas de segurança.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 45f5dc840f015793912e314ab3d47e54a409708e
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126671"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Proteger o serviço SQL do Azure e os dados no Centro de segurança do Azure
Centro de segurança do Azure analisa o estado de segurança dos seus recursos do Azure. Quando o Centro de segurança identifica potenciais vulnerabilidades de segurança, cria recomendações que descreve o processo de configuração de controlos necessários.  Recomendações se aplicam a tipos de recursos do Azure: máquinas virtuais (VMs), redes, SQL e dados e aplicações.

Este artigo aborda recomendações que se aplicam ao serviço SQL do Azure e dados. Centro de recomendações relativas à ativação da auditoria para servidores SQL do Azure e bases de dados, ativar a encriptação de bases de dados SQL e a ativação da encriptação da sua conta de armazenamento do Azure.  Utilize a tabela abaixo como referência para ajudar a compreender as recomendações de serviço e de dados SQL disponíveis e o que cada um deles faz se as aplicar.
### <a name="monitor-data-security"></a>Monitorizar segurança de dados

Quando clica em **Segurança de dados** na secção **Prevenção**, é aberto **Recursos de Dados** com recomendações para SQL e Armazenamento. Também tem [recomendações](security-center-sql-service-recommendations.md) para o estado de funcionamento geral da base de dados. Para obter mais informações sobre a encriptação de armazenamento, leia o artigo [Ativar a encriptação para a conta de armazenamento do Azure no Centro de Segurança do Azure](security-center-enable-encryption-for-storage-account.md).

![Recursos de Dados](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

Em **Recomendações SQL**, pode clicar em qualquer recomendação e obter mais detalhes sobre mais ações para resolver um problema. O exemplo seguinte mostra a expansão da recomendação **Auditoria de Base de Dados e Deteção de ameaças em bases de dados SQL**.

![Detalhes sobre uma recomendação de SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**Ativar Auditoria e Deteção de ameaças nas bases de dados SQL** tem as seguintes informações:

* Uma lista de bases de dados SQL
* O servidor no qual estão localizadas
* Informações sobre se esta definição foi herdada a partir do servidor ou se é exclusiva desta base de dados
* O estado atual
* A gravidade do problema

Quando clica na base de dados para abordar esta recomendação, é aberto **Deteção de Ameaças e Auditoria**, conforme mostrado no ecrã seguinte.

![Auditoria e Deteção de Ameaças](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Para ativar a auditoria, selecione **ATIVAR**, na opção **Auditoria**.

## <a name="available-sql-service-and-data-recommendations"></a>Recomendações disponíveis de serviço e os dados SQL
| Recomendação | Descrição |
| --- | --- |
| [Ativar a auditoria e a deteção de ameaças em servidores SQL](security-center-enable-auditing-on-sql-servers.md) |Recomenda-se que ative a deteção de ameaças e auditoria para servidores SQL do Azure (serviço do SQL do Azure apenas; não inclui SQL em execução nas suas máquinas virtuais). |
| [Ativar a auditoria e a deteção de ameaças nas bases de dados SQL](security-center-enable-auditing-on-sql-databases.md) |Recomenda-se que ative a deteção de ameaças e auditoria para bases de dados SQL do Azure (serviço do SQL do Azure apenas; não inclui SQL em execução nas suas máquinas virtuais). |
| [Ativar a encriptação de dados transparente nas bases de dados SQL](security-center-enable-transparent-data-encryption.md) |Recomenda-se de que ativar a encriptação para bases de dados do SQL (apenas para serviço de SQL do Azure). |

## <a name="see-also"></a>Consulte também
Para saber mais sobre as recomendações que se aplicam a outros tipos de recursos do Azure, consulte o seguinte:

* [Proteger as máquinas virtuais no Centro de Segurança do Azure](security-center-virtual-machine-recommendations.md)
* [Proteger as aplicações no Centro de Segurança do Azure](security-center-application-recommendations.md)
* [Proteger a sua rede no Centro de Segurança do Azure](security-center-network-recommendations.md)

Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições e grupos de recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
