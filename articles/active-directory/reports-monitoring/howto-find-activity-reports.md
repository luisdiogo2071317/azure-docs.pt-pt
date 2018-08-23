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
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 182537d6f07b624f2395f591681ed4596579bde0
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42058449"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Encontrar relatórios de atividade no portal do Azure

Neste artigo, descrevemos como encontrar relatórios de atividade de utilizador do Azure Active Directory no portal do Azure.

## <a name="activity-and-integrated-app-reports"></a>Atividade e os relatórios de aplicação integrada

Para relatórios com o botão com base no contexto no portal do Azure, os relatórios existentes são mesclados numa única exibição. Uma única subjacentes API fornecem os dados para o modo de exibição.

Para ver esta vista, o **do Azure Active Directory** painel, em **ATIVIDADE**, selecione **registos de auditoria**.

![Registos de auditoria](./media/howto-find-activity-reports/482.png "Registos de auditoria")

Os seguintes relatórios são consolidados nesta vista:

* Relatório de auditoria
* Atividade de reposição de palavra-passe
* Atividade de registo de reposição de palavra-passe
* Atividade de grupos self-service
* Alterações de nome de grupo do Office 365
* Atividade de aprovisionamento de contas
* Estado de rollover de palavra-passe
* Erros de aprovisionamento de contas


O relatório de utilização da aplicação foi aprimorado e está incluído nos **inícios de sessão** vista. Para ver esta vista, o **do Azure Active Directory** painel, em **ATIVIDADE**, selecione **inícios de sessão**.

![Vista de inícios de sessão](./media/howto-find-activity-reports/483.png "ver inícios de sessão")

O **inícios de sessão** vista inclui todos os utilizadores inícios de sessão. Pode usar essas informações para obter informações de utilização da aplicação. Também pode ver informações de utilização da aplicação no **aplicações empresariais** descrição geral, na **GERIR** secção.

![Aplicações empresariais](./media/howto-find-activity-reports/484.png "aplicações empresariais")

## <a name="access-a-specific-report"></a>Aceder a um relatório específico

Embora o portal do Azure oferece uma única exibição, também pode ver relatórios específicos.

### <a name="audit-logs"></a>Registos de auditoria

Em resposta a comentários de clientes, no portal do Azure, pode utilizar a filtragem avançada para acessar os dados que pretende. É um filtro que pode utilizar um *categoria de atividade*, que apresenta uma lista os diferentes tipos de atividade de registos no Azure AD. Para restringir os resultados para o que está procurando, pode selecionar uma categoria.

Por exemplo, se estiver interessado apenas em atividades relacionadas ao reposições de palavras-passe self-service, pode escolher o **gestão de palavras-passe Self-Service** categoria. As categorias que vir baseiam-se no recurso que está a trabalhar.  

![Opções de categorias na página de registos de auditoria de filtro](./media/howto-find-activity-reports/06.png "opções de categorias na página de registos de auditoria de filtro")

Categorias de atividade incluem:

- Diretório do Núcleo
- Gestão de Palavra-passe Personalizada
- Gestão de Grupos Personalizada
- Aprovisionamento de Contas

### <a name="application-usage"></a>Utilização da aplicação

Para ver os detalhes sobre a utilização da aplicação para todas as aplicações ou para uma única aplicação, em **ATIVIDADE**, selecione **inícios de sessão**. Para limitar os resultados, pode filtrar no nome de utilizador ou o nome da aplicação.

![Página de início de sessão de eventos de filtro](./media/howto-find-activity-reports/07.png "página Eventos de início de sessão de filtragem")

### <a name="security-reports"></a>Relatórios de segurança

#### <a name="azure-ad-anomalous-activity-reports"></a>Relatórios de atividade anómala de AD do Azure

São consolidados para lhe fornecer uma visão central de relatórios de segurança de atividade anómala do Azure AD. Esta vista mostra todos os eventos de risco relacionado com a segurança que o Azure AD pode detetar e comunicar em.

A seguinte tabela apresenta o Azure AD atividade anómala segurança os relatórios e tipos correspondentes de eventos de risco no portal do Azure.

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

Para obter mais informações, consulte [Eventos de risco do Azure Active Directory](concept-risk-events.md).  


#### <a name="detected-risk-events"></a>Eventos de risco detetados

No portal do Azure, pode aceder aos relatórios sobre eventos de risco detetados no **do Azure Active Directory** painel, em **segurança**. Eventos de risco detetados são controlados nos seguintes relatórios:   

- Utilizadores em risco
- Inícios de Sessão de Risco

![Relatórios de segurança](./media/howto-find-activity-reports/04.png "relatórios de segurança")

Para obter mais informações sobre os relatórios de segurança, consulte:

- [Utilizadores no relatório de segurança de risco no portal do Azure Active Directory](concept-user-at-risk.md)
- [Relatório de inícios de sessão de risco no portal do Azure Active Directory](concept-risky-sign-ins.md)


Para ver o **utilização da aplicação** no relatório, o **Azure Active Directory** painel, em **GERIR**, selecione **aplicações empresariais**, e, em seguida, selecione **inícios de sessão**.


![Relatório de inícios de sessão do aplicativos empresariais](./media/howto-find-activity-reports/199.png)

## <a name="next-steps"></a>Passos Seguintes

Para obter uma descrição geral dos relatórios, veja [Relatórios do Azure Active Directory](overview-reports.md).
