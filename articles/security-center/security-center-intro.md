---
title: "O que é o Centro de Segurança do Azure? | Microsoft Docs"
description: "Saiba mais sobre o Centro de Segurança do Azure, as suas capacidades principais e como funciona."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: terrylan
ms.openlocfilehash: 08102ce4caead003925aa600f4f7f005b1c336e0
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="what-is-azure-security-center"></a>O que é o Centro de Segurança do Azure?
O Centro de Segurança do Azure oferece gestão de segurança unificada e proteção contra ameaças avançada entre cargas de trabalho na cloud híbrida. Com o Centro de Segurança, pode aplicar políticas de segurança pelas suas cargas de trabalho, limitar a sua exposição a ameaças e detetar e responder a ataques.

Porquê utilizar o Centro de Segurança?

- **Gestão da política centralizada** – Certifique-se de que cumpre os requisitos de segurança regulamentar ou da empresa ao gerir centralmente políticas de segurança em todas as suas cargas de trabalho da cloud híbridas.
- **Avaliação de segurança contínua** – monitorize a segurança das máquinas, das redes, do armazenamento e dos serviços de dados e aplicações para detetar potenciais problemas de segurança.
- **Recomendações acionáveis** – corrija vulnerabilidades de segurança antes de serem exploradas pelos atacantes com recomendações de segurança prioritárias e acionáveis.
- **Defesas da cloud avançadas** – Reduza as ameaças com acesso just-in-time às portas de gestão e à lista branca para controlar aplicações em execução nas suas VMs.
- **Alertas e incidentes priorizados** - concentre-se primeiro nas ameaças mais importantes com alertas de segurança e de incidentes definidos.
- **Soluções de segurança integradas** - Recolha, pesquise e analise os dados de segurança de várias origens, incluindo soluções de parceiros associados.

O **Centro de Segurança - Descrição Geral** oferece uma visão rápida da postura de segurança das suas cargas de trabalho do Azure e não só, permitindo-lhe detetar e avaliar a segurança das cargas de trabalho e identificar e mitigar o risco. O dashboard incorporado apresenta informações aprofundadas de alertas de segurança e vulnerabilidades que precisam de atenção.

![Descrição geral][1]

## <a name="centralized-policy-management"></a>Gestão de políticas centralizada
As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares. No Centro de Segurança, o utilizador define as políticas e personaliza-as para o tipo de carga de trabalho ou da sensibilidade dos seus dados.

As políticas do Centro de Segurança contêm os componentes seguintes:

- **Recolha de dados**: determina as definições do aprovisionamento do agente e da [recolha de dados](security-center-enable-data-collection.md) de segurança.
- **Política de segurança**: determina os controlos que o Centro de Segurança monitoriza e recomenda ao editar a [política de segurança](security-center-policies.md).
- **Notificações por e-mail**: determina as definições dos contactos de segurança e da [notificação por e-mail](security-center-provide-security-contact-details.md).
- **Escalão de preço**: define a [seleção de preços](security-center-pricing.md) Gratuita ou Standard. O escalão que escolhe determina as funcionalidades do Centro de Segurança que estão disponíveis para recursos no âmbito.

![Política de segurança][2]

Veja [Descrição geral das políticas](security-center-policies-overview.md) para obter mais informações.

## <a name="continuous-security-assessment"></a>Avaliação de segurança contínua
O Centro de Segurança analisa o estado de segurança dos seus recursos de computação, das redes virtuais, do armazenamento e dos serviços de dados e aplicações. A avaliação contínua ajuda-o a detetar potenciais problemas de segurança, como sistemas com atualizações de segurança em falta ou portas de rede expostas. Selecione um mosaico na secção Prevenção para ver mais informações, incluindo uma lista de recursos e quaisquer vulnerabilidades que tenham sido identificadas.

![Monitorização do estado de funcionamento de segurança][3]

Veja [Monitorização de estado de funcionamento de segurança](security-center-monitoring.md), para obter mais informações.

