---
title: Sobre o funcionamento em rede no recuperação de desastre do Azure para o Azure utilizando o Azure Site Recovery | Microsoft Docs
description: Fornece uma descrição geral do funcionamento em rede para a replicação de VMs do Azure utilizando o Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/16/2018
ms.author: sujayt
ms.openlocfilehash: 55eed1afa9d9dbf2c16643cf9186a7e6dafa847e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="about-networking-in-azure-to-azure-replication"></a>Sobre o funcionamento em rede no Azure para o Azure replicação

>[!NOTE]
> Replicação de recuperação de site para máquinas virtuais do Azure está atualmente em pré-visualização.

Este artigo fornece orientações para a rede quando estiver a replicar e a recuperar as VMs do Azure de uma região para outro, utilizando [do Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Antes de começar

Saiba como a recuperação de sites fornece recuperação após desastre para [neste cenário](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Infraestrutura de rede típica

O diagrama seguinte ilustra um ambiente do Azure típico, para aplicações em execução em VMs do Azure:

![customer-environment](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Se estiver a utilizar uma ligação de VPN ou Azure ExpressRoute da sua rede no local para o Azure, o ambiente é o seguinte:

![customer-environment](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Normalmente, redes estão protegidas através de firewalls e os grupos de segurança de rede (NSGs). Firewalls utilizam o URL ou baseada em IP a listas brancas para controlar a conectividade de rede. Os NSGs fornecem regras que utilizem intervalos de endereços IP para controlar a conectividade de rede.

>[!IMPORTANT]
> Utilizar um proxy autenticado, a conectividade de rede do controlo não é suportada pela recuperação de Site e não é possível ativar a replicação.


## <a name="outbound-connectivity-for-urls"></a>Conectividade de saída para URLs

Se estiver a utilizar um proxy de firewall baseada no URL para controlar a conectividade de saída, permitir que estes URLs de recuperação do Site:


**URL** | **Detalhes**  
--- | ---
*.blob.core.windows.net | Necessário para que os dados podem ser escritos para a conta de armazenamento de cache na região de origem da VM.
login.microsoftonline.com | Necessário para autorização e autenticação para os URLs do serviço de recuperação de sites.
*.hypervrecoverymanager.windowsazure.com | Necessário para que a comunicação de serviço de recuperação de Site pode ocorrer a partir da VM.
*.servicebus.windows.net | Necessário para que os dados de monitorização e diagnóstico de recuperação de sites podem ser escritos a partir da VM.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Conectividade de saída para intervalos de endereços IP

Se estiver a utilizar um proxy de firewall baseada em IP, ou as regras do NSG para controlar a conectividade de saída, estes intervalos IP tem de ser permitido.

- Todos os intervalos de endereços IP que correspondem às contas de armazenamento na região de origem
    - Criar um [etiqueta do serviço de armazenamento](../virtual-network/security-overview.md#service-tags) com base em regras NSG para a região de origem.
    - Permitir que estes endereços, de modo a que os dados podem ser escritos para a conta de armazenamento de cache da VM.
- Todos os intervalos de endereços IP que correspondem ao Office 365 [pontos finais de endereço IP V4 de autenticação e identidade](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
    - Se os novos endereços são adicionados para os intervalos do Office 365 no futuro, terá de criar novas regras NSG.
- Site de recuperação serviço IP endereços de ponto final - disponíveis num [ficheiro XML](https://aka.ms/site-recovery-public-ips) e dependem da sua localização de destino.
-  Pode [transferir e utilizar este script](https://aka.ms/nsg-rule-script), para criar automaticamente as regras necessárias no NSG.
- Recomendamos que crie as regras do NSG necessárias num NSG de teste e, certifique-se de que existem não existem problemas antes de criar as regras sobre um NSG de produção.


Intervalos de endereços IP de recuperação de site são as seguintes:

   **Target** | **Recuperação de sites IP** |  **Recuperação de site do IP de monitorização**
   --- | --- | ---
   Ásia Oriental | 52.175.17.132 | 13.94.47.61
   Sudeste Asiático | 52.187.58.193 | 13.76.179.223
   Índia Central | 52.172.187.37 | 104.211.98.185
   Sul da Índia | 52.172.46.220 | 104.211.224.190
   EUA Centro-Norte | 23.96.195.247 | 168.62.249.226
   Europa do Norte | 40.69.212.238 | 52.169.18.8
   Europa Ocidental | 52.166.13.64 | 40.68.93.145
   EUA Leste | 13.82.88.226 | 104.45.147.24
   EUA Oeste | 40.83.179.48 | 104.40.26.199
   EUA Centro-Sul | 13.84.148.14 | 104.210.146.250
   EUA Central | 40.69.144.231 | 52.165.34.144
   EUA Leste 2 | 52.184.158.163 | 40.79.44.59
   Leste do Japão | 52.185.150.140 | 138.91.1.105
   Oeste do Japão | 52.175.146.69 | 138.91.17.38
   Sul do Brasil | 191.234.185.172 | 23.97.97.36
   Leste da Austrália | 104.210.113.114 | 191.239.64.144
   Sudeste da Austrália | 13.70.159.158 | 191.239.160.45
   Canadá Central | 52.228.36.192 | 40.85.226.62
   Leste do Canadá | 52.229.125.98 | 40.86.225.142
   EUA Centro-Oeste | 52.161.20.168 | 13.78.149.209
   EUA Oeste 2 | 52.183.45.166 | 13.66.228.204
   Reino Unido Oeste | 51.141.3.203 | 51.141.14.113
   Reino Unido Sul | 51.140.43.158 | 51.140.189.52
   Sul do Reino Unido 2 | 13.87.37.4| 13.87.34.139
   Norte do Reino Unido | 51.142.209.167 | 13.87.102.68
   Coreia Central | 52.231.28.253 | 52.231.32.85
   Coreia do Sul | 52.231.298.185 | 52.231.200.144
   Centro de França | 52.143.138.106 | 52.143.136.55
   Sul de França | 52.136.139.227 |52.136.136.62


## <a name="example-nsg-configuration"></a>Exemplo de configuração de NSG

Este exemplo mostra como configurar as regras do NSG para uma VM replicar.

- Se estiver a utilizar as regras do NSG para controlar a conectividade de saída, utilize "Permitir HTTPS a saída" regras para a porta: 443 para todos os os necessários intervalos de endereços IP.
- O exemplo presumes que a localização de origem da VM é "Leste EUA" e a localização de destino é "Central EUA".

### <a name="nsg-rules---east-us"></a>Regras do NSG - EUA leste

1. Crie uma regra de segurança HTTPS (443) saída para "Storage.EastUS" no NSG, conforme mostrado na captura de ecrã abaixo.

      ![etiqueta de armazenamento](./media/azure-to-azure-about-networking/storage-tag.png)

2. Criar regras de saída de HTTPS (443) para todos os intervalos de endereços IP que correspondem ao Office 365 [pontos finais de endereço IP V4 de autenticação e identidade](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Crie regras de saída HTTPS (443) para os IPs de recuperação de Site que corresponde à localização de destino:

   **Localização** | **Endereço de IP de recuperação de site** |  **Endereço IP de monitorização de recuperação do site**
    --- | --- | ---
   EUA Central | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>Regras do NSG - e.u. a Central

Estas regras são necessárias para que a replicação pode ser ativada a partir da região de destino para a origem região pós-falha:

1. Crie uma regra de segurança HTTPS (443) saída para "Storage.CentralUS" no NSG.

2. Criar regras de saída de HTTPS (443) para todos os intervalos de endereços IP que correspondem ao Office 365 [pontos finais de endereço IP V4 de autenticação e identidade](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

3. Crie regras de saída HTTPS (443) para os IPs de recuperação de Site que corresponde à localização de origem:

   **Localização** | **Endereço de IP de recuperação de site** |  **Endereço IP de monitorização de recuperação do site**
    --- | --- | ---
   EUA Central | 13.82.88.226 | 104.45.147.24

## <a name="network-virtual-appliance-configuration"></a>Configuração de aplicação virtual de rede

Se estiver a utilizar dispositivos de rede virtual (NVAs) para controlar o tráfego de rede de saída de VMs, poderá obter limitada a aplicação se todo o tráfego de replicação atravessa a NVA. Recomendamos que crie um ponto final de serviço de rede na sua rede virtual para "Armazenamento", para que o tráfego de replicação não ir para a NVA.

### <a name="create-network-service-endpoint-for-storage"></a>Criar o ponto final do serviço de rede de armazenamento
Pode criar um ponto final de serviço de rede na sua rede virtual para "Armazenamento", para que o tráfego de replicação não deixe o limite do Azure.

- Selecione a rede virtual do Azure e clique em 'Pontos finais de serviço'

    ![ponto final de armazenamento](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- Clique em 'Adicionar' e 'Adicionar pontos finais de serviço' separador é aberto
- Selecione 'Microsoft' em 'Service' e as sub-redes em campo 'Sub-redes' necessárias e clique em 'Add'

>[!NOTE]
>Não é restringir o acesso de rede virtual para as contas de armazenamento utilizadas para ASR. Deve permitir acesso a partir de 'Todas as redes'

### <a name="forced-tunneling"></a>Túnel forçado

Pode substituir a rota de sistema predefinida do Azure para o prefixo do endereço 0.0.0.0/0 com um [rotas personalizadas](../virtual-network/virtual-networks-udr-overview.md#custom-routes) e divert tráfego da VM para uma aplicação de virtual de rede no local (NVA), mas esta configuração não é recomendada para a recuperação de Site replicação. Se estiver a utilizar rotas personalizadas, deve [criar um ponto final de serviço de rede virtual](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) no seu virtual de rede para "Armazenamento", para que o tráfego de replicação deixam o limite do Azure.

## <a name="next-steps"></a>Passos Seguintes
- Começar a proteger as cargas de trabalho por [replicar máquinas virtuais do Azure](site-recovery-azure-to-azure.md).
- Saiba mais sobre [retenção de endereço IP](site-recovery-retain-ip-azure-vm-failover.md) para ativação pós-falha da máquina virtual do Azure.
- Saiba mais sobre a recuperação após desastre de [máquinas virtuais do Azure com o ExpressRoute ](azure-vm-disaster-recovery-with-expressroute.md).
