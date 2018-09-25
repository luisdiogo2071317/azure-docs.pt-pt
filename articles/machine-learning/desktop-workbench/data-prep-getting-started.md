---
title: Introdução à preparação de dados do Azure Machine Learning | Documentos da Microsoft
description: Este é o guia de introdução para a secção de preparação de dados da bancada de trabalho AML
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
ROBOTS: NOINDEX
ms.openlocfilehash: a3f50e8fbaa396d9ca5a0d14fe9f54c30bc24f41
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996286"
---
# <a name="getting-started-with-data-preparation"></a>Introdução à preparação de dados

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Bem-vindo ao dados preparação guia de introdução ao. 

Preparação de dados fornece um conjunto de ferramentas para explorar de forma eficiente, entender e corrigir problemas nos dados. Pode consumir dados de várias formas e transformar dados em dados limpos, que é mais adequados para utilização de downstream.

Preparação de dados é instalada como parte da experiência do Azure Machine Learning Workbench.  Utilize a preparação de dados localmente ou implementar um cluster de destino e como um ambiente de tempo de execução ou de execução na nuvem.

O tempo de execução do tempo de design utiliza o Python para extensibilidade e depende de várias bibliotecas de Python como Pandas. Como com outros componentes do Azure ML Workbench não é necessário para instalar o Python, é instalado para. No entanto se é necessário instalar bibliotecas adicionais essas bibliotecas tem de ser instalado no diretório do Azure ML Workbench Python não é o diretório de Python habitual. Podem encontrar mais detalhes sobre como instalar pacotes [aqui](data-prep-python-extensibility-overview.md).

Um projeto é necessário antes de poder utilizar preparação de dados, uma vez criado desse projeto que podem preparar os dados. 

Navegue para a secção de dados do projeto ao selecionar o ícone de dados ![ícone de origem de dados](media/data-prep-getting-started/data-source-icon.png) no lado esquerdo do ecrã.  Clique em "+" novamente ao **adicionar uma origem de dados**. O Assistente de origem de dados deve iniciar e adiciona uma **origem de dados** ficheiro (.dsource) ao projeto depois de concluir o assistente. Por predefinição, a visualização dos dados é a grade. Acima da grelha, também é possível selecionar a vista de métricas. Na vista de métricas, estatísticas de resumo são apresentadas.  Depois de rever as estatísticas de resumo, clique em **preparar** na parte superior do ecrã. [Obter mais informações sobre o Assistente de origem de dados](data-source-wizard.md) 

## <a name="building-blocks-of-data-preparation"></a>Blocos de construção de preparação de dados ##
### <a name="the-package"></a>O pacote ###

Um pacote é o contentor principal para o seu trabalho. Um pacote é o artefacto que está guardado carregado a partir do disco. Ao trabalhar dentro do cliente, o pacote é constantemente AutoSaved em segundo plano. 

Os resultados/saída de um pacote pode ser verificados em Python ou através de um bloco de notas do Jupyter.

Um pacote pode ser executado em vários runtimes, incluindo local Python, o Spark (inclusive no Docker) e HDInsight.

Um pacote contém um ou mais fluxos de dados que são os passos e as transformações aplicadas aos dados.

Um pacote pode usar outro pacote como uma origem de dados (referido como um fluxo de dados de referência).

### <a name="the-dataflow"></a>O fluxo de dados ###
Um fluxo de dados tem uma origem e transformações opcional, que é disposto através de uma série de passos e opções de destino. Clicar num passo executa novamente todas as origens e antes de transformações e incluindo o passo selecionado.  Os dados transformados nessa etapa são mostrados na grade. Passos podem ser adicionados, movidos e eliminados dentro de um fluxo de dados através da lista de passo.

A lista de passo no lado direito do cliente pode ser aberta e fechada para fornecer mais espaço na tela.

Vários fluxos de dados podem existir na interface do Usuário ao mesmo tempo, cada fluxo de dados é representado como uma guia na interface do Usuário.

### <a name="the-source"></a>A origem
Uma origem é onde vêm os dados e o formato é na. Um pacote de preparação de dados de origens sempre os seus dados a partir de outro dados Flow(Data Source). É este fluxo de dados que contém as informações de referência. Cada origem tem uma experiência de utilizador diferente para permitir que ele seja configurado. A origem produz uma exibição de "retangular" / tabela dos dados. Se a origem de dados tiver originalmente o "direito desalinhado", a estrutura com a, em seguida, é normalizada para ser "retangular". [Apêndice 2 fornece a lista atual de fontes suportadas](data-prep-appendix2-supported-data-sources.md).

