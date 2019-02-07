---
title: Executar experimentações & inferência de tipos numa rede virtual
titleSuffix: Azure Machine Learning service
description: Executar experimentações de machine learning e inferência proteger dentro de uma rede Virtual do Azure. Saiba como criar destinos de computação de preparação de modelos e como inferência de tipos dentro de uma rede Virtual do Azure. Também aborda os requisitos para redes virtuais protegidos, como exigir portas de entrada e saídas.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 01/08/2019
ms.openlocfilehash: 2e7f6c066ea254fff90ba2f9ff1f559fdb680ddf
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766695"
---
# <a name="securely-run-experiments-and-inferencing-inside-an-azure-virtual-network"></a>Executar com segurança inferência dentro de uma rede Virtual do Azure e de experimentações

Neste artigo, irá aprender a executar as experimentações e inferência numa rede Virtual. Uma rede virtual funciona como um limite de segurança, isolamento de recursos do Azure a partir da internet pública. Também pode associar uma rede Virtual do Azure à sua rede no local. Permite-lhe preparar seus modelos e aceder aos seus modelos implementados para inferência de forma segura.

O serviço Azure Machine Learning depende de outros serviços do Azure para recursos de computação de coisas. Recursos de computação (destinos de computação) são utilizados para preparar e implementar modelos. Estes computação destinos podem ser criados numa rede virtual. Por exemplo, pode utilizar uma VM de ciência de dados para preparar um modelo e, em seguida, implementar o modelo de serviço de Kubernetes do Azure. Para obter mais informações sobre as redes virtuais, consulte a [descrição geral de redes virtuais](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) documento.

## <a name="storage-account-for-your-workspace"></a>Conta de armazenamento para a área de trabalho

Quando cria uma área de trabalho do serviço do Azure Machine Learning, ele requer uma conta de armazenamento do Azure. Não ative as regras de firewall para esta conta de armazenamento. O serviço Azure Machine Learning requer acesso sem restrições à conta de armazenamento.

