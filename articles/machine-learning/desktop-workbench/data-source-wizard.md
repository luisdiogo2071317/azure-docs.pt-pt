---
title: Assistente para o Azure Machine Learning de origem de dados do Azure | Documentos da Microsoft
description: Explica o Assistente de origem de dados da bancada de trabalho AML
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: c74229504a43179673cc99ccff321b65e3f6ed4f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650297"
---
# <a name="data-source-wizard"></a>Assistente de origem de dados #

O Assistente de origem de dados é uma forma rápida e fácil para que passem um conjunto de dados do Azure ML Workbench sem código. É onde também pode selecionar uma estratégia de exemplo para os conjunto de dados e tipos de dados para cada coluna. 

## <a name="step-1-trigger-the-data-source-wizard"></a>Passo 1: Acionar o Assistente de origem de dados ## 

Para colocar dados num projeto usando o Assistente de origem de dados. Selecione o **+** botão junto à caixa de pesquisa na vista de dados e escolha Adicionar origem de dados. 

![Adicionar origem de dados](media/data-source-wizard/add-data-source.png)

## <a name="step-2-select-where-data-is-stored"></a>Passo 2: Selecione de onde os dados são armazenados ##
Em primeiro lugar, especifique como estão atualmente em seus dados. Ele poderia ser armazenado num arquivo simples ou um diretório, um ficheiro parquet, um ficheiro do Excel ou uma base de dados. Para obter mais informações, consulte [origens de dados suportada](data-prep-appendix2-supported-data-sources.md).

![Passo 1](media/data-source-wizard/step1.png)

## <a name="step-3-select-data-file"></a>Passo 3: Selecione o ficheiro de dados ##
Para um ficheiro/diretório, especifique o caminho do ficheiro. Escolha, na lista pendente, a localização dos dados – pode ser um caminho de ficheiro local ou o armazenamento de Blobs do Azure. 

Especificar o caminho ao escrevê-lo na ou clicar no **procurar...** botão para navegar até ele. Pode navegar para um diretório ou um ou mais ficheiros.

Clique em **concluir** para aceitar as predefinições para o resto dos passos ou, em seguida, prossiga para o passo seguinte.


![Passo 4](media/data-source-wizard/step2.png)

## <a name="step-4-choose-file-parameters"></a>Passo 4: Escolher parâmetros do ficheiro ##

O Assistente de origem de dados pode detetar automaticamente o ficheiro de tipo, separadores e codificação. Isso também seria mostrado um exemplo da aparência os dados. Também pode alterar qualquer um destes parâmetros manualmente. 

![Passo 5](media/data-source-wizard/step3.png)

## <a name="step-5-set-data-types-for-columns"></a>Passo 5: Definir os tipos de dados para colunas ##

O Assistente de origem de dados Deteta automaticamente os tipos de dados das colunas do conjunto de dados. Se ele erros de uma ou que deseja impor um tipo de dados, pode alterar o tipo de dados manualmente. O **dados de saída de exemplo** coluna mostra-lhe exemplos do aspeto os dados.

Para colunas de Data Prep infere a conter datas, poderá ser-lhe pedido para selecionar a ordem do mês e dia no formato de data. Por exemplo, 1/2/2013 poderia representar 2nd de Janeiro (para isso, selecione *mês dias*) ou no dia 1 de Fevereiro (selecionar *dia do mês*).

![Passo 6](media/data-source-wizard/step4.png)

## <a name="step-6-choose-sampling-strategy-for-data"></a>Passo 6: Escolher a estratégia de amostragem de dados ##

Pode especificar um ou mais estratégias de amostragem para o conjunto de dados e escolha uma como a estratégia ativa. A predefinição é carregar as primeiros 10 000 linhas. Pode editar este exemplo ao clicar no **editar** botão na barra de ferramentas ou adicione uma nova estratégia, clicar em + novo. As estratégias que oferecem suporte ao são

-     Número superior de linhas
-     Número aleatório de linhas
-     Percentagem aleatória de linhas
-     Ficheiro completo

Pode especificar quantas estratégias de amostragem que quiser, mas existe apenas um que pode ser definida como ativa quando preparar seus dados. Pode definir qualquer estratégia para ser o ativo ao selecionar a estratégia e clique em definir como o Active Directory na barra de ferramentas.

Dependendo de onde vêm os dados, podem não ser suportadas algumas estratégias de exemplo. Para obter mais informações sobre a amostragem, veja a secção de amostragem em [neste documento](data-prep-user-guide.md) 

![Passo 6](media/data-source-wizard/step5.png)

## <a name="step-7-path-column-handling"></a>Passo 7: Manipulação de coluna de caminho ##

Se o caminho do ficheiro inclui dados importantes, pode optar por incluí-la como a primeira coluna no conjunto de dados. Esta opção pode ser útil se as incluírem em múltiplos ficheiros. Caso contrário, pode optar por não incluí-lo.

![Passo 7](media/data-source-wizard/step6.png)

Depois de clicar em Concluir, uma nova origem de dados será adicionada ao projeto. Pode encontrá-lo sob o grupo de origens de dados na vista de dados ou como um ficheiro de dsource no **File View**.