### <a name="the-transform"></a>A transformação ###
Transformações de consumam dados num determinado formato, realize algumas operações nos dados (por exemplo, alterando o tipo de dados) e, em seguida, produzem os dados no novo formato. Cada transformação tem sua própria interface do Usuário e behavior(s). Encadeando várias transformações através de passos no fluxo de dados é o núcleo da funcionalidade de preparação de dados. [Apêndice 3 fornece a lista atual de transformações suportadas](data-prep-appendix3-supported-transforms.md).

### <a name="the-inspector"></a>O inspetor de ###

Inspetores são visualizações dos dados e estão disponíveis para melhorar a compreensão dos dados.  Compreender os dados e dados os problemas de qualidade ajuda a que decidir quais ações (transformações) deverão ser executadas. Alguns Inspetores suportam ações que geram transformações. Por exemplo, o Inspetor de contagem de valores permite-lhe selecionar um valor e, em seguida, aplicar um filtro para incluir esse valor ou para excluir esse valor. Inspetores também podem fornecer contexto para transformações. Por exemplo, se selecionar uma ou mais colunas altera as transformações possíveis que podem ser aplicadas.

Uma coluna pode ter vários Inspetores em qualquer ponto no tempo (por exemplo, estatísticas de coluna e um histograma). Também pode haver instâncias de um Inspetor em várias colunas. Por exemplo, todas as colunas numéricas poderiam ter histogramas ao mesmo tempo.

Inspetores são apresentados na criação de perfis bem na parte inferior do ecrã.  Maximize a inspetores para vê-los maiores dentro da área de conteúdo principal. Considere a grade de dados como o inspetor de predefinição. Qualquer Inspetor pode ser expandido para a área de conteúdo principal. Minimizar inspetores dentro da área de conteúdo principal para a criação de perfis bem. Personalize um Inspetor ao clicar no ícone de lápis no Inspetor de. Inspetores de reordenação dentro bem usando arrastar e soltar.

Alguns Inspetores suportam um modo de "Halo". Este modo mostra o valor ou o estado antes da última transformação foi aplicada. O valor antigo é exibido em cinza com o valor atual em primeiro plano e mostra o impacto de uma transformação. [Apêndice 4 fornece a lista atual dos Inspetores suportados](data-prep-appendix4-supported-inspectors.md).

### <a name="the-destination"></a>O destino
 Um destino é onde escrita/exportar os dados para depois de ter preparado-lo num fluxo de dados. Um determinado fluxo de dados pode ter vários destinos. Cada destino tem uma experiência de utilizador diferente para permitir que ele seja configurado. O destino consome dados num formato tabular / "retangular" e escreve para algum local num determinado formato. [Apêndice 5 fornece a lista atual de destinos suportados](data-prep-appendix5-supported-destinations.md).

### <a name="using-data-preparation"></a>Usando a preparação de dados ###
Preparação de dados assume uma cinco etapas metodologia/abordagem básica para preparação de dados.

#### <a name="step-1-ingestion"></a>Passo 1: ingestão ####
Importar dados para preparação de dados utilizando o **Adicionar origem de dados** opção na vista de projeto.  Todos os ingestão inicial de dados é processada através do Assistente de origem de dados.

#### <a name="step-2-understandprofile-the-data"></a>Passo 2: Compreender/perfil os dados ####

Primeiro, como a barra de qualidade de dados na parte superior de cada coluna. Verde representa as linhas com valores. Cinzento representa linhas com um etc de nulo, valor, em falta. Vermelho indica os valores de erro. Coloque o cursor sobre a barra para obter uma dica de ferramenta com os números exatos de linhas em cada um dos três grupos. As utilizações de barra de qualidade de dados uma escala logarítmica por conseguinte, verifique os números reais para obter uma noção aproximada para o volume de dados em falta

![Colunas](media/data-prep-getting-started/columns.png)

Em seguida, utilize uma combinação de outros Inspetores mais a grade para compreender melhor as características dos dados.  Começar a formular hipóteses sobre a preparação de dados necessários para análise adicional. A maioria dos inspetores trabalham numa única coluna ou um pequeno número de colunas.  

