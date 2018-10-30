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
ms.openlocfilehash: 97ac405db3de4fa2c6f1173f813eafd41a5361ad
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209450"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implementar modelos com o serviço Azure Machine Learning

O serviço Azure Machine Learning fornece várias formas que pode implementar o modelo preparado. Neste documento, saiba como implementar o seu modelo como um serviço web na cloud do Azure ou para dispositivos do IoT edge.

Pode implementar modelos para os seguintes destinos de computação:

- [O Azure Container Instances (ACI)](#aci): rápida de implementação. Bom para desenvolvimento ou teste.
- [O Azure Kubernetes Service (AKS)](#aks): boa para implementações de produção de grande escala. Fornece o dimensionamento automático e tempos de resposta rápidos.
- [O Azure IoT Edge](#iotedge): implementar modelos em dispositivos IoT. Inferência acontece no dispositivo.
- [Matriz de porta de campos programáveis (FPGA)](#fpga): latência ultrabaixa para inferência em tempo real.

O restante deste documento fala sobre cada uma destas opções em detalhes.

## <a id="aci"></a>Instâncias de contentor do Azure

Utilização do Azure Container Instances para implantar seus modelos como um ponto de final de REST API, se um ou mais das seguintes condições for verdadeiro:
- Precisa implementar e validar o seu modelo rapidamente. Conclusão da implementação do ACI em menos de 5 minutos.
- Pretende implementar o seu modelo no desenvolvimento de um ou ambiente de teste. ACI permite-lhe implementar 20 grupos de contentores por subscrição. Para obter mais informações, consulte a [Quotas e disponibilidade das regiões do Azure Container Instances](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) documento.

Para obter mais informações, consulte a [implementar um modelo no Azure Container Instances](how-to-deploy-to-aci.md) documento.

## <a id="aks"></a>Serviço Kubernetes do Azure

Para cenários de produção de grande escala, utilize o Azure Kubernetes Service (AKS). Pode utilizar um cluster do AKS existente ou crie um novo com o SDK do Azure Machine Learning, a CLI ou o portal do Azure.

Criar um cluster do AKS é um processo de tempo para a área de trabalho. Pode reutilizar este cluster para várias implementações. Se eliminar o cluster, em seguida, tem de criar um novo cluster da próxima vez que precisa implantar.

Serviço Kubernetes do Azure fornece as seguintes capacidades:

* Dimensionamento automático
* Registo
* Recolha de dados do modelo
* Tempos de resposta rápidos para os seus serviços web

O processo de criação de um cluster do AKS demora cerca de 20 minutos.

Para obter mais informações, consulte a [implementar um modelo de serviço de Kubernetes do Azure](how-to-deploy-to-aks.md) documento.

## <a id="iotedge"></a>O Azure IoT Edge

Com dispositivos de IoT, é mais rápido pontuações no dispositivo em vez de enviar dados para a cloud para a classificação. Com o Azure IoT Edge, pode alojar o seu modelo em dispositivos periféricos. Implemente o seu modelo para o IoT Edge se precisar de um ou mais das seguintes capacidades:
- Lidar com tarefas de prioridade localmente, mesmo sem uma ligação de cloud
- Trabalhar com dados gerados, que é demasiado grandes para extrair rapidamente a partir da cloud
- Ativar o processamento em tempo real através de informações em ou próximo dispositivos locais
- Acomodar os requisitos relacionados com a privacidade de dados 

Para obter mais informações, consulte a [implementar no Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) documento.

Para obter mais informações sobre o serviço de IoT Edge, veja a [documentação do Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/).


## <a id="fpga"></a>Matrizes de porta de campos programáveis (FPGA)

Hardware aceleradas modelos com tecnologia do Project Brainwave tornam possível obter a latência ultrabaixa para pedidos de inferência em tempo real. Project Brainwave acelera redes neurais profundas (DNN) implementadas nas matrizes de porta de campos programáveis na cloud do Azure. Normalmente utilizado o DNNs estão disponíveis como featurizers para aprendizagem de transferência ou personalizável com pesos preparado a partir de seus próprios dados.

Para obter mais informações, consulte a [implementar para um FPGA](how-to-deploy-fpga-web-service.md) documento.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como implementar um modelo para um destino de computação específicos, consulte os documentos seguintes:

* [Implementar um modelo no Azure Container Instances](how-to-deploy-to-aci.md)
* [Implementar um modelo de serviço de Kubernetes do Azure](how-to-deploy-to-aks.md)
* [Implementar um modelo para o Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning)
* [Implementar um modelo para FPGA](how-to-deploy-fpga-web-service.md)
