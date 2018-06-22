---
title: Conjuntos de Máquina Virtual de ciência de dados - Azure | Microsoft Docs
description: A implementação de conjuntos de VMs de ciência de dados como um recurso partilhado de uma equipa
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
ms.openlocfilehash: 0740ff7542d066442146b8e80e188ad5ba49a2b5
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309403"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Criar um agrupamento partilhado de máquinas de virtuais de ciência de dados

Este artigo descreve como pode criar um agrupamento partilhado de dados de ciência de máquinas virtuais (DSVMs) para uma equipa para utilizar. As vantagens de utilizar um agrupamento partilhado são uma melhor utilização de recursos, facilitação de partilha e de colaboração e gestão mais eficiente dos recursos DSVM. 

Pode utilizar vários métodos e as tecnologias para criar um conjunto de DSVMs. Este artigo incida no conjuntos para o processamento em lote e VMs interativas.

## <a name="batch-processing-pool"></a>Conjunto de processamento de batches
Se pretender configurar um conjunto de DSVMs principalmente para executar tarefas no batch offline, pode utilizar o [do Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) ou [do Azure Batch](https://docs.microsoft.com/azure/batch/) serviço. Este artigo incida no Azure Batch AI.

A edição de Ubuntu do DSVM é suportada como uma das imagens no Azure Batch AI. CLI do Azure ou o SDK Python, onde criar o cluster de AI do Azure Batch, pode especificar o `image` parâmetro e defina-o como `UbuntuDSVM`. Pode escolher o tipo de processamento de nós que pretende: instâncias com base em GPU versus instâncias de só de CPU, o número de CPUs e memória de um [ao nível choice de instâncias VM](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) disponíveis no Azure. 

Quando utiliza a imagem de Ubuntu DSVM na AI do Batch connosco com base em GPU, são pré-instalado todos os controladores GPU e avançada learning estruturas necessárias. A pré-instalação do poupa tempo considerável na preparação os nós de batch. Na verdade, se estiver a desenvolver num DSVM Ubuntu interativamente, irá notar que os nós de AI do Batch estão exatamente a mesma configuração e a configuração do ambiente. 

Normalmente, quando criar um cluster de AI do Batch, também criar uma partilha de ficheiros que está montada por todos os nós. A partilha de ficheiros é utilizada para a entrada e saída de dados, bem como armazenar os tarefa batch código/scripts. 

Depois de criar um cluster de AI do Batch, pode utilizar o mesmo CLI ou Python SDK para submeter tarefas ao ser executado. Paga apenas o momento em que é utilizado para executar as tarefas de lote. 

Para obter mais informações, consulte:
* Instruções passo a passo da utilização [CLI do Azure](https://docs.microsoft.com/azure/batch-ai/quickstart-cli) para gerir AI do Batch
* Instruções passo a passo da utilização [Python](https://docs.microsoft.com/azure/batch-ai/quickstart-python) para gerir AI do Batch
* [Lote receitas de AI](https://github.com/Azure/BatchAI) que demonstram como utilizar vários AI e avançada learning estruturas com AI do Batch

## <a name="interactive-vm-pool"></a>Conjunto VM interativo

Um conjunto de VMs interativas que são partilhados pela equipa de ciência de dados/AI toda permite que os utilizadores iniciem sessão para uma instância disponível de DSVM em vez de ter uma instância dedicada para cada conjunto de utilizadores. Esta configuração ajuda-o com uma melhor disponibilidade e utilização mais eficiente dos recursos. 

A tecnologia que utilizar para criar um conjunto VM interativo é [conjuntos de dimensionamento de máquina virtual do Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Pode utilizar conjuntos de dimensionamento para criar e gerir um grupo de idênticas, com balanceamento de carga e as VMs de dimensionamento automático. 

O utilizador inicia sessão no endereço IP ou o DNS do conjunto principal. O conjunto de dimensionamento automaticamente as rotas a sessão para um DSVM disponível no conjunto de dimensionamento. Uma vez que os utilizadores querem num ambiente semelhante, independentemente da VM está a iniciar sessão, todas as instâncias da VM no conjunto de dimensionamento montagem a unidade de rede partilhado, como uma partilha de ficheiros do Azure ou uma partilha NFS. Área de trabalho partilhado do utilizador normalmente é mantida no arquivo de ficheiro partilhado que está montado em cada uma das instâncias. 

Pode encontrar um modelo de Gestor de recursos do Azure de exemplo que cria um conjunto com instâncias de Ubuntu DSVM de dimensionamento [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json). Um exemplo do [ficheiro de parâmetros](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) é de modelo para o Azure Resource Manager na mesma localização. 

Pode criar o conjunto do modelo Azure Resource Manager, especificando valores para o ficheiro de parâmetros na CLI do Azure de dimensionamento. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Os comandos anteriores partem do princípio de que tem:
* Uma cópia do ficheiro de parâmetros com os valores especificados na sua instância do conjunto de dimensionamento.
* O número de instâncias de VM.
* Partilham ponteiros para os ficheiros do Azure.
* Credenciais para a conta de armazenamento que será montado em cada VM. 

O ficheiro de parâmetros é referenciado localmente nos comandos. Também pode passar parâmetros inline ou da linha de comandos para os mesmos no script.  

O modelo anterior permite que o SSH e a porta de JupyterHub da escala front-end definida para o conjunto de back-end do Ubuntu DSVMs. Como um utilizador, basta iniciar sessão a VM no SSH ou JupyterHub de forma normal. Porque as instâncias de VM podem ser escaladas para cima ou para baixo dinamicamente, qualquer Estado tem de ser guardado o montado no partilha de ficheiros do Azure. Pode utilizar a mesma abordagem para criar um conjunto de DSVMs do Windows. 

O [script monta a partilha de ficheiros do Azure](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) também está disponível no repositório de DataScienceVM do Azure no GitHub. O script monta a partilha de ficheiros do Azure no ponto de montagem especificado no ficheiro de parâmetros. O script cria também ligações suaves para na unidade montada no diretório raiz do utilizador inicial. Um diretório de utilizadores específicos bloco de notas na partilha de ficheiros do Azure é recuperável ligado a `$HOME/notebooks/remote` diretório para que os utilizadores possam aceder, executar e guardar os seus blocos de notas do Jupyter. Pode utilizar a mesma Convenção quando criar utilizadores adicionais na VM para a área de trabalho de Jupyter de cada utilizador de apontar para a partilha de ficheiros do Azure. 

Dimensionamento automático de suporte de conjuntos de dimensionamento de máquina virtual. Pode definir regras em quando criar instâncias adicionais e reduzir verticalmente instâncias. Por exemplo, pode reduzir verticalmente a zero instâncias para guardar os custos de utilização de hardware na nuvem quando as máquinas virtuais não são utilizadas de todo. As páginas de documentação de conjuntos de dimensionamento de máquina virtual fornecem passos detalhados para [dimensionamento automático](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Passos Seguintes

* [Configurar uma identidade comum](dsvm-common-identity.md)
* [Armazene de maneira segura credenciais para aceder aos recursos na nuvem](dsvm-secure-access-keys.md)















