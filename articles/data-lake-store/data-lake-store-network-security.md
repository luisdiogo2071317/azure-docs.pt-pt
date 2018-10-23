---
title: Segurança de rede no Azure Data Lake Storage Gen1 | Microsoft Docs
description: Compreenda de que forma é que a firewall de IP e a integração da rede virtual funcionam no Azure Data Lake Storage Gen1.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/09/2018
ms.author: elsung
ms.openlocfilehash: 0da5962bc0b48a387ee82a1db36099682e14bca3
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49168191"
---
# <a name="virtual-network-integration-for-azure-data-lake-storage-gen1---preview"></a>Integração da Rede Virtual para o Azure Data Lake Storage Gen1 - Pré-visualização

Introdução à integração da Rede Virtual para o Azure Data Lake Storage Gen1 (em pré-visualização). A integração da VNet permite-lhe impedir acessos não autorizados às suas contas do Azure Data Lake Storage Gen1 ao bloqueá-las nas suas redes virtuais e sub-redes específicas. Agora, pode configurar a sua conta do ADLS Gen1 para aceitar apenas tráfego de redes virtuais e sub-redes especificadas e bloquear o acesso de todas as outras proveniências. Isto ajuda a proteger a conta do ADLS de ameaças externas.

A integração da VNet para o ADLS Gen1 utiliza a segurança do ponto final de serviço da rede virtual entre a sua Rede Virtual e o serviço Azure Active Directory para gerar afirmações de segurança adicionais no token de acesso. Essas afirmações são, posteriormente, utilizadas para autenticar a rede virtual na conta do ADLS Gen1 e permitir o acesso.

