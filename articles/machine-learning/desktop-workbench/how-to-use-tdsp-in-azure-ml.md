---
title: Estruturar projetos com o modelo de processo de ciência de dados de equipa | Documentos da Microsoft
description: Como criar uma instância de modelos de processo de ciência de dados de equipa (TDSP) no Azure Machine Learning, que estrutura projetos para colaboração
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 2fd0fe55b975010a1eaafeb455d61a6cb3ebff77
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970928"
---
# <a name="structure-projects-with-the-team-data-science-process-template"></a>Estruturar projetos com o modelo de processo de ciência de dados de equipa

Este documento fornece instruções sobre como criar projetos de ciência de dados numa versão anterior do Azure Machine Learning através de modelos de processo de ciência de dados de equipa (TDSP). Estes modelos ajudam a estruturar projetos para colaboração e a capacidade de reprodução. 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

## <a name="what-is-the-team-data-science-process"></a>O que é o Processo de Ciência de Dados de Equipa?
O TDSP é um processo de ciência de dados ágil e iterativa, para executar e fornecimento de soluções de análise avançada. Foi concebido para melhorar a colaboração e a eficiência das equipes de ciência de dados em organizações empresariais. Ele oferece suporte a esses objetivos com quatro componentes principais:

   * Um padrão [ciclo de vida de ciência de dados](../team-data-science-process/lifecycle.md) definição.
   * Uma estrutura de projeto padronizado [documentação, relatórios e modelos de projeto](https://github.com/Azure/Azure-TDSP-ProjectTemplate).
   * Uma infra-estrutura e recursos para a execução do projeto, por exemplo, respectivamente, uma computação e armazenamento infraestrutura e o código repositórios.
   * [Ferramentas e utilitários](https://github.com/Azure/Azure-TDSP-Utilities) para ciência de dados do projeto tarefas, tais como:
      - Controle de versão de colaboração
      - Revisão de código
      - Exploração de dados e modelação
      - Planejamento de trabalho

Para uma discussão mais completa sobre o TDSP, consulte a [descrição geral do processo de ciência de dados de equipa](../team-data-science-process/overview.md).

## <a name="why-should-you-use-the-tdsp-structure-and-templates"></a>Por que deve usar a estrutura TDSP e modelos?
Padronização da estrutura, ciclo de vida e documentação de projetos de ciência de dados é fundamental para facilita a colaboração eficaz em equipas de ciência de dados. Crie projetos do machine learning com o modelo TDSP para fornecer uma estrutura de tal para trabalho em equipe coordenado.

Lançámos anteriormente uma [repositório do GitHub para a estrutura do projeto TDSP e os modelos](https://github.com/Azure/Azure-TDSP-ProjectTemplate) para ajudar a atingir esses objetivos. Mas não foi possível, até agora, para instanciar a estrutura TDSP e modelos de dentro de uma ferramenta de ciência de dados. Agora é possível criar um projeto do machine learning que cria uma instância os modelos de estrutura e a documentação do TDSP. 

## <a name="things-to-note-before-creating-a-new-project"></a>Observações a serem feitas antes de criar um novo projeto
Reveja os seguintes itens *antes de* criar um novo projeto:
* Reveja o TDSP Machine Learning [modelo](https://aka.ms/tdspamlgithubrepo).
* O conteúdo (que não seja o que já se encontra presente na pasta "documentos") têm de ser inferior a 25 MB de tamanho. Consulte a nota que se segue nesta lista.
* O exemplo\_pasta de dados é apenas para ficheiros de dados pequenos (menos de 5 MB) com a qual pode testar o seu código ou começar o desenvolvimento inicial.
* Armazenamento de arquivos, como o Word e PowerPoint, o pode aumentar substancialmente o tamanho da pasta "documentos". Aconselhamos que encontre um Wiki de colaboração, [SharePoint](https://products.office.com/sharepoint/collaboration), ou outros recursos de colaboração para armazenar esses ficheiros.
* Para saber como lidar com ficheiros grandes e saídas no Machine Learning, leia [persistir alterações e lidar com arquivos grandes](http://aka.ms/aml-largefiles).

> [!NOTE]
> Todos os relacionados com a documentação do conteúdo (texto, markdowns, imagens e outros ficheiros de documento) que seja *não* usado durante a execução do projeto, exceto o ficheiro readme.md, tem de residir na pasta "documentos" (em minúsculas). A pasta "documentos" é uma pasta especial ignorada pela execução de Machine Learning, para que o conteúdo nesta pasta não copiado para destinos de computação desnecessariamente. Objetos nesta pasta também não contam para o limite máximo de 25 MB para o tamanho do projeto. A pasta "documentos", por exemplo, é o local para armazenar os arquivos de imagem grande necessários na sua documentação. Estes ficheiros ainda são controlados pelo Git através do histórico de execuções. 

## <a name="instantiate-the-tdsp-structure-and-templates-from-the-machine-learning-template-gallery"></a>Criar uma instância a estrutura TDSP e modelos a partir da Galeria de modelos de Machine Learning
Para criar um novo projeto com os modelos de estrutura e a documentação do TDSP, conclua os procedimentos seguintes.

### <a name="create-a-new-project"></a>Criar um novo projeto
Para criar um novo projeto, abra o Azure Machine Learning. Sob **projetos** no painel superior esquerdo, selecione o sinal de adição (**+**) e, em seguida, selecione **novo projeto**.

![Novo projeto](./media/how-to-use-tdsp-in-azure-ml/instantiation-1.png)


### <a name="create-a-new-tdsp-structured-project"></a>Criar um novo projeto de TDSP-estruturado
   1. Especifique os parâmetros e as informações na caixa relevante ou lista:

      - Nome do projeto
      - Diretório do projeto
      - Descrição do projeto
      - Um caminho de repositório de Git vazio
      - Nome da área de trabalho

   2. Em seguida, no **pesquisa** , introduza **TDSP**. 
   3. Quando o **estruturar um projeto com TDSP** for apresentada a opção, selecione esse modelo. 
   4. Selecione o **criar** botão para criar o novo projeto com uma estrutura TDSP. Se fornecer um repositório de Git vazio ao criar o projeto (na caixa de texto adequado), em seguida, esse repositório será preenchido com a estrutura do projeto e o conteúdo após a criação do projeto.

![Criar um projeto TDSP](./media/how-to-use-tdsp-in-azure-ml/instantiation-2.png)


## <a name="examine-the-tdsp-project-structure"></a>Examinar a estrutura de projeto TDSP
Depois de criado o novo projeto, pode examinar sua estrutura (veja o painel esquerdo na figura a seguir). Contém todos os aspectos de uma documentação padronizada para compreensão de negócios. Esses itens incluem os estágios do ciclo de vida do TDSP, localização de dados, definições e a arquitetura deste modelo de documentação. 

A estrutura mostrada é derivada da estrutura de TDSP que está publicada na [modelos de estrutura, documentos e artefactos do projeto TDSP](https://github.com/Azure/Azure-TDSP-ProjectTemplate), com algumas modificações. Por exemplo, vários dos modelos de documentos são mesclados no markdown de uma, ou seja, ele [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport). 

### <a name="project-folder-structure"></a>Estrutura de pasta do projeto
O modelo de projeto TDSP contém as seguintes pastas de nível superior:
   - **código**: contém o código.
   - **Docs**: contém os documentos sobre o projeto (por exemplo, ficheiros de markdown e mídia relacionadas).
   - **sample_data**: contém **exemplo (pequena)** dados que podem ser usados para desenvolvimento antecipado ou teste. Normalmente, estes conjuntos não são maiores do que vários (5) MB. Esta pasta não é para completos ou grandes conjuntos de dados.

![Dados de exemplo](./media/how-to-use-tdsp-in-azure-ml/instantiation-3.png)


## <a name="use-the-tdsp-structure-and-templates"></a>Utilizar a estrutura TDSP e modelos
Tem de adicionar informações específicas do projeto para a estrutura e modelos. Deverá preenchê-los com o código e as informações necessárias para executar e fornecer o seu projeto. O [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) ficheiro é um modelo que precisa modificar com informações relevantes ao seu projeto. Ele vem com um conjunto de perguntas que o ajudam a preencher as informações para cada um dos quatro estágios do [ciclo de vida do TDSP](../team-data-science-process/lifecycle.md).

Um exemplo de uma estrutura de projeto semelhante ao seguinte durante a execução ou após a conclusão é mostrada no painel esquerdo na figura a seguir. Este projeto é a partir da [projeto de exemplo do processo de ciência de dados de equipa: classificar incomes de dados de censo dos EUA no Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) projeto de exemplo.

![Estrutura do projeto de exemplo](./media/how-to-use-tdsp-in-azure-ml/instantiation-4.png)

## <a name="document-your-project"></a>Documente seu projeto
Consulte a [modelos do TDSP](https://github.com/Azure/Azure-TDSP-ProjectTemplate) para obter informações sobre como documentar seu projeto. O modelo de documentação do Machine Learning TDSP atual, recomendamos que inclui todas as informações no [ProjectReport](https://aka.ms/tdspamlgithubrepoprojectreport) ficheiro. Este modelo deve ser preenchido com informações específicas ao seu projeto. 

Também fornecemos uma [ProjectLearnings](https://aka.ms/tdspamlgithubrepoprojectlearnings) modelo. Pode utilizar este modelo para incluir todas as informações que não está incluído no documento de projeto principal, mas isso é útil para documentar. 

### <a name="example-project-report"></a>Relatório de projeto de exemplo
Pode obter um [relatório de projeto de exemplo](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). Este relatório de projeto para o [projeto de exemplo de classificação de receitas dos E.U.A.](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) mostra como criar uma instância e utilizar o modelo TDSP para um projeto de ciência de dados.

## <a name="next-steps"></a>Passos Seguintes
Para facilitar a sua compreensão sobre como utilizar a estrutura TDSP e modelos em projetos de Machine Learning, fornecemos vários exemplos de projetos concluídos na documentação de Machine Learning:

- Para obter um exemplo que mostra como criar um projeto TDSP no Machine Learning, veja [projeto de exemplo do processo de ciência de dados de equipa: classificar incomes de dados de censo-no Azure Machine Learning](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome).
- Para obter um exemplo que utiliza a aprendizagem profunda em linguagem natural (NLP) de processamento num projeto de TDSP-instanciado no Machine Learning, veja [reconhecimento de entidades de biografia medical com linguagem natural com aprendizagem profunda de processamento](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction).

