---
title: "Integrar o seu pipeline integração e entrega contínua VSTS Azure DevTest Labs | Microsoft Docs"
description: "Saiba como integrar o Azure DevTest Labs a integração contínua VSTS e o pipeline de entrega"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: tarcher
ms.openlocfilehash: 3cd6939e890f59e7c5b188edd8620da5c9fcc935
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2017
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Integrar o Azure DevTest Labs a integração contínua VSTS e o pipeline de entrega
Pode utilizar o **extensão do Azure DevTest Labs tarefas** instalado no Visual Studio Team Services (VSTS) para integrar a compilação de CI/CD e versão pipeline com o Azure DevTest Labs facilmente. A instalação da extensão três tarefas para criar uma VM, crie uma imagem personalizada de uma VM e elimine uma VM. Este processo torna mais fácil, por exemplo, rapidamente implementar uma "imagem dourada" para a tarefa de teste específico, em seguida, elimine-o quando o teste estiver concluído.

Este artigo mostra como criar e implementar uma VM, criar uma imagem personalizada, em seguida, elimine a VM, todos os como um pipeline concluído. Normalmente, no entanto, pretende utilizar as tarefas individualmente na sua própria pipeline personalizado de implementação de teste de compilação.

## <a name="before-you-begin"></a>Antes de começar
Antes de pode integrar o pipeline de CI/CD com a Azure DevTest Labs, primeiro tem de instalar a extensão do Visual Studio Marketplace.
1. Aceda a [Azure DevTest Labs tarefas](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)
1. Escolha a instalação
1. Concluir o assistente

## <a name="create-a-resource-manager-template"></a>Criar um modelo do Resource Manager
Esta secção descreve como criar o modelo Azure Resource Manager que utilizar para criar uma Máquina Virtual do Azure a pedido.

1. Siga os passos indicados em [utilizar um modelo do Resource Manager](devtest-lab-use-resource-manager-template.md) para criar um modelo do Resource Manager na sua subscrição.
1. Adicionar o [WinRM artefactos](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) como parte da criação da VM (antes de gerar o modelo do Resource Manager).

   Acesso do WinRM é necessário para utilização implementar tarefas, tais como **a cópia de ficheiros do Azure** e **PowerShell em máquinas de destino**.

   > [!NOTE]
   > Ao utilizar o WinRM com um endereço IP partilhado, tem de adicionar uma regra NAT para mapear uma porta externa para a porta do WinRM. Isto não é necessário se a VM é criada com um endereço IP público.
   >
   >

1. Guarde o modelo como um ficheiro no seu computador. Nome de ficheiro **CreateVMTemplate.json**.
1. Verifique o modelo no seu sistema de controlo de origem.
1. Abra um editor de texto e copie o seguinte script para a mesma.

   ```powershell
   Param( [string] $labVmId)

   $labVmComputeId = (Get-AzureRmResource -Id $labVmId).Properties.ComputeId

   # Get lab VM resource group name
   $labVmRgName = (Get-AzureRmResource -Id $labVmComputeId).ResourceGroupName

   # Get the lab VM Name
   $labVmName = (Get-AzureRmResource -Id $labVmId).Name

   # Get lab VM public IP address
   $labVMIpAddress = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
                   -Name $labVmName).IpAddress

   # Get lab VM FQDN
   $labVMFqdn = (Get-AzureRmPublicIpAddress -ResourceGroupName $labVmRgName
              -Name $labVmName).DnsSettings.Fqdn

   # Set a variable labVmRgName to store the lab VM resource group name
   Write-Host "##vso[task.setvariable variable=labVmRgName;]$labVmRgName"

   # Set a variable labVMIpAddress to store the lab VM Ip address
   Write-Host "##vso[task.setvariable variable=labVMIpAddress;]$labVMIpAddress"

   # Set a variable labVMFqdn to store the lab VM FQDN name
   Write-Host "##vso[task.setvariable variable=labVMFqdn;]$labVMFqdn"
   ```

