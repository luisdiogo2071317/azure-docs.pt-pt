---
title: O que aconteceu ao Machine Learning Workbench?
titleSuffix: Azure Machine Learning service
description: Saiba mais sobre o que aconteceu com o Machine Learning Workbench, aplicativos, o que mudou no serviço Azure Machine Learning e o que é a linha cronológica de suporte.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: ff6b61874363bbc869bd509174e58640a2487f56
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811312"
---
# <a name="whats-happening-to-machine-learning-workbench-in-azure-machine-learning-service"></a>O que acontece ao Machine Learning Workbench no serviço Azure Machine Learning?

A aplicação Azure Machine Learning Workbench e alguns outros recursos antecipados foram preteridos e substituídos na versão de Setembro de 2018 para tornar a forma de uma melhor [arquitetura](concept-azure-machine-learning-architecture.md). Para melhorar a sua experiência, a versão contém várias atualizações significativas solicitadas pelo comentários dos clientes. A funcionalidade principal de execuções da experimentação para a implementação do modelo não mudou. Mas agora, pode usar a robusta <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> e o [CLI do Azure](reference-azure-machine-learning-cli.md) para realizar de aprendizagem, tarefas e pipelines.  

Neste artigo, ficará a conhecer o que foi alterado e como ele afeta o seu trabalho preexistente com o Azure Machine Learning Workbench e das respetivas APIs.

## <a name="what-changed"></a>O que mudou?

A versão mais recente do serviço Azure Machine Learning inclui as seguintes funcionalidades:
+ R [modelo de recursos do Azure simplificada](concept-azure-machine-learning-architecture.md).
+ R [novo portal da interface do Usuário](how-to-track-experiments.md) para gerir as suas experimentações e destinos de computação.
+ Uma nova e mais abrangente de Python <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>.
+ O novo expandido [extensão de CLI do Azure](reference-azure-machine-learning-cli.md) para machine learning.

