---
title: Extensões VM do Azure e de funcionalidades para Linux | Microsoft Docs
description: Saiba que extensões estão disponíveis para máquinas virtuais do Azure, agrupadas por que forneça ou melhorar.
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.openlocfilehash: 760f832bc12bccbf1cce77db25bf60413ad9a36b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942831"
---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Extensões de máquina virtual e funcionalidades para Linux

Extensões de máquina virtual do Azure (VM) são aplicações de pequenas a indicar tarefas de configuração e a automatização de pós-implementação em VMs do Azure. Por exemplo, se uma máquina virtual requer a instalação de software, proteção de software antivírus, ou para executar um script dentro do mesmo, uma extensão da VM pode ser utilizada. Extensões VM do Azure podem ser executadas com a CLI do Azure, PowerShell, os modelos Azure Resource Manager e o portal do Azure. Extensões podem ser incluídas com uma nova implementação de VM ou executar qualquer sistema existente.

Este artigo fornece uma descrição geral das extensões de VM, pré-requisitos para utilizar extensões de VM do Azure, e orientações sobre como detetar, gerir e remover as extensões de VM. Este artigo fornece informações generalizadas porque há várias extensões VM, cada um com uma configuração potencialmente exclusiva. Extensão específicos podem ser encontrados detalhes cada documento específicos para a extensão individuais.

## <a name="use-cases-and-samples"></a>Casos de utilização e amostras

Estão disponíveis várias extensões de VM do Azure diferentes, cada um com específico utilizar maiúsculas e minúsculas. Alguns exemplos incluem:

- Aplicar as configurações de PowerShell Desired estado para uma VM com a extensão de DSC para Linux. Para obter mais informações, consulte [extensão de configuração do Azure Desired estado](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Configure a monitorização de uma VM com a extensão de VM de agente de monitorização da Microsoft. Para obter mais informações, consulte [como monitorizar uma VM com Linux](../linux/tutorial-monitoring.md).
- Configure a monitorização da infraestrutura do Azure com a extensão Chef ou Datadog. Para obter mais informações, consulte o [Chef docs](https://docs.chef.io/azure_portal.html) ou [Datadog blogue](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).

Para além das extensões de específicos de processos, uma extensão de Script personalizado está disponível para máquinas virtuais Windows e Linux. A extensão de Script personalizado para Linux permite que os scripts de Bash ser executado numa VM. Scripts personalizados são úteis para estruturar implementações do Azure que necessitam de configuração para além dos quais as nativa ferramentas do Azure podem fornecer. Para obter mais informações, consulte [a extensão de Script de personalizada de VM Linux](custom-script-linux.md).

## <a name="prerequisites"></a>Pré-requisitos

Para lidar com a extensão da VM, terá do agente Linux do Azure instalados. Algumas extensões individuais têm pré-requisitos, tais como o acesso a recursos ou dependências.

### <a name="azure-vm-agent"></a>Agente da VM do Azure

O agente da VM do Azure gere as interações entre uma VM do Azure e o controlador de recursos de infraestrutura do Azure. O agente da VM é responsável por muitos aspetos funcionais de implementar e gerir as VMs do Azure, incluindo a executar as extensões VM. O agente da VM do Azure está pré-instalado imagens do Azure Marketplace e pode ser instalado manualmente nos sistemas operativos suportados. O agente da VM do Azure para Linux é conhecido como o agente Linux.

Para obter informações sobre sistemas operativos suportados e instruções de instalação, consulte [agente da máquina virtual do Azure](agent-linux.md).

#### <a name="supported-agent-versions"></a>Versões do agente suportado

Para fornecer a melhor experiência possível, existem versões mínimas do agente. Para obter mais informações, consulte [este artigo](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

#### <a name="supported-oses"></a>Sos suportados

O agente Linux é executado em vários sos, no entanto, a arquitetura de extensões tem um limite para os sos que extensões. Para obter mais informações, consulte [neste artigo] (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Algumas extensões não são suportadas em todos os sos e pode emitir *51 de código de erro, 'SO não suportada'*. Consulte a documentação de extensões individuais para suporte.

#### <a name="network-access"></a>Acesso à rede

Pacotes de extensão são transferidos do repositório de extensões de armazenamento do Azure e carregamentos de estado de extensão são publicados no armazenamento do Azure. Se utilizar [suportado](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) versão dos agentes, não é necessário permitir o acesso ao Storage do Azure na região de VM, como pode utilizar o agente para redirecionar a comunicação com o controlador de recursos de infraestrutura do Azure para as comunicações do agente. Se tiver uma versão não suportada do agente, terá de permitir o acesso de saída para o armazenamento do Azure nessa região da VM.

> [!IMPORTANT]
> Se tiver bloqueou o acesso à *168.63.129.1* utilizando a firewall do convidado, em seguida, extensões falharem independentemente acima.

Agentes só podem ser utilizados para transferir os pacotes de extensão e comunicar o estado. Por exemplo, se uma instalação da extensão tem de transferir um script a partir do GitHub (de Script personalizado) ou necessita de acesso ao Storage do Azure (cópia de segurança do Azure), em seguida, adicionais firewall/rede segurança portas de grupo tem de ser aberta. As diferentes extensões têm requisitos diferentes, uma vez que estes são aplicações na sua própria direito. Para as extensões que necessitam de acesso ao Storage do Azure, pode permitir o acesso utilizando as etiquetas de serviço do Azure NSG para [armazenamento](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview#service-tags).

Para redirecionar os pedidos de tráfego de agente, o agente Linux tem suporte para o servidor proxy. No entanto, este suporte de servidor proxy não se aplica as extensões. Tem de configurar cada extensão individuais para trabalhar com um proxy.

## <a name="discover-vm-extensions"></a>Detetar as extensões de VM

Várias extensões VM diferentes estão disponíveis para utilização com as VMs do Azure. Para ver uma lista completa, utilize [lista de imagens de extensão de vm az](/cli/azure/vm/extension/image#az-vm-extension-image-list). O exemplo seguinte apresenta uma lista de todas as extensões disponíveis no *westus* localização:

```azurecli
az vm extension image list --location westus --output table
```

## <a name="run-vm-extensions"></a>Executar as extensões VM

Extensões VM do Azure é executada em VMs existentes, que é útil quando for necessário efetuar alterações de configuração ou recuperar conectividade numa VM já implementada. Extensões VM também podem ser incluídas com implementações de modelos Azure Resource Manager. Ao utilizar extensões com modelos do Resource Manager, VMs do Azure pode ser implementadas e configuradas sem a intervenção de pós-implementação.

Os métodos seguintes podem ser utilizados para executar uma extensão contra uma VM existente.

### <a name="azure-cli-20"></a>CLI 2.0 do Azure

Extensões VM do Azure podem ser executadas contra uma VM existente com o [conjunto de extensão de vm az](/cli/azure/vm/extension#az-vm-extension-set) comando. O exemplo seguinte executa a extensão de Script personalizado contra uma VM com o nome *myVM* num grupo de recursos denominado *myResourceGroup*:

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/me/project/hello.sh"],"commandToExecute": "./hello.sh"}'
```

Quando a extensão é executada corretamente, o resultado será semelhante ao seguinte exemplo:

```bash
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Portal do Azure

Extensões VM podem ser aplicadas a uma VM existente através do portal do Azure. Selecione a VM no portal, escolha **extensões**, em seguida, selecione **adicionar**. Escolha a extensão que pretende na lista de extensões disponíveis e siga as instruções no assistente.

A imagem seguinte mostra a instalação da extensão de Script personalizado de Linux do portal do Azure:

![Instalar a extensão de script personalizado](./media/features-linux/installscriptextensionlinux.png)

### <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Extensões VM podem ser adicionadas a um modelo Azure Resource Manager e executadas com a implementação do modelo. Quando implementa uma extensão com um modelo, pode criar totalmente configuradas implementações do Azure. Por exemplo, o seguinte JSON é obtido a partir de um modelo do Resource Manager que implementa um conjunto de VMs com balanceamento de carga e uma base de dados SQL do Azure, em seguida, instala uma aplicação .NET Core em cada VM. A extensão da VM encarrega-se de que a instalação de software.

Para obter mais informações, consulte o completo [modelo do Resource Manager](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Para obter mais informações sobre como criar modelos do Resource Manager, consulte [modelos Authoring Azure Resource Manager](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Proteger os dados de extensão VM

Quando executa uma extensão VM, poderá ser necessário incluir informações confidenciais, tais como as credenciais, os nomes das contas de armazenamento e chaves de acesso de conta de armazenamento. Várias extensões VM incluem uma configuração protegida que encripta os dados e apenas desencripta a mesma dentro da VM de destino. Cada extensão com um esquema de configuração protegido específica, e cada está detalhada na documentação de extensão específica.

O exemplo seguinte mostra uma instância da extensão de Script personalizado para o Linux. O comando a executar inclui um conjunto de credenciais. Neste exemplo, o comando a executar não está encriptado:

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Mover o **comando a executar** propriedade para o **protegidos** configuração protege a cadeia de execução, conforme mostrado no exemplo seguinte:

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

### <a name="how-do-agents-and-extensions-get-updated"></a>Como agentes e as extensões for atualizadas?

Os agentes e as extensões de partilham o mesmo mecanismo de atualização. Algumas atualizações não necessitam de regras de firewall adicional.

Quando estiver disponível uma atualização, está instalada na VM apenas quando há uma alteração às extensões e outras alterações de modelo de VM, tais como:

- Discos de dados
- Extensões
- Contentor de diagnóstico de arranque
- Segredos de SO convidado
- Tamanho da VM
- Perfil de rede

Os publicadores disponibilizar atualizações para regiões de em alturas diferentes, pelo que é possível que pode ter VMs em regiões diferentes em diferentes versões.

#### <a name="agent-updates"></a>Atualizações de agente

O agente de VM Linux contém *código do agente de aprovisionamento* e *código de processamento de extensão* um pacote, o que não é possível separados. Pode desativar o *aprovisionamento agente* quando pretender aprovisionar no Azure através de nuvem init. Para tal, consulte [utilizando nuvem init](../linux/using-cloud-init.md).

Versões suportadas dos agentes podem utilizar as atualizações automáticas. O código só pode ser atualizado está a *código de processamento de extensão*, não o código de aprovisionamento. O *código do agente de aprovisionamento* é o código de execução-uma vez.

O *código de processamento de extensão* é responsável para comunicar com os recursos de infraestrutura do Azure e processar, tais como as operações de extensões VM instala, comunicar o estado, atualizar as extensões individuais e removê-los. Atualizações contêm correções de segurança, correções de erros e melhoramentos para o *código de processamento de extensão*.

Quando o agente está instalado, é criado um daemon principal. Este principal, em seguida, cria um processo subordinado que é utilizado para processar as extensões. Se uma atualização fica disponível para o agente, é transferida, o principal interrompe o processo de subordinados, atualiza-lo, em seguida, reinicia-lo. Deve existir um problema com a atualização, o processo de principal reverte para a versão anterior do subordinado.

O processo de principal não pode ser automaticamente atualizado. O elemento principal só pode ser atualizado por uma atualização do pacote distro.

Para verificar a versão que está a executar, verifique o `waagent` da seguinte forma:

```bash
waagent --version
```

O resultado é semelhante ao seguinte exemplo:

```bash
WALinuxAgent-2.2.17 running on ubuntu 16.04
Python: 3.5.2
Goal state agent: 2.2.18
```

Na saída de exemplo anterior, é o principal ou 'pacote implementado versão' *WALinuxAgent 2.2.17*

O 'agente de estado de objetivos' é a versão de atualização automática.

É vivamente recomendado que terá sempre atualização automática para o agente, [AutoUpdate.Enabled=y](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent). Não ter significa ativada que tem de manter manualmente o agente de atualização e não a obter as correções de erros e de segurança.

#### <a name="extension-updates"></a>Atualizações de extensão

Quando estiver disponível uma atualização da extensão, o agente Linux transfere e atualiza a extensão. Atualizações automáticas de extensão são *secundárias* ou *correção*. Pode optar ativamente por participar no ou par excluir extensões *secundárias* atualizações quando Aprovisiona a extensão. O exemplo seguinte mostra como atualizar automaticamente as versões de secundárias num modelo do Resource Manager com *autoUpgradeMinorVersion ": true,'*:

```json
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
```

Para obter as correções de erros de versão secundária mais recentes, recomendamos que selecione sempre atualização automática das implementações de extensão. Atualizações de correção que transportar as correções de erros de segurança ou de chave não podem ser excluídas.

### <a name="how-to-identify-extension-updates"></a>Como identificar as atualizações de extensão

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Identificar se a extensão está definida com a etiqueta autoUpgradeMinorVersion numa VM

Pode ver o modelo de VM se a extensão foi aprovisionada com 'autoUpgradeMinorVersion'. Para verificar, utilize [mostrar de vm az](/cli/azure/vm#az-vm-show) e forneça o nome do grupo de recursos e VM da seguinte forma:

```azurecli
az vm show --resource-group myResourceGroup --name myVM
```

O seguinte resultado de exemplo mostra que *autoUpgradeMinorVersion* está definido como *verdadeiro*:

```json
  "resources": [
    {
      "autoUpgradeMinorVersion": true,
      "forceUpdateTag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/CustomScriptExtension",
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Identificar quando ocorreu uma etiqueta autoUpgradeMinorVersion

Para ver quando ocorreu uma atualização para a extensão, reveja o agente de registos na VM em */var/log/waagent.log*.

No exemplo abaixo, a VM tinha *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9025* instalado. Uma correção estava disponível para *Microsoft.OSTCExtensions.LinuxDiagnostic 2.3.9027*:

```bash
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Expected handler state: enabled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Decide which version to use
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Use version: 2.3.9027
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Current handler state is: NotInstalled
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Download extension package
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Unpack extension package
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Download, message=Download succeeded
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Initialize extension directory
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update settings file: 0.settings
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Disable extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9025] Launch command:diagnostic.py -disable
...
INFO Event: name=Microsoft.OSTCExtensions.LinuxDiagnostic, op=Disable, message=Launch command succeeded: diagnostic.py -disable
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Update extension.
INFO [Microsoft.OSTCExtensions.LinuxDiagnostic-2.3.9027] Launch command:diagnostic.py -update
2017/08/14 20:21:57 LinuxAzureDiagnostic started to handle.
```

## <a name="agent-permissions"></a>Permissões de agente

Para efetuar as respetivas tarefas, o agente tem de ser executado como *raiz*.

## <a name="troubleshoot-vm-extensions"></a>Resolver problemas de extensões VM

Cada extensão VM poderá ter passos específicos para a extensão de resolução de problemas. Por exemplo, quando utiliza a extensão de Script personalizado, detalhes de execução do script podem ser encontrados localmente na VM em que a extensão foi executada. Quaisquer passos de resolução de problemas específicos da extensão estão descritos na documentação de extensão específica.

Os passos de resolução de problemas seguintes aplicam-se a todas as extensões VM.

1. Para verificar o registo do agente Linux, observe a atividade quando a extensão estava a ser aprovisionado no */var/log/waagent.log*

2. Verifique os registos de extensão real para obter mais detalhes no   */var/iniciar/azure /<extensionName>*

3. Consulte a documentação específica da extensão de secções de códigos de erro, etc de problemas conhecidos de resolução de problemas.

3. Consulte os registos de sistema. Verificação das outras operações que pode ter interfered com a extensão, tal como uma instalação de execução demorada de outra aplicação que é necessário acesso de Gestor de pacote exclusivo.

### <a name="common-reasons-for-extension-failures"></a>Os motivos mais comuns de falhas de extensão

1. Extensões têm de 20 minutos para executar (exceções são o CustomScript extensões, Chef e DSC com 90 minutos). Se a implementação exceder neste momento, está marcado como um tempo limite. A causa desta situação pode dever-se recursos baixo VMs, outro VM configurações/arranque tarefas consumir elevadas quantidades de recursos, embora a extensão está a tentar aprovisionar.

2. Pré-requisitos mínimos não foram cumpridos. Algumas extensões terem dependências de SKUs de VM, tais como imagens HPC. As extensões podem exigir determinados requisitos de acesso redes, tais como a comunicação com o Storage do Azure ou serviços de público. Outros exemplos podem ser acesso para os repositórios de pacote, a ficar sem espaço em disco ou restrições de segurança.

3. Acesso do Gestor de pacote exclusivo. Em alguns casos, poderá encontrar uma execução demorada de VM de configuração e instalação da extensão em conflito, em que ambos os necessitam de acesso exclusivo para o Gestor de pacotes.

### <a name="view-extension-status"></a>Ver o estado de extensão

Depois de uma extensão VM tiver sido executada contra uma VM, utilize [az vm get--vista de instância](/cli/azure/vm#az-vm-get-instance-view) para devolver o estado da extensão da seguinte forma:

```azurecli
az vm get-instance-view \
    --resource-group rgName \
    --name myVM \
    --query "instanceView.extensions"
```

O resultado será semelhante ao seguinte exemplo de saída:

```bash
  {
    "name": "customScript",
    "statuses": [
      {
        "code": "ProvisioningState/failed/0",
        "displayStatus": "Provisioning failed",
        "level": "Error",
        "message": "Enable failed: failed to execute command: command terminated with exit status=127\n[stdout]\n\n[stderr]\n/bin/sh: 1: ech: not found\n",
        "time": null
      }
    ],
    "substatuses": null,
    "type": "Microsoft.Azure.Extensions.customScript",
    "typeHandlerVersion": "2.0.6"
  }
```

Também é possível encontrar o estado de execução da extensão no portal do Azure. Para ver o estado de uma extensão, selecione a VM, escolha **extensões**, em seguida, selecione a extensão pretendida.

### <a name="rerun-a-vm-extension"></a>Executar novamente uma extensão VM

Pode haver casos em que uma extensão VM tem de ser executada novamente. Pode executar novamente uma extensão, removendo-a e, em seguida, execute novamente a extensão com um método de execução da sua preferência. Para remover uma extensão, utilize [delete de extensão de vm az](/cli/azure/vm/extension#az-vm-extension-delete) da seguinte forma:

```azurecli
az vm extension delete \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --name customScript
```

Pode também remover uma extensão no portal do Azure da seguinte forma:

1. Selecione uma VM.
2. Escolha **extensões**.
3. Selecione a extensão pretendida.
4. Escolha **desinstalar**.

## <a name="common-vm-extension-reference"></a>Referência de extensão VM comuns

| Nome de extensão | Descrição | Mais informações |
| --- | --- | --- |
| Extensão de Script personalizada para Linux |Executar scripts em relação a uma máquina virtual do Azure |[Extensão de Script personalizada para Linux](custom-script-linux.md) |
| Extensão de acesso da VM |Recuperar o acesso a uma máquina virtual do Azure |[Extensão de acesso da VM](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Extensão de diagnóstico do Azure |Gerir o diagnóstico do Azure |[Extensão de diagnóstico do Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Extensão de acesso de VM do Azure |Gerir utilizadores e as credenciais |[Extensão de acesso de VM para Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as extensões VM, consulte [descrição geral de funcionalidades e as extensões de máquina virtual do Azure](overview.md).
