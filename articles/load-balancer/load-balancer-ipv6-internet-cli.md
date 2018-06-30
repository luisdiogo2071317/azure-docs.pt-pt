---
title: Criar um balanceador de carga públicos com o IPv6 - CLI do Azure | Microsoft Docs
description: Saiba como criar um balanceador de carga públicos com o IPv6 utilizando a CLI do Azure.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
tags: azure-resource-manager
keywords: IPv6, o Balanceador de carga do azure, pilha dupla, ip público, ipv6 nativo, móveis, iot
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/25/2018
ms.author: kumud
ms.openlocfilehash: 10698c79b11a47a465604f90bf63e180615a5ed7
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112739"
---
# <a name="create-a-public-load-balancer-with-ipv6-using-azure-cli"></a>Criar um balanceador de carga públicos com o IPv6 utilizando a CLI do Azure


Um balanceador de carga do Azure é um balanceador de carga de Camada 4 (TCP, UDP). Balanceadores de carga fornecem elevada disponibilidade através de distribuição de tráfego de entrada entre instâncias de bom estado de funcionamento de serviço nos serviços em nuvem ou de máquinas virtuais num conjunto de Balanceador de carga. Balanceadores de carga podem também apresentar estes serviços em várias portas ou vários endereços IP ou ambos.

## <a name="example-deployment-scenario"></a>Cenário de implementação de exemplo

O diagrama seguinte ilustra a solução que é implementada utilizando o modelo de exemplo descrito neste artigo de balanceamento de carga.

