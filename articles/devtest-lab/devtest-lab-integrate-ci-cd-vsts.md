---
title: "Integrar o seu pipeline integração e entrega contínua VSTS Azure DevTest Labs | Microsoft Docs"
description: "Saiba como integrar o Azure DevTest Labs a integração contínua VSTS e o pipeline de entrega"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: a26df85e-2a00-462b-aac1-dd3539532569
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: v-craic
ms.openlocfilehash: 6c6bd4fbd89ec87cbbdbfb9ed42f86a484acf7ad
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="integrate-azure-devtest-labs-into-your-vsts-continuous-integration-and-delivery-pipeline"></a>Integrar o Azure DevTest Labs a integração contínua VSTS e o pipeline de entrega
Pode utilizar o *Azure DevTest Labs tarefas* extensão que se encontra instalado facilmente no Visual Studio Team Services (VSTS) para integrar o pipeline de compilação e versão de CI/CD Azure DevTest Labs. A instalação da extensão três tarefas: 
* Criar uma VM
* Criar uma imagem personalizada a partir de uma VM
* Eliminar uma VM 

O processo torna mais fácil, por exemplo, implementar rapidamente a "imagem dourada" para uma tarefa de teste específica e, em seguida, elimine-o quando o teste está concluído.

Este artigo mostra como criar e implementar uma VM, criar uma imagem personalizada e, em seguida, elimine a VM, todos os como um pipeline concluído. Normalmente iria executar cada tarefa individualmente na sua própria pipeline personalizado de implementação de teste de compilação.

## <a name="before-you-begin"></a>Antes de começar
Pode integrar o pipeline de CI/CD com a Azure DevTest Labs, tem de instalar a extensão do Visual Studio Marketplace.
1. Aceda a [Azure DevTest Labs tarefas](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).
1. Selecione **instalar**.
1. Conclua o assistente.

## <a name="create-a-resource-manager-template"></a>Criar um modelo do Resource Manager
Esta secção descreve como criar o modelo Azure Resource Manager que utilizar para criar uma máquina virtual do Azure a pedido.

1. Para criar um modelo do Resource Manager na sua subscrição, conclua o procedimento no [utilizar um modelo do Resource Manager](devtest-lab-use-resource-manager-template.md).
1. Antes de gerar o modelo do Resource Manager, adicione o [WinRM artefactos](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-winrm) como parte da criação da VM.

   É necessário acesso de WinRM para utilizar tarefas de implementação, tais como *a cópia de ficheiros do Azure* e *PowerShell em máquinas de destino*.

   > [!NOTE]
   > Quando utilizar o WinRM com um endereço IP partilhado, tem de adicionar uma regra NAT para mapear uma porta externa para a porta do WinRM. Este passo não é necessário se criar a VM com um endereço IP público.
   >
   >

1. Guarde o modelo como um ficheiro no seu computador. Nome de ficheiro **CreateVMTemplate.json**.
1. Verifique o modelo seu sistema de controlo de origem.
1. Abra um editor de texto e cole o seguinte script para a mesma.

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

1. Certifique-se o script seu sistema de controlo de origem. Nome semelhante ao seguinte **GetLabVMParams.ps1**.

   Quando executar este script no agente como parte da definição de versão e, se utilizar os passos de tarefas, tais como *a cópia de ficheiros do Azure* ou *PowerShell em máquinas de destino*, o script recolhe os valores que precisa Implemente a sua aplicação para a VM. Normalmente utilizaria estas tarefas para implementar aplicações para uma VM do Azure. As tarefas necessitam de valores, tais como o nome do grupo de recursos de VM, o endereço IP e o nome de domínio completamente qualificado (FDQN).

## <a name="create-a-release-definition-in-release-management"></a>Criar uma definição de versão na gestão de versão
Para criar a definição de versão, efetue o seguinte:

1. No **versões** separador do **compilar & versão** hub, selecione o botão de sinal de adição (+).
2. No **criar versão definição** janela, selecione o **vazio** modelo e, em seguida, selecione **seguinte**.
3. Selecione **escolha mais tarde**e, em seguida, selecione **criar** para criar uma nova definição de versão com o ambiente de um predefinido e não existem artefactos ligados.
4. Para abrir o menu de atalho, na nova definição da versão, selecione o botão de reticências (…) junto ao nome do ambiente e, em seguida, selecione **configurar variáveis**. 
5. No **configurar - ambiente** janela, para as variáveis que utilizar as tarefas de definição de versão, introduza os seguintes valores:

   a. Para **vmName**, introduza o nome atribuído à VM quando criou o modelo do Resource Manager no portal do Azure.

   b. Para **userName**, introduza o nome de utilizador atribuído à VM quando criou o modelo do Resource Manager no portal do Azure.

   c. Para **palavra-passe**, introduza a palavra-passe que atribuiu à VM quando criou o modelo do Resource Manager no portal do Azure. Utilize o ícone de "cadeado" para ocultar e a manter a palavra-passe.

### <a name="create-a-vm"></a>Criar uma VM

A próxima fase da implementação está a criar a VM para utilizar como a "imagem dourada" para implementações subsequentes. Criar a VM na sua instância do Azure DevTest laboratório utilizando a tarefa que é especialmente desenvolvida para esta finalidade. 

1. Na definição da versão, selecione **adicionar tarefas**.
2. No **implementar** separador, adicione um *VM do Azure DevTest Labs criar* tarefas. Configure a tarefa da seguinte forma:

   > [!NOTE]
   > Para criar a VM a utilizar para implementações subsequentes, consulte [tarefas Azure DevTest Labs](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Para **subscrição do Azure RM**, selecione uma ligação no **ligações de serviço do Azure disponíveis** lista ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   b. Para **nome do laboratório**, selecione o nome da instância que criou anteriormente.

   c. Para **nome do modelo**, introduza o caminho completo e nome do ficheiro de modelo que tenha guardado para o seu repositório de código de origem. Pode utilizar as propriedades de gestão de versões incorporadas para simplificar o caminho, por exemplo:

   ```
   $(System.DefaultWorkingDirectory)/Contoso/ARMTemplates/CreateVMTemplate.json
   ```

   d. Para **os parâmetros do modelo**, introduza os parâmetros para as variáveis que estão definidas no modelo. Utilize os nomes das variáveis que definiu no ambiente, por exemplo:

   ```
   -newVMName '$(vmName)' -userName '$(userName)' -password (ConvertTo-SecureString -String '$(password)' -AsPlainText -Force)
   ```

   e. Para **variáveis de saída - ID de VM de laboratório**, terá do ID da VM recentemente criado para os passos subsequentes. Definir o nome predefinido da variável de ambiente que é automaticamente preenchida com este ID no **variáveis de saída** secção. Pode editar a variável, se necessário, mas não se esqueça de utilizar o nome correto em tarefas subsequentes. O ID de VM de laboratório é escrito no seguinte formato:

   ```
   /subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.DevTestLab/labs/{labName}/virtualMachines/{vmName}
   ```

3. Execute o script que criou anteriormente para recolher os detalhes da DevTest Labs VM. 
4. Na definição da versão, selecione **adicionar tarefas** e, em seguida, no **implementar** separador, adicione um *Azure PowerShell* tarefas. Configure a tarefa da seguinte forma:

   > [!NOTE]
   > Para recolher os detalhes da DevTest Labs VM, consulte [implementar: Azure PowerShell](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/AzurePowerShell) e execute o script.

   a. Para **tipo de ligação do Azure**, selecione **do Azure Resource Manager**.

   b. Para **subscrição do Azure RM**, selecione uma ligação na lista em **ligações de serviço do Azure disponíveis**, ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   c. Para **tipo de Script**, selecione **ficheiro de Script**.
 
   d. Para **caminho do Script**, introduza o caminho completo e nome do script que guardou para o seu repositório de código de origem. Pode utilizar as propriedades de gestão de versões incorporadas para simplificar o caminho, por exemplo:
      ```
      $(System.DefaultWorkingDirectory/Contoso/Scripts/GetLabVMParams.ps1
      ```
   e. Para **argumentos do Script**, introduza o nome da variável de ambiente que foi preenchido automaticamente com o ID do laboratório de VM pela tarefa anterior, por exemplo: 
      ```
      -labVmId '$(labVMId)'
      ```
    O script recolhe os valores necessários e armazena-os em variáveis de ambiente na definição da versão, para que as possa facilmente consultá-las nos passos subsequentes.

5. Implemente a aplicação no novo DevTest Labs VM. As tarefas normalmente utiliza para implementar a aplicação são *a cópia de ficheiros do Azure* e *PowerShell em máquinas de destino*.
   As informações sobre a VM é necessário para os parâmetros destas tarefas são armazenadas em três variáveis de configuração com o nome **labVmRgName**, **labVMIpAddress**, e **labVMFqdn**dentro da definição de versão. Se apenas pretender experimentar criar uma VM do DevTest Labs e uma imagem personalizada, sem a implementar uma aplicação, pode ignorar este passo.

### <a name="create-an-image"></a>Criar uma imagem

A fase seguinte é criar uma imagem da VM recentemente implementada na sua instância do Azure DevTest Labs. Em seguida, pode utilizar a imagem para criar cópias de VM a pedido, sempre que pretender executar uma tarefa de desenvolvimento ou execute alguns testes. 

1. Na definição da versão, selecione **adicionar tarefas**.
2. No **implementar** separador, adicione um **Azure DevTest Labs criar personalizada imagem** tarefas. Configure da seguinte forma:

   > [!NOTE]
   > Para criar a imagem, consulte [tarefas Azure DevTest Labs](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Para **subscrição do Azure RM**, além de **ligações de serviço do Azure disponíveis** lista, selecione uma ligação a partir da lista ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

   b. Para **nome do laboratório**, selecione o nome da instância que criou anteriormente.

   c. Para **nome da imagem personalizada**, introduza um nome para a imagem personalizada.

   d. (Opcional) Para **Descrição**, introduza uma descrição para o facilitar a selecionar a imagem correta mais tarde.

   e. Para **origem laboratório VM - ID de VM de laboratório de origem**, se tiver alterado o nome predefinido da variável de ambiente que foi preenchido automaticamente com o ID de laboratório de VM por uma tarefa anterior, editá-lo aqui. O valor predefinido é **$(labVMId)**.

   f. Para **variáveis de saída - ID de imagem personalizada**, terá do ID da imagem recentemente criado quando pretender gerir ou eliminá-lo. O nome predefinido da variável de ambiente que é automaticamente preenchida com este ID está definido **variáveis de saída** secção. Pode editar a variável, se necessário.

### <a name="delete-the-vm"></a>Eliminar a VM

A etapa final consiste em eliminar a VM que tenha implementado na sua instância do Azure DevTest Labs. Normalmente seriam eliminar a VM depois de executar as tarefas de desenvolvimento ou executar os testes que precisa na VM implementada. 

1. Na definição da versão, selecione **adicionar tarefas** e, em seguida, no **implementar** separador, adicione um *VM do Azure DevTest Labs eliminar* tarefas. Configure da seguinte forma:

      > [!NOTE]
      > Para eliminar a VM, consulte [Azure DevTest Labs tarefas](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks).

   a. Para **subscrição do Azure RM**, selecione uma ligação no **ligações de serviço do Azure disponíveis** lista ou criar uma ligação de permissões mais restrita a sua subscrição do Azure. Para obter mais informações, consulte [ponto final de serviço do Azure Resource Manager](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).
 
   b. Para **ID de VM de laboratório**, se tiver alterado o nome predefinido da variável de ambiente que foi preenchido automaticamente com o ID de laboratório de VM por uma tarefa anterior, editá-lo aqui. O valor predefinido é **$(labVMId)**.

2. Introduza um nome para a definição de versão e, em seguida, guardá-lo.
3. Criar uma nova versão, selecione de compilação mais recente e implementá-la para o ambiente único na definição do.

Em cada fase, atualize a vista da sua instância DevTest Labs no portal do Azure para ver a VM e imagem que estão a ser criada e a VM que está a ser eliminada novamente.

Agora, pode utilizar a imagem personalizada para criar as VMs quando forem necessários.


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passos Seguintes
* Saiba como [criar ambientes de várias VMS com modelos do Resource Manager](devtest-lab-create-environment-from-arm.md).
* Explorar mais modelos de Gestor de recursos de início rápido para a automatização de DevTest Labs do [público repositório do GitHub do DevTest Labs](https://github.com/Azure/azure-quickstart-templates).
* Se necessário, consulte o [resolução de problemas de VSTS](https://docs.microsoft.com/vsts/build-release/actions/troubleshooting) página.
 