É provável que vários Inspetores em várias colunas são necessárias para compreender os dados. Pode rolar pelas várias Inspetores na criação de perfis bem. Dentro do poço, também pode mover Inspetores ao cabeçalho da lista para vê-las na área imediatamente visível.

![inspetores](media/data-prep-getting-started/inspectors.PNG)

Inspetores diferentes são fornecidos para variáveis categóricos vs contínua/colunas. O menu de Inspetor ativa e desativa opções consoante o tipo de variáveis/colunas que tiver.

Ao trabalhar com conjuntos de dados grandes que têm o número de colunas, uma abordagem pragmática de trabalhar com subconjuntos é aconselhável. Esta abordagem inclui com foco num pequeno número de colunas (por exemplo, 5 a 10), a prepará-los e, em seguida, superando as restantes colunas. O inspetor de grid suporta a criação de partições verticais de colunas e, portanto, se tiver mais de 300 colunas, em seguida, terá de "page" por eles.
 

#### <a name="step-3-transform-the-data"></a>Passo 3: Transformar os dados ####
Transformações de alterar os dados e permitem a execução de dados para suportar a hipótese de trabalho atual. Transformações são exibidas como passos na lista de passo no lado direito. É possível para "tempo de viagem" através da lista de passo ao clicar em qualquer ponto arbitrário na lista de passo.

Um ícone verde à esquerda de um determinado passo indica que foi executada e os dados refletem a execução da transformação. Barra vertical para a esquerda do passo indica o estado atual dos dados dos Inspetores.

![passos](media/data-prep-getting-started/steps.PNG)

Tente fazer pequenas alterações frequentes aos dados e validar (etapa 4) após cada alteração como a hipótese evolui.

#### <a name="step-4-verify-the-impact-of-the-transformation"></a>Passo 4: Verificar o impacto da transformação. 
Decida se a hipótese foi correta. Caso esteja correta, em seguida, desenvolver a hipótese de seguinte e repita os passos 2 e 3 para o novo. Se incorreta, em seguida, anular a última transformação e desenvolver uma hipótese de novo e repita os passos 2 a 3.

A principal maneira de determinar se a transformação teve o impacto de certo é usar os Inspetores. Utilizar existente. Inspetores de utilização com o Halo efeito ativada ou iniciar vários Inspetores para ver os dados em fornecido pontos no tempo.

![inspetor de Halo](media/data-prep-getting-started/halo1.PNG) ![inspetor de Halo](media/data-prep-getting-started/halo2.PNG)

Para anular uma transformação, aceda a lista de passos no lado direito da interface do Usuário. (O painel de lista de passos poderá ser exibido. out. Para abri-lo, clique em apontar o dupla divisa à esquerda). No painel, selecione a transformação que foi executada a que pretende anular. Selecione na lista pendente à direita do bloco da interface do Usuário. Selecione **edite** para fazer alterações ou **eliminar** para remover a transformação da lista de passos e o fluxo de dados.

#### <a name="step-5-output"></a>Passo 5: saída 
Quando terminar a preparação de dados, pode escrever o fluxo de dados para uma saída. Um fluxo de dados pode ter várias saídas. No menu transformações, pode selecionar qual a saída de que pretende que o conjunto de dados para ser gravado como. Também pode selecionar o destino da saída. 

## <a name="list-of-appendices"></a>Lista de apêndices 
[Apêndice 2 – origens de dados suportadas](data-prep-appendix2-supported-data-sources.md)  
[Transforma apêndice 3 – suportado](data-prep-appendix3-supported-transforms.md)  
[Apêndice 4 - Inspetores suportados](data-prep-appendix4-supported-inspectors.md)  
[Apêndice 5 - destinos suportados](data-prep-appendix5-supported-destinations.md)  
[Apêndice 6 - expressões de filtro de exemplo em Python](data-prep-appendix6-sample-filter-expressions-python.md)  
[Apêndice 7 – exemplo transformar expressões de fluxo de dados em Python](data-prep-appendix7-sample-transform-data-flow-python.md)  
[Apêndice 8 - origens de dados de exemplo em Python](data-prep-appendix8-sample-source-connections-python.md)  
[Apêndice 9 - exemplo de ligações de destino no Python](data-prep-appendix9-sample-destination-connections-python.md)  
[Apêndice 10 - coluna do exemplo transforma em Python](data-prep-appendix10-sample-custom-column-transforms-python.md)  

## <a name="see-also"></a>Consultar Também

[Tutorial de preparação de dados avançada](tutorial-bikeshare-dataprep.md)