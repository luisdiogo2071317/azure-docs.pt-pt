---
title: Como criar DSVM e HDI como destinos de computação para o Azure ML
description: Crie DSVM e HDI Spark cluster como destinos de computação para experimentação do Azure ML.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/26/2017
ms.openlocfilehash: 211f60b9c25b4bd20769f6a4840afaecf8373b9f
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782350"
---
# <a name="create-dsvm-and-hdi-spark-cluster-as-compute-targets"></a>Criar DSVM e HDI Spark cluster como destinos de computação

Pode facilmente aumentar verticalmente ou aumente horizontalmente a sua experimentação de machine learning, adicionar destinos de computação adicionais, tais como a DSVM baseada em Ubuntu (máquina de Virtual de ciência de dados) e do Apache Spark para Azure HDInsight cluster. Este movimentações de artigo lhe os passos de criação de esses destinos no Azure de computação. Para obter mais informações sobre destinos de computação do Azure ML, consulte [descrição geral do serviço de experimentação do Azure Machine Learning](experimentation-service-configuration.md).

>[!NOTE]
>Certifique-se de que tem permissões adequadas para criar recursos, tais como a VM e HDI clusters no Azure antes de continuar. Também ambos estes recursos podem consumir muitos núcleos de computação consoante a configuração. Certifique-se de que a sua subscrição tem capacidade suficiente para os núcleos de CPU virtual. Pode obter sempre entre em contato com o suporte do Azure para aumentar o número máximo de núcleos permitidos na sua subscrição.

## <a name="create-an-ubuntu-dsvm-in-azure-portal"></a>Criar um DSVM em Ubuntu no portal do Azure

Pode criar uma DSVM do portal do Azure. 

1. Inicie sessão no portal do Azure https://portal.azure.com
2. Clique nas **+ novo** link e procure "dados máquina de virtual de ciência para Linux".
    ![Ubuntu](media/how-to-create-dsvm-hdi/ubuntu_dsvm.png)
4. Escolher **Máquina Virtual de ciência de dados para Linux (Ubuntu)** na lista e, siga na tela instruções para criar a DSVM.

>[!IMPORTANT]
>Certifique-se de que escolher **palavra-passe** como o _tipo de autenticação_.

![utilizar pwd](media/how-to-create-dsvm-hdi/use_pwd.png)

## <a name="create-an-ubuntu-dsvm-using-azure-cli"></a>Criar um DSVM em Ubuntu com a cli do azure

Também pode utilizar um modelo de gestão de recursos do Azure para implementar uma DSVM do.

>[!NOTE]
>Todos os comandos seguintes devem ser emitido a partir da pasta raiz de um projeto do Azure ML.

Primeiro, crie uma `mydsvm.json` de ficheiros com o seu editor de texto favorito no `docs` pasta. (Se não tiver um `docs` pasta na pasta de raiz do projeto, criar um.) Podemos usar esse arquivo para configurar alguns parâmetros básicos para o modelo de gestão de recursos do Azure. 

Copie e cole o fragmento JSON seguinte para o `mydsvm.json` de ficheiros e preencha os valores apropriados:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "<admin username>"},
     "adminPassword": { "value" : "<admin password>"},
     "vmName": { "value" : "<vm name>"},
     "vmSize": { "value" : "<vm size>"}
  }
}
```

Para o _vmSize_ campo, pode usar qualquer tamanho VM suportado listado na [modelo de gestão de recursos do Azure de DSVM do Ubuntu](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/multiazuredeploywithext.json). Recomendamos que utilize um do abaixo tamanhos como destinos de computação para o Azure ML. 


>[!TIP]
> Para [cargas de trabalho de aprendizagem profunda](how-to-use-gpu.md) pode implementar em VMs com tecnologia de GPU.

- [VMs de fins gerais](../../virtual-machines/linux/sizes-general.md)
  - Standard_DS2_v2 
  - Standard_DS3_v2 
  - Standard_DS4_v2 
  - Standard_DS12_v2 
  - Standard_DS13_v2 
  - Standard_DS14_v2 
- [As VMs com tecnologia de GPU](../../virtual-machines/linux/sizes-gpu.md)
  - Standard_NC6 
  - Standard_NC12 
  - Standard_NC24 
 

Saiba mais sobre estes [tamanhos de máquinas de virtuais do Linux no Azure](../../virtual-machines/linux/sizes.md) e seus [informações sobre preços](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Inicie a janela da CLI a partir da aplicação do Azure ML Workbench ao clicar em **arquivo** --> **linha de comandos aberta**, ou **PowerShell aberto** item de menu. 

>[!NOTE]
>Também pode fazer isso em qualquer ambiente de linha de comando em que tem instalada o az-cli.

Na janela de linha de comandos, introduza os comandos abaixo:

```azurecli
# first make sure you have a valid Azure authentication token
$ az account get-access-token

# if you don't have a valid token, please log in to Azure first. 
# if you already do, you can skip this step.
$ az login

# list all subscriptions you have access to
$ az account list -o table

# make sure you set the subscription you want to use to create DSVM as the current subscription
$ az account set -s <subscription name or Id>

# it is always a good idea to create a resource group for the VM and associated resources to live in.
# you can use any Azure region, but it is best to create them in the region where your Azure ML Experimentation account is, e.g. eastus2, westcentralus or australiaeast.
# also, only certain Azure regions has GPU-equipped VMs available.
$ az group create -n <resource group name> -l <azure region>

