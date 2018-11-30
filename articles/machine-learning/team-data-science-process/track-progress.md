---
title: Execução de projetos de ciência de dados - Azure Machine Learning | Documentos da Microsoft
description: Como um cientista de dados poderá acompanhar o progresso de um projeto de ciência de dados.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 202ac89b8a281012dbcf5f4c4df11e97ba2c8c65
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441473"
---
# <a name="track-progress-of-data-science-projects"></a>Controlar o progresso dos projetos de ciência de dados

Gestores de grupo de ciência de dados, a líderes de equipe e a necessidade de oportunidades potenciais de projeto para controlar o progresso de seus projetos, o qual trabalho tiver sido realizado nos mesmos e por quem e permanece nas listas de tarefas. 

## <a name="azure-devops-dashboards"></a>Dashboards de DevOps do Azure
Se estiver a utilizar o Azure DevOps, é possível criar dashboards para controlar as atividades e os itens de trabalho associados um determinado projeto Agile. 

Para obter mais informações sobre como criar e personalizar os dashboards e widgets em DevOps do Azure, consulte os seguintes conjuntos de instruções:

- [Adicionar e gerir dashboards](https://docs.microsoft.com/azure/devops/report/dashboards/dashboards)
- [Adicionar widgets ao dashboard](https://docs.microsoft.com/azure/devops/report/dashboards/add-widget-to-dashboard).

## <a name="example-dashboard"></a>Dashboard de exemplo

Aqui está um dashboard de exemplo simples que foi criado para controlar as atividades de sprint de um projeto de ciência de dados ágil, bem como o número de consolidações para repositórios associados. O **principais esquerda** painel mostra:

- a contagem regressiva do sprint atual, 
- o número de consolidações de cada repositório nos últimos 7 dias
- o item de trabalho para utilizadores específicos. 

Os painéis restantes mostram o diagrama de fluxo cumulativo (CFD), burndown e burnup para um projeto:

- **Parte inferior esquerda**: CFD a quantidade de trabalho num determinado Estado, que mostra aprovados em cinza, aplicadas a azul e, feito em verde.
- **Canto superior direito**: gráfico de burndown do trabalho restante para concluir versus o tempo restante).
- **Parte inferior direita**: burnup o trabalho que foi concluído em comparação com a quantidade total de trabalho do gráfico.

![dashboard](./media/track-progress/dashboard.png)

Para obter uma descrição de como criar estes gráficos, veja os inícios rápidos e tutoriais na [Dashboards](https://docs.microsoft.com/azure/devops/report/dashboards/).
 
## <a name="next-steps"></a>Passos Seguintes

Orientações passo a passo que demonstram todas as etapas do processo para **cenários específicos** também são fornecidas. Se estão listadas e estão associados ao descrições em miniatura da [instruções passo a passo do exemplo](walkthroughs.md) artigo. Eles ilustram como combinar a cloud, ferramentas no local e serviços num fluxo de trabalho ou um pipeline para criar uma aplicação inteligente. 
