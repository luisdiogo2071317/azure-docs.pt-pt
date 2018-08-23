---
title: Classificação de receitas - processo de ciência de dados de equipa - Azure Machine Learning | Documentos da Microsoft
description: Como utilizar o modelo de processo de ciência de dados de agrupamento para criar um projeto no Azure Machine Learning que classifica incomes E.U.A.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.openlocfilehash: edc3fc5e2a625a14bcb48b03f32cd99069a0ad53
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "42057108"
---
# <a name="income-classification-with-team-data-science-process-tdsp-project"></a>Classificação de receitas com o projeto do Team Data Science Process (TDSP)

## <a name="introduction"></a>Introdução

Padronização da estrutura e a documentação da ciência de dados de projetos, ou seja ancorada a uma estabelecido [ciclo de vida de ciência de dados](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), é fundamental para promovendo a colaboração eficaz em equipes de ciência de dados. Criação de projetos de Azure Machine Learning com o [Team Data Science Process (TDSP)](https://github.com/Azure/Microsoft-TDSP) modelo fornece uma estrutura para tal padronização.

Tinha anteriormente lançámos um [repositório do GitHub para a estrutura do projeto TDSP e os modelos](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Mas não foi possível, até agora instanciar a estrutura TDSP e modelos de dentro de uma ferramenta de ciência de dados. Agora ativámos a criação de projetos de Azure Machine Learning, que são instanciadas com [modelos de estrutura e a documentação do TDSP para o Azure Machine Learning](https://github.com/amlsamples/tdsp). São fornecidas instruções sobre como utilizar a estrutura do TDSP e modelos no Azure Machine Learning [aqui](https://aka.ms/how-to-use-tdsp-in-aml). Aqui fornecemos um exemplo de como um projeto de aprendizagem real pode ser criado com a estrutura do TDSP, preenchido com código específico do projeto, artefatos e documentos e executado no Azure Machine Learning.

## <a name="link-to-github-repository"></a>Ligar para o repositório do GitHub
Fornecemos documentação resumida [aqui](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) sobre o exemplo. Mais extensa documentação pode ser encontrada no site do GitHub.

### <a name="purpose"></a>Objetivo
A principal finalidade deste exemplo é mostrar como criar uma instância e executar um projeto com de aprendizagem automática da [Team Data Science Process (TDSP)](https://github.com/Azure/Microsoft-TDSP) estrutura e modelos no Azure Machine Learning. Para essa finalidade, podemos usar o conhecido [dados de censo-nos de 1994 do repositório de Aprendizado de máquina de UCI](https://archive.ics.uci.edu/ml/datasets/adult). A tarefa de modelagem consiste em prever classes rendimento anuais de US das informações de censo de nós (por exemplo, idade, corrida, nível de formação, país de origem, etc.)

### <a name="scope"></a>Âmbito
 * Exploração de dados, formação e implementação de um modelo de aprendizagem automática que resolva o problema de predição descrito na descrição geral de caso de utilização. 
 * Execução do projeto no Azure Machine Learning com o modelo de processo de ciência de dados de equipa (TDSP) do Azure Machine Learning para este projeto. Para a execução do projeto e relatórios, vamos utilizar o ciclo de vida do TDSP.
 * Operacionalização da solução diretamente a partir do Azure Machine Learning no Azure Container Service.

 O projeto destaca vários recursos do Azure Machine Learning, tais Instanciação de estrutura do TDSP e a utilização, a execução de código em blocos de notas do Jupyter, bem como arquivos de Python e fácil operacionalização nos serviços de contentor do Azure utilizando o Docker e Kubernetes.


## <a name="team-data-science-process-tdsp-lifecycle"></a>O ciclo de vida do Team Data Science Process (TDSP)
Consulte [da Equipe do ciclo de vida de processo (TDSP) de ciência de dados](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)

![](./media/scenario-tdsp-classifying-us-incomes/tdsp-lifecycle.jpg)

## <a name="prerequisites"></a>Pré-requisitos
### <a name="required-subscription-hardware-software"></a>Obrigatório: subscrição, hardware, software
1. Do Azure [subscrição](https://azure.microsoft.com). Pode obter um [assinatura gratuita da](https://azure.microsoft.com/free/?v=17.16&WT.srch=1&WT.mc_id=AID559320_SEM_cZGgGOIg) para executar este exemplo também.
2. Uma [Máquina Virtual de ciência de dados (DSVM) do Azure Windows Server 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.windows-data-science-vm), (tamanho da VM: [DS3_V2](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), com 4 CPUs virtuais e 14 Gb de RAM). Embora testado numa DSVM do Azure, é muito provável que funcione em qualquer computador Windows 10.
3. Reveja a documentação sobre o Azure Machine Learning e o respetivo relacionadas a serviços (veja abaixo para obter ligações).
4. Certifique-se de que instalou corretamente do Azure Machine Learning, o [guia de início rápido de instalação](../service/quickstart-installation.md).

O conjunto de dados para este exemplo é a partir do repositório de ML UCI [[link]](https://archive.ics.uci.edu/ml/datasets/adult). Ele é retirado da base de dados de censo nos 1994 e contém informações de censo e receitas para cerca de 50 000 indivíduos. Este é o conjunto de dados estruturado ter numéricos e categóricos recursos e um destino categórico consiste em duas categorias de renda ("> 50k ' ou ' < = 50 mil '). 

### <a name="optional-version-control-repository"></a>Opcional: Repositório de controle de versão
Se gostaria de guardar e versão do seu projeto e o respetivo conteúdo, tem de ter um repositório de controle de versão em que isso pode ser feito. Pode introduzir a localização do repositório de Git ao criar o novo projeto usando o modelo TDSP no Azure Machine Learning. Ver [como utilizar o Git no Azure Machine Learning](using-git-ml-project.md) para obter mais detalhes.

### <a name="informational-about-azure-machine-learning"></a>Informativo: Sobre o Azure Machine Learning
* [FAQ - como começar](frequently-asked-questions.md)
* [Descrição geral](../service/overview-what-is-azure-ml.md)
* [Instalação](../service/quickstart-installation.md)
* [Execução](experimentation-service-configuration.md)
* [Utilizar o TDSP](https://aka.ms/how-to-use-tdsp-in-aml)
* [Ler e escrever ficheiros](how-to-read-write-files.md)
* [Using Git with Azure Machine Learning](using-git-ml-project.md) (Utilizar o Git com o Azure Machine Learning)
* [Implementar um modelo de ML como um serviço web](model-management-service-deploy.md)

### <a name="create-a-new-workbench-project"></a>Criar um novo projeto do workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abrir o Azure Machine Learning Workbench
2.  Sobre o **projetos** página, clique no **+** iniciar sessão e selecionar **novo projeto**
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Incomes classificar dos E.U.A. - projeto de TDSP" e selecione o modelo
5.  Clique em **Criar**.

Se fornecer uma localização do repositório de Git vazia durante a criação de projeto (na caixa apropriada,), em seguida, esse repositório será preenchido com a estrutura do projeto e o conteúdo após a criação do projeto.

## <a name="use-case-overview"></a>Descrição geral de caso de utilização
O problema é compreender como socio econômico dados capturados no recenseamento nos podem ajudar a prever o rendimento anual de indivíduos na região E.U.A. Com base em tais recursos de censo, a tarefa de aprendizagem da máquina é prever se o rendimento do indivíduo for superior a US $ 50 000 ou não (tarefa de classificação binária).

## <a name="data-description"></a>Descrição de dados
Para obter informações detalhadas sobre os dados, consulte a [Descrição](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names) no repositório de UCI. 

Estes dados foram extraídos da base de dados Census Bureau encontrado em: https://www.census.gov/en.html. 


* Há um total de 48,842 instâncias (antes de qualquer filtragem), misturar de contínuos e discretos (treinar = 32, 561, testar = 16, 281)
* Probabilidade para a etiqueta "> 50k ': 23.93% / % de 24.78 (sem incertezas)
* Probabilidade para a etiqueta "< = 50 mil ': 76.07% / % de 75.22 (sem incertezas)  

* **DESTINO**: classe rendimento ' > 50k ', ' < = 50 mil '. Estes são substituídas por 1 e 0 respetivamente na fase de preparação de dados.
* **FUNCIONALIDADES**: idade, a classe de trabalho, nível de formação, nível de formação, corrida, sexo, horas de trabalho por semana, etc.


## <a name="project-structure-execution-and-reporting"></a>Estrutura do projeto, execução e relatórios

### <a name="structure"></a>estrutura
Para este projeto, usamos os modelos de estrutura e a documentação de pasta TDSP (abaixo), que segue a [ciclo de vida do TDSP](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). 

Projeto é criado com base nas instruções fornecidas [aqui](https://aka.ms/how-to-use-tdsp-in-aml). Depois de é preenchido com código e artefatos do projeto, a estrutura é semelhante à seguinte (veja a estrutura do projeto demarcada em vermelho na figura abaixo).


<img src="./media/scenario-tdsp-classifying-us-incomes/instantiation-4.png" width="900" height="700">

### <a name="execution"></a>Execução
Neste exemplo, podemos executar o código na **ambiente de computação local**. Consulte a documentos do Azure Machine Learning para obter mais detalhes sobre [opções de execução](experimentation-service-configuration.md).

Executar um script de Python num tempo de execução local do Python é fácil:

    az ml experiment submit -c local my_script.py

Ficheiros de bloco de notas IPython podem ser double-clicked da estrutura de projeto à esquerda da interface do Usuário do Azure Machine Learning e executar no servidor de bloco de notas Jypyter.


O fluxo de trabalho de ciência de dados passo a passo foi da seguinte forma:

* [**Aquisição de dados e compreensão**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/01_data_acquisition_and_understanding)

Dados foi transferidos no formato. csv a partir de URLs no repositório de ML de UCI [[link]](https://archive.ics.uci.edu/ml/datasets/adult). Recursos de destino e seus transformações são descritas em detalhe no ficheiro ProjectReport.md.

Código de aquisição de dados e compreensão está localizado em: / código/01_data_acquisition_and_understanding.

Exploração de dados é realizada com o Python 3 [utilitário IDEAR (exploração interativa de dados e relatórios)](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils/Python) publicado como parte da [suite TDSP das ferramentas de ciência de dados](https://github.com/Azure/Azure-TDSP-Utilities). Esse utilitário ajuda a gerar relatórios de exploração de dados padronizado para dados que contenham recursos numéricos e categóricos e de destino. Detalhes de como o utilitário de Python 3 IDEAR foi usado é fornecido abaixo. 

A localização do relatório de exploração de dados final é [IDEAR.html](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/docs/deliverable_docs). Uma vista do relatório IDEAR é mostrada abaixo:

![](./media/scenario-tdsp-classifying-us-incomes/idear.png)

* [**Modelagem**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/02_modeling)

Criamos dois modelos com 3-fold validação cruzada: floresta elástico Net e Random. Usamos [amostragem de ponto de 59](http://www.jmlr.org/papers/volume13/bergstra12a/bergstra12a.pdf) para pesquisa de grade aleatórios como uma estratégia para a otimização de parâmetro de validação cruzada e modelos. A precisão dos modelos foram medidos com AUC (área em curva) no conjunto de dados de teste. 

Código para modelagem encontra-se em: / código/02_modeling.

AUC elástico Net e modelos de floresta de Random foi > 0.85. Vamos salvar ambos os modelos no pickled.pkl ficheiros e o ROC multiclasse para ambos os modelos de saída. Modelo de floresta AUC aleatório foi 0.92 e que o modelo de rede elástico foi 0,90. Além disso, para a interpretação de modelo, importância de funcionalidade para o modelo de floresta de Random são de saída num ficheiro. csv e desenhados num pdf (20 melhores recursos de previsão apenas).

Curva cor MULTICLASSE dos **modelo de floresta de Random** em teste dados são mostrados abaixo. Isso foi o modelo foi implementado:

![](./media/scenario-tdsp-classifying-us-incomes/rf-auc.png)

A importância de funcionalidade (os 20) do modelo de floresta de Random é mostrada abaixo. Ele mostra a quantidade de capital ganho de recursos, eduction, estado civil, dispõe de maior importância de funcionalidade.

![](./media/scenario-tdsp-classifying-us-incomes/featImportance.png)

* [**Implementação**](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/tree/master/code/03_deployment)

Implementar o modelo de floresta de Random como um serviço web num cluster no [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). O ambiente de operacionalização Aprovisiona o Docker e Kubernetes no cluster para gerenciar a implantação de serviço web. Pode encontrar mais informações sobre o processo de operacionalização [aqui](model-management-service-deploy.md). 

Código para implementação encontra-se em: / código/03_deployment.


### <a name="final-project-reporthttpsgithubcomazuremachinelearningsamples-tdspuciadultincomeblobmasterdocsdeliverabledocsprojectreportmd"></a>[Relatório de projeto final](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md)
Detalhes sobre cada uma das seções acima são fornecidas no relatório final do projeto compilados [ProjectReport](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome/blob/master/docs/deliverable_docs/ProjectReport.md). O relatório de projeto também contém mais detalhes sobre o caso de utilização, métricas de desempenho do modelo, implementação e infraestrutura em que o projeto foi desenvolvido e implementado.

O relatório de projeto, juntamente com o conteúdo da pasta de projeto inteiro e versão do repositório de controle pode ser entregue ao cliente.


## <a name="conclusion"></a>Conclusão

Neste exemplo, mostramos agora para utilizar a estrutura do TDSP e modelos no Azure Machine Learning. Por meio de modelos de documento e artefactos, pode:
1. Definir corretamente o objetivo e o âmbito de um projeto
2. Criar uma equipe de projeto com distribuído de funções e responsabilidades
3. Estruturar e executar projetos, de acordo com as fases do ciclo de vida TDSP
4. Desenvolva relatórios padronizados usar TDSP utilitários de ciência de dados (por exemplo, o IDEAR exploração e visualização de relatório de dados).
5. Prepare um relatório de projeto de ciência de dados final que pode ser entregue a um cliente

Esperamos que usar esse recurso do Azure Machine Learning para facilitar a padronização e colaboração de suas equipes de ciência de dados.

## <a name="next-steps"></a>Próximos Passos

Consulte referências abaixo para começar a utilizar:

[Como utilizar o processo de ciência de dados de equipa (TDSP) no Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)

[Processo de ciência de dados de equipa (TDSP)](https://github.com/Azure/Microsoft-TDSP)

[Modelo de projeto TDSP para o Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)

[E.U.A. rendimento conjunto de dados de repositório de UCI ML](https://archive.ics.uci.edu/ml/datasets/adult)