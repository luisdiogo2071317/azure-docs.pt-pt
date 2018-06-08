---
title: Conjuntos de Máquina Virtual de ciência de dados - Azure | Microsoft Docs
description: A implementação de conjuntos de VM de ciência de dados como um recurso partilhado para a equipa
keywords: ligação avançada learning AI, ferramentas de ciência de dados, máquina de virtual de ciência de dados, geoespacial análise, o processo de ciência de dados de equipa
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: c7aab0435ecbd0aee57a15008ac0270159ec2eb3
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837087"
---
# <a name="creating-a-shared-pool-of-data-science-virtual-machines"></a>Criar um agrupamento partilhado de máquinas de virtuais de ciência de dados

Este artigo aborda como um agrupamento partilhado de máquinas virtuais de ciência de dados (DSVM) pode ser criado para utilização pela equipa do. A vantagem de utilizar um agrupamento partilhado é uma melhor utilização de recursos, o facilitar o início de partilha e de colaboração e permitindo que as TI gerir os recursos DSVM com mais eficiência. 

Existem muitas formas e as diferentes tecnologias que podem ser utilizadas para criar um conjunto de DSVM.  Seguem-se cenários principais:

* Conjunto para o processamento em lote
* Agrupamento para VMs interativas

## <a name="batch-processing-pool"></a>Conjunto de processamento em lote
Se pretender configurar um conjunto de DSVM principalmente para executar tarefas no batch offline, em seguida, pode utilizar [do Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) serviço ou [do Azure Batch](https://docs.microsoft.com/azure/batch/). 

### <a name="azure-batch-ai"></a>IA do Azure Batch
A edição de Ubuntu do DSVM é suportada como uma das imagens no Azure Batch AI. A CLI do Azure ou o SDK Python onde criar o cluster de AI do Azure Batch, pode especificar o ```image``` parâmetro e defina-o como ```UbuntuDSVM```. Pode escolher o tipo de processamento de nós que pretende - instâncias com base em GPU vs CPU apenas instâncias de, número de CPUs, memória a partir de um [ao nível choice de instâncias VM](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) disponíveis no Azure. Quando utilizar a imagem de Ubuntu DSVM no AI do Batch connosco com base em GPU, todos os controladores GPU necessários e estruturas de aprendizagem profunda são pré-instalado guardar um tempo considerável na preparação os nós de batch. Na verdade, se estiver a desenvolver num DSVM Ubuntu interativamente, irá reparar que os nós de AI do Batch estão exatamente a mesma configuração e a configuração do ambiente. Normalmente, quando é criado um cluster de AI do Batch também criar uma partilha de ficheiros que está montada por todos os nós e é utilizada para a entrada e saída de dados, bem como armazenar o código de tarefa do batch / scripts. 

Assim que o cluster de AI do Batch é criado, pode utilizar o mesmo CLI ou Python SDK para submeter tarefas ao ser executado. Apenas paga a hora em que é utilizada para executar as tarefas de lote. 

#### <a name="more-information"></a>Mais informações
* Instruções passo a passo da utilização [CLI do Azure](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) para a gestão de AI do Batch
* Instruções passo a passo da utilização [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) para a gestão de AI do Batch
* [Lote receitas de AI](https://github.com/Azure/BatchAI) estão disponíveis demonstra como utilizar vários learning de AI/avançada estruturas com AI do Batch.

## <a name="interactive-vm-pool"></a>Conjunto VM interativo

Um agrupamento de DSVMs interativas que são partilhados pelo AI todo / equipa de ciência de dados permite que os utilizadores iniciem sessão para uma instância disponível de DSVM em vez de ter uma instância dedicada para cada conjunto de utilizadores. Isto ajuda-o com uma melhor disponibilidade e utilização mais eficiente dos recursos. 

A tecnologia utilizada para criar um conjunto VM interativo é o [conjuntos de dimensionamento de Máquina Virtual do Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) (VMSS), que lhe permite criar e gerir um grupo de idênticas, com balanceamento de carga e dimensionamento automático VMs. Os registos de utilizador para o endereço IP ou o DNS do conjunto principal. O conjunto de dimensionamento automaticamente as rotas a sessão para um DSVM disponível num conjunto de dimensionamento. Uma vez que o utilizador pretende que o ambiente semelhante, independentemente da VM que estão a iniciar, todas as instâncias da VM num conjunto de dimensionamento monta uma unidade de rede partilhada como um ficheiros do Azure ou uma partilha NFS. Área de trabalho partilhado do utilizador normalmente é mantida no arquivo de ficheiro partilhado que está montado em cada uma das instâncias. 

Um modelo de Gestor de recursos do Azure de exemplo que cria um conjunto de dimensionamento de VM com Ubuntu DSVMs instâncias pode ser encontrado no [github](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Um exemplo do modelo Azure Resource Manager [ficheiro de parâmetros](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) também são fornecidos na mesma localização. 

Pode criar o conjunto do modelo Azure Resource Manager, especificando valores adequados para o ficheiro de parâmetros utilizando a CLI do Azure de dimensionamento VM. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Os comandos acima partem do princípio de que tem uma cópia do ficheiro de parâmetros com os valores especificados para a instância do conjunto de dimensionamento VM, número de instâncias VM, ponteiros para os ficheiros do Azure, juntamente com as credenciais para a conta de armazenamento que será montada em cada VM. O ficheiro de parâmetros é referenciado localmente no comando acima. Também pode passar parâmetros inline ou da linha de comandos para os mesmos no script.  

O modelo acima permite que o SSH e a porta de Jupyterhub do dimensionamento de VM de front-end definida para o conjunto de back-end de Ubuntu DSVMs.  Como um utilizador apenas iniciar sessão VM SSH ou JupyterHub da forma normal. Uma vez que as instâncias de VM podem ser escaladas para cima ou para baixo dinamicamente, qualquer Estado tem de ser guardado o montado no partilha de ficheiros do Azure. A mesma abordagem pode ser utilizada para criar um conjunto de DSVMs do Windows. 

O [script que monta os ficheiros de Azure](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) também está disponível no Github de DataScienceVM do Azure. Para além de montar os ficheiros do Azure no ponto de montagem especificado no ficheiro de parâmetros, também cria adicionais ligações suaves para na unidade montada no diretório raiz do utilizador inicial e um diretório de bloco de notas específicas do utilizador no Azure ficheiros partilhados é recuperável ligado ao ```$HOME/notebooks/remote``` diretório ativar utilizador aceder, executar e guardar os seus blocos de notas do Jupyter.  Pode ser utilizada a mesma Convenção quando criar utilizadores adicionais na VM para apontar para a área de trabalho de Jupyter de cada utilizador para os ficheiros do Azure. 

Dimensionamento de VM do Azure define o dimensionamento automático de suporte onde pode definir regras em quando criar instâncias adicionais e, em que circunstâncias para reduzir verticalmente instâncias incluindo colocá-lo para baixo para zero instâncias ao guardar na nuvem custos de utilização de hardware quando as máquinas virtuais não são utilizadas de todo . As páginas de documentação de conjuntos de dimensionamento VM fornecem passos detalhados para [escala automática](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Passos Seguintes

* [Configurar a identidade comum](dsvm-common-identity.md)
* [Armazene de maneira segura credenciais para aceder aos recursos na nuvem](dsvm-secure-access-keys.md)















