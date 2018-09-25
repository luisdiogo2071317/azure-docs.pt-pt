---
title: Onde pretende implementar modelos com o serviço Azure Machine Learning | Documentos da Microsoft
description: Saiba mais sobre as diferentes formas que pode implementar os seus modelos na produção com o serviço Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/29/2018
ms.openlocfilehash: 606e8aed42bce69d5b3210b4e97f8cbfeaaf104c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961013"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementar modelos com o serviço Azure Machine Learning

O serviço Azure Machine Learning fornece várias formas que pode implementar o modelo preparado. Neste documento, saiba como implementar o seu modelo como um serviço web na cloud do Azure ou para dispositivos do IoT edge.

Pode implementar modelos para os seguintes destinos de computação:

- Azure Container Instances (ACI)
- Serviço de Kubernetes do Azure (AKS)
- Azure IoT Edge
- Matriz de porta de campos programáveis (FPGA)

O restante deste documento fala sobre cada uma destas opções em detalhes.

## <a name="azure-container-instances"></a>Azure Container Instances

Utilização do Azure Container Instances para implantar seus modelos como um ponto de final de REST API, se um ou mais das seguintes condições for verdadeiro:
- Está à procura de uma implementação rápida para pontuar e validar o seu modelo. Implementação do ACI normalmente estiverem concluída em menos de 5 minutos.
- Pretende implementar o seu modelo no desenvolvimento de um ou ambiente de teste. ACI permite-lhe implementar 20 grupos de contentores por subscrição. Para obter mais informações, consulte a [Quotas e disponibilidade das regiões do Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) documento.

Para obter mais informações, consulte a [implementar um modelo no Azure Container Instances](how-to-deploy-to-aci.md) documento.

## <a name="azure-kubernetes-service"></a>Serviço Kubernetes do Azure

Para cenários de produção de grande escala, pode implementar o seu modelo para o Azure Kubernetes Service (AKS). Pode utilizar um cluster do AKS existente ou crie um novo com o SDK do Azure Machine Learning, a CLI ou o portal do Azure.

AKS a criar o cluster é um processo de tempo para a área de trabalho. Depois de criado, pode reutilizar este cluster para várias implementações. Se eliminar o cluster ou o grupo de recursos que o contém, em seguida, tem de criar um novo cluster da próxima vez que precisa implantar.

Implementar no AKS fornece o dimensionamento automático, registo, a recolha de dados do modelo e tempos de resposta rápidos para os seus serviços web. 

O processo de criação de um cluster do AKS demora cerca de 20 minutos.

Para obter mais informações, consulte a [implementar um modelo de serviço de Kubernetes do Azure](how-to-deploy-to-aks.md) documento.

## <a name="azure-iot-edge"></a>Azure IoT Edge

Com dispositivos de IoT, é mais rápido executar no dispositivo em vez de enviar dados para a cloud e à espera de um modelo de alojamento na cloud para retornar dados de classificação. Com o Azure IoT Edge, pode alojar o seu modelo em dispositivos periféricos. Implemente o seu modelo para o IoT Edge se precisar de um ou mais das seguintes capacidades:
- Lidar com tarefas de prioridade localmente, mesmo sem uma ligação de cloud
- Trabalhar com dados gerados, que é demasiado grandes para extrair rapidamente a partir da cloud
- Ativar o processamento em tempo real através de informações em ou próximo dispositivos locais
- Acomodar os requisitos relacionados com a privacidade de dados 

Para obter mais informações, consulte a [implementar no Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) documento.

Para obter mais informações sobre o serviço de IoT Edge, veja a [documentação do Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/).


## <a name="field-programmable-gate-arrays-fpga"></a>Matrizes de porta de campos programáveis (FPGA)

Hardware aceleradas modelos com tecnologia do Project Brainwave tornam possível obter a latência ultrabaixa para pedidos de inferência em tempo real. Project Brainwave acelera redes neurais profundas (DNN) implementadas nas matrizes de porta de campos programáveis na cloud do Azure. Normalmente utilizado o DNNs estão disponíveis como featurizers para aprendizagem de transferência ou personalizável com pesos preparado a partir de seus próprios dados.

Para obter mais informações, consulte a [implementar para um FPGA](how-to-deploy-fpga-web-service.md) documento.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como implementar um modelo para um destino de computação específicos, consulte os documentos seguintes:

* [Implementar um modelo no Azure Container Instances](how-to-deploy-to-aci.md)
* [Implementar um modelo de serviço de Kubernetes do Azure](how-to-deploy-to-aks.md)
* [Implementar um modelo para o Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning)
* [Implementar um modelo para FPGA](how-to-deploy-fpga-web-service.md)
