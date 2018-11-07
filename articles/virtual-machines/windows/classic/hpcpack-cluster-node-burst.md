---
title: Adicionar nós de rajada para um cluster HPC Pack | Documentos da Microsoft
description: Saiba como expandir um cluster HPC Pack no Azure a pedido através da adição de instâncias de função de trabalho em execução num serviço cloud
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: 7d42c026975a18c7574e4bc64ec28ab3ed0082bc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248457"
---
# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Adicionar nós pedido "explosão" para um cluster HPC Pack no Azure
Se configurar uma [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster no Azure, talvez tenha uma forma de aumentar a capacidade de cluster ou reduzir verticalmente, sem manutenção de um conjunto de VMs de nó de computação de pré-configurada. Este artigo mostra-lhe como adicionar nós de sob demanda "explosão" (instâncias de função de trabalho em execução num serviço cloud) como recursos de computação para um nó principal no Azure. 

> [!IMPORTANT] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

![Nós de rajada][burst]

Os passos neste artigo ajudam-na adicionar nós do Azure rapidamente para um baseado na nuvem HPC Pack nó principal VM para um teste ou a implementação da prova de conceito. Os passos de alto nível são os mesmos que os passos para "expansão para o Azure" para adicionar capacidade para cluster HPC Pack no local de computação na cloud. Para obter um tutorial, veja [configurar um cluster de cálculo híbrido com o Microsoft HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Para obter instruções detalhadas e considerações para implementações de produção, consulte [expansão para o Azure com o Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Pré-requisitos
* **Nó principal HPC Pack implementado na VM do Azure** -pode utilizar um VM de nó principal autónomo ou um que faz parte de um cluster maior. Para criar um nó principal autónomo, consulte [implementar um nó principal do HPC Pack numa VM do Azure](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Para opções de implementação do cluster HPC Pack automatizadas, consulte [opções para criar e gerir um HPC do Windows cluster no Azure com o Microsoft HPC Pack](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Se utilizar o [script de implementação do HPC Pack IaaS](hpcpack-cluster-powershell-script.md) para criar o cluster no Azure, pode incluir nós de rajada do Azure na sua implementação automatizada. Veja os exemplos nesse artigo.
  > 
  > 
* **Subscrição do Azure** - para adicionar nós do Azure, pode escolher a mesma subscrição utilizada para implementar o nó principal VM, ou uma subscrição diferente (ou subscrições).
* **Quota de núcleos** -poderá ter de aumentar a quota de núcleos, especialmente se optar por implantar vários nós do Azure com tamanhos de vários núcleos. Para aumentar uma quota [abra um pedido de suporte do cliente online](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) sem encargos.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Passo 1: Criar um serviço em nuvem e de uma conta de armazenamento para os nós do Azure
Utilize o portal do Azure ou ferramentas equivalentes para configurar os seguintes recursos que são necessários para implementar os nós do Azure:

* Um novo serviço cloud do Azure (clássico)
* Uma nova conta de armazenamento do Azure (clássico)

> [!NOTE]
> Não reutilize um serviço cloud existente na sua subscrição. 
> 
> 

**Considerações**

* Configure um serviço cloud separadas para cada modelo de nó do Azure que pretende criar. No entanto, pode utilizar a mesma conta de armazenamento para vários modelos de nó.
* Recomendamos que localize o serviço cloud e a conta de armazenamento para a implementação na mesma região do Azure.

## <a name="step-2-configure-an-azure-management-certificate"></a>Passo 2: Configurar um certificado de gestão do Azure
Para adicionar nós do Azure como recursos de computação, precisa de um certificado de gestão no nó principal e carregar um correspondentes de certificados para a subscrição do Azure utilizada para a implementação.

Para este cenário, pode escolher o **certificado de gestão do Azure de HPC predefinido** que HPC Pack instala e configura automaticamente no nó principal. Este certificado é útil para testar os efeitos e implementações de prova de conceito. Para utilizar este certificado, carregue o ficheiro C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer do nó principal VM para a subscrição. Para carregar o certificado no [portal do Azure](https://portal.azure.com):

1. Clique em **subscrições** > *your_subscription_name*.

2. Clique em **certificados de gestão** > **carregar**.

Para obter opções adicionais configurar o certificado de gestão, consulte [cenários para configurar o certificado de gestão do Azure para implementações de períodos de rajada de Azure](https://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Passo 3: Implementar nós do Azure para o cluster
Os passos para adicionar e iniciar nós do Azure neste cenário são geralmente as mesmas que as etapas com um nó principal no local. Para obter mais informações, consulte as secções seguintes [passos para implementar nós do Azure com o Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Criar um modelo de nó do Azure
* Adicionar nós do Azure para o cluster Windows HPC
* Iniciar nós (aprovisionar) do Azure

Depois de adicionar e iniciar os nós, eles estão prontos a utilizar para executar trabalhos de cluster.

Se tiver problemas ao implementar nós do Azure, veja [resolver problemas de implementações de nós do Azure com o Microsoft HPC Pack](https://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Passos Seguintes
* Para utilizar um tamanho de instância de computação intensiva para os nós de rajada, veja as considerações de presentes [tamanhos de VM de computação de alto desempenho](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Se quiser automaticamente aumentar ou diminuir o Azure recursos, de acordo com a carga de trabalho do cluster de computação, veja [aumentar e diminuir os recursos de computação do Azure num cluster HPC Pack automaticamente](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
