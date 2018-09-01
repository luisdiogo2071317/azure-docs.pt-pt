---
title: Configurar uma rede Virtual do Azure (clássico) - ficheiro de configuração de rede | Documentos da Microsoft
description: Saiba como criar e modificar redes virtuais (clássico) através da exportação, alterar e importar um ficheiro de configuração de rede.
services: virtual-network
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.custom: ''
ms.openlocfilehash: 57ad5541bb7b61f8d26002168bb069fad3058965
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339080"
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Configurar uma rede virtual através de um ficheiro de configuração de rede (clássico)
> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Este artigo cobre a utilização do modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo de implementação do Resource Manager.

Pode criar e configurar uma rede virtual (clássico) com um arquivo de configuração de rede usando a interface de linha de comandos (CLI) 1.0 do Azure ou o Azure PowerShell. Não é possível criar ou modificar uma rede virtual por meio do modelo de implementação Azure Resource Manager através de um ficheiro de configuração de rede. Não é possível utilizar o portal do Azure para criar ou modificar uma rede virtual (clássico) com um ficheiro de configuração de rede, no entanto, pode utilizar o portal do Azure para criar uma rede virtual (clássico), sem utilizar um ficheiro de configuração de rede.

Criar e configurar uma rede virtual (clássico) com um ficheiro de configuração de rede requerem a exportar, alterar e importação do ficheiro.

## <a name="export"></a>Exportar um ficheiro de configuração de rede

Pode utilizar o PowerShell ou a CLI do Azure para exportar um ficheiro de configuração de rede. PowerShell exporta um arquivo XML, embora a CLI do Azure exporta um ficheiro json.

### <a name="powershell"></a>PowerShell
 
1. [Instalar o Azure PowerShell e inicie sessão no Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. Altere o diretório (e certifique-se existir) e nome de ficheiro no comando seguinte conforme pretendido, em seguida, execute o comando para exportar o ficheiro de configuração de rede:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>CLI do Azure 1.0

1. [Instalar a CLI do Azure 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Conclua os passos restantes de uma linha de comandos da CLI 1.0 do Azure.
2. Inicie sessão no Azure ao introduzir o `azure login` comando.
3. Certifique-se de que está no modo asm introduzindo o `azure config mode asm` comando.
4. Altere o diretório (e certifique-se existir) e nome de ficheiro no comando seguinte conforme pretendido, em seguida, execute o comando para exportar o ficheiro de configuração de rede:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Criar ou modificar um ficheiro de configuração de rede

Um ficheiro de configuração de rede é um arquivo XML (ao utilizar o PowerShell) ou um ficheiro json (ao utilizar a CLI do Azure). Pode editar o ficheiro em qualquer linha de texto ou editor de XML/json. O [definições de esquema de ficheiro de configuração de rede](https://msdn.microsoft.com/library/azure/jj157100.aspx) artigo inclui detalhes para todas as definições. Para obter uma explicação adicional das definições, consulte [ver redes virtuais e as definições](manage-virtual-network.md#view-virtual-networks-and-settings). As alterações que fizer para o ficheiro:

- Deve estar em conformidade com o esquema, ou ao importar que o ficheiro de configuração de rede irá falhar.
- Substituindo quaisquer configurações de rede existente para a sua subscrição, por isso, tenha muito cuidado ao fazer modificações. Por exemplo, referencie os ficheiros de configuração de rede de exemplo que se seguem. Digamos que o ficheiro original contido dois **VirtualNetworkSite** instâncias e alterou, conforme mostrado nos exemplos. Quando importar o ficheiro, o Azure elimina a rede virtual para o **VirtualNetworkSite** removido no arquivo de instância. Esse cenário simplificado pressupõe que não existem recursos foram na rede virtual, como se estivesse, não foi possível eliminar a rede virtual e a importação falhará.

> [!IMPORTANT]
> Uma sub-rede que tem algo implementadas no mesmo como o Azure considera **em utilização**. Quando uma sub-rede está a ser utilizado, não pode ser modificado. Antes de modificar informações de sub-rede num arquivo de configuração de rede, é necessário mova nada que tenha implementado para a sub-rede para uma sub-rede diferente que não está a ser modificada. Ver [mover uma VM ou instância de função a uma sub-rede diferente](virtual-networks-move-vm-role-to-subnet.md) para obter detalhes.

### <a name="example-xml-for-use-with-powershell"></a>XML de exemplo para uso com o PowerShell

O ficheiro de configuração de rede de exemplo seguinte cria uma rede virtual denominada *myVirtualNetwork* com um espaço de endereços de *10.0.0.0/16* no *E.U.A. Leste* Azure região. A rede virtual contém uma sub-rede designada *mySubnet* com o prefixo de endereço *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

Se o ficheiro de configuração de rede que exportou não contiver nenhum conteúdo, pode copiar o XML no exemplo anterior e cole-o num novo arquivo.

### <a name="example-json-for-use-with-the-azure-cli-10"></a>JSON de exemplo para uso com a CLI 1.0 do Azure

O ficheiro de configuração de rede de exemplo seguinte cria uma rede virtual denominada *myVirtualNetwork* com um espaço de endereços de *10.0.0.0/16* no *E.U.A. Leste* Azure região. A rede virtual contém uma sub-rede designada *mySubnet* com o prefixo de endereço *10.0.0.0/24*.

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

Se o ficheiro de configuração de rede que exportou não contiver nenhum conteúdo, pode copiar o json no exemplo anterior e cole-o num novo arquivo.

## <a name="import"></a>Importar um ficheiro de configuração de rede

Pode utilizar o PowerShell ou a CLI do Azure para importar um ficheiro de configuração de rede. PowerShell importa um arquivo XML, embora a CLI do Azure importa um ficheiro json. Se a importação falhar, confirme que o ficheiro está em conformidade com o [esquema de configuração de rede](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Instalar o Azure PowerShell e inicie sessão no Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. Altere o diretório e o nome de ficheiro no comando seguinte, se necessário, em seguida, execute o comando para importar o ficheiro de configuração de rede:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>CLI do Azure 1.0

1. [Instalar a CLI do Azure 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Conclua os passos restantes de uma linha de comandos da CLI 1.0 do Azure.
2. Inicie sessão no Azure ao introduzir o `azure login` comando.
3. Certifique-se de que está no modo asm introduzindo o `azure config mode asm` comando.
4. Altere o diretório e o nome de ficheiro no comando seguinte, se necessário, em seguida, execute o comando para importar o ficheiro de configuração de rede:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