## <a name="actionable-recommendations"></a>Recomendações acionáveis
O Centro de Segurança analisa o estado de segurança dos seus recursos do Azure e não só, para identificar potenciais vulnerabilidades de segurança. Uma lista de recomendações de segurança prioritárias orientam-no ao longo do processo de resolução dos problemas de segurança.

![Recomendações][4]

Veja [Gerir recomendações de segurança](security-center-recommendations.md), para obter mais informações.

## <a name="just-in-time-vm-access"></a>Acesso à VM just-in-time
Reduza a superfície do ataque da rede com o acesso controlado just-in-time às portas de gestão em VMs do Azure, para reduzir significativamente a exposição a ataques de força bruta e de outro tipo à rede.

![Acesso à VM just-in-time][5]

Especifique as regras sobre a forma como os utilizadores se podem ligar a máquinas virtuais. Quando for preciso, o acesso pode ser pedido ao Centro de Segurança ou através do PowerShell. Desde que o pedido esteja em conformidade com as regras, o acesso é concedido automaticamente no período de tempo de pedido.

Veja [Gerir o acesso da máquina virtual através do just in time](security-center-just-in-time.md), para obter mais informações.

## <a name="adaptive-application-controls"></a>Controlos de aplicações adaptáveis
Bloqueie o malware e outras aplicações indesejadas ao aplicar recomendações de lista branca adaptadas às suas cargas de trabalho do Azure específicas e com tecnologia de aprendizagem automática.

![Controlos de aplicações adaptáveis][6]

Reveja e clique para aplicar as regras de lista branca recomendadas da aplicação geradas pelo Centro de Segurança ou edite regras já configuradas.

Veja [Controlos de aplicação adaptável](security-center-adaptive-application.md), para obter mais informações.

## <a name="prioritized-alerts-and-incidents"></a>Alertas e incidentes prioritários
O Centro de Segurança utiliza a análise avançada e informações sobre ameaças globais para detetar ataques recebidos e atividades pós-violação. Os alertas são priorizados e agrupados em incidentes, ajudando-o a focar-se primeiro nas ameaças mais importantes. Pode criar os seus próprios alertas de segurança personalizada.

![Alertas e incidentes prioritários][7]

Pode rapidamente avaliar o âmbito e o impacto de um ataque com uma experiência de investigação visual e interativa, e utilizar consultas ad hoc ou predefinidas para uma exploração mais profunda de dados de segurança.

Veja [Gerir e responder a alertas de segurança](security-center-managing-and-responding-alerts.md) para obter mais informações.

## <a name="integrate-your-security-solutions"></a>Integrar as suas soluções de segurança
Pode recolher, procurar e analisar dados de segurança a partir de uma variedade de origens, incluindo soluções de parceiros associadas, como firewalls de rede e outros serviços Microsoft, no Centro de Segurança.

![Integrar soluções de segurança][8]

Veja [Integrar soluções de segurança](security-center-partner-integration.md), para obter mais informações.

## <a name="next-steps"></a>Passos seguintes

- Para começar a utilizar o Centro de Segurança, precisa de uma subscrição do Microsoft Azure. Se não tiver uma subscrição, pode inscrever-se numa [avaliação gratuita](https://azure.microsoft.com/free/).
- O escalão de preço Gratuito do Centro de Segurança está ativado com a sua subscrição do Azure. Para tirar partido da gestão de segurança avançada e das capacidades de deteção de ameaças, tem de atualizar para o escalão de preço Standard. O escalão Standard é gratuito durante os 60 dias iniciais. Veja a [página de preços do Centro de Segurança do Azure](https://azure.microsoft.com/pricing/details/security-center/), para obter mais informações.
- Se está pronto para ativar o Centro de Segurança Standard agora, o [Início rápido: Incluir a sua subscrição do Azure no Centro de Segurança Standard](security-center-get-started.md) acompanha-o ao longo dos passos.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