> [!NOTE]
> Esta é uma tecnologia em pré-visualização e não recomendamos utilizá-la em ambientes de produção.
>
> A utilização destas capacidades não tem custos adicionais. A sua conta será cobrada à tarifa padrão do ADLS Gen1 ([preços](https://azure.microsoft.com/pricing/details/data-lake-store/?cdn=disable)) e todos os serviços do Azure que utilizar ([preços](https://azure.microsoft.com/pricing/#product-picker)).

## <a name="scenarios-for-vnet-integration-for-adls-gen1"></a>Cenários para a integração da VNET no ADLS Gen1

A Integração da VNet no ADLS Gen1 permite-lhe restringir o acesso à sua conta do ADLS Gen1 a redes virtuais e sub-redes especificadas.  Quando a conta estiver bloqueada para a VNet / sub-rede especificada, as outras VNets / VMs no Azure não poderão aceder à mesma.  Em termos funcionais, a Integração da VNet do ADLS Gen1 proporciona o mesmo cenário dos [pontos finais de serviço da rede virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview).  Existem algumas diferenças, que estão detalhadas nas secções abaixo. 

![Diagrama do cenário da Integração da VNet do ADLS Gen1](media/data-lake-store-network-security/scenario-diagram.png)

> [!NOTE]
> As regras da firewall de IP existentes podem ser utilizadas como complemento às regras da VNet, de modo a permitir o acesso também a redes no local. 

## <a name="optimal-routing-with-adls-gen1-vnet-integration"></a>Encaminhamento ideal com a integração da VNet do ADLS Gen1

Uma das principais vantagens dos pontos finais de serviço da VNet é o [encaminhamento ideal](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview#key-benefits) a partir da rede virtual.  Para realizar a mesma otimização de rotas para as contas do ADLS Gen1, utilize as [rotas definidas pelo utilizador](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined) seguintes a partir da VNet para a conta do ADLS Gen1:

- **Endereço IP público do ADLS** – utilize o endereço IP público das contas do ADLS Gen1 de destino.  Para identificar os endereços IP da conta do ADLS Gen1, pode [resolver os nomes DNS](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-connectivity-from-vnets#enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity) da conta.  Crie uma entrada separada para cada endereço.

```azurecli
# Create a Route table for your resource group
az network route-table create --resource-group $RgName --name $RouteTableName

# Create Route Table Rules for ADLS Public IP Addresses
# There will be one rule per ADLS Public IP Addresses 
az network route-table route create --name toADLSregion1 --resource-group $RgName --route-table-name $RouteTableName --address-prefix <ADLS Public IP Address> --next-hop-type Internet

# Update the VNet and apply the newly created Route Table to it
az network vnet subnet update --vnet-name $VnetName --name $SubnetName --resource-group $RgName --route-table $RouteTableName
```

## <a name="data-exfiltration-from-the-customer-vnet"></a>Exfiltração de dados da VNet do cliente

Para além de proteger as contas do ADLS para acessos a partir da Rede Virtual, também poderá estar interessado em garantir que não existem exfiltrações a uma conta não autorizada.

A nossa recomendação é utilizar uma solução de firewall na VNet para filtrar o tráfego de saída com base no URL da conta de destino e permitir o acesso apenas a contas do ADLS Gen1 autorizadas.

Algumas opções disponíveis são:
- [Azure Firewall](https://docs.microsoft.com/azure/firewall/overview): pode [implementar e configurar uma firewall do Azure Firewall](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) na sua VNet e proteger o tráfego de saída do ADLS e bloqueá-lo no URL da conta conhecida e autorizada.
- Firewall da [Aplicação de Rede Virtual](https://azure.microsoft.com/solutions/network-appliances/): se o seu administrador apenas autorizar a utilização de determinados fornecedores de firewalls comerciais, pode utilizar uma solução de firewall de NVA, disponível no Azure Marketplace, para realizar a mesma função.

> [!NOTE]
> A utilização de firewalls no caminho dos dados introduzirá um salto adicional nesses caminhos e poderá ter impacto no desempenho de rede relativamente a trocas de dados completas, incluindo o débito disponível e a latência da ligação. 

## <a name="limitations"></a>Limitações
1.  Os clusters do HDInsight têm de ser recriados depois de serem adicionados à pré-visualização.  Os clusters criados antes de o suporte da Integração da VNet do ADLS Gen1 estar disponível têm de ser recriados para suportar esta funcionalidade nova. 
2.  Ao criar um cluster do HDInsight novo, selecionar uma conta do ADLS Gen1 com a Integração de VNet ativada fará com que a criação falhe. Primeiro, tem de desativar a regra de VNet ou pode **Permitir acesso a partir de todas as redes e serviços** através do painel **Firewall and virtual networks** (Firewall e redes virtuais) da conta do ADLS.  Veja a secção [Exceções](##Exceptions) para obter mais informações.
3.  A pré-visualização da Integração de VNet do ADLS Gen1 não funciona com as [identidades geridas de recursos do Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).  
4.  Os dados de ficheiros/pastas na sua conta do ADLS Gen1 ativada para VNet não estão acessíveis a partir do portal.  Isto inclui acesso a partir de uma VM que esteja dentro da VNet, bem como atividades como a utilização do Data Explorer.  As atividades de gestão da conta continuam a funcionar.  Os dados de ficheiros/pastas na conta do ADLS ativada para VNet estão acessíveis através de todos os recursos externos ao portal: acesso por SDK, scripts do PowerShell, outros serviços do Azure (se não tiverem origem no portal), etc. 

## <a name="configuration"></a>Configuração

### <a name="step1-configure-your-vnet-to-use-aad-service-endpoint"></a>Passo 1: Configurar a VNet para utilizar o Ponto Final de Serviço do AAD
1.  Aceda ao portal do Azure e inicie sessão na sua conta. 
2.  [Crie uma rede virtual nova ](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)na sua subscrição ou navegue para uma já existente.  A VNet tem de estar na mesma região que a conta do ADLS Gen 1. 
3.  No painel “Virtual network” (“Rede virtual”), escolha **Service endpoints** (Pontos finais de serviço). 
4.  Clique em **Add** (Adicionar) para adicionar um ponto final de serviço novo.
![Adicionar ponto final de serviço de VNet](media/data-lake-store-network-security/config-vnet-1.png)
5.  Escolha **Microsoft.AzureActiveDirectory** como o serviço do ponto final.
![Seleção do ponto final de serviço Microsoft.AzureActiveDirectory](media/data-lake-store-network-security/config-vnet-2.png)
6.  Escolha as sub-redes nas quais pretende permitir a conectividade e clique em **Add** (Adicionar).
![Seleção da sub-rede](media/data-lake-store-network-security/config-vnet-3.png)
7.  A adição do ponto final de serviço pode demorar até 15 minutos. Depois de adicionado, aparece na lista. Confirme que o ponto final aparece e que estão configurados todos os detalhes. 
![Adição bem-sucedida do ponto final de serviço](media/data-lake-store-network-security/config-vnet-4.png)

### <a name="step-2-set-up-the-allowed-vnetsubnet-for-your-adls-gen1-account"></a>Passo 2: Configurar a VNet/Sub-rede permitida para a conta do ADLS Gen1
1.  Depois de configurar a VNet, [crue uma conta do Azure Data Lake Storage Gen1 nova](data-lake-store-get-started-portal.md#create-a-data-lake-storage-gen1-account) na sua subscrição ou navegue para uma já existente. A conta do ADLS Gen1 tem de estar na mesma região que a VNet. 
2.  Escolha **Firewall and virtual networks** (Firewall e redes virtuais).

  > [!NOTE]
  > Se não vir **Firewall and virtual networks** (Firewall e redes virtuais) nas definições, termine sessão no portal. Feche o browser. Limpe a cache do browser. Reinicie a máquina e repita.

  ![Adicionar uma regra de VNet à conta do ADLS](media/data-lake-store-network-security/config-adls-1.png)
3.  Escolha **Selected networks** (Redes selecionadas). 
4.  Clique em **Add existing virtual network** (Adicionar rede virtual existente).
  ![Adição de rede virtual existente](media/data-lake-store-network-security/config-adls-2.png)
5.  Escolha s VNets e as sub-redes nas quais pretende permitir a conectividade e clique em **Add** (Adicionar).
  ![Escolha da VNet e das sub-redes](media/data-lake-store-network-security/config-adls-3.png)
6.  Confirme que as VNets e as sub-redes aparecem corretamente na lista e clique em **Save** (Guardar).
  ![Guardar a regra nova](media/data-lake-store-network-security/config-adls-4.png)

  > [!NOTE]
  > Depois de guardar, as definições podem demorar até cinco minutos a entrar em vigor.

7.  [Opcional] Para além das VNets e das sub-redes, se quiser permitir a conectividade a partir de endereços IP específicos, pode fazê-lo na secção **Firewall** da mesma página. 

## <a name="exceptions"></a>Exceções
O painel **Firewall and virtual networks** (Firewall e redes virtuais) tem duas caixas de verificação na área Exceptions (Exceções) que permitem a conectividade a partir de um conjunto de serviços e máquinas virtuais no Azure.
![Exceções de firewall e rede virtual](media/data-lake-store-network-security/firewall-exceptions.png)
- **Allow all Azure services to access this Data Lake Storage Gen1 account** (Permitir que todos os serviços do Azure acedam a esta conta do Azure Data Lake Storage Gen1) permite que todos os serviços do Azure, como o Azure Data Factory, os Hubs de Eventos, as VMs do Azure, etc., comuniquem com a conta do ADLS.

- **Allow Azure Data Lake Analytics to access this Data Lake Storage Gen1 account** (Permitir que o Azure Data Lake Analytics aceda a esta conta do Azure Data Lake Storage Gen1) permite a conectividade do serviço Azure Data Lake Analytics a esta conta do ADLS. 

Recomendamos que mantenha estas exceções desativadas e que as ative apenas se precisar de conectividade a partir desses outros serviços de fora da sua VNet.