![Cenário do Balanceador de carga](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Neste cenário, crie os seguintes recursos do Azure:

* Duas máquinas virtuais (VMs)
* Interface de rede virtual para cada VM com endereços IPv4 e IPv6 atribuído
* Um balanceador de carga públicos com o endereço IP público IPv6 e IPv4
* Um conjunto de disponibilidade que contém as duas VMs
* Duas regras para mapear os VIPs públicos para os pontos finais privados de balanceamento de carga

## <a name="deploy-the-solution-by-using-azure-cli"></a>Implementar a solução ao utilizar a CLI do Azure

Os passos seguintes mostram como criar um balanceador de carga público utilizando a CLI do Azure. Utilizar a CLI, criar e configurar cada objeto individualmente e, em seguida, colocá-los em conjunto para criar um recurso.

Para implementar um balanceador de carga, criar e configurar os seguintes objetos:

* **Configuração de IP Front-end**: contém endereços IP públicos para tráfego de rede recebido.
* **Conjunto de endereços de back-end**: contém interfaces de rede (NICs) para as máquinas virtuais para receber o tráfego de rede do Balanceador de carga.
* **As regras de balanceamento de carga**: contém regras que mapeiam uma porta pública no balanceador de carga para uma porta no conjunto de endereços de back-end.
* **Regras NAT de entrada**: contém regras de tradução (NAT) de endereços de rede mapeiam uma porta pública no balanceador de carga para uma porta para uma máquina virtual específica no conjunto de endereços de back-end.
* **As sondas**: contém sondas de estado de funcionamento que são utilizadas para verificar a disponibilidade das instâncias de máquina virtual no conjunto de endereços de back-end.

## <a name="set-up-azure-cli"></a>Configurar a CLI do Azure

Neste exemplo, executar as ferramentas da CLI do Azure numa janela de comandos do PowerShell. Para melhorar a legibilidade e reutilização, utilizar capacidades de scripts do PowerShell, não cmdlets PowerShell do Azure.

1. [Instalar e configurar a CLI do Azure]((https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)) seguindo os passos no artigo ligado e inicie sessão na sua conta do Azure.

2. Configure variáveis de PowerShell para utilização com os comandos da CLI do Azure:

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Criar um grupo de recursos, um balanceador de carga, uma rede virtual e sub-redes

1. Criar um grupo de recursos:

    ```azurecli
    az group create $rgName $location
    ```

2. Crie um balanceador de carga:

    ```azurecli
    $lb = az network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. Criar uma rede virtual:

    ```azurecli
    $vnet = az network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

4. Nesta rede virtual, crie duas sub-redes:

    ```azurecli
    $subnet1 = az network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = az network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Crie endereços IP públicos para o conjunto de front-end

1. Configure as variáveis de PowerShell:

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. Crie um endereço IP público para o conjunto IP Front-end:

    ```azurecli
    $publicipV4 = az network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = az network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > O Balanceador de carga utiliza a etiqueta de domínio do IP público, como o respetivo nome de domínio completamente qualificado (FQDN). Isto uma alteração de implementação clássica, que utiliza o serviço em nuvem nome como o FQDN do Balanceador de carga.
    >
    > Neste exemplo, o FQDN é *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Criar conjuntos de front-end e back-end

Nesta secção, crie os seguintes conjuntos IP:
* O conjunto IP Front-end que recebe o tráfego de rede de entrada no balanceador de carga.
* O conjunto de IP back-end em que o conjunto de front-end envia o tráfego de rede com balanceamento de carga.

1. Configure as variáveis de PowerShell:

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. Criar um conjunto IP Front-end e associá-lo com o IP público que criou no passo anterior e o Balanceador de carga.

    ```azurecli
    $frontendV4 = az network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = az network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = az network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = az network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-load-balancer-rules"></a>Criar a sonda, regras NAT e as regras de Balanceador de carga

Este exemplo cria os seguintes itens:

* Uma regra de sonda para verificar a conectividade para a porta TCP 80.
* Uma regra NAT traduzir todo o tráfego de entrada na porta 3389 à porta 3389 para RDP.\*
* Uma regra NAT traduzir todo o tráfego de entrada na porta 3391 à porta 3389 para o protocolo de ambiente de trabalho remoto (RDP).\*
* Uma regra de Balanceador de carga para equilibrar a todo o tráfego de entrada na porta 80 para a porta 80 nos endereços do conjunto de back-end.

\* Regras NAT que estão associadas uma instância específica da máquina virtual por trás do Balanceador de carga. O tráfego de rede que são recebidos na porta 3389 é enviado para a máquina virtual específica e a porta que está associada a regra NAT. Tem de especificar um protocolo (UDP ou TCP) para uma regra NAT. Não é possível atribuir os dois protocolos para a mesma porta.

1. Configure as variáveis de PowerShell:

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. Crie a sonda.

    O exemplo seguinte cria uma sonda TCP que verifica a conectividade à porta TCP 80 de back-end a cada 15 segundos. Após duas falhas consecutivas, marca o recurso de back-end como indisponíveis.

    ```azurecli
    $probeV4V6 = az network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. Crie regras NAT de entrada que permitem ligações do RDP para os recursos de back-end:

    ```azurecli
    $inboundNatRuleRdp1 = az network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = az network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. Crie regras de Balanceador de carga que enviam tráfego para as portas de back-end diferentes, consoante o front-end que recebeu o pedido.

    ```azurecli
    $lbruleIPv4 = az network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = az network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. Verifique as definições:

    ```azurecli
    az network lb show --resource-group $rgName --name $lbName
    ```

    Resultado esperado:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a>Criar NICs

Criar o NICs e associá-las com as regras NAT, as regras de Balanceador de carga e pesquisas.

1. Configure as variáveis de PowerShell:

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. Crie um NIC para cada back-end e adicionar uma configuração de IPv6:

    ```azurecli
    $nic1 = az network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = az network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = az network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Criar os recursos VM de back-end e anexar a cada NIC

Para criar VMs, tem de ter uma conta de armazenamento. Balanceamento de carga, as VMs têm de ser membros de um conjunto de disponibilidade. Para obter mais informações sobre a criação de VMs, consulte [criar uma VM do Azure utilizando o PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

1. Configure as variáveis de PowerShell:

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > Este exemplo utiliza o nome de utilizador e palavra-passe para as VMs como texto simples. Ter cuidado ao utilizar estas credenciais como texto simples. Para um método mais seguro de processamento de credenciais no PowerShell, consulte o [ `Get-Credential` ](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

2. Crie o conjunto de disponibilidade e da conta de armazenamento.

    Pode utilizar uma conta de armazenamento existente ao criar as VMs. Criar uma nova conta de armazenamento utilizando o seguinte comando:

    ```azurecli
    $storageAcc = az storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

3. Crie o conjunto de disponibilidade:

    ```azurecli
    $availabilitySet = az vm availability-set create --name $availabilitySetName --resource-group $rgName --location $location
    ```

4. Crie as máquinas virtuais com os NICs associados:

    ```azurecli
    $vm1 = az vm create --resource-group $rgname --location $location --availability-set $availabilitySet --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availability-set $availabilitySet --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a>Passos Seguintes

[Começar a configurar um balanceador de carga interno](load-balancer-get-started-ilb-arm-cli.md)  
[Configurar um modo de distribuição de balanceador de carga](load-balancer-distribution-mode.md)  
[Configurar definições de tempo limite TCP inativo para o balanceador de carga](load-balancer-tcp-idle-timeout.md)
