---
title: Como funciona o serviço Azure Machine Learning?
description: Saiba mais sobre a arquitetura, terminologia e conceitos que constituem o serviço Azure Machine Learning. Também Saiba mais sobre o fluxo de trabalho geral de usar o serviço e os serviços do Azure que são utilizados pelo serviço Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: larryfr
ms.date: 10/24/2018
ms.openlocfilehash: b00f72c987b6ce8c44796bd036af670ec39fa7a6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093637"
---
# <a name="how-the-azure-machine-learning-service-works-architecture-and-concepts"></a>Como funciona o serviço Azure Machine Learning: conceitos e arquitetura

Este documento descreve a arquitetura e conceitos para o serviço Azure Machine Learning. O diagrama seguinte mostra os principais componentes do serviço e ilustra o fluxo de trabalho geral ao utilizar o serviço: 

[![Arquitetura do serviço Machine Learning e fluxo de trabalho do Azure](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

O fluxo de trabalho em geral, segue estas etapas:

1. Desenvolver a aprendizagem, treinamento scripts no __Python__.
1. Criar e configurar uma __destino de computação__.
1. __Submeter os scripts__ para o destino de computação configurada para ser executado nesse ambiente. Durante o treinamento, o destino de computação armazena registos de execução para um __arquivo de dados__. Existem os registos são salvas num __experimentar__.
1. __Consultar a experimentação__ para as métricas registadas de execuções a atuais e anteriores. Se as métricas não indicam um resultado desejado, fazer um loop novamente para o passo 1 e iterar em seus scripts.
1. Depois de uma execução satisfatória for encontrada, registe o modelo persistente na __registo de modelo__.
1. Desenvolva um script de classificação.
1. __Criar uma imagem__ e registe-na __registo de imagem__. 
1. __Implementar a imagem__ como uma __serviço web__ no Azure.


[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

> [!NOTE]
> Embora este documento define os termos e conceitos utilizados pelo Azure Machine Learning, não definir termos e conceitos para a plataforma Azure. Para obter mais informações sobre a terminologia da plataforma do Azure, consulte a [Glossário do Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Área de trabalho

A área de trabalho é o recurso de nível superior para o serviço Azure Machine Learning. Ele fornece um local centralizado para trabalhar com todos os artefactos que cria ao utilizar o serviço do Azure Machine Learning.

A área de trabalho mantém uma lista de destinos de computação que pode ser usada para preparar o seu modelo. Além disso, mantém um histórico de execuções de preparação, incluindo registos, métricas, saída e um instantâneo dos seus scripts. Estas informações são utilizadas para determinar qual execução de treinamento produz o melhor modelo.

Modelos são registrados com a área de trabalho. Um modelo registado e scripts de classificação são utilizados para criar uma imagem. A imagem pode, em seguida, ser implementada no Azure Container Instances, Azure Kubernetes Service, ou a uma matriz de porta de campos programáveis (FPGA) como um ponto de final HTTP baseado em REST. Também pode ser implementado para um dispositivo Azure IoT Edge como um módulo.

Pode criar várias áreas de trabalho, e cada área de trabalho pode ser partilhada por várias pessoas. Ao partilhar uma área de trabalho, controle o acesso à área de trabalho ao atribuir as seguintes funções para os utilizadores:

* Proprietário
* Contribuinte
* Leitor

Quando cria uma nova área de trabalho, este cria automaticamente vários recursos do Azure que são utilizados pela área de trabalho:

* [O Azure Container Registry](https://azure.microsoft.com/services/container-registry/) -regista os contentores do docker que são utilizados durante o treinamento e, ao implementar um modelo.
* [O armazenamento do Azure](https://azure.microsoft.com/services/storage/) - utilizado como o arquivo de dados predefinido para a área de trabalho.
* [O Azure Application Insights](https://azure.microsoft.com/services/application-insights/) - armazena informações sobre os modelos de monitorização.
* [O Azure Key Vault](https://azure.microsoft.com/services/key-vault/) - segredos de arquivos utilizados pelos destinos de computação e outras informações confidenciais necessários para a área de trabalho.

> [!NOTE]
> Em vez de criar novas versões, também pode utilizar os serviços do Azure existentes. 

O diagrama seguinte é uma taxonomia da área de trabalho:

[![Taxonomia da área de trabalho](./media/concept-azure-machine-learning-architecture/taxonomy.png)](./media/concept-azure-machine-learning-architecture/taxonomy.png#lightbox)

## <a name="model"></a>Modelo

Em sua forma mais simples, um modelo é um trecho de código que usa entrada e produz um resultado. A criação de um modelo de aprendizagem automática envolve selecionar um algoritmo, fornecendo dados e ajuste hiperparâmetros. O treinamento é um processo iterativo que produz um modelo preparado, que encapsula o que o modelo aprendidas durante o processo de treinamento.

Um modelo é produzido por uma execução no Azure Machine Learning. Também pode utilizar um modelo treinado fora do Azure Machine Learning. Um modelo pode ser registrado numa área de trabalho do serviço do Azure Machine Learning.

Serviço do Azure Machine Learning é agnóstico quanto a estrutura. Pode utilizar qualquer estrutura de aprendizagem populares durante a criação de um modelo, como scikit-saiba, xgboost, PyTorch, TensorFlow, Chainer e CNTK.

Para obter um exemplo de preparar um modelo, consulte a [início rápido: criar um área de trabalho do serviço de aprendizagem automática](quickstart-get-started.md) documento.

### <a name="model-registry"></a>Registo de modelo

O registo de modelo mantém um registo de todos os modelos na sua área de trabalho do serviço do Azure Machine Learning. 

Modelos são identificados pelo nome e versão. Sempre que registar um modelo com o mesmo nome que um já existente, o registro parte do princípio de que é uma nova versão. A versão é incrementada e o novo modelo é registrado com o nome.

Pode fornecer marcas de metadados adicionais quando registar o modelo e, em seguida, utilizar estas etiquetas ao procurar modelos.

Não é possível eliminar modelos que estão a ser utilizados por uma imagem.

Para obter um exemplo de um modelo de registo, consulte a [preparar um modelo de classificação de imagem com o Azure Machine Learning](tutorial-train-models-with-aml.md) documento.

## <a name="image"></a>Imagem

Imagens fornecem uma forma de forma fiável implementar um modelo, juntamente com todos os componentes necessários para usar o modelo. Uma imagem contém os seguintes itens:

* Um modelo.
* Um script de classificação ou um aplicativo. Este script é utilizado para passar a entrada para o modelo e devolver o resultado do modelo.
* Dependências necessárias para o modelo ou a classificação/aplicação de scripts.  Por exemplo, pode incluir um ficheiro de ambiente de Conda que lista as dependências de pacote do Python.

Existem dois tipos de imagens que podem ser criadas por Azure Machine Learning:

* Imagem FPGA: utilizada na implementação a uma matriz de porta de campos programáveis na cloud do Azure.
* Imagem do docker: utilizada na implementação para destinos que não seja FPGA de computação. Por exemplo, o Azure Container Instances e o Azure Kubernetes Service.

Para obter um exemplo de criação de uma imagem, consulte a [implementar um modelo de classificação de imagem na instância de contentor do Azure](tutorial-deploy-models-with-aml.md) documento.

### <a name="image-registry"></a>Registo de imagem

O registo de imagem mantém um registo de imagens criadas a partir de seus modelos. Pode fornecer as marcas de metadados adicionais ao criar a imagem. Marcas de metadados são armazenadas pelo registo de imagem e podem ser consultadas para localizar a sua imagem.

## <a name="deployment"></a>Implementação

Uma implementação é uma instanciação de sua imagem em qualquer um de um serviço da Web que pode estar alojado na cloud ou um módulo de IoT para Implantações de dispositivos integrada. 

### <a name="web-service"></a>Serviço Web

Um serviço web implementado pode utilizar o Azure Container Instances, Azure Kubernetes Service ou matrizes de porta de campos programáveis (FPGA).
O serviço é criado a partir de uma imagem que encapsula o seu modelo, com scripts e ficheiros associados. A imagem tem um ponto final do HTTP com balanceamento de carga, recebe a classificação pedidos enviados para o serviço web.

Azure ajuda-o a monitorizar a implementação do serviço Web através da recolha de telemetria do Application Insight e/ou de telemetria de modelo se tiver escolhido para ativar esta funcionalidade. Os dados de telemetria só está acessíveis para e armazenados no Application Insights e instâncias de conta de armazenamento.

Se tiver ativado o dimensionamento automático, o Azure irá dimensionar automaticamente a implementação.

Para obter um exemplo de implementação de um modelo como um serviço web, consulte a [implementar um modelo de classificação de imagem na instância de contentor do Azure](tutorial-deploy-models-with-aml.md) documento.

### <a name="iot-module"></a>Módulo de IoT

Um módulo de IoT implementado é um contentor de Docker que inclui o seu modelo e script associado ou aplicação e dependências adicionais. Estes módulos são implementados com o Azure IoT Edge em dispositivos periféricos. 

Se tiver ativado a monitorização, o Azure recolhe dados de telemetria do modelo dentro do módulo do Azure IoT Edge. Os dados de telemetria só está acessíveis para e armazenados na sua instância de conta de armazenamento.

O Azure IoT Edge irá garantir que seu módulo está a executar e monitorizar o dispositivo que está hospedando.

## <a name="datastore"></a>Arquivo de dados

Um arquivo de dados é uma abstração de armazenamento através de uma conta de armazenamento do Azure. O arquivo de dados pode utilizar um contentor de Blobs do Azure ou uma partilha de ficheiros do Azure, como o armazenamento de back-end. Cada área de trabalho tem um arquivo de dados padrão, e pode registrar arquivos de dados adicionais. 

Utilize a API do SDK de Python ou da CLI do Azure Machine Learning para armazenar e recuperar ficheiros a partir do arquivo de dados. 

## <a name="run"></a>Executar

Uma execução é um registo que contém as seguintes informações:

* Metadados sobre a execução (timestamp, duração etc.)
* Métricas registadas pelo seu script
* Ficheiros de saída recolhidas automaticamente pela experimentação ou explicitamente carregados por si.
* Um instantâneo do diretório que contém os scripts, antes da execução

Uma execução é produzida quando submete um script para preparar um modelo. Uma execução pode ter zero ou mais execuções de subordinados. Para que a execução de nível superior poderá ter duas execuções de subordinados, cada um dos quais pode ter seus próprios subordinado é executado.

Para um exemplo de visualização é executado produzido por preparar um modelo, consulte a [início rápido: introdução ao serviço Azure Machine Learning](quickstart-get-started.md) documento.

## <a name="experiment"></a>Experimentação

Uma experimentação é um agrupamento de várias execuções de um determinado script. Sempre pertence a uma área de trabalho. Quando submete uma execução, fornece um nome de experimentação. Informações para a execução são armazenadas desse experimento. Se submeter uma execução e especifique um nome de experimentação que não existe, é automaticamente criada uma nova experimentação com esse nome.

Para obter um exemplo da utilização de uma experimentação, consulte a [início rápido: introdução ao serviço Azure Machine Learning](quickstart-get-started.md) documento.


## <a name="pipelines"></a>Pipelines

Pipelines são utilizados para criar e gerir os fluxos de trabalho que fases de aprendizagem automática em conjunto inserir manualmente (ML), como a preparação de dados, preparação de modelos, implementação do modelo e inferência. Cada fase pode incluir vários passos, cada um dos quais pode ser executado automaticamente em vários destinos de computação.

Para obter mais informações sobre o machine learning pipelines com este serviço, consulte o artigo [Pipelines e o Azure Machine Learning](concept-ml-pipelines.md).

## <a name="compute-target"></a>Destino de computação

Um destino de computação é o recurso de computação utilizado para executar o script de treinamento ou alojar a sua implementação do serviço web. Os destinos de computação suportados são: 

* Seu computador local
* Uma VM do Linux no Azure (por exemplo, a Máquina Virtual de ciência de dados)
* Cluster do Azure Batch AI
* Apache Spark para HDInsight
* Instância de Contentor do Azure
* Serviço Kubernetes do Azure

Destinos de computação são anexados a uma área de trabalho. Destinos que não seja o computador local são partilhados por utilizadores da área de trabalho de computação.

A maioria de computação destinos podem ser criados diretamente através da área de trabalho ao utilizar o portal do Azure, o SDK do Azure Machine Learning ou a CLI do Azure. Se tiver de destinos de computação que foram criados por outro processo (por exemplo, o portal do Azure ou a CLI do Azure), pode adicionar (anexar)-os para a área de trabalho. Alguns de computação destinos devem ser criados fora da área de trabalho e, em seguida, anexados.

Para obter informações sobre como selecionar um destino de computação para a formação, consulte a [selecionar e utilizar um destino de computação para preparar o seu modelo](how-to-set-up-training-targets.md) documento.

Para obter informações sobre como selecionar um destino de computação para a implementação, consulte a [implementar modelos com o serviço Azure Machine Learning](how-to-deploy-and-where.md) documento.

## <a name="run-configuration"></a>Configuração de execução

Uma configuração de execução é um conjunto de instruções que define como executar um script num destino de computação determinado. Ele inclui uma grande variedade de definições de comportamento, por exemplo, se pretende utilizar um ambiente de Python existente ou utilizar um ambiente de Conda criado a partir de especificação.

Uma configuração de execução pode ser mantida num arquivo dentro do diretório que contém o script de treinamento, ou construída como um objeto na memória e utilizado para submeter uma execução.

Por exemplo, executar configurações, consulte a [selecionar e utilizar um destino de computação para preparar o seu modelo](how-to-set-up-training-targets.md) documento.

## <a name="training-script"></a>Script de treinamento

Para preparar um modelo, especifique o diretório que contém o script de formação e os ficheiros associados. Também especificar um nome de experimentação, o que é utilizado para armazenar as informações recolhidas durante o treinamento. Durante o treinamento, o diretório é copiado para o ambiente de treinamento (destino de computação) e o script especificado na configuração da execução é iniciado. Um instantâneo do diretório também é armazenado abaixo a experimentação na área de trabalho.

Para obter um exemplo do uso de scripts para preparar um modelo, consulte [criar uma área de trabalho com Python](quickstart-get-started.md)

## <a name="logging"></a>Registo

Ao desenvolver a sua solução, utilize o SDK de Python do Azure Machine Learning no seu script de Python para registar o métricas arbitrárias. Após a execução, consulte as métricas para determinar se a execução produziu o modelo que pretende implementar. 

## <a name="snapshot"></a>Instantâneo

Quando submeter uma execução, o Azure Machine Learning compacta o diretório que contém o script como um ficheiro zip e envia-os para o destino de computação. O zip, em seguida, é expandido e o script é executado lá. O Azure Machine Learning também armazena o ficheiro zip como um instantâneo como parte do registo de execução. Qualquer pessoa com acesso à área de trabalho pode procurar um registo de execução e transferir o instantâneo.

## <a name="activity"></a>Atividade

Uma atividade representa uma operação de longa execução. As seguintes operações são exemplos de atividades:

* Criar ou eliminar um destino de computação
* Executar um script num destino de computação

Atividades podem fornecer notificações através da SDK ou a IU da Web, pelo que pode monitorizar facilmente o progresso de uma destas operações.

## <a name="next-steps"></a>Passos Seguintes

Utilize as seguintes ligações para começar a utilizar o Azure Machine Learning:

* [O que é o serviço Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Guia de Início Rápido: criar uma área de trabalho com Python](quickstart-get-started.md)
* [Tutorial: preparar um modelo](tutorial-train-models-with-aml.md)
