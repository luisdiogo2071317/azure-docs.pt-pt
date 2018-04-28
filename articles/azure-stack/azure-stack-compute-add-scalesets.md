---
title: Dimensionamento de máquina virtual de disponibilizar define disponível na pilha do Azure | Microsoft Docs
description: Saiba como um operador da nuvem adicionar dimensionamento da máquina virtual para a pilha do Azure Marketplace
services: azure-stack
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.topic: article
ms.date: 04/06/2018
ms.author: brenduns
ms.reviewer: anajod
keywords: ''
ms.openlocfilehash: cdabd2a9d336cdd8ac83d27460fe129c45b7e1c6
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="make-virtual-machine-scale-sets-available-in-azure-stack"></a>Disponibilizar os conjuntos de dimensionamento de máquina virtual na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Conjuntos de dimensionamento de máquina virtual são um recurso de computação de pilha do Azure. Pode utilizá-los para implementar e gerir um conjunto de máquinas virtuais idênticas. Com todas as máquinas virtuais configurados da mesma, conjuntos de dimensionamento não necessitam de pré-aprovisionamento de máquinas virtuais. É mais fácil criar serviços em grande escala que macrocomputação, macrodados e de cargas de trabalho de destino.

Este artigo orienta-o processo para disponibilizar conjuntos de dimensionamento no mercado de pilha do Azure. Depois de concluir este procedimento, os utilizadores podem adicionar conjuntos de dimensionamento de máquina virtual para as suas subscrições.

