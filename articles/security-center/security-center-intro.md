---
title: O que é o Centro de Segurança do Azure? | Microsoft Docs
description: Saiba mais sobre o Centro de Segurança do Azure, as suas capacidades principais e como funciona.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: rkarlin
ms.openlocfilehash: 682291cfe0e7a06aca1d4b1bef48ba36c72f48d9
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52839879"
---
# <a name="what-is-azure-security-center"></a>O que é o Centro de Segurança do Azure?
O Centro de Segurança do Azure oferece gestão de segurança unificada e proteção contra ameaças avançada entre cargas de trabalho na cloud híbrida. Com o Centro de Segurança, pode aplicar políticas de segurança pelas suas cargas de trabalho, limitar a sua exposição a ameaças e detetar e responder a ataques.

Porquê utilizar o Centro de Segurança?

- **Gestão da política centralizada** – Certifique-se de que cumpre os requisitos de segurança regulamentar ou da empresa ao gerir centralmente políticas de segurança em todas as suas cargas de trabalho da cloud híbridas.
- **Avaliação de segurança contínua** – monitorize a postura de segurança das máquinas, das redes, do armazenamento e dos serviços de dados e aplicações para detetar potenciais problemas de segurança.
- **Recomendações acionáveis** – corrija vulnerabilidades de segurança antes de serem exploradas pelos atacantes com recomendações de segurança prioritárias e acionáveis.
- **Alertas e incidentes priorizados** - concentre-se primeiro nas ameaças mais importantes com alertas de segurança e de incidentes definidos.
- **Defesas da cloud avançadas** – Reduza as ameaças com acesso just-in-time às portas de gestão e aos controlos de aplicação adaptativos em execução nas suas VMs.
- **Soluções de segurança integradas** - Recolha, pesquise e analise os dados de segurança de várias origens, incluindo soluções de parceiros associados.

O **Centro de Segurança - Descrição Geral** oferece uma visão rápida da postura de segurança das suas cargas de trabalho do Azure e não só, permitindo-lhe detetar e avaliar a segurança das cargas de trabalho e identificar e mitigar o risco. O dashboard incorporado apresenta informações aprofundadas de alertas de segurança e vulnerabilidades que precisam de atenção.

![Descrição geral][1]

## <a name="centralized-policy-management"></a>Gestão de políticas centralizada
A secção **Política e conformidade** (mostrada acima) proporciona informação rápida sobre a abrangência da subscrição, conformidade da política e postura de segurança.

### <a name="subscription-coverage"></a>Abrangência da subscrição
Esta secção apresenta o número total de subscrições a que tem acesso (leitura ou escrita) e o nível de abrangência do Centro de Segurança (Standard ou Gratuito) em que uma subscrição está a ser executada:

- **Abrangida (Standard)** – As subscrições abrangidas são executadas no nível de proteção máximo oferecido pelo Centro de Segurança
- **Abrangida (Gratuito)** – As subscrições abrangidas são executadas no nível de proteção limitado, gratuito oferecido pelo Centro de Segurança
- **Não abrangida** – As subscrições neste estado não são monitorizadas pelo Centro de Segurança

Selecionar o gráfico abre a janela **Abrangência**. Selecionar um separador (**Não abrangida**, **Abrangência básica** ou **Abrangência padrão**) proporciona uma lista de subscrições em cada estado. Selecionar uma subscrição num dos separadores, proporciona informação adicional sobre a subscrição. Esta informação permite-lhe identificar o proprietário de uma subscrição e contactá-lo para ativar o Centro de Segurança ou aumentar a abrangência da subscrição.

![Abrangência do Centro de Segurança][9]

### <a name="policy-compliance"></a>Conformidade com a política
A conformidade com a política é determinada pelos fatores de conformidade de todas as políticas atribuídas. A pontuação da conformidade geral para um grupo de gestão, subscrição ou área de trabalho é a média ponderada das atribuições. A média ponderada aparece no número de políticas de uma única atribuição e o número de recursos aos quais a atribuição se aplica.

Por exemplo, se a sua subscrição tiver duas VMs e uma iniciativa com cinco políticas atribuídas, terá dez avaliações na sua subscrição. Se uma das VMs não estiver em conformidade com duas das políticas, a pontuação de conformidade geral da atribuição da sua subscrição será de 80%.

Esta secção apresenta o seu rácio de conformidade geral e as subscrições com menor conformidade. Selecionar **Mostrar conformidade com a sua política** abre a janela **Gestão de Políticas**. A **Gestão de Políticas** mostra a estrutura hierárquica dos grupos de gestão, subscrições e áreas de trabalho. Aqui, pode gerir as suas políticas de segurança ao escolher uma subscrição ou um grupo de gestão.

![Gestão de políticas][10]

As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares. No Centro de Segurança, o utilizador define as políticas e personaliza-as para o tipo de carga de trabalho ou da sensibilidade dos seus dados, ao determinar quais os controlos que são monitorizados e recomendados pelo Centro de Segurança. Pode editar a política de segurança no Centro de Segurança ao clicar num grupo de gestão ou numa subscrição. Também pode utilizar o Azure Policy para criar novas definições, definir políticas adicionais e atribuir políticas em vários grupos de gestão.

