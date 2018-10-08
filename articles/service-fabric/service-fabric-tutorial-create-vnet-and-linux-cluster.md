---
title: Criar um cluster do Service Fabric do Linux no Azure | Microsoft Docs
description: Neste tutorial, saiba como implementar um cluster do Service Fabric do Linux numa rede virtual do Azure existente, com a CLI do Azure.
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
ms.openlocfilehash: 27600cd4656f70b4cd01745667c0e0fd2a2f4997
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405824"
---
# <a name="tutorial-deploy-a-linux-service-fabric-cluster-into-an-azure-virtual-network"></a>Tutorial: Implementar um cluster do Service Fabric do Linux numa rede virtual do Azure

Este tutorial é a primeira parte de uma série. Ficará a saber como implementar um cluster do Service Fabric do Linux numa [rede virtual do Azure (VNET)](../virtual-network/virtual-networks-overview.md) com a CLI do Azure e um modelo. Quando tiver terminado, terá um cluster em execução na cloud, no qual poderá implementar aplicações. Para criar um cluster do Windows com o PowerShell, veja [Criar um cluster do Windows seguro no Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma VNET no Azure com a CLI do Azure
> * Criar um cluster do Service Fabric seguro no Azure com a CLI do Azure
> * Proteger o cluster com um certificado x. 509
> * Ligar ao cluster com a CLI do Service Fabric
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
* Instale a [CLI do Service Fabric](service-fabric-cli.md)
* Instalar a [CLI do Azure](/cli/azure/install-azure-cli)

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

Este tutorial implementa um cluster com cinco nós num tipo de nó único. Para qualquer implementação de clusters de produção, no entanto, o [planeamento da capacidade](service-fabric-cluster-capacity.md) é um passo importante. Seguem-se alguns aspetos a considerar como parte do processo.

* O número de nós e os tipos de nó de que o cluster precisa
* As propriedades de cada tipo de nó (por exemplo, o tamanho, o nó primário, com acesso à Internet e o número de VMs)
* As características de fiabilidade e durabilidade do cluster

## <a name="download-and-explore-the-template"></a>Transferir e explorar o modelo

Transfira os seguintes ficheiros de modelos do Resource Manager:

* [AzureDeploy.json][template]
* [AzureDeploy.Parameters.json][parameters]

Este modelo implementa um cluster seguro de cinco máquinas virtuais e um tipo de nó único numa rede virtual.  Outros modelos de exemplo podem ser encontrados no [GitHub](https://github.com/Azure-Samples/service-fabric-cluster-templates). O [AzureDeploy.json][template] implementa vários recursos, incluindo os seguintes.

### <a name="service-fabric-cluster"></a>Cluster do Service Fabric

Um cluster do Linux é implementado com as seguintes características:

* um tipo de nó único
* cinco nós no tipo de nó primário (configurável nos parâmetros do modelo)
* SO: Ubuntu 16.04 LTS (configurável nos parâmetros do modelo)
* protegido por certificado (configurável nos parâmetros do modelo)
* O [serviço DNS](service-fabric-dnsservice.md) está ativado
* [Nível de durabilidade](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) de Bronze (configurável nos parâmetros do modelo)
* [Nível de fiabilidade](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) de Prata (configurável nos parâmetros do modelo)
* ponto final de ligação de cliente: 19000 (configurável nos parâmetros do modelo)
* ponto final de gateway HTTP: 19080 (configurável nos parâmetros do modelo)

### <a name="azure-load-balancer"></a>Balanceador de carga do Azure

Um balanceador de carga é implementado, e são configuradas sondas e regras para as seguintes portas:

* ponto final de ligação de cliente: 19000
* ponto final de gateway HTTP: 19080
* porta da aplicação: 80
* porta da aplicação: 443

### <a name="virtual-network-and-subnet"></a>Rede virtual e sub-rede

Os nomes da rede virtual e sub-rede são declarados nos parâmetros do modelo.  Os espaços de endereços da rede virtual e sub-rede também são declarados nos parâmetros do modelo:

* espaço de endereços de rede virtual: 10.0.0.0/16
* espaço de endereços de sub-rede do Service Fabric: 10.0.2.0/24

Se forem necessárias quaisquer outras portas da aplicação, terá de ajustar o recurso Microsoft.Network/loadBalancers para permitir o tráfego nas mesmas.

## <a name="set-template-parameters"></a>Definir parâmetros de modelo

O ficheiro de parâmetro [AzureDeploy.Parameters][parameters] declara vários valores utilizados para implementar o cluster e os recursos associados. Alguns dos parâmetros que poderá ser necessário modificar para a sua implementação:

|Parâmetro|Valor de exemplo|Notas|
|---|---||
|adminUserName|vmadmin| O nome de utilizador administrador para as VMs do cluster. |
|adminPassword|Password#1234| A palavra-passe de utilizador administrador para as VMs do cluster.|
|clusterName|mysfcluster123| O nome do cluster. |
|localização|southcentralus| A localização do cluster. |
|certificateThumbprint|| <p>O valor deve estar vazio, se criar um certificado autoassinado ou fornecer um ficheiro de certificado.</p><p>Para utilizar um certificado existente carregado anteriormente para um cofre de chaves, preencha o valor do thumbprint SHA-1 do certificado. Por exemplo, “6190390162C988701DB5676EB81083EA608DCCF3”. </p>|
|certificateUrlValue|| <p>O valor deve estar vazio, se criar um certificado autoassinado ou fornecer um ficheiro de certificado.</p><p>Para utilizar um certificado existente carregado anteriormente para um cofre de chaves, preencha o URL do certificado. Por exemplo, "https://mykeyvault.vault.azure.net:443/secrets/mycertificate/02bea722c9ef4009a76c5052bcbf8346".</p>|
|sourceVaultValue||<p>O valor deve estar vazio, se criar um certificado autoassinado ou fornecer um ficheiro de certificado.</p><p>Para utilizar um certificado existente carregado anteriormente para um cofre de chaves, preencha o valor no cofre de origem. Por exemplo, "/subscriptions/333cc2c84-12fa-5778-bd71-c71c07bf873f/resourceGroups/MyTestRG/providers/Microsoft.KeyVault/vaults/MYKEYVAULT".</p>|

<a id="createvaultandcert" name="createvaultandcert_anchor"></a>

## <a name="deploy-the-virtual-network-and-cluster"></a>Implementar a rede virtual e o cluster

Em seguida, configure a topologia de rede e implemente o cluster do Service Fabric. O modelo do Resource Manager [AzureDeploy.json][template] cria uma rede virtual (VNET) e uma sub-rede para o Service Fabric. O modelo também implementa um cluster com a segurança do certificado ativada.  Para clusters de produção, utilize um certificado de uma autoridade de certificação (AC) como o certificado de cluster. Um certificado autoassinado pode ser utilizado para proteger clusters de teste.

### <a name="create-a-cluster-using-an-existing-certificate"></a>Criar um cluster através de um certificado existente

O script seguinte utiliza o comando [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) e o modelo para implementar um novo cluster protegido por um certificado existente. O comando também cria um novo cofre de chaves no Azure e carrega o certificado.

```azurecli
ResourceGroupName="sflinuxclustergroup"
Location="southcentralus"
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates\MyCertificate.pem"

# sign in to your Azure account and select your subscription
az login
az account set --subscription <guid>

# Create a new resource group for your deployment and give it a name and a location.
az group create --name $ResourceGroupName --location $Location

# Create the Service Fabric cluster.
az sf cluster create --resource-group $ResourceGroupName --location $Location \
   --certificate-password $Password --certificate-file $CertPath \
   --vault-name $VaultName --vault-resource-group $ResourceGroupName  \
   --template-file AzureDeploy.json --parameter-file AzureDeploy.Parameters.json
```

### <a name="create-a-cluster-using-a-new-self-signed-certificate"></a>Criar um cluster ao utilizar um novo certificado autoassinado

O script seguinte utiliza o comando [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) e o modelo para implementar um novo cluster no Azure. O cmcommand também cria um novo cofre de chaves no Azure, adiciona um novo certificado autoassinado ao cofre de chaves e transfere o ficheiro de certificado localmente.

```azurecli
ResourceGroupName="sflinuxclustergroup"
ClusterName="sflinuxcluster"
Location="southcentralus"
Password="q6D7nN%6ck@6"
VaultName="linuxclusterkeyvault"
VaultGroupName="linuxclusterkeyvaultgroup"
CertPath="C:\MyCertificates"

az sf cluster create --resource-group $ResourceGroupName --location $Location --cluster-name $ClusterName --template-file C:\temp\cluster\AzureDeploy.json --parameter-file C:\temp\cluster\AzureDeploy.Parameters.json --certificate-password $Password --certificate-output-folder $CertPath --certificate-subject-name $ClusterName.$Location.cloudapp.azure.com --vault-name $VaultName --vault-resource-group $ResourceGroupName
```

## <a name="connect-to-the-secure-cluster"></a>Ligar ao cluster seguro

Ligue ao cluster com o comando `sfctl cluster select` da CLI do Service Fabric e a sua chave.  Tenha em atenção, utilize apenas a opção **--no-verify** para um certificado autoassinado.

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Certifique-se de que está ligado e que o cluster está em bom estado de funcionamento, com o comando `sfctl cluster health`.

```azurecli
sfctl cluster health
```

## <a name="clean-up-resources"></a>Limpar recursos

Os outros artigos nesta série de tutoriais utilizam o cluster que acabou de criar. Se não passar imediatamente para o artigo seguinte, poderá [eliminar o cluster](service-fabric-cluster-delete.md) para evitar incorrer em custos.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma VNET no Azure com a CLI do Azure
> * Criar um cluster do Service Fabric seguro no Azure com a CLI do Azure
> * Proteger o cluster com um certificado x. 509
> * Ligar ao cluster com a CLI do Service Fabric
> * Remover um cluster

Em seguida, avance para o tutorial seguinte para saber como dimensionar o seu cluster.
> [!div class="nextstepaction"]
> [Dimensionar um Cluster](service-fabric-tutorial-scale-cluster.md)

[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Ubuntu-1-NodeTypes-Secure/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/5-VM-Ubuntu-1-NodeTypes-Secure/AzureDeploy.Parameters.json