Conjuntos de dimensionamento de máquina virtual na pilha do Azure são como conjuntos de dimensionamento de máquina virtual no Azure. Para obter mais informações, consulte os vídeos seguintes:
* [Mark Russinovich talks Azure Scale Sets (Mark Russinovich fala sobre os conjuntos de dimensionamento do Azure)](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)
* [Virtual Machine Scale Sets with Guy Bowerman (Conjuntos de Dimensionamento de Máquinas Virtuais, com Guy Bowerman)](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

Na pilha do Azure, conjuntos de dimensionamento de máquina virtual não suportam o dimensionamento automático. Pode adicionar mais instâncias a uma escala definida utilizando o portal de pilha do Azure, modelos do Resource Manager ou PowerShell.

## <a name="prerequisites"></a>Pré-requisitos
* **PowerShell e ferramentas**

   Instalar e PowerShell configurado para a pilha do Azure e as ferramentas de pilha do Azure. Consulte [começar a trabalhar com o PowerShell na pilha de Azure](azure-stack-powershell-configure-quickstart.md).

   Depois de instalar as ferramentas de pilha do Azure, certifique-se de importar o módulo do PowerShell seguinte (caminho relativa a. \ComputeAdmin pasta na pasta master de ferramentas de AzureStack):

        Import-Module .\AzureStack.ComputeAdmin.psm1

* **Imagem do sistema operativo**

   Se ainda não adicionou uma imagem do sistema operativo para a pilha do Azure Marketplace, consulte o artigo [adicionar a imagem de VM do Windows Server 2016 para o mercado de pilha do Azure](azure-stack-add-default-image.md).

   Para obter suporte do Linux, transfira Ubuntu Server 16.04 e adicioná-lo utilizando ```Add-AzsVMImage``` com os seguintes parâmetros: ```-publisher "Canonical" -offer "UbuntuServer" -sku "16.04-LTS"```.


## <a name="add-the-virtual-machine-scale-set"></a>Adicionar o conjunto de dimensionamento de máquina virtual

Editar o seguinte script do PowerShell para o seu ambiente e, em seguida, execute-o para adicionar um dimensionamento de máquina virtual definido para a pilha do Azure Marketplace. 

``$User`` é a conta que utiliza para ligar o portal de administrador. Por exemplo, serviceadmin@contoso.onmicrosoft.com.

```
$Arm = "https://adminmanagement.local.azurestack.external"
$Location = "local"

Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint $Arm

$Password = ConvertTo-SecureString -AsPlainText -Force "<your Azure Stack administrator password>"

$User = "<your Azure Stack service administrator user name>"

$Creds =  New-Object System.Management.Automation.PSCredential $User, $Password

$AzsEnv = Get-AzureRmEnvironment AzureStackAdmin
$AzsEnvContext = Add-AzureRmAccount -Environment $AzsEnv -Credential $Creds

Select-AzureRmSubscription -SubscriptionName "Default Provider Subscription"

Add-AzsVMSSGalleryItem -Location $Location
```

## <a name="update-images-in-a-virtual-machine-scale-set"></a>Atualizar imagens de um conjunto de dimensionamento de máquina virtual 
Depois de criar um conjunto de dimensionamento de máquina virtual, os utilizadores podem atualizar imagens na escala definir sem o conjunto ter de ser recriadas de dimensionamento. O processo para atualizar uma imagem depende os seguintes cenários:

1. Modelo de implementação de conjunto de dimensionamento de máquina virtual **especifica mais recente** para *versão*:  

   Quando o *versão* está definido como **mais recente** no *imageReference* secção do modelo para uma escala de definir, aumentar verticalmente operações no conjunto de dimensionamento, utilize a versão mais recente disponível da imagem para a escala de conjunto de instâncias. Após a conclusão do trabalho de dimensionamento, pode eliminar mais antigas instâncias de conjuntos de dimensionamento de máquina virtual.  (Os valores para *publicador*, *oferecem*, e *sku* permanecem inalterados). 

   Segue-se um exemplo de especificação *mais recente*:  

          "imageReference": {
             "publisher": "[parameters('osImagePublisher')]",
             "offer": "[parameters('osImageOffer')]",
             "sku": "[parameters('osImageSku')]",
             "version": "latest"
             }

   Antes de trabalho de dimensionamento pode utilizar uma nova imagem, tem de transferir essa nova imagem:  

   - Quando a imagem do Marketplace é uma versão mais recente que a imagem no conjunto de dimensionamento: Transferir a nova imagem que substitui a imagem antiga. Depois da imagem é substituída, um utilizador pode avançar para aumentar verticalmente. 

   - Quando a versão de imagem no Marketplace é o mesmo que a imagem no conjunto de dimensionamento: elimine a imagem que está a ser utilizado no conjunto de dimensionamento e, em seguida, transferir a nova imagem. Durante o tempo entre a remoção da imagem original e a transferência da imagem do novo, não é possível aumentar verticalmente. 
      
     Este processo é necessário para resyndicate imagens que utilizam o formato de ficheiro disperso, introduzido com a versão 1803. 
 

2. Modelo de implementação de conjunto de dimensionamento de máquina virtual **não especifique mais recente** para *versão* e especifica um número de versão em vez disso:  

     Se transferir uma imagem com uma versão mais recente (que as alterações a versão disponível), o conjunto de dimensionamento não é possível aumentar verticalmente. Isto é propositado como a versão de imagem especificada no modelo de conjunto de dimensionamento tem de estar disponível.  

Para obter mais informações, consulte [discos de sistema operativo e imagens](.\user\azure-stack-compute-overview.md#operating-system-disks-and-images).  


## <a name="remove-a-virtual-machine-scale-set"></a>Remover um conjunto de dimensionamento de máquina virtual

Para remover uma máquina virtual item da Galeria de conjunto de dimensionamento, execute o seguinte comando do PowerShell:

    Remove-AzsVMSSGalleryItem

> [!NOTE]
> O item da Galeria não pode ser removido imediatamente. Noite tem de atualizar o portal várias vezes antes do item mostra como removido do Marketplace.


## <a name="next-steps"></a>Passos Seguintes
[Perguntas mais frequentes sobre pilha do Azure](azure-stack-faq.md)