1. Verifique o script no seu sistema de controlo de origem. Nome algo semelhante a **GetLabVMParams.ps1**.

   Este script, quando executado no agente como parte da definição de versão, recolhe valores que precisa de implementar a sua aplicação para a VM se utilizar os passos de tarefas, tais como **a cópia de ficheiros do Azure** ou **PowerShell em máquinas de destino**. Normalmente utiliza estas tarefas para implementar aplicações para uma VM do Azure e necessitam de valores, tais como o nome do grupo de recursos de VM, o endereço IP e o nome de domínio completamente qualificado (FDQN).

## <a name="create-the-release-definition-in-release-management"></a>Criar a definição de versão na gestão de versão
Efetue estes passos para criar a definição de versão.

1. Abra o **versões** separador do **compilar & versão** hub e escolha o "**+**" ícone.
1. No **criar versão definição** caixa de diálogo, selecione o **vazio** modelo e escolha **seguinte**.
1. Na página seguinte, selecione **escolha mais tarde** e, em seguida, escolha **criar** para criar uma nova definição de versão com o ambiente de um predefinido e não existem artefactos ligados.
1. Na nova definição da versão, selecione as reticências (…) junto ao nome do ambiente para abrir o menu de atalho e selecionar **configurar variáveis**. 
1. No **configurar - ambiente** caixa de diálogo, introduza os seguintes valores para as variáveis a utiliza as tarefas de definição da versão:
   - **vmName**: introduza o nome atribuído à VM quando criou o modelo do Resource Manager no portal do Azure.
   - **nome de utilizador**: introduza o nome de utilizador atribuído à VM quando criou o modelo do Resource Manager no portal do Azure.
   - **palavra-passe**: introduza a palavra-passe atribuído à VM quando criou o modelo do Resource Manager no portal do Azure. Utilize o ícone de "cadeado" para ocultar e a manter a palavra-passe.

   <a name="create-a-vm"></a>Criar uma VM
   ---

   É a primeira fase nesta implementação para criar a VM para utilizar como a "imagem dourada" para implementações subsequentes. Criar a VM na sua instância do Azure DevTest laboratório através da tarefa especial desenvolvida para esta finalidade. Na definição da versão, selecione **+ adicionar tarefas** e adicione um **VM do Azure DevTest Labs criar** tarefas a partir do separador implementar. Configure da seguinte forma:

   Consulte [Azure DevTest Labs tarefas](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) para criar a VM a utilizar para implementações subsequentes.
   - **Subscrição do Azure RM**: selecione uma ligação na lista em **ligações de serviço do Azure disponíveis** ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Nome do laboratório**: selecione o nome da instância que criou anteriormente.
   - **Nome do modelo**: introduza o caminho completo e nome do ficheiro de modelo guardada no seu repositório de código de origem. Pode utilizar as propriedades de gestão de versões incorporadas para simplificar o caminho, por exemplo:
      ```
      $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
      ```
   - **Os parâmetros do modelo**: introduza os parâmetros para as variáveis definidas no modelo. Utilize os nomes das variáveis definidas no ambiente, por exemplo:
      ```
      -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
      ```
   - **Variáveis - ID de VM de laboratório de saída**: terá do ID da VM recentemente criado para os passos subsequentes. O nome predefinido da variável de ambiente que é automaticamente preenchida com este ID está definido **variáveis de saída** secção. Pode editar a variável, se necessário, mas não se esqueça de utilizar o nome correto em tarefas subsequentes. O ID de VM de laboratório tem o formato:
      ```
      /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
      ```
1. Execute o script que criou anteriormente para recolher os detalhes da DevTest Labs VM. Na definição da versão, selecione **+ adicionar tarefas** e adicione um **Azure PowerShell** tarefas a partir de **implementar** separador. Configure a tarefa da seguinte forma:

   Consulte [implementar: Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) e execute o script para recolher os detalhes da DevTest Labs VM.

   - **Tipo de ligação do Azure**: o Azure Resource Manager.
   - **Subscrição do Azure RM**: selecione uma ligação na lista em **ligações de serviço do Azure disponíveis** ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Tipo de script**: ficheiro de Script.
   - **Caminho do script**: introduza o caminho completo e nome do script guardada no seu repositório de código de origem. Pode utilizar as propriedades de gestão de versões incorporadas para simplificar o caminho, por exemplo:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   - **Argumentos do script**: introduza, como o argumento de script, o nome da variável de ambiente que foi preenchido automaticamente com o ID do laboratório de VM pela tarefa anterior, por exemplo: 
      ```
      -labVmId '$(labVMId)'
      ```
   O script recolhe os valores necessários e armazena-os em variáveis de ambiente na definição da versão, para que as possa facilmente consultá-las nos passos de tarefas subsequentes.

