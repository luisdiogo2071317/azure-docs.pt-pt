---
title: Descrição geral conceptual da gestão de modelos do Azure Machine Learning | Documentos da Microsoft
description: Este documento explica os conceitos de gestão de modelos do Azure Machine Learning.
services: machine-learning
author: hjerezmsft
ms.author: hjerez
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ROBOTS: NOINDEX
ms.openlocfilehash: d3f7e206e7f4aa61a8ec1272ff2670d81bb7a33e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974685"
---
# <a name="azure-machine-learning-model-management"></a>Gestão de Modelos do Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Gestão de modelos de aprendizagem do Azure Machine permite-lhe gerir e implementar fluxos de trabalho e modelos de machine learning. 

Gestão de modelos fornece capacidades para:
- Controle de versão do modelo
- Modelos de controle na produção
- Implementação de modelos para produção através de ambiente de computação do AzureML com [Azure Container Service](https://azure.microsoft.com/services/container-service/) e [Kubernetes](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- A criação de contentores do Docker com os modelos e testá-los localmente
- Reparametrização do modelo automatizado
- Captura de telemetria de modelo para obter informações acionáveis. 

Gestão de modelo do Azure Machine Learning fornece um registo das versões do modelo. Ele também fornece fluxos de trabalho automatizados para empacotamento e implementação de contentores de aprendizagem automática como REST APIs. Os modelos e suas dependências de tempo de execução são empacotadas num contentor do Docker baseado em Linux com a API de predição. 

Ambientes de computação de aprendizagem de máquina do Azure ajudam a configurar e gerir clusters dimensionáveis para os modelos de hospedagem. O ambiente de computação baseia-se nos serviços de contentor do Azure. O Azure Container Service fornece automática exposição de APIs de Machine Learning, como pontos finais da API de REST com as seguintes funcionalidades:

- Autenticação
- Balanceamento de carga
- Escalamento horizontal automática
- Encriptação

Gestão de modelo do Azure Machine Learning oferece estas capacidades através da CLI, API e o portal do Azure. 

Gestão de modelos do Azure Machine Learning utiliza as seguintes informações:

 - Ficheiro de modelo ou um diretório com os arquivos de modelo
 - Utilizador criado o ficheiro de Python implementar um modelo de função de classificação
 - Ficheiro de dependências de Conda listagem dependências de tempo de execução
 - Escolha de ambiente de tempo de execução, e 
 - Ficheiro de esquema para os parâmetros de API 

Estas informações são utilizadas ao realizar as seguintes ações:

- Registar um modelo
- Criar um manifesto que é utilizado ao criar um contentor
- Criando um Docker imagem de contentor
- Implementar um contentor no Azure Container Service
 
A figura seguinte mostra uma descrição geral de como os modelos são registados e implementados no cluster do. 

![](media/model-management-overview/modelmanagement.png)

## <a name="create-and-manage-models"></a>Criar e gerir modelos 
Pode registrar modelos com gestão de modelos do Azure Machine Learning para controlar as versões do modelo na produção. Para facilitar a capacidade de reprodução e governação, o serviço captura todas as dependências e as informações associadas. Para obter informações mais detalhadas sobre o desempenho, pode capturar a telemetria de modelo com o SDK fornecido. Telemetria de modelo é arquivada no armazenamento fornecidos pelo usuário. A telemetria de modelo pode ser utilizada mais tarde para analisar o desempenho do modelo, reparametrização e obtendo conhecimentos aprofundados para a sua empresa.

## <a name="create-and-manage-manifests"></a>Criar e gerir manifestos 
Modelos de exigem artefatos adicionais para implementar para produção. O sistema fornece a capacidade de criar um manifesto que abrange o modelo, dependências, o script de inferência de tipos (também conhecido como classificação script), dados de exemplo, o esquema etc. Esse manifesto atua como uma receita para criar uma imagem de contentor do Docker. As empresas podem gerar automaticamente manifesto, versões diferentes de criar e gerir o seus manifestos. 

## <a name="create-and-manage-docker-container-images"></a>Criar e gerir imagens de contentor do Docker 
Pode usar o manifesto do passo anterior para criar imagens de contentor Docker com base em seus respectivos ambientes. As imagens em contentores, baseado no Docker fornecem as empresas com a flexibilidade para executar essas imagens nos seguintes ambientes de computação:

- [Kubernetes com a base do Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
- Serviços de contentores no local
- Ambientes de desenvolvimento
- Dispositivos IoT

Estas imagens baseadas no Docker em contentores são autônomas com todas as dependências necessárias necessárias para gerar as previsões de indisponibilidade. 

## <a name="deploy-docker-container-images"></a>Implementar imagens de contentor do Docker 
Com a gestão de modelos do Azure Machine Learning, pode implantar imagens de contentor Docker com base em com um único comando para o Azure Container Service geridos pelo ambiente de computação de ML. Estas implementações são criadas com um servidor front-end que fornece as seguintes funcionalidades:

- Previsões de baixa latência à escala
- Balanceamento de carga
- Dimensionamento automático de pontos de extremidade de ML
- Autorização da chave de API
- Documento de swagger da API

Pode controlar a escala de implementação e a telemetria por meio das seguintes definições de configuração:

- Registo do sistema e telemetria de modelo para cada nível de serviço da web. Se estiver ativada, todos os registos de stdout são transmitidos para [do Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Telemetria de modelo é arquivada no armazenamento que fornece. 
- Limites de dimensionamento automático e simultaneidade. Estas definições aumentam automaticamente o número de contentores implementados com base na carga dentro do cluster existente. Eles também controlam o débito e consistência da latência de predição.

## <a name="consumption"></a>Consumo 
Gestão de modelo do Azure Machine Learning cria a REST API para o modelo implementado juntamente com o documento swagger. Pode consumir modelos implementados ao chamar as APIs de REST com API de chave e modelar entradas para obter as previsões como parte dos aplicativos de linha de negócio. O código de exemplo está disponível no GitHub para linguagens Java, [Python](https://github.com/CortanaAnalyticsGallery-Int/digit-recognition-cnn-tf/blob/master/client.py)e o c# para chamar as APIs REST. O CLI de gestão de modelo do Azure Machine Learning fornece uma forma fácil de trabalhar com essas APIs REST. Pode consumir as APIs com um único comando da CLI, aplicativos ativada no swagger, ou com o curl. 

## <a name="retraining"></a>Reparametrização 
Gestão de modelo do Azure Machine Learning fornece APIs que pode utilizar para voltar a preparar seus modelos. Também pode utilizar as APIs para atualizar as implementações existentes com as versões atualizadas do modelo. Como parte do fluxo de trabalho de ciência de dados, é possível recriar o modelo no seu ambiente de experimentação. Em seguida, registe o modelo com a gestão de modelos e atualizar as implementações existentes. As atualizações são executadas com um único comando CLI de ATUALIZAÇÃO. O comando de ATUALIZAÇÃO atualiza as implementações existentes sem alterar o URL da API ou a chave. Os aplicativos consumir o modelo de continuam a funcionar sem qualquer alteração no código e começar a obter a obter melhores predições com o novo modelo.

O fluxo de trabalho completo, que descrevem esses conceitos é capturado na figura a seguir:

![](media/model-management-overview/modelmanagementworkflow.png)

## <a name="frequently-asked-questions-faq"></a>Perguntas mais frequentes (FAQ) 
- **Que tipos de dados são suportados? Pode transmitir matrizes NumPy diretamente como entrada para o serviço web?**

   Se está a fornecer o ficheiro de esquema que foi criado com generate_schema SDK, em seguida, pode passar NumPy e/ou Pandas DF. Também é possível passar qualquer entradas serializáveis de JSON. Pode passar a imagem como cadeia com codificação binária também.

- **O web service suporte várias entradas ou analisar entradas diferentes?**

   Sim, pode demorar várias entradas empacotadas numa solicitação JSON como um dicionário. Cada entrada seria correspondem a uma chave de único dicionário exclusivo.

- **É a chamada ativada por um pedido para o web service uma chamada de bloqueio ou uma chamada assíncrona?**

   Se o serviço foi criado usando a opção em tempo real como parte da CLI ou API, em seguida, é uma chamada de bloqueio/síncrona. Espera-se para ser rápido em tempo real. Embora no lado do cliente, que pode chamá-lo a utilizar a biblioteca de async HTTP para evitar bloquear o thread de cliente.

- **O número de pedidos pode o serviço web processar em simultâneo?**

   Depende da escala de serviço web e de cluster. Pode aumentar horizontalmente o seu serviço para 100 x de réplicas e, em seguida, ele pode lidar com muitos pedidos em simultâneo. Também pode configurar o máximo do pedido em simultâneo por réplica para aumentar o débito de serviço.

- **O número de pedidos pode enfileirar o serviço web?**

   É configurável. Por predefinição, está definido como 10 ~ por réplica individual, mas pode aumentar/diminuir a ele para os requisitos da aplicação. Normalmente,-lo a aumentar o número de pedidos em fila aumenta o débito de serviço, mas torna o pior de latências em percentis superior. Para manter as latências consistente, poderá querer definir a colocação em fila para um valor baixo (1 a 5) e aumentar o número de réplicas para lidar com a taxa de transferência. Também pode ativar o dimensionamento automático para fazer com que o número de réplicas ajustar automaticamente com base na carga. 

- **A mesma máquina ou o cluster pode ser utilizado para vários pontos de extremidade de serviço web?**

   Com certeza. Pode executar 100 x de serviços/pontos finais no mesmo cluster. 

## <a name="next-steps"></a>Passos Seguintes
Para começar a trabalhar com a gestão de modelos, consulte [configurar a gestão de modelo](deployment-setup-configuration.md).
