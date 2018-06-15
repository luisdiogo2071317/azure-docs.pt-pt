---
title: Configurar a ligação a um site secundário no local após a ativação pós-falha com o Azure Site Recovery de endereçamento IP | Microsoft Docs
description: Descreve como configurar o endereçamento IP para ligar a VMs num site secundário no local após a ativação pós-falha do Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: rayne
ms.openlocfilehash: 531705bc704b3366c1c670ecf07c809ade67bc55
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
ms.locfileid: "29378891"
---
# <a name="set-up-ip-addressing-to-connect-to-a-secondary-on-premises-site-after-failover"></a>Configurar a ligação a um site secundário no local após a ativação pós-falha de endereçamento IP

Após a ativação pós-falha de VMs de Hyper-V em nuvens do System Center Virtual Machine Manager (VMM) para um site secundário, tem de ser capaz de ligar para a réplica VMs. Este artigo ajuda-o para efetuar este procedimento. 

## <a name="connection-options"></a>Opções de ligação

Após a ativação pós-falha, existem algumas formas de lidar com endereçamento IP para as VMs da réplica: 

- **Manter o mesmo endereço IP após a ativação pós-falha**: neste cenário, a VM replicada tem o mesmo endereço IP da VM principal. Isto simplifica a rede relacionado com problemas após a ativação pós-falha, mas requer alguns cálculos de infraestrutura.
- **Utilizar um endereço IP diferente após a ativação pós-falha**: neste cenário a VM obtém um novo endereço IP após a ativação pós-falha. 
 

## <a name="retain-the-ip-address"></a>Manter o endereço IP

Se pretender manter os endereços IP do site primário, após a ativação pós-falha para o site secundário, pode:

- Implemente uma sub-rede Stretch entre a primária e os sites secundários.
- Execute uma ativação pós-falha de sub-rede completa do primário para o site secundário. Terá de atualizar as rotas para indicar a nova localização dos endereços IP.


### <a name="deploy-a-stretched-subnet"></a>Implementar uma sub-rede Stretch

Numa configuração de Stretch, a sub-rede está disponível em simultâneo em sites primários e secundários. Numa sub-rede Stretch, quando move uma máquina e a respetiva configuração de endereço IP (Layer 3) para o site secundário, a rede automaticamente encaminha o tráfego para a nova localização. 

- De uma perspetiva de (camada de ligação de dados) de camada 2, terá de equipamento de rede que pode gerir uma VLAN Stretch.
- Por alongar a VLAN, o domínio de falhas potenciais expande a ambos os sites. Isto torna-se um ponto único de falha. Enquanto pouco provável, neste cenário, poderá não conseguir isolar um incidente como um storm difusão. 


### <a name="fail-over-a-subnet"></a>Efetuar a ativação pós-falha de uma sub-rede

Pode falhar toda a sub-rede desfrutar das vantagens da Stretch sub-rede, sem realmente alongá-lo. Nesta solução, uma sub-rede está disponível no site de origem ou de destino, mas não em ambas em simultâneo.

- Para manter o espaço de endereços IP na eventualidade de ocorrer uma ativação pós-falha, pode dispor programaticamente para a infraestrutura de router para mover as sub-redes de um site para outro.
- Quando ocorre uma ativação pós-falha, sub-redes mover-se com as respetivas VMs associados.
- A principal desvantagem desta abordagem é que o se ocorrer uma falha, terá de mover toda a sub-rede.

#### <a name="example"></a>Exemplo

Eis um exemplo de ativação pós-falha de sub-rede concluída. 

- Antes da ativação pós-falha, o site primário tem aplicações sejam executadas na sub-rede 192.168.1.0/24.
- Durante a ativação pós-falha, todas as VMs nesta sub-rede executar a ativação pós-falha para o site secundário e manter os seus endereços IP. 
- As rotas entre todos os sites tenham de ser modificados para refletir o facto de que todas as VMs numa sub-rede 192.168.1.0/24 agora movido para o site secundário.

Os gráficos seguintes ilustram as sub-redes antes e após a ativação pós-falha.


**Antes da ativação pós-falha**

![Antes da ativação pós-falha](./media/hyper-v-vmm-networking/network-design2.png)

**Após a ativação pós-falha**

![Após a ativação pós-falha](./media/hyper-v-vmm-networking/network-design3.png)

Após a ativação pós-falha, a recuperação de sites atribui um endereço IP para cada interface de rede na VM. O endereço é alocado do conjunto de endereços IP estático na rede de relevante, para cada instância VM.

- Se o conjunto de endereços IP no site secundário é o mesmo que o site de origem, a recuperação de Site atribui o mesmo endereço IP (da VM de origem), para a VM de réplica. O endereço IP é reservado no VMM, mas não está definida como o endereço IP de ativação pós-falha no anfitrião Hyper-V. O endereço IP de ativação pós-falha num anfitrião Hyper-v estiver definido para apenas antes da ativação pós-falha.
- Se o mesmo endereço IP não estiver disponível, a recuperação de sites aloca outro endereço IP disponível do conjunto.
- Se as VMs utilizam DHCP, a recuperação de Site não gere os endereços IP. Tem de verificar que o servidor DHCP no site secundário pode alocar endereços do mesmo intervalo de que o site de origem.

### <a name="validate-the-ip-address"></a>Validar o endereço IP

Depois de ativar a proteção para uma VM, pode utilizar seguintes script de exemplo para verificar o endereço atribuído à VM. Este endereço IP é definido como o endereço IP de ativação pós-falha e atribuído à VM no momento da ativação pós-falha:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="use-a-different-ip-address"></a>Utilizar um endereço IP diferentes

Neste cenário, os endereços IP de VMs com ativação pós-falha são alterados. A desvantagem desta solução é a manutenção necessária.  Entradas DNS e a cache poderão ter de ser atualizados. Isto pode resultar num período de inatividade, que pode ser mitigado da seguinte forma:

- Utilize os valores TTL baixos para aplicações de intranet.
- Utilize o seguinte script de um plano de recuperação do Site Recovery para uma atualização atempada do servidor DNS. Não precisa do script se utilizar o registo de DNS dinâmico.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Exemplo 

Neste exemplo temos de endereços IP diferentes em sites primários e secundários e existe um site terceiro que aplicações alojadas no site primário ou de recuperação de site pode ser acedido.

- Antes de ativação pós-falha, as aplicações são 192.168.1.0/24 sub-rede alojada no site primário.
- Após a ativação pós-falha, as aplicações são configuradas na sub-rede 172.16.1.0/24 no site secundário.
- Todos os três sites podem aceder umas às outras.
- Após a ativação pós-falha, serão restauradas as aplicações na sub-rede de recuperação.
- Este cenário não é necessário para efetuar a ativação pós-falha de toda a sub-rede e sem alterações são necessárias para reconfigurar as rotas de rede ou VPN. A ativação pós-falha e algumas atualizações DNS, certifique-se de que as aplicações permanecem acessíveis.
- Se o DNS está configurado para permitir atualizações dinâmicas, em seguida, as VMs irão registar-se utilizando o novo endereço IP, quando for iniciado após a ativação pós-falha.

**Antes da ativação pós-falha**

![Endereço IP diferente - antes da ativação pós-falha](./media/hyper-v-vmm-networking/network-design10.png)

**Após a ativação pós-falha**

![Endereço IP diferente - após a ativação pós-falha](./media/hyper-v-vmm-networking/network-design11.png)


## <a name="next-steps"></a>Passos Seguintes

[Executar uma ativação pós-falha](hyper-v-vmm-failover-failback.md)

