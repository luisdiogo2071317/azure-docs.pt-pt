---
title: Criar um cluster do Service Fabric do Windows no Azure | Microsoft Docs
description: Neste tutorial, saiba como implementar um cluster do Service Fabric do Windows numa rede virtual do Azure e no grupo de segurança de rede com o PowerShell.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/27/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: a720bb906192731b8b636939e22b13a8e52bbe76
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52632896"
---
# <a name="tutorial-deploy-a-service-fabric-windows-cluster-into-an-azure-virtual-network"></a>Tutorial: Implementar um cluster do Service Fabric do Windows numa rede virtual do Azure

Este tutorial é a primeira parte de uma série. Ficará a saber como implementar um cluster do Service Fabric do Windows numa [rede virtual do Azure (VNET)](../virtual-network/virtual-networks-overview.md) e o [grupo de segurança de rede](../virtual-network/virtual-networks-nsg.md) utilizando o PowerShell e um modelo. Quando tiver terminado, terá um cluster em execução na cloud, no qual poderá implementar aplicações.  Para criar um cluster do Linux através do CLI do Azure, consulte [Criar um cluster do Linux seguro no Azure](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

Este tutorial descreve um cenário de produção.  Se quiser rapidamente a criar um cluster mais pequeno para fins de teste, consulte [criar um cluster de teste](./scripts/service-fabric-powershell-create-secure-cluster-cert.md).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma VNET no Azure com o PowerShell
> * Criar um cofre de chaves e carregar um certificado
> * Criar um cluster do Service Fabric seguro no Azure PowerShell
> * Proteger o cluster com um certificado X.509
> * Ligar ao cluster com o PowerShell
> * Remover um cluster

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * Criar um cluster seguro no Azure
> * [Reduzir ou aumentar um cluster horizontalmente](service-fabric-tutorial-scale-cluster.md)
> * [Atualizar o tempo de execução de um cluster](service-fabric-tutorial-upgrade-cluster.md)
> * [Eliminar um cluster](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Instalar o [módulo de PowerShell e SDK do Service Fabric](service-fabric-get-started.md)
* Instalar o [módulo do Azure PowerShell versão 4.1 ou superior](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Os procedimentos seguintes criam um cluster do Service Fabric com cinco nós. Para calcular o custo incorrido pela execução de um cluster do Service Fabric no Azure, utilize a [Calculadora de Preços do Azure](https://azure.microsoft.com/pricing/calculator/).

## <a name="key-concepts"></a>Conceitos-chave

Um [cluster do Service Fabric](service-fabric-deploy-anywhere.md) é um conjunto ligado à rede de máquinas virtuais ou físicas, no qual os microsserviços são implementados e geridos. Os clusters podem ser dimensionados para milhares de máquinas. Uma máquina ou VM que faça parte de um cluster é denominada um nó. É atribuído um nome de nó (uma cadeia) a cada nó. Os nós têm características, como as propriedades de colocação.

Um tipo de nó define o tamanho, o número e as propriedades de um conjunto de máquinas virtuais no cluster. Cada tipo de nó definido está configurado como um [conjunto de dimensionamento da máquina virtual](/azure/virtual-machine-scale-sets/), um recurso de computação do Azure que é utilizado para implementar e gerir uma coleção de máquinas virtuais como um conjunto. Cada tipo de nó pode então ser aumentado ou reduzido verticalmente de forma independente, pode ter conjuntos diferentes de portas abertas e ter métricas de capacidade diferente. Os tipos de nó são utilizados para definir funções para um conjunto de nós de cluster, como o "front-end" ou o "back-end".  O cluster pode ter mais do que um tipo de nó, mas o tipo de nó primário tem de ter, pelo menos, cinco VMs para clusters de produção (ou, pelo menos, três VMs para clusters de teste).  Os [serviços do sistema do Service Fabric](service-fabric-technical-overview.md#system-services) são colocados em nós do tipo de nó primário.

O cluster está protegido por um certificado de cluster. Um certificado de cluster é um certificado X.509 utilizado para proteger a comunicação entre nós e autenticar os pontos finais de gestão do cluster para um cliente de gestão.  O certificado de cluster também fornece um SSL para a API de gestão HTTPS e para o Service Fabric Explorer através de HTTPS. Os certificados autoassinados são úteis nos clusters de teste.  Para clusters de produção, utilize um certificado de uma autoridade de certificação (AC) como o certificado de cluster.

O certificado de cluster tem de:

* conter uma chave privada.
* ser criado para troca de chaves, que é exportável para um ficheiro Personal Information Exchange (.pfx).
* ter um nome de requerente que corresponde ao domínio que utiliza para aceder ao cluster do Service Fabric. Esta correspondência é necessária para fornecer o SSL para os pontos finais de gestão HTTPS do cluster e o Service Fabric Explorer. Não é possível obter um certificado SSL de uma autoridade de certificação (AC) para o domínio cloudapp.azure.com. Tem de obter um nome de domínio personalizado para o cluster. Quando pedir um certificado de uma AC, o nome de requerente do certificado tem de corresponder ao nome do domínio personalizado que utilizar para o cluster.

O Azure Key Vault é utilizado para gerir os certificados de clusters do Service Fabric no Azure.  Quando um cluster é implementado no Azure, o fornecedor de recursos do Azure responsável pela criação de clusters do Service Fabric obtém os certificados do Key Vault e instala-os nas VMs do cluster.

Este tutorial apresenta um cluster com cinco nós num tipo de nó único. Para qualquer implementação de clusters de produção, no entanto, o [planeamento da capacidade](service-fabric-cluster-capacity.md) é um passo importante. Seguem-se alguns aspetos a considerar como parte do processo.

* O número de nós e os tipos de nó de que o cluster precisa
* As propriedades de cada tipo de nó (por exemplo, o tamanho, o nó primário, com acesso à Internet e o número de VMs)
* As características de fiabilidade e durabilidade do cluster

## <a name="download-and-explore-the-template"></a>Transferir e explorar o modelo

Transfira os seguintes ficheiros de modelos do Resource Manager:

* [azuredeploy.json][template]
* [azuredeploy.parameters.json][parameters]

Este modelo implementa um cluster seguro de cinco máquinas virtuais e um tipo de nó único numa rede virtual e um grupo de segurança de rede.  Outros modelos de exemplo podem ser encontrados no [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates).  O [azuredeploy.json][template] implementa vários recursos, incluindo os seguintes.

### <a name="service-fabric-cluster"></a>Cluster do Service Fabric

No recurso **Microsoft.ServiceFabric/clusters**, é configurado um cluster Windows com as seguintes características:

* um tipo de nó único
* cinco nós no tipo de nó primário (configurável nos parâmetros do modelo)
* SO: Windows Server 2016 Datacenter com Contentores (configurável nos parâmetros do modelo)
* protegido por certificado (configurável nos parâmetros do modelo)
* O [proxy inverso](service-fabric-reverseproxy.md) está ativado
* O [serviço DNS](service-fabric-dnsservice.md) está ativado
* [Nível de durabilidade](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) de Bronze (configurável nos parâmetros do modelo)
* [Nível de fiabilidade](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) de Prata (configurável nos parâmetros do modelo)
* ponto final de ligação de cliente: 19000 (configurável nos parâmetros do modelo)
* ponto final de gateway HTTP: 19080 (configurável nos parâmetros do modelo)

### <a name="azure-load-balancer"></a>Balanceador de carga do Azure

No recurso **Microsoft.Network/loadBalancers**, é configurado um balanceador de carga e são configuradas pesquisas e regras para as seguintes portas:

* ponto final de ligação de cliente: 19000
* ponto final de gateway HTTP: 19080
* porta da aplicação: 80
* porta da aplicação: 443
* Proxy inverso do Service Fabric: 19081

Se forem necessárias quaisquer outras portas da aplicação, terá de ajustar o recurso **Microsoft.Network/loadBalancers** e o recurso **Microsoft.Network/networkSecurityGroups** para permitir o tráfego de entrada.

### <a name="virtual-network-subnet-and-network-security-group"></a>Rede virtual, sub-rede e o grupo de segurança de rede

Os nomes da rede virtual, da sub-rede e do grupo de segurança de rede são declarados nos parâmetros do modelo.  Os espaços de endereços da rede virtual e da sub-rede também são declarados nos parâmetros do modelo e configurados no recurso **Microsoft.Network/virtualNetworks**:

* espaço de endereços de rede virtual: 172.16.0.0/20
* espaço de endereços de sub-rede do Service Fabric: 172.16.2.0/23

As seguintes regras de tráfego de entrada estão ativadas no recurso **Microsoft.Network/networkSecurityGroups**. Pode alterar os valores das portas, alterando as variáveis no modelo.

* ClientConnectionEndpoint (TCP): 19000
* HttpGatewayEndpoint (HTTP/TCP): 19080
* SMB : 445
* Comunicação entre nós - 1025, 1026, 1027
* Intervalo de portas efémeras – da 49152 até à 65534 (precisa de um mínimo de 256 portas)
* Portas para utilização de aplicações: 80 e 443
* Intervalo de portas da aplicação – da 49152 até à 65534 (utilizadas para comunicação de serviços e, ao contrário, não estão abertas no Balanceador de carga)
* Bloquear todas as outras portas

Se forem necessárias quaisquer outras portas da aplicação, terá de ajustar o recurso **Microsoft.Network/loadBalancers** e o recurso **Microsoft.Network/networkSecurityGroups** para permitir o tráfego de entrada.

## <a name="set-template-parameters"></a>Definir parâmetros de modelo

O ficheiro de parâmetro [azuredeploy.parameters.json][parameters] declara vários valores utilizados para implementar o cluster e os recursos associados. Alguns dos parâmetros que poderá ser necessário modificar para a sua implementação:

|Parâmetro|Valor de exemplo|Notas|
|---|---||
|adminUserName|vmadmin| O nome de utilizador de administrador para as VMs de cluster.[Requisitos de nome de utilizador para VM](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-username-requirements-when-creating-a-vm) |
|adminPassword|Password#1234| A palavra-passe de utilizador administrador para as VMs do cluster. [Requisitos de palavra-passe para a VM](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)|
|clusterName|mysfcluster123| O nome do cluster. Pode conter apenas letras e números. O comprimento pode ter entre 3 e 23 carateres.|
|localização|southcentralus| A localização do cluster. |
|certificateThumbprint|| <p>O valor deve estar vazio, se criar um certificado autoassinado ou fornecer um ficheiro de certificado.</p><p>Para utilizar um certificado existente carregado anteriormente para um cofre de chaves, preencha o valor do thumbprint SHA-1 do certificado. Por exemplo, "6190390162C988701DB5676EB81083EA608DCCF3"</p>. |
|certificateUrlValue|| <p>O valor deve estar vazio, se criar um certificado autoassinado ou fornecer um ficheiro de certificado. </p><p>Para utilizar um certificado existente carregado anteriormente para um cofre de chaves, preencha o URL do certificado. Por exemplo, "https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>O valor deve estar vazio, se criar um certificado autoassinado ou fornecer um ficheiro de certificado.</p><p>Para utilizar um certificado existente carregado anteriormente para um cofre de chaves, preencha o valor no cofre de origem. Por exemplo, "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Implementar a rede virtual e o cluster

Em seguida, configure a topologia de rede e implemente o cluster do Service Fabric. O modelo [azuredeploy.json][template] do Resource Manager cria uma rede virtual (VNET) e também um grupo de segurança de sub-rede e rede (NSG) para o Service Fabric. O modelo também implementa um cluster com a segurança do certificado ativada.  Para clusters de produção, utilize um certificado de uma autoridade de certificação (AC) como o certificado de cluster. Um certificado autoassinado pode ser utilizado para proteger clusters de teste.

### <a name="create-a-cluster-using-an-existing-certificate"></a>Criar um cluster através de um certificado existente

O seguinte script utiliza o cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) e um modelo para implementar um novo cluster no Azure. O cmdlet também cria um novo cofre de chaves no Azure e carrega o certificado.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$clustername = "mysfcluster123"  # must match the clusterName parameter in the template
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateFile $certpath
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Criar um cluster ao utilizar um novo certificado autoassinado

O seguinte script utiliza o cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) e um modelo para implementar um novo cluster no Azure. O cmdlet também cria um novo cofre de chaves no Azure, adiciona um novo certificado autoassinado ao cofre de chaves e transfere o ficheiro de certificado localmente.

```powershell
# Variables.
$groupname = "sfclustertutorialgroup"
$clusterloc="southcentralus"  # must match the location parameter in the template
$templatepath="C:\temp\cluster"

$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
$certfolder="c:\mycertificates\"
$clustername = "mysfcluster123"
$vaultname = "clusterkeyvault123"
$vaultgroupname="clusterkeyvaultgroup123"
$subname="$clustername.$clusterloc.cloudapp.azure.com"

# sign in to your Azure account and select your subscription
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Create the Service Fabric cluster.
New-AzureRmServiceFabricCluster  -ResourceGroupName $groupname -TemplateFile "$templatepath\azuredeploy.json" `
-ParameterFile "$templatepath\azuredeploy.parameters.json" -CertificatePassword $certpwd `
-CertificateOutputFolder $certfolder -KeyVaultName $vaultname -KeyVaultResourceGroupName $vaultgroupname -CertificateSubjectName $subname

```

## <a name="connect-to-the-secure-cluster"></a>Ligar ao cluster seguro

Ligue ao cluster ao utilizar o módulo do Service Fabric do PowerShell instalado com o SDK do Service Fabric.  Primeiro, instale o certificado no arquivo Pessoal (Meu) do utilizador atual no seu computador.  Execute o seguinte comando do PowerShell:

```powershell
$certpwd="q6D7nN%6ck@6" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Está agora pronto para ligar ao seu cluster seguro.

O módulo **Service Fabric** do PowerShell fornece muitos cmdlets para gerir clusters, aplicações e serviços do Service Fabric.  Utilize o cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) para ligar ao cluster seguro. Os detalhes do thumbprint SHA-1 do certificado e ponto final de ligação encontram-se nos resultados do passo anterior.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Certifique-se de que está ligado e o cluster está em bom estado através do cmdlet [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth).

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Limpar recursos

Os outros artigos nesta série de tutoriais utilizam o cluster que acabou de criar. Se não passar imediatamente para o artigo seguinte, poderá [eliminar o cluster](service-fabric-cluster-delete.md) para evitar incorrer em custos.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma VNET no Azure com o PowerShell
> * Criar um cofre de chaves e carregar um certificado
> * Criar um cluster do Service Fabric seguro no Azure com o PowerShell
> * Proteger o cluster com um certificado X.509
> * Ligar ao cluster com o PowerShell
> * Remover um cluster

Em seguida, avance para o tutorial seguinte para saber como dimensionar o seu cluster.
> [!div class="nextstepaction"]
> [Dimensionar um Cluster](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Windows-1-NodeTypes-Secure-NSG/azuredeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Windows-1-NodeTypes-Secure-NSG/azuredeploy.parameters.json
