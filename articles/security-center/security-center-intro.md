---
title: "O que é o Centro de segurança do Azure? | Microsoft Docs"
description: "Saiba mais sobre o Centro de Segurança do Azure, as suas capacidades principais e como funciona."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 50a54b8d2a73807aa9a0217f7ccf971b8c516494
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="what-is-azure-security-center"></a>O que é o Centro de Segurança do Azure?
Centro de segurança do Azure fornece gestão unificada de segurança e proteção avançada contra ameaças a cargas de trabalho de nuvem híbrida. Centro de segurança, pode aplicar políticas de segurança entre as cargas de trabalho, limitar a exposição a ameaças e detetar e responder a ataques.

Porquê utilizar o Centro de Segurança?

- **Gestão da política centralizada** – Certifique-se requisitos de segurança regulamentação ou conformidade com a empresa ao gerir centralmente políticas de segurança em todas as suas híbridos na nuvem cargas de trabalho.
- **Avaliação de segurança contínua** – monitorizar a segurança das máquinas, redes, armazenamento e os dados de serviços e aplicações para detetar potenciais problemas de segurança.
- **Recomendações acionáveis** – remediar vulnerabilidades de segurança antes de estes podem ser forem exploradas pelos atacantes com recomendações de segurança prioritários e passíveis de ação.
- **Avançadas nuvem defesas** – reduzir a ameaças com apenas no acesso de tempo para as portas de gestão e a listas brancas para aplicações de controlo em execução nas suas VMs.
- **Definida alertas e incidentes** -focarem-se nas ameaças mais importantes primeiro com definida alertas de segurança e de incidentes.
- **Integrado soluções de segurança** - recolher, procurar e analisar dados de segurança de uma variedade de origens, incluindo as soluções de parceiros ligadas.

O **Centro de segurança - descrição geral** fornece uma vista rápida para a postura de segurança do Azure e cargas de trabalho não do Azure, permitindo-lhe para detetar e avaliar a segurança das cargas de trabalho e identificar e mitigar o risco. O dashboard incorporado fornece aprofundadas instantâneas de alertas de segurança e vulnerabilidades que necessitam da atenção.

![Descrição geral][1]

## <a name="centralized-policy-management"></a>Gestão da política centralizada
As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares. No Centro de segurança, pode definir políticas e personalizá-los para o tipo de carga de trabalho ou da sensibilidade dos seus dados.

As políticas do Centro de Segurança contêm os componentes seguintes:

- **Recolha de dados**: determina o agente de aprovisionamento e segurança [recolha de dados](security-center-enable-data-collection.md) definições.
- **Política de segurança**: determinar que controla os monitores de centro de segurança e recomenda editando o [política de segurança](security-center-policies.md).
- **Notificações por correio eletrónico**: determina contactos de segurança e [notificação de correio electrónico](security-center-provide-security-contact-details.md) definições.
- **Escalão de preço**: define gratuita ou Standard [preços seleção](security-center-pricing.md). A camada que escolha determina as funcionalidades do Centro de segurança estão disponíveis para os recursos no âmbito.

![Política de segurança][2]

Consulte [descrição geral das políticas de segurança](security-center-policies-overview.md) para obter mais informações.

## <a name="continuous-security-assessment"></a>Avaliação de segurança contínua
Centro de segurança analisa o estado de segurança dos seus recursos de computação, redes virtuais, armazenamento e serviços de dados e aplicações. Avaliação contínua ajuda-o a detetar potenciais problemas de segurança, tais como os sistemas com em falta atualizações de segurança ou as portas de rede expostos. Selecione um mosaico na secção de prevenção para ver mais informações, incluindo uma lista de recursos e as vulnerabilidades que tenham sido identificadas.

![Monitorização de estado de funcionamento de segurança][3]

Consulte [monitorização de estado de funcionamento de segurança](security-center-monitoring.md) para obter mais informações.

## <a name="actionable-recommendations"></a>Recomendações passíveis de ação
Centro de segurança analisa o estado de segurança do Azure e não do Azure recursos para identificar potenciais vulnerabilidades de segurança. Uma lista de recomendações de segurança prioritários orienta-o ao longo do processo de resolver problemas de segurança.

![Recomendações][4]

Consulte [gerir recomendações de segurança](security-center-recommendations.md) para obter mais informações.

## <a name="just-in-time-vm-access"></a>Acesso à VM just-in-time
Reduza a superfície de ataque de rede com apenas no tempo, o acesso controlado para as portas de gestão em VMs do Azure, reduzindo significativamente a exposição a força bruta e outros ataques de rede.

![Acesso à VM just-in-time][5]

Especifique as regras para a forma como os utilizadores podem ligar a máquinas virtuais. Quando for necessário, pode ser pedido acesso do Centro de segurança ou através do PowerShell. Desde que o pedido de suporte de dados está em conformidade com as regras, o acesso é concedido automaticamente durante o período de tempo de pedido.

Consulte [gerir o acesso de máquina virtual utilizando apenas no tempo](security-center-just-in-time.md) para obter mais informações.

## <a name="adaptive-application-controls"></a>Controlos de aplicações adaptáveis
Bloquear o software maligno e outras aplicações indesejáveis aplicando a listas brancas recomendações adaptada descritos para as cargas de trabalho do Azure específicas e utiliza a tecnologia de aprendizagem.

![Controlos de aplicações adaptáveis][6]

Rever e clique em para aplicar as regras de adicionar à lista branca recomendada de aplicação geradas pelo centro de segurança ou editar regras já configuradas.

Consulte [controlos de aplicação adaptável](security-center-adaptive-application.md) para obter mais informações.

## <a name="prioritized-alerts-and-incidents"></a>Alertas prioritários e incidentes
Centro de segurança utiliza a análise avançada e informações sobre ameaças globais para detetar ataques de entrada e pós-cópia infringir atividade. Os alertas são definidos e agrupados em incidentes, ajuda a focar-se nas ameaças mais importantes primeiro. Pode criar, bem como os seus próprios alertas de segurança personalizada.

![Alertas prioritários e incidentes][7]

Pode rapidamente avaliar o âmbito e o impacto de um ataque com uma experiência de investigação visual, interativa e utilizar consultas ad hoc ou predefinidas para mais profunda exploração de dados de segurança.

Consulte [gerir e responder a alertas de segurança](security-center-managing-and-responding-alerts.md) para obter mais informações.

## <a name="integrate-your-security-solutions"></a>Integrar as suas soluções de segurança
Pode recolher, procurar e analisar dados de segurança de uma variedade de origens, incluindo as soluções de parceiros ligadas, como firewalls de rede e outros serviços Microsoft, no Centro de segurança.

![Integrar soluções de segurança][8]

Consulte [integrar soluções de segurança](security-center-partner-integration.md) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

- Para começar a utilizar com o Centro de segurança, precisa de uma subscrição do Microsoft Azure. Se não tiver uma subscrição, pode inscrever-se para obter um [avaliação gratuita](https://azure.microsoft.com/free/).
- Escalão de preço gratuito de centro de segurança está ativado com a sua subscrição do Azure. Para tirar partido da gestão de segurança avançada e as capacidades de deteção de ameaças, tem de atualizar para o escalão de preço padrão. O escalão Standard é gratuito durante os 60 dias iniciais. Consulte o [Centro de segurança a página de preços](https://azure.microsoft.com/pricing/details/security-center/) para obter mais informações.
- Se estiver pronto para ativar o padrão de centro de segurança agora, o [início rápido: carregar a subscrição do Azure Security Center Standard](security-center-get-started.md) orienta-o através dos passos.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[2]: ./media/security-center-intro/security-policy.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
