---
title: Encontrar relatórios de atividade de utilizador do Azure Active Directory no portal do Azure | Documentos da Microsoft
description: Saiba onde os relatórios de atividade de utilizador do Azure Active Directory estão no portal do Azure.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: fab94088d1d54012a955b0663b078d03b13d6299
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624917"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Encontrar relatórios de atividade no portal do Azure

Neste artigo, irá aprender a localizar os relatórios de atividade de utilizador do Azure Active Directory (Azure AD) no portal do Azure.

## <a name="audit-logs-report"></a>Relatório de registos de auditoria

O relatório de registos de auditoria combina vários relatórios em torno de atividades do aplicativo numa única exibição para relatórios com base no contexto. Para aceder ao relatório de registos de auditoria:

1. Navegue para o [portal do Azure](https://portal.azure.com).
2. Selecione o seu diretório no canto superior direito, em seguida, selecione o **do Azure Active Directory** painel a partir do painel de navegação à esquerda.
3. Selecione **registos de auditoria** partir do **atividade** seção do painel Azure Active Directory. 

    ![Registos de auditoria](./media/howto-find-activity-reports/482.png "Registos de auditoria")

O relatório de registos de auditoria consolida os seguintes relatórios:

* Relatório de auditoria
* Atividade de reposição de palavra-passe
* Atividade de registo de reposição de palavra-passe
* Atividade de grupos self-service
* Alterações de nome de grupo do Office 365
* Atividade de aprovisionamento de contas
* Estado de rollover de palavra-passe
* Erros de aprovisionamento de contas

### <a name="filtering-on-audit-logs"></a>Filtragem em registos de auditoria

Pode utilizar a filtragem avançada no relatório de auditoria para aceder a uma determinada categoria de dados de auditoria, especificando-na **categoria de atividade** filtro. Por exemplo, para ver todas as atividades relacionadas com a reposição de palavra-passe self-service, selecione o **gestão de palavras-passe self-service** categoria. 

    ![Category options on the Filter Audit Logs page](./media/howto-find-activity-reports/06.png "Category options on the Filter Audit Logs page")

Categorias de atividade incluem:

- Diretório do Núcleo
- Gestão de Palavra-passe Personalizada
- Gestão de Grupos Personalizada
- Aprovisionamento de Contas


## <a name="sign-ins-report"></a>Relatório de inícios de sessão 

O **inícios de sessão** modo de exibição inclui todos os utilizadores inícios de sessão, bem como os **utilização da aplicação** relatório. Também pode ver informações de utilização da aplicação no **gerir** secção a **aplicações empresariais** descrição geral.

    ![Enterprise applications](./media/howto-find-activity-reports/484.png "Enterprise applications")

Para aceder ao relatório de inícios de sessão:

1. Navegue para o [portal do Azure](https://portal.azure.com).
2. Selecione o seu diretório no canto superior direito, em seguida, selecione o **do Azure Active Directory** painel a partir do painel de navegação à esquerda.
3. Selecione **Signins** partir do **atividade** seção do painel Azure Active Directory. 

    ![Vista de inícios de sessão](./media/howto-find-activity-reports/483.png "ver inícios de sessão")


### <a name="filtering-on-application-name"></a>Filtrar em nome da aplicação

Pode utilizar o relatório de inícios de sessão para ver os detalhes sobre a utilização de aplicações, ao filtrar em nome de utilizador ou o nome da aplicação.

![Página de início de sessão de eventos de filtro](./media/howto-find-activity-reports/07.png "página Eventos de início de sessão de filtragem")

## <a name="security-reports"></a>Relatórios de segurança

### <a name="anomalous-activity-reports"></a>Relatórios de atividade anómala

Relatórios de atividade anómala fornecem informações sobre eventos de risco relacionado com a segurança que o Azure AD pode detetar e comunicar em.

A seguinte tabela apresenta o Azure AD atividade anómala segurança os relatórios e tipos correspondentes de eventos de risco no portal do Azure. Para obter mais informações, consulte [Eventos de risco do Azure Active Directory](concept-risk-events.md).  


| Relatório de atividade anómala do Azure AD |  Tipo de evento de risco de proteção de identidade|
| :--- | :--- |
| Utilizadores com fuga de credenciais | Fuga de credenciais |
| Atividades irregulares de início de sessão | Deslocação impossível para localizações atípicas |
| Inícios de sessão de dispositivos possivelmente infetados | Inícios de sessão de dispositivos infetados|
| Inícios de sessão de fontes desconhecidas | Inícios de sessão de endereços IP anónimos |
| Inícios de sessão de endereços IP com atividade suspeita | Inícios de sessão de endereços IP com atividade suspeita |
| - | Inícios de sessão de localizações desconhecidas |

Relatórios de segurança seguinte para a atividade anómala do Azure AD não são incluídos como eventos de risco no portal do Azure:

* Inícios de sessão após várias falhas
* Inícios de sessão de várias localizações geográficas


### <a name="detected-risk-events"></a>Eventos de risco detetados

Pode aceder a relatórios sobre eventos de risco detetados no **segurança** secção a **Azure Active Directory** painel no [portal do Azure](https://portal.azure.com). Eventos de risco detetados são controlados nos seguintes relatórios:   

- [Utilizadores em risco](concept-user-at-risk.md)
- [Inícios de sessão de risco](concept-risky-sign-ins.md)

    ![Relatórios de segurança](./media/howto-find-activity-reports/04.png "relatórios de segurança")

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral de registos de auditoria](concept-audit-logs.md)
* [Descrição geral de inícios de sessão](concept-sign-ins.md)
* [Visão geral de eventos de risco](concept-risk-events.md)