Selecione **Editar definições>** para editar as seguintes definições do Centro de Segurança ao nível de subscrição, grupo de gestão, grupo de recursos ou área de trabalho:

- **Recolha de dados**: determina as definições do aprovisionamento do agente e da [recolha de dados](security-center-enable-data-collection.md) de segurança.
- **Notificações por e-mail**: determina as definições dos contactos de segurança e da [notificação por e-mail](security-center-provide-security-contact-details.md).
- **Escalão de preço**: define a [seleção de preços](security-center-pricing.md) Gratuita ou Standard. O escalão que escolhe determina as funcionalidades do Centro de Segurança que estão disponíveis para recursos no âmbito.
- **Editar configurações de segurança**: permite-lhe ver e modificar as configurações do SO que são avaliadas pelo Centro de Segurança para identificar potenciais vulnerabilidades de segurança.

Para obter mais informações, veja [Integrar políticas de segurança do Centro de Segurança no Azure Policy](security-center-azure-policy.md).

### <a name="manage-and-govern-your-security-posture"></a>Gerir e governar a sua postura de segurança
O lado direito do dashboard em **Política e conformidade** oferece informações com as quais pode atuar de imediato para melhorar a postura de segurança geral. Os exemplos são:

- Definir e atribuir políticas do Centro de Segurança para rever e controlar a conformidade com os padrões de segurança
- Disponibilizar alertas de segurança do Centro de Segurança para um conector SIEM
- Conformidade da política ao longo do tempo

## <a name="continuous-security-assessment"></a>Avaliação de segurança contínua
A secção de Higiene de segurança de recursos em **Centro de Segurança – Descrição Geral** oferece uma visão rápida da higiene de segurança dos seus recursos, ao apresentar o número de problemas identificados e o estado de segurança para cada tipo de recurso. A avaliação contínua ajuda-o a detetar potenciais problemas de segurança, como sistemas com atualizações de segurança em falta ou portas de rede expostas.

### <a name="secure-score"></a>Classificação de segurança
A classificação de segurança do Centro de Segurança do Azure avalia as suas recomendações de segurança e classifica-as em vez de si, de modo que pode saber quais as recomendações a efetuar em primeiro lugar, ajudando-o a detetar as vulnerabilidades de segurança mais graves para que possa dar prioridade à investigação. A classificação de segurança é uma ferramenta de medição que o ajuda a aumentar a segurança para conseguir uma carga de trabalho segura. Para obter mais informações, veja [Classificação de segurança no Centro de Segurança do Azure](security-center-secure-score.md).

### <a name="health-monitoring"></a>Monitorização do estado de funcionamento
Selecionar um tipo de recurso em **Monitorização do estado de funcionamento de recursos** proporciona uma lista de recursos e quaisquer vulnerabilidades que tenham sido identificadas. Os tipos de recurso são computação e aplicações, redes, dados e armazenamento, e identidade e acesso.

Selecionámos **Computação e aplicações**. Em **Computação**, existem quatro separadores:

- **Descrição geral**: monitorização e recomendações identificadas pelo Centro de Segurança.
- **VMs e computadores**: lista das suas VMS, computadores, máquinas virtuais e os atuais estados de segurança de cada um.
- **Serviços Cloud**: lista das suas funções de trabalho e na Web monitorizadas pelo Centro de Segurança.
- **Serviços de aplicações (Pré-visualização)**: lista de aplicações Web e ambientes de serviço de aplicações e estado de segurança atual de cada um.

![Monitorização do estado de funcionamento de segurança][3]

Para obter mais informações, veja [Monitorização de estado de funcionamento da segurança](security-center-monitoring.md).

## <a name="actionable-recommendations"></a>Recomendações acionáveis
O Centro de Segurança analisa o estado de segurança dos seus recursos do Azure e não só, para identificar potenciais vulnerabilidades de segurança. Selecionar **Recomendações** em **Recursos** proporciona uma lista de recomendações de segurança prioritárias que o orientam ao longo do processo de resolução dos problemas de segurança.

![Recomendações][4]

Para obter mais informações, veja [Gerir recomendações de segurança](security-center-recommendations.md).

### <a name="most-prevalent-recommendations"></a>Recomendações mais prevalecentes
O lado direito do dashboard em **Recursos** oferece uma lista das recomendações mais prevalecentes que existem para o maior número de recursos. As recomendações mais prevalecentes destacam onde deve concentrar-se a sua atenção. Selecionar a seta para a direita proporciona a recomendação de impacto mais elevado.

![Recomendações mais prevalecentes][11]

Esta é a única recomendação de maior impacto que tiver no seu ambiente. Resolver esta recomendação irá melhorar a sua conformidade ao máximo. Neste exemplo, a recomendação é "aplicar encriptação de disco." Selecionar **Melhorar a sua conformidade** proporciona uma descrição da recomendação e uma lista dos recursos afetados.