O [arquitetura](concept-azure-machine-learning-architecture.md) foi reestruturado para facilidade de utilização. Em vez de vários recursos e contas do Azure, precisa apenas de uma [Área de Trabalho do serviço do Azure Machine Learning](concept-azure-machine-learning-architecture.md#workspace). Pode criar áreas de trabalho rapidamente no [portal do Azure](quickstart-get-started.md). Ao utilizar uma área de trabalho, vários usuários podem armazenar a formação e implementação de computação destinos, experiências de modelo, imagens do Docker, modelos implementados e assim por diante.

Apesar de existirem novos clientes CLI e SDK aprimorados na versão atual, o próprio aplicativo da bancada de trabalho de área de trabalho foi preterido. Agora, pode monitorizar as suas experimentações no [dashboard da área de trabalho no portal do Azure](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Utilize o dashboard para obter o histórico de experimentações, gerir os destinos de computação associados à sua área de trabalho, gerir os seus modelos e imagens do Docker e até implementar serviços Web.

## <a name="how-do-i-migrate"></a>Como posso migrar?

A maioria dos artefatos que foram criadas na versão anterior do serviço Azure Machine Learning é armazenada no seu próprio local ou de armazenamento na nuvem. Estes artefactos nunca irão desaparecer. Para migrar, terá de registar novamente os artefactos com o serviço Azure Machine Learning atualizado. Saiba o que pode migrar e como o pode fazer neste [artigo sobre a migração](how-to-migrate.md).

<a name="timeline"></a>

## <a name="support-timeline"></a>Linha cronológica de suporte

Pode continuar a utilizar as suas contas de experimentação do Machine Learning e gestão de modelos e a aplicação de Machine Learning Workbench, depois de Setembro de 2018. Suporte para os seguintes recursos será removido progressivamente de três ou quatro meses depois que a versão. Pode ainda encontrar a documentação relativa às funcionalidades antigas na [secção Recursos](../desktop-workbench/tutorial-classifying-iris-part-1.md), na parte inferior do índice.

|Extinção&nbsp;fase|Detalhes de suporte para funcionalidades anteriores|
|:---:|----------------|
|4 de Dezembro de 2018|Terminou a capacidade de criar contas de experimentação do Azure Machine Learning e gestão de modelos no portal do Azure e a partir da CLI. A capacidade de criar ambientes de computação do Machine Learning a partir da CLI também terminou. Se tiver uma conta existente, a CLI e o ambiente de trabalho do Machine Learning Workbench é continuar a trabalhar nesta fase.|
|9 de Janeiro de 2019|Suporte para todo o resto termina a esta data. Os exemplos são as APIs do restantes e o ambiente de trabalho do Machine Learning Workbench.|

[Comece a migrar](how-to-migrate.md) hoje mesmo. Todas as funcionalidades mais recentes estão disponíveis ao utilizar a nova <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, o [CLI](reference-azure-machine-learning-cli.md)e o [portal](quickstart-get-started.md).

## <a name="what-about-run-histories"></a>E em relação aos históricos de execução?

Históricos de execução estarão acessíveis por algum tempo. Quando estiver pronto para mudar para a versão atualizada do serviço Azure Machine Learning, pode exportar estas históricos de execução, se quiser manter uma cópia.

Históricos de execução são chamados **experimentações** na versão atual. Pode recolher experimentações do seu modelo e explorá-los utilizando o SDK, a CLI ou o portal do Azure.

Dashboard de área de trabalho do portal é suportado nos browsers Microsoft Edge, Chrome e Firefox:

[ ![Online portal](./media/overview-what-happened-to-workbench/image001.png)] (. / media/overview-what-happened-to-workbench/image001.png#lightbox)


## <a name="can-i-still-prep-data"></a>Ainda posso preparar os dados?

Os ficheiros de preparação de dados já existentes não são portáteis na versão mais recente porque não temos Machine Learning Workbench mais. No entanto, pode continuar a preparar os seus dados de modelação.  

Com conjuntos de dados de qualquer tamanho, pode utilizar o [SDK do Azure Machine Learning Data Prep](http://aka.ms/data-prep-sdk) para preparar rapidamente os seus dados antes da modelagem, escrevendo o código de Python. 

Pode seguir [deste tutorial](tutorial-data-prep.md) para saber mais sobre como utilizar o SDK do Azure Machine Learning Data Prep.

## <a name="will-projects-persist"></a>Os projetos serão mantidos?

Não irá perder qualquer código ou trabalho. Na versão mais antiga, os projetos são entidades na cloud com um diretório local. A versão mais recente, anexar diretórios locais para o área de trabalho de serviço do Azure Machine Learning, utilizando um ficheiro de configuração locais. Veja uma [diagrama da arquitetura do mais recente](concept-azure-machine-learning-architecture.md).

Grande parte do conteúdo do projeto já foi no seu computador local. Então, só precisa criar um ficheiro de configuração nesse diretório e referenciá-lo em seu código para ligar à sua área de trabalho. Saiba como [migrar os projetos existentes](how-to-migrate.md#projects).

Saiba como começar a utilizar [Python com o SDK principal](quickstart-create-workspace-with-python.md) ou a utilizar [portal do Azure](quickstart-get-started.md).

## <a name="what-about-my-registered-models-and-images"></a>E o meu modelos registados e imagens?
 
Os modelos que registou no seu registo do modelo antigo devem ser migrados para nova área de trabalho, se quiser continuar a utilizá-los. Para migrar os seus modelos [transferir os modelos e voltar a registar](how-to-migrate.md) na sua nova área de trabalho. 

As imagens que criou no seu registo de imagens antigo têm de ser recriadas na nova área de trabalho para continuar a utilizá-las. Pode recriar essas imagens ao seguir a [configurar e criar a imagem](how-to-deploy-and-where.md#configureimage) secções. 

## <a name="what-about-deployed-web-services"></a>E em relação aos serviços Web implementados?

Os modelos que implementou como serviços da web com a sua conta de gestão de modelos do Machine Learning funcionará, desde que o serviço de contentor do Azure é suportado. Esses serviços web irão funcionar mesmo após o final do suporte para contas de gestão de modelos do Machine Learning. No entanto, quando o suporte para a antiga CLI terminar, terminará também a capacidade para gerir esses serviços Web.

Na versão mais recente, os modelos são implementados como serviços da web para clusters do Azure Container Instances ou do Azure Kubernetes Service (AKS). Também pode implementar para FPGAs e para o Azure IoT Edge. Para obter mais informações, consulte o artigo sobre [como implementar e, em que](how-to-deploy-and-where.md). Sem ter de alterar qualquer um dos seus ficheiros, dependências e esquemas de classificação, pode voltar a implementar de seus modelos com o novo SDK ou da CLI. 

## <a name="what-about-the-old-sdk-and-cli"></a>E quanto a SDK antigo e a CLI?

Sim, eles irá continuar a funcionar até Janeiro. Consulte precedente [linha cronológica](#timeline). Recomendamos que começar a criar os seus modelos e novas experimentações com o SDK mais recente ou a CLI.

Com o SDK de Python de novo na versão mais recente, pode interagir com o serviço Azure Machine Learning em qualquer ambiente de Python. Saiba como instalar a versão o <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> mais recente. Também pode utilizar o atualizada [extensão da CLI do Azure Machine Learning](reference-azure-machine-learning-cli.md) com o conjunto avançado de `az ml` comandos para interagir com o serviço em qualquer ambiente de linha de comando, incluindo o Azure Cloud Shell.

## <a name="what-about-azure-machine-learning-for-visual-studio-code"></a>E o Azure Machine Learning para Visual Studio Code?

Nesta versão mais recente, Azure Machine Learning para Visual Studio Code foi expandido e melhorado para funcionar com os novos recursos anteriores.

[ ![Do azure Machine Learning para Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png)] (. / media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>E em relação aos pacotes de domínios?

O domínio de pacotes para [de imagem digitalizada, análise de texto e previsão](../desktop-workbench/reference-python-package-overview.md) não pode ser utilizado com a versão mais recente do Azure Machine Learning. No entanto, ainda pode criar e preparar a imagem digitalizada, texto e modelos de previsão com o Azure Machine Learning Python mais recente <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>. Para saber como migrar os modelos pré-existentes criados usando-o de imagem digitalizada, análise de texto e previsão de pacotes, contacte [ AML-Packages@microsoft.com ](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [arquitetura da mais recente do serviço Azure Machine Learning](concept-azure-machine-learning-architecture.md). Experimente um dos inícios rápidos ou tutoriais:

* [O que é o serviço Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Início rápido: Criar uma área de trabalho com Python](quickstart-get-started.md)
* [Tutorial: Preparar um modelo](tutorial-train-models-with-aml.md)
