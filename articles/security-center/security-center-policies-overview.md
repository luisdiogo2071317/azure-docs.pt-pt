---
title: Definições de política de segurança do Centro de segurança do Azure | Documentos da Microsoft
description: Configure definições de política de segurança do Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: ab8a289ea0de263871b76788514052c09a6bf4da
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44295742"
---
# <a name="security-policy-settings"></a>Definições da política de segurança
Este artigo fornece uma descrição geral da segurança, definições de política no Centro de segurança.

## <a name="what-are-security-policies"></a>O que são políticas de segurança?
As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares. No Centro de segurança do Azure, pode definir políticas para as suas subscrições do Azure e adaptá-los para o tipo de carga de trabalho ou à sensibilidade dos seus dados. Por exemplo, as aplicações que utilizam dados regulados como, por exemplo, informações de identificação pessoal, podem exigir um nível mais elevado de segurança que outras cargas de trabalho.

Pode definir o seguinte em política de segurança:

- **Recolha de dados**: determina o aprovisionamento do agente e [recolha de dados](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection) definições.
- **Política de segurança**: determina que controla o Centro de segurança monitoriza e recomenda. Pode editar a [política de segurança](security-center-policies.md) no Centro de segurança. Também pode utilizar [do Azure Policy](security-center-azure-policy.md) para criar definições novas, definir políticas adicionais e atribuir políticas a vários grupos de gestão. 
- **Notificações por e-mail**: determina os contactos de segurança, e [notificação por e-mail](security-center-provide-security-contact-details.md) definições.
- **Escalão de preço**: define gratuito ou standard [seleção de preço](security-center-pricing.md). O escalão que escolhe determina as funcionalidades do Centro de Segurança que estão disponíveis para recursos no âmbito. Pode especificar um escalão para subscrições, grupos de recursos e áreas de trabalho.

> [!NOTE]
> Pode definir tudo isso por subscrição. Para áreas de trabalho, pode definir apenas a recolha de dados e o escalão de preço. Para grupos de recursos, pode definir apenas escalão de preço.
>


## <a name="who-can-edit-security-policies"></a>Quem pode editar as políticas de segurança?
Centro de segurança utiliza baseada em funções controlo de acesso (RBAC), que fornece funções incorporadas que podem ser atribuídas a utilizadores, grupos e serviços no Azure. Quando os utilizadores abrem o Centro de segurança, veem apenas informações relacionadas com a recursos que aos quais têm acesso. O que significa que os utilizadores são atribuídos a função de *proprietário*, *contribuinte*, ou *leitor* para o subscrição ou grupo de recursos que um recurso pertence. Além destas funções, há duas funções específicas do Centro de Segurança:

- **Leitor de segurança**: ter direitos de exibição ao centro de segurança, que incluem recomendações, alertas, políticas e estado de funcionamento, mas eles não podem fazer alterações.
- **Administrador de segurança**: têm os mesmos direitos de exibição como *leitor de segurança*, e também pode atualizar a política de segurança e ignorar recomendações e alertas.


## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu sobre as políticas de segurança no Centro de segurança do Azure. Para saber mais sobre o Centro de segurança do Azure, veja os artigos seguintes:

* [Definir políticas de segurança no Centro de segurança do Azure](security-center-policies.md): Saiba como configurar políticas de segurança para as suas subscrições do Azure e grupos de recursos.
* [Gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md): Saiba como as recomendações do Centro de segurança ajudam a proteger os seus recursos do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md): saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md): saiba como gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros com o Centro de Segurança do Azure](security-center-partner-solutions.md): saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
- [Segurança de dados do Centro de segurança do Azure](security-center-data-security.md): Saiba como o Centro de segurança gere e protege os dados.
* [Azure Security Center FAQ](security-center-faq.md) (FAQ do Centro de Segurança do Azure): obtenha respostas às perguntas mais frequentes sobre como utilizar o serviço.
* [Blogue de segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): Obtenha as mais recentes notícias de segurança do Azure e informações.