# now let's create the DSVM based on the JSON configuration file you created earlier.
# note we assume the mydsvm.json config file is placed in the "docs" sub-folder.
$ az group deployment create -g <resource group name> --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters @docs/mydsvm.json

# find the FQDN (fully qualified domain name) of the VM just created. 
# you can also use IP address from the next command if FQDN is not set.
$ az vm show -g <resource group name> -n <vm name> --query "fqdns"

# find the IP address of the VM just created
$ az vm show -g <resource group name> -n <vm name> --query "publicIps"
```
## <a name="attach-a-dsvm-compute-target"></a>Anexar um destino de computação DSVM
Quando a DSVM estiver criada, agora pode anexá-lo ao seu projeto do Azure ML.

```azurecli
# attach the DSVM compute target
# it is a good idea to use FQDN in case the IP address changes after you deallocate the VM and restart it
$ az ml computetarget attach remotedocker --name <compute target name> --address <ip address or FQDN> --username <admin username> --password <admin password> 

# prepare the Docker image on the DSVM 
$ az ml experiment prepare -c <compute target name>
```
Agora, deve estar pronto para executar experimentações neste DSVM.

## <a name="deallocate-a-dsvm-and-restart-it-later"></a>Desalocar uma DSVM e reiniciá-lo mais tarde
Quando concluir as tarefas de computação do Azure ML, possível desalocar a DSVM. Esta ação encerra a VM, liberta os recursos de computação, mas que ela vai preservando os discos virtuais. Não são cobradas pelo custo de computação quando a VM é desalocada.

Para desalocar uma VM:

```azurecli
$ az vm deallocate -g <resource group name> -n <vm name>
```

Para que a VM voltem a vida, utilize o `az ml start` comando:

```azurecli
$ az vm start -g <resource group name> -n <vm name>
```

## <a name="expand-the-dsvm-os-disk"></a>Expanda o disco do SO DSVM
A DSVM do Ubuntu vem com um disco de SO de 50GB e o disco de dados de 100GB. Docker armazena suas imagens do disco de dados, conforme mais espaço há disponível ali. Quando utilizado como destino de computação para o Azure ML, este disco pode ser utilizado pelo motor de Docker puxando imagens do Docker e a criação de camadas de conda com base no mesmo. Poderá ter de expandir o disco para um tamanho superior (por exemplo, 200 GB) para evitar o erro de "disco cheio" enquanto estiver no meio de uma execução. Referência [como expandir os discos rígidos virtuais numa VM do Linux com a CLI do Azure](../../virtual-machines/linux/expand-disks.md) para saber como fazer isso facilmente no azure-cli. 

## <a name="create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal"></a>Criar um Apache Spark para Azure HDInsight cluster no portal do Azure

Para executar tarefas do Spark de escalamento horizontal, terá de criar um Apache Spark para Azure HDInsight cluster no portal do Azure.

1. Inicie sessão no portal do Azure https://portal.azure.com
2. Clique nas **+ novo** link e procure "HDInsight".

    ![encontrar hdi](media/how-to-create-dsvm-hdi/hdi.png)
    
3. Escolher **HDInsight** na lista e, em seguida, clique na **criar** botão.
4. Na **Noções básicas** ecrã de configuração, **tipo de Cluster** definições, certifique-se de que escolher **Spark** como o _tipo de Cluster_, **Linux** como o _sistema operativo_, e **Spark 2.1.0 (HDI 3.6)** como a verze rozhraní.

    ![Configurar hdi](media/how-to-create-dsvm-hdi/configure_hdi.png)

    >[!IMPORTANT]
    >Observe que o ecrã acima, o cluster tem um _nome de utilizador de início de sessão de Cluster_ campo e um _nome de utilizador de Secure Shell (SSH)_ campo. Estes são duas identidades de utilizador diferente, mesmo que para sua comodidade, pode especificar a mesma palavra-passe para ambos os inícios de sessão. O _nome de utilizador de início de sessão de Cluster_ é utilizada para iniciar sessão para a gestão da IU web do cluster do HDI. O _nome de utilizador de início de sessão SSH_ é utilizada para iniciar sessão para o nó principal do cluster, e esse é o que é necessário para o Azure ML expedir as tarefas do Spark.

5. Escolha o tamanho do cluster e o tamanho de nó que precisa e concluir o Assistente de criação. Pode demorar até 30 minutos para o cluster concluir o aprovisionamento. 

## <a name="attach-an-hdi-spark-cluster-compute-target"></a>Anexar um destino de computação de cluster do HDI Spark

Depois de criar o cluster do Spark HDI, agora pode anexá-lo ao seu projeto do Azure ML.

```azurecli
# attach the HDI compute target
$ az ml computetarget attach cluster --name <compute target name> --address <cluster name, such as myhdicluster123.azurehdinsight.net> --username <ssh username> --password <ssh password> 

# prepare the conda environment on HDI
$ az ml experiment prepare -c <compute target name>
```
Agora, deve estar pronto para executar experimentações deste cluster do Spark.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre:
- [Descrição geral do serviço de experimentação do Azure Machine Learning](experimentation-service-configuration.md)
- [Azure ficheiros de configuração do serviço de experimentação do Machine Learning Workbench](experimentation-service-configuration-reference.md)
- [Apache Spark para Azure HDInsight cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/)
- [Máquina de Virtual de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)