1. Agora pode implementar a aplicação no novo DevTest Labs VM. As tarefas que é normalmente utilizada para implementar são **a cópia de ficheiros do Azure** e **PowerShell em máquinas de destino**.
   - As informações sobre a VM é necessário para os parâmetros destas tarefas são armazenadas em três variáveis de configuração com o nome **labVmRgName**, **labVMIpAddress**, e **labVMFqdn**dentro da definição de versão.
   - Se pretender experimentar criar uma VM do DevTest Labs e uma imagem personalizada, sem a implementar uma aplicação, pode ignorar este passo.

   <a name="create-an-image"></a>Criar uma imagem
   ---

   A fase seguinte é criar uma imagem da VM recentemente implementada na sua instância do Azure DevTest Labs. Em seguida, pode utilizar esta imagem para criar cópias de VM a pedido, sempre que pretender executar uma tarefa de desenvolvimento ou execute alguns testes. Na definição da versão, selecione **+ adicionar tarefas** e adicione um **Azure DevTest Labs criar personalizada imagem** tarefas do **implementar** separador. Configure da seguinte forma:

   Consulte [Azure DevTest Labs tarefas](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) para criar a imagem.
   - **Subscrição do Azure RM**: selecione uma ligação na lista em **ligações de serviço do Azure disponíveis** ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **Nome do laboratório**: selecione o nome da instância que criou anteriormente.
   - **Nome da imagem personalizada**: introduza um nome para a imagem personalizada.
   - **Descrição**: Opcionalmente, introduza uma descrição para o facilitar a selecionar a imagem correta mais tarde.
   - **Origem de laboratório de VM - ID de VM de laboratório de origem**: Se tiver alterado o nome predefinido da variável de ambiente que foi preenchido automaticamente com o ID de laboratório de VM por uma tarefa anterior, editá-lo aqui. A predefinição é *$(labVMId)*.
   - **Saída variáveis - ID de imagem personalizada**: terá do ID da imagem recentemente criado quando pretender gerir ou eliminá-lo. O nome predefinido da variável de ambiente que é automaticamente preenchida com este ID está definido **variáveis de saída** secção. Pode editar a variável, se necessário.
   
   <a name="delete-the-vm"></a>Eliminar a VM
   ---
   
   A etapa final neste exemplo consiste em eliminar a VM tenha implementado na sua instância do Azure DevTest Labs. Normalmente, tem de eliminar a VM depois de executar as tarefas de desenvolvimento ou executar os testes que precisa na VM implementada. Na definição da versão, selecione **+ adicionar tarefas** e adicione um **VM do Azure DevTest Labs eliminar** tarefas a partir de **implementar** separador. Configure da seguinte forma:

   Consulte [Azure DevTest Labs tarefas](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) para eliminar a VM.
   - **Subscrição do Azure RM**: selecione uma ligação na lista em **ligações de serviço do Azure disponíveis** ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/en-us/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
   - **ID de VM de laboratório**: Se tiver alterado o nome predefinido da variável de ambiente que foi preenchido automaticamente com o ID de laboratório de VM por uma tarefa anterior, editá-lo aqui. A predefinição é *$(labVMId)*.
1. Introduza um nome para a definição de versão e guardá-lo.
1. Criar uma nova versão, selecione de compilação mais recente e implementá-la para o ambiente único na definição do.

Em cada fase, atualize a vista de instância DevTest Labs no portal do Azure para ver a VM e a imagem a ser criada e a VM que está a ser eliminado novamente.

Agora, pode utilizar a imagem personalizada para criar as VMs quando necessário.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passos Seguintes
* Saiba como [criar ambientes de várias VMS com modelos do Resource Manager](devtest-lab-create-environment-from-arm.md).
* Explorar mais modelos de Gestor de recursos de início rápido para a automatização de DevTest Labs do [público repositório do GitHub do DevTest Labs](https://github.com/Azure/azure-quickstart-templates).
* Se necessário, consulte o [resolução de problemas de VSTS](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) página.
