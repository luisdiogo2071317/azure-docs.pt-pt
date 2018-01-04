---
title: "Artigo de início rápido para Visual Studio Tools para Machine Learning no Azure | Microsoft Docs"
description: "Este artigo descreve como começar a utilizar o Visual Studio Tools para Machine Learning, desde criar uma experimentação a preparar um modelo e operar um serviço Web."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/15/2017
ms.openlocfilehash: bbcb2ea5a7ceeb976f590393608cc29c67d9a49e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools para IA
O Visual Studio Tools para IA é uma extensão de desenvolvimento para criar, testar e implementar soluções de Aprendizagem Profunda e IA. Proporciona integração total com o Azure Machine Learning, nomeadamente uma vista de histórico de execução que detalha o desempenho de preparações anteriores e métricas personalizadas. Oferece uma vista de explorador de amostras que permite procurar e efetuar o arranque de sistema do novo projeto com o [Microsoft Cognitive Toolkit (anteriormente designado CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit), [Google TensorFlow](https://www.tensorflow.org)e outra arquitetura de aprendizagem profunda. Por fim, fornece um explorador de destinos de computação que permite submeter tarefas de preparação de modelos em ambientes remotos como as Máquinas Virtuais do Azure ou os servidores Linux com GPU. Também proporciona um acesso facilitado ao [Azure Batch AI (Preview)](https://docs.microsoft.com/azure/batch-ai/).
 
## <a name="getting-started"></a>Introdução 
Para começar, tem primeiro de transferir e instalar o [Visual Studio](https://www.visualstudio.com/downloads/). Assim que abrir o Visual Studio, efetue os seguintes passos:
1. Clique na barra de menus no Visual Studio e selecione "Extensões e Atualizações..."
2. Clique no separador "Online" e selecione "Procurar no Visual Studio Marketplace".
3. Procure "Visual Studio para IA". 
3. Clique no botão **Transferir**. 
4. Após a instalação, reinicie o Visual Studio. 

Depois de recarregar o Visual Studio, a extensão é ativada. [Saiba mais sobre como encontrar extensões](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions).

> [!NOTE]
> O Visual Studio Tools para IA requer o Visual Studio 2015 ou 2017 nas edições Professional ou Enterprise. Não suporta a versão Apple OSX. 


## <a name="exploring-project-samples"></a>Explorar amostras de projeto
O Visual Studio Tools para IA inclui um explorador de amostras. O explorador de amostras torna mais fácil detetar amostras e experimentá-las com apenas alguns cliques. Para abrir o explorador, faça o seguinte:   
1. Na barra de menus, clique em **Ferramentas de IA**.
2. Clique em “Galeria do Azure Machine Learning”.

É aberto um separador com todos os exemplos do Azure ML.

## <a name="creating-a-new-project-from-the-sample-explorer"></a>Criar um novo projeto a partir do explorador de amostras 
Pode procurar diferentes amostras e obter mais informações sobre as mesmas. Comece a procurar até encontrar a amostra "Classifying Iris". Para criar um projeto novo com base neste exemplo, faça o seguinte:
1. Clique no botão **Instalar** no projeto de exemplo, o que abre uma caixa de diálogo nova. 
2. Selecione um grupo de recursos, uma conta e uma área de trabalho.
3. Pode deixar o tipo de projeto como Geral.
4. Introduza um caminho de projeto e um nome para o mesmo e prima Enter. 
5. É aberta uma caixa de diálogo a pedir que guarde uma solução; clique em Guardar. 

Após a conclusão, é aberto um projeto novo numa instância nova do Visual Studio. 

> [!TIP]
> Tem de ter sessão iniciada para aceder ao seu recurso do Azure. No terminal incorporado, introduza "início de sessão az" e siga as instruções. 

## <a name="submitting-experiment-with-the-new-project"></a>Submeter a experimentação com o novo projeto
Para o novo projeto que está a ser aberto no Visual Studio, submeta um trabalho para um destino de computação (local ou VM com Docker).
Para submeter um trabalho, faça o seguinte: 
1. No explorador de soluções, clique com o botão do lado direito do rato no ficheiro que pretende submeter e selecione **Definir como Ficheiro de Arranque**.
2. Selecione o nome do projeto, clique com o botão do lado direito do rato e selecione **Submeter Trabalho...**.
3. É aberta uma caixa de diálogo nova, que lhe permite escolher o cluster (ou destino de computação) para executar o script.
4. Clique em **Submeter**

Depois de submeter a tarefa, o terminal incorporado mostra o progresso da execução.

## <a name="view-list-of-jobs"></a>Ver a lista de tarefas
Depois de submeter o trabalho, pode listar os trabalhos a partir do histórico de execuções.
1. No **Explorador de Servidores**, clique em **Ferramentas de IA**.
2. Em seguida, selecione **Azure Machine Learning**.
3. Clique no menu **Trabalhos**.

O explorador de trabalhos lista todas as experimentações submetidas para este projeto. 

## <a name="view-job-details"></a>Ver detalhes da tarefa
Com a vista do Explorador de trabalhos aberta, clique na primeira execução da lista.
É carregado o painel Resumo do Trabalho e o painel Registos e Saídas.

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Como configurar o Azure Machine Learning para trabalhar com um IDE](./how-to-configure-your-IDE.md)