![Aplicar encriptação de discos][12]

## <a name="threat-protection"></a>Proteção contra ameaças
Esta área oferece visibilidade para os alertas de segurança detetados nos seus recursos e para o nível de gravidade desses alertas.

### <a name="prioritized-alerts-and-incidents"></a>Alertas e incidentes prioritários
O Centro de Segurança utiliza a análise avançada e informações sobre ameaças globais para detetar ataques recebidos e atividades pós-violação. Os alertas são priorizados e agrupados em incidentes, ajudando-o a focar-se primeiro nas ameaças mais importantes. Pode criar os seus próprios alertas de segurança personalizada.

Selecionar **Alertas de segurança por gravidade** ou **Alertas de segurança ao longo do tempo** proporciona informações detalhadas sobre os alertas.

![Alertas e incidentes prioritários][7]

Pode rapidamente avaliar o âmbito e o impacto de um ataque com uma experiência de investigação visual e interativa, e utilizar consultas ad hoc ou predefinidas para uma exploração mais profunda de dados de segurança.

Para obter mais informações, veja [Gerir e responder a alertas de segurança](security-center-managing-and-responding-alerts.md).

O lado direito do dashboard oferece informações para ajudá-lo a classificar quais os alertas a resolver em primeiro lugar e para compreender onde estão as vulnerabilidades mais comuns:

- **Recursos mais atacados**: recursos específicos que têm o número de alertas mais alto
- **Alertas mais prevalentes**: tipos de alerta que afetam o maior número de recursos

## <a name="just-in-time-vm-access"></a>Acesso à VM just-in-time
Reduza a superfície do ataque da rede com o acesso controlado just-in-time às portas de gestão em VMs do Azure, para reduzir significativamente a exposição a ataques de força bruta e de outro tipo à rede.

![Acesso à VM just-in-time][5]

Especifique as regras sobre a forma como os utilizadores se podem ligar a máquinas virtuais. Quando for preciso, o acesso pode ser pedido ao Centro de Segurança ou através do PowerShell. Desde que o pedido esteja em conformidade com as regras, o acesso é concedido automaticamente no período de tempo de pedido.

Para obter mais informações, veja [Manage virtual machine access using just in time](security-center-just-in-time.md) (Gerir o acesso da máquina virtual através do just in time).

## <a name="adaptive-application-controls"></a>Controlos de aplicações adaptáveis
Bloqueie o malware e outras aplicações indesejadas ao aplicar recomendações de lista branca adaptadas às suas cargas de trabalho do Azure específicas e com tecnologia de aprendizagem automática.

![Controlos de aplicações adaptáveis][6]

Reveja e clique para aplicar as regras de lista branca recomendadas da aplicação geradas pelo Centro de Segurança ou edite regras já configuradas.

Para obter mais informações, veja [Controlos de aplicação adaptável](security-center-adaptive-application.md).

## <a name="integrate-your-security-solutions"></a>Integrar as suas soluções de segurança
Pode recolher, procurar e analisar dados de segurança a partir de uma variedade de origens, incluindo soluções de parceiros associadas, como firewalls de rede e outros serviços Microsoft, no Centro de Segurança.

![Integrar soluções de segurança][8]

Para obter mais informações, veja [Integrar soluções de segurança](security-center-partner-integration.md).

## <a name="next-steps"></a>Passos Seguintes

- Para começar a utilizar o Centro de Segurança, precisa de uma subscrição do Microsoft Azure. Se não tiver uma subscrição, pode inscrever-se numa [avaliação gratuita](https://azure.microsoft.com/free/).
- O escalão de preço Gratuito do Centro de Segurança está ativado com a sua subscrição do Azure. Para tirar partido da gestão de segurança avançada e das capacidades de deteção de ameaças, tem de atualizar para o escalão de preço Standard. O escalão Standard pode ser tentado gratuitamente. Veja a [página de preços do Centro de Segurança do Azure](https://azure.microsoft.com/pricing/details/security-center/), para obter mais informações.
- Se está pronto para ativar o Centro de Segurança Standard agora, o [Início rápido: Incluir a sua subscrição do Azure no Centro de Segurança Standard](security-center-get-started.md) acompanha-o ao longo dos passos.


<!--Image references-->
[1]: ./media/security-center-intro/overview.png
[3]: ./media/security-center-intro/compute.png
[4]: ./media/security-center-intro/recommendations.png
[5]: ./media/security-center-intro/just-in-time-vm-access.png
[6]: ./media/security-center-intro/adaptive-app-controls.png
[7]: ./media/security-center-intro/security-alerts.png
[8]: ./media/security-center-intro/security-solutions.png
[9]: ./media/security-center-intro/coverage.png
[10]: ./media/security-center-intro/policy-management.png
[11]: ./media/security-center-intro/highest-impact.png
[12]: ./media/security-center-intro/apply-disk-encryption.png