Se não estiver-se de que quer modificar estas definições ou não, consulte o __alterar a regra de acesso de rede predefinida__ secção a [configurar o armazenamento do Azure firewalls e redes virtuais](https://docs.microsoft.com/azure/storage/common/storage-network-security) documentar e utilizar os passos para _permitir o acesso_ partir __todas as redes__.

## <a name="use-machine-learning-compute"></a>Utilizar o Machine Learning de computação

Para utilizar a computação do Machine Learning numa rede virtual, utilize as seguintes informações para compreender os requisitos de rede:

- A rede virtual tem de estar na mesma subscrição e região como a área de trabalho do serviço do Azure Machine Learning.

- A sub-rede especificada para o cluster de computação do Machine Learning tem de ter suficiente não atribuídos endereços IP para acomodar o número de VMs destinada para o cluster. Se a sub-rede não tiver suficiente não atribuídos endereços IP, o cluster será atribuído parcialmente.

- Se pretender proteger a rede virtual, restringindo o tráfego, tem de deixar algumas portas aberta para o serviço de computação do Machine Learning. Para obter mais informações, consulte a [necessárias portas](#mlcports) secção.

- Verifique se as políticas de segurança ou bloqueios na subscrição ou grupo de recursos da rede virtual restringem permissões para gerir a rede virtual.

- Se quiser colocar vários clusters de computação do Machine Learning numa rede virtual, terá de pedir um aumento de quota para um ou mais dos recursos.

    Computação do Machine Learning aloca automaticamente recursos de rede adicionais no grupo de recursos que contém a rede virtual. Para cada cluster de computação do Machine Learning, o serviço Azure Machine Learning aloca os seguintes recursos: 

    - Grupo de segurança de uma rede (NSG)

    - Um endereço IP público

    - Um balanceador de carga

    Estes recursos estão limitados pelas [quotas de recursos](https://docs.microsoft.com/azure/azure-subscription-service-limits) da subscrição. 

### <a id="mlcports"></a> Portas necessárias

Computação do Machine Learning utiliza atualmente o serviço de Batch do Azure para aprovisionar VMs na rede virtual especificada. A sub-rede tem de permitir comunicação de entrada do serviço Batch. Esta comunicação é utilizada para agendar é executado em nós de computação do Machine Learning e para comunicar com o armazenamento do Azure e outros recursos. O batch adiciona NSGs ao nível de interfaces de rede (NICs) ligadas a VMs. Estes NSGs configuram automaticamente regras de entrada e saída para permitir o tráfego seguinte:

- Tráfego TCP de entrada nas portas 29876 e 29877 a partir de endereços IP de função do serviço Batch. 
 
- Tráfego TCP de entrada na porta 22 para permitir o acesso remoto.
 
- Tráfego de saída em qualquer porta para a rede virtual.

- Tráfego de saída em qualquer porta para a Internet.

Tenha cuidado se modifica ou adicionar regras de entrada/saída de NSGs configurados de Batch. Se uma comunicação de blocos NSG para os nós de computação, em seguida, os serviços de computação do Machine Learning define o estado de nós de computação para inutilizável.

Não é necessário especificar NSGs ao nível da sub-rede porque o Batch configura o seus próprios NSGs. No entanto, se a sub-rede especificada tiver associados NSGs e/ou uma firewall, configure as regras de segurança de entrada e saída, conforme mencionado anteriormente. As capturas de ecrã seguintes mostram como a configuração da regra procura no portal do Azure:

![Captura de ecrã da entrada de regras NSG para a computação do Machine Learning](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Regras de NSG de captura de ecrã da saída para a computação do Machine Learning](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Criar a computação do Machine Learning numa rede virtual

Para criar um cluster de computação do Machine Learning com o **portal do Azure**, utilize os seguintes passos:

1. Partir do [portal do Azure](https://portal.azure.com), selecione a sua área de trabalho do serviço do Azure Machine Learning.

1. Partir do __aplicativo__ secção, selecione __computação__. Em seguida, selecione __adicionar computação__. 

    ![Como adicionar uma computação no serviço Azure Machine Learning](./media/how-to-enable-virtual-network/add-compute.png)

1. Para configurar este recurso de computação para utilizar uma rede virtual, utilize estas opções:

    - __Configuração de rede__: Selecione __Avançadas__.

    - __Grupo de recursos__: Selecione o grupo de recursos que contém a rede virtual.

    - __Rede virtual__: Selecione a rede virtual que contém a sub-rede.

    - __Sub-rede__: Selecione a sub-rede a utilizar.

    ![Uma captura de ecrã a mostrar as definições de rede virtual para a computação do machine learning](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Também pode criar um cluster computação do Machine Learning com o **SDK do Azure Machine Learning**. O código seguinte cria um novo cluster de computação do Machine Learning no `default` sub-rede de uma rede virtual com o nome `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure Virtual Network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")
    
    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name = vnet_resourcegroup_name,
                                                           vnet_name = vnet_name,
                                                           subnet_name = subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)
    
    # Wait for the cluster to complete, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Quando concluir o processo de criação, pode treinar o seu modelo usando o cluster. Para obter mais informações, consulte a [selecione e utilize um destino de computação de treinamento](how-to-set-up-training-targets.md) documento.

## <a name="use-a-virtual-machine-or-hdinsight"></a>Utilizar uma Máquina Virtual ou o HDInsight

Para utilizar uma máquina virtual ou um cluster do HDInsight numa rede Virtual com a sua área de trabalho, utilize os seguintes passos:

> [!IMPORTANT]
> O serviço Azure Machine Learning suporta apenas máquinas virtuais com o Ubuntu.

1. Criar um cluster VM ou o HDInsight com o portal do Azure, CLI do Azure, etc. e colocá-la numa rede Virtual do Azure. Para obter mais informações, consulte os seguintes documentos:
    * [Criar e gerir redes virtuais do Azure para VMs do Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Expandir o HDInsight com redes virtuais do Azure](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network) 

1. Para permitir que o serviço Azure Machine Learning comunicar com a porta SSH na VM ou cluster, tem de configurar uma entrada de origem para o NSG. A porta SSH é normalmente a porta 22. Para permitir tráfego a partir desta origem, utilize as seguintes informações:

    * __origem__: Selecione __Etiqueta do Serviço__

    * __Etiqueta de serviço de origem__: Select __AzureMachineLearning__

    * __Intervalos de portas de origem__: Selecione __*__

    * __Destino__: Selecione __qualquer__

    * __Intervalos de portas de destino__: Selecione __22__

    * __Protocolo__: Selecione __qualquer__

    * __Ação__: Selecione __permitir__

   ![Captura de ecrã de regras de entrada para fazer a experimentação numa VM ou HDInsight numa rede virtual](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Mantenha a predefinição as regras de saída para o NSG. Para obter mais informações, consulte a secção de regras de segurança padrão do [grupos de segurança](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) documentação.
    
1. Anexe o cluster VM ou o HDInsight à sua área de trabalho do serviço do Azure Machine Learning. Para obter mais informações, consulte a [configurar destinos de computação de preparação de modelos](how-to-set-up-training-targets.md) documento.

## <a name="use-azure-kubernetes-service-aks"></a>Utilizar o serviço Kubernetes do Azure (AKS)

> [!IMPORTANT]
> Verifique os pré-requisitos e planeie o endereçamento IP para o seu cluster, antes de continuar com os passos mencionados abaixo. Pode consultar [configurar avançadas no Azure Kubernetes Service (AKS) de rede](https://docs.microsoft.com/azure/aks/configure-advanced-networking)
> 
> Mantenha a predefinição as regras de saída para o NSG. Para obter mais informações, consulte a secção de regras de segurança padrão do [grupos de segurança](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules) documentação.
>
> Serviço Kubernetes do Azure e a rede Virtual do Azure devem estar na mesma região.

Para adicionar o serviço Kubernetes do Azure numa rede Virtual à sua área de trabalho, utilize os passos seguintes a __portal do Azure__:

1. Partir do [portal do Azure](https://portal.azure.com), selecione a sua área de trabalho do serviço do Azure Machine Learning.

1. Partir do __aplicativo__ secção, selecione __computação__. Em seguida, selecione __adicionar computação__. 

    ![Como adicionar uma computação no serviço Azure Machine Learning](./media/how-to-enable-virtual-network/add-compute.png)

1. Para configurar este recurso de computação para utilizar uma rede virtual, utilize estas opções:

    - __Configuração de rede__: Selecione __Avançadas__.

    - __Grupo de recursos__: Selecione o grupo de recursos que contém a rede virtual.

    - __Rede virtual__: Selecione a rede virtual que contém a sub-rede.

    - __Sub-rede__: Selecione a sub-rede.

    - __Intervalo de endereços do serviço do Kubernetes__: Selecione o intervalo de endereços do serviço do Kubernetes. Este intervalo de endereços usa um intervalo IP de notação CIDR para definir os endereços IP disponíveis para o cluster. Ele não pode sobrepor com quaisquer intervalos de IP de sub-rede. Por exemplo: 10.0.0.0/16.

    - __Endereço IP do serviço de DNS do Kubernetes__: Selecione o endereço IP do serviço de DNS do Kubernetes. Este endereço IP é atribuído para o serviço DNS do Kubernetes. Tem de estar no intervalo de endereços de serviço do Kubernetes. Por exemplo: 10.0.0.10.

    - __Endereço de bridge do docker__: Selecione o endereço de bridge do Docker. Este endereço IP é atribuído a Bridge do Docker. Não pode estar em quaisquer intervalos de sub-rede IP ou o intervalo de endereços do serviço do Kubernetes. Por exemplo: 172.17.0.1/16

   ![Azure Machine Learning service: Definições de rede Virtual de computação de aprendizagem automática](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

    > [!TIP]
    > Se já tiver um cluster do AKS numa rede Virtual, pode anexá-lo para a área de trabalho. Para obter mais informações, consulte [como implementar no AKS](how-to-deploy-to-aks.md).

Também pode utilizar o **SDK do Azure Machine Learning** para adicionar o serviço Kubernetes do Azure numa rede Virtual. O código seguinte cria um novo serviço de Kubernetes do Azure no `default` sub-rede de uma rede virtual com o nome `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace = ws,
                                  name = "myaks",
                                  provisioning_configuration = config)
```

Assim que concluir o processo de criação, pode fazer inferência num cluster do AKS por trás de uma rede virtual. Para obter mais informações, consulte [como implementar no AKS](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Passos Seguintes

* [Configurar ambientes de treinamento](how-to-set-up-training-targets.md)
* [Onde pretende implementar modelos](how-to-deploy-and-where.md)
* [Implantar com segurança os modelos com SSL](how-to-secure-web-service.md)

