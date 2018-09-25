---
title: Gerir e implementar modelos no Azure Machine Learning
description: Saiba como utilizar o Azure Machine Learning para implementar, gerir e monitorizar os seus modelos para melhorar continuamente. Pode implementar os modelos treinados com o Azure Machine Learning, no seu computador local ou de outras origens.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: hjerez
ms.author: hjerez
ms.date: 09/24/2018
ms.openlocfilehash: 634f33b6d4ed6e272dfb3d1443b0afc63f822d43
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055888"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning"></a>Gerir, implementar e monitorizar os modelos com o Azure Machine Learning

Neste artigo, pode saber como utilizar o Azure Machine Learning para implementar, gerir e monitorizar os seus modelos para melhorar continuamente. Pode implementar os modelos treinados com o Azure Machine Learning, no seu computador local ou de outras origens. 

[!["O azure Machine Learning integração contínua/contínua (CI/CD) ciclo de implantação"](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

## <a name="deployment-workflow"></a>Fluxo de trabalho de implantação

O fluxo de trabalho de implantação consiste em: 
1. **Registe o modelo** num registo alojado na sua área de trabalho do Azure Machine Learning
1. **Registe-se de uma imagem** que emparelha um modelo com um script de classificação e dependências num contêiner portátil 
1. **Implementar** a imagem como um serviço da web na cloud ou para dispositivos de ponta
1. **Monitorização e recolha de dados**

Pode fazer cada passo de forma independente ou como parte de um comando de implementação única. 

O diagrama seguinte ilustra o pipeline de implantação completa:

[![Pipeline de implementação](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

## <a name="step-1-model-registration"></a>Passo 1: Registo de modelo

O registo de modelo mantém um registo de todos os modelos na sua área de trabalho do Azure Machine Learning.
Modelos são identificados pelo nome e versão. Sempre que registar um modelo com o mesmo nome que um já existente, o registro incrementa a versão. Também pode fornecer marcas de metadados adicionais durante o registo que pode ser utilizado ao pesquisar para modelos.

Não é possível eliminar modelos que estão a ser utilizados por uma imagem.

## <a name="step-2-image-registration"></a>Passo 2: Registo de imagem

Imagens permitem a implementação do modelo fiável, juntamente com todos os componentes necessários para usar o modelo. Uma imagem contém os seguintes itens:

* O modelo
* O motor de classificação
* O ficheiro de classificação ou a aplicação
* Todas as dependências necessárias para o modelo de pontuação

A imagem também pode incluir os componentes do SDK para o registo e monitorização. Os dados de registos do SDK podem servir para ajustar ou voltar a preparar seu modelo, incluindo a entrada e saída do modelo.

O Azure Machine Learning suporta as estruturas mais populares, mas em geral pode trabalhar qualquer arquitetura que pode ser o pip instalados.

Em sua área de trabalho foi criada, por isso, foram outros vários outros recursos do Azure utilizados por essa área de trabalho.
Todos os objetos usados para criar a imagem são armazenados na conta de armazenamento do Azure na sua área de trabalho. A imagem é criada e armazenada no Azure Container Registry. Pode fornecer as marcas de metadados adicionais durante a criação de imagem, que também são armazenados pelo registo de imagem e pode ser consultada para localizar a sua imagem.

## <a name="step-3-deployment"></a>Passo 3: implementação

Pode implantar imagens registadas para a cloud ou para dispositivos periféricos. O processo de implantação cria todos os recursos necessários para monitorizar, balanceamento de carga e dimensionamento automático seu modelo. Acesso aos serviços implementados pode ser protegido com autenticação baseada em certificado ao fornecer os recursos de segurança durante a implementação. Também é possível atualizar uma implementação existente para utilizar uma imagem mais recente.

Implementações de serviços da Web também podem ser pesquisadas. Por exemplo, pode procurar todas as implementações de um modelo específico ou a imagem.

Pode implantar suas imagens aos destinos seguintes na cloud:

* Instância de Contentor do Azure
* Serviço Kubernetes do Azure
* Máquinas FPGA do Azure
* Dispositivos do IoT Edge do Azure

[Saiba mais sobre o qual pode implementar](how-to-deploy-and-where.md).

Como o seu serviço é implementado, o pedido de inferência é automaticamente com balanceamento de carga e o cluster for aumentado para satisfazer picos a pedido. Telemetria sobre o seu serviço é capturada no serviço do Azure Application Insights associado à sua área de trabalho.

## <a name="step-4-monitoring-models-and-data-collection"></a>Passo 4: Modelos de monitorização e recolha de dados

Um SDK para captura de registo e dados de modelo estará disponível, para que possa monitorizar a entrada, saída e outros dados relevantes do seu modelo. Os dados são armazenados como um blob na conta de armazenamento do Azure para a área de trabalho.

Para utilizar o SDK com o seu modelo, é possível importar o SDK no seu script de classificação ou a aplicação. Em seguida, pode utilizar o SDK para registos de dados como parâmetros, os resultados ou detalhes de entrada.

Se decidir [ativar a recolha de dados de modelo](how-to-enable-data-collection.md) sempre que implanta a imagem, os detalhes necessários para capturar os dados, como as credenciais para seu armazenamento de BLOBs pessoal, são provisionados automaticamente.

> [!Important]
> Microsoft não vê os dados coletados do seu modelo. Os dados são enviados diretamente para a conta de armazenamento do Azure para a área de trabalho.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [como e onde pode implementar modelos](how-to-deploy-and-where.md) com o serviço Azure Machine Learning.
