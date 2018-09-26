---
title: Configuração de VNet de instância de gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Este tópico descreve opções de configuração para uma rede virtual (VNet) com uma instância de gerida de base de dados do Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 9d3f867dad40017e8e97ec4f5e370533b018263c
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181179"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Configurar uma VNet para a instância gerida de base de dados SQL do Azure

Instância de gerida de base de dados de SQL do Azure tem de ser implementada dentro do Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md). Esta implementação permite que os seguintes cenários: 
- Ligar-se para uma instância gerida diretamente a partir de uma rede no local 
- Ligar uma instância gerida para o servidor ligado ou de outra-arquivo de dados no local 
- Ligar uma instância gerida nos recursos do Azure  

## <a name="plan"></a>Planear

Planeie como implementar uma instância gerida na rede virtual com as suas respostas às seguintes perguntas: 
- Planeja implantar únicas ou múltiplas instâncias geridas? 

  O número de instâncias geridas determina o tamanho mínimo da sub-rede para alocar para as suas instâncias geridas. Para obter mais informações, consulte [determinar o tamanho da sub-rede da instância gerida](#determine-the-size-of-subnet-for-managed-instances). 
- Precisa para implementar a sua instância gerida numa rede virtual existente ou estiver a criar uma nova rede? 

   Se planeia utilizar uma rede virtual existente, terá de modificar essa configuração de rede para acomodar a sua instância gerida. Para obter mais informações, consulte [modificar a rede virtual existente para a instância gerida](#modify-an-existing-virtual-network-for-managed-instances). 

   Se planeja criar nova rede virtual, veja [criar nova rede virtual para a instância gerida](#create-a-new-virtual-network-for-a-managed-instance).

## <a name="requirements"></a>Requisitos

Para criar uma instância gerida, crie uma sub-rede dedicada (sub-rede da instância gerida) dentro da rede virtual que está em conformidade com os seguintes requisitos:
- **Dedicado sub-rede**: sub-rede a instância gerida não pode conter qualquer outro serviço de cloud associado à mesma, e não tem de ser uma sub-rede de Gateway. Não será possível criar uma instância gerida numa sub-rede que contém os recursos que não seja a instância gerida e, posteriormente, pode não adicionar outros recursos na sub-rede.
- **Grupo de segurança de rede compatível (NSG)**: um NSG associado a uma sub-rede de instância gerida tem de conter regras mostradas nas tabelas seguintes (regras de segurança de entrada obrigatória e regras de segurança de saída obrigatórios) à frente de quaisquer outras regras. Pode utilizar um NSG para controlar totalmente o acesso para o ponto final de dados de instância gerida ao filtrar o tráfego na porta 1433. 
- **Tabela de compatíveis rota definida pelo utilizador (UDR)**: sub-rede a instância gerida tem de ter uma tabela de rotas de utilizador com **Internet de salto seguinte 0.0.0.0/0** como o UDR obrigatório atribuído ao mesmo. Além disso, pode adicionar um UDR que encaminha o tráfego que tem intervalos de IP privados no local como um destino através do gateway de rede virtual ou a aplicação de rede virtual (NVA). 
- **DNS personalizado opcional**: Se não for especificado um DNS personalizado na rede virtual, endereço IP de resolução de recursiva do Azure (por exemplo, 168.63.129.16) tem de ser adicionado à lista. Para obter mais informações, consulte [configurar o DNS de personalizado](sql-database-managed-instance-custom-dns.md). O servidor DNS personalizado tem de ser capaz de resolver nomes de anfitriões para os seguintes domínios e os respetivos subdomínios: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, e *microsoftonline-p.com*. 
- **Não existem pontos finais de serviço**: sub-rede a instância gerida não pode ter um ponto de extremidade de serviço associado a si. Certifique-se de que a opção de pontos finais de serviço é desativada quando criar a rede virtual.
- **Endereços IP suficientes**: sub-rede a instância gerida tem de ter o mínimo de 16 endereços IP (recomendado mínimo é de 32 endereços IP). Para obter mais informações, consulte [determinar o tamanho da sub-rede para instâncias geridas](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Não será capaz de implantar uma nova instância gerida, se a sub-rede de destino não é compatível com todos esses requisitos. Quando é criada uma instância gerida, uma *política de intenção de rede* é aplicado na sub-rede para impedir alterações não conformes para configuração de rede. Após a última instância é removida a partir da sub-rede, o *política de intenção de rede* é removido também

### <a name="mandatory-inbound-security-rules"></a>Regras de segurança de entrada obrigatório 

| NOME       |PORTA                        |PROTOCOLO|SOURCE           |DESTINO|AÇÃO|
|------------|----------------------------|--------|-----------------|-----------|------|
|gestão  |9000, 9003, 1438, 1440, 1452|TCP     |Qualquer              |Qualquer        |Permitir |
|mi_subnet   |Qualquer                         |Qualquer     |SUB-REDE DE MI        |Qualquer        |Permitir |
|health_probe|Qualquer                         |Qualquer     |AzureLoadBalancer|Qualquer        |Permitir |

### <a name="mandatory-outbound-security-rules"></a>Regras de segurança de saída obrigatórios 

| NOME       |PORTA          |PROTOCOLO|SOURCE           |DESTINO|AÇÃO|
|------------|--------------|--------|-----------------|-----------|------|
|gestão  |80, 443, 12000|TCP     |Qualquer              |Qualquer        |Permitir |
|mi_subnet   |Qualquer           |Qualquer     |Qualquer              |SUB-REDE DE MI  |Permitir |

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Determinar o tamanho da sub-rede para instâncias geridas

Quando cria uma instância gerida, o Azure aloca um número de máquinas virtuais, consoante o escalão selecionado durante o aprovisionamento. Uma vez que estas máquinas virtuais estão associadas a sua sub-rede, necessitam de endereços IP. Para garantir a elevada disponibilidade durante operações normais e de manutenção do serviço, o Azure pode alocar máquinas virtuais adicionais. Como resultado, o número de endereços IP necessários numa sub-rede é maior do que o número de instâncias geridas nessa sub-rede. 

Por design, uma instância gerida necessita de um mínimo de 16 endereços IP numa sub-rede e pode utilizar até 256 endereços IP. Como resultado, pode utilizar máscaras de sub-rede /28 para /24 quando definir os intervalos IP de sub-rede. 

> [!IMPORTANT]
> Tamanho de sub-rede com 16 endereços IP é o mínimo absoluto com potencial de limitado para a outra instância gerida de ampliação. Ao escolher sub-rede com o prefixo /27 ou abaixo é altamente recomendado. 

Se planeia implementar várias instâncias geridas dentro da sub-rede e precisa para otimizar o tamanho da sub-rede, utilize estes parâmetros para formar um cálculo: 

- O Azure utiliza cinco endereços IP na sub-rede para as suas próprias necessidades 
- Cada instância de fins gerais tem dois endereços 
- Cada uma delas críticas para a empresa precisa quatro endereços

**Exemplo**: planeia ter três para fins gerais e duas empresas instâncias da críticos geridos. Que significa que terá de 5 + 3 * 2 + 2 * 4 = 19 endereços IP. Como intervalos de IP estão definidos na potência de 2, precisa do intervalo IP de 32 (2 ^ 5) endereços IP. Por conseguinte, terá de reservar a sub-rede com a máscara de sub-rede de/27. 

> [!IMPORTANT]
> Cálculo exibido acima se tornará obsoleto com ainda mais melhorias. 

## <a name="create-a-new-virtual-network-for-a-managed-instance"></a>Criar uma nova rede virtual para uma instância gerida

A maneira mais fácil para criar e configurar a rede virtual é usar o modelo de implementação Azure Resource Manager.

1. Inicie sessão no Portal do Azure.

2. Uso **implementar no Azure** botão para implementar uma rede virtual na cloud do Azure:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Este botão abre um formulário que pode utilizar para configurar o ambiente de rede onde pode implementar a instância gerida.

  > [!Note]
  > Este modelo do Azure Resource Manager irá implementar uma rede virtual com duas sub-redes. Uma sub-rede denominada **ManagedInstances** está reservado para instâncias geridas e configurou previamente tabela de rotas, enquanto a outra sub-rede denominada **predefinido** é utilizada para outros recursos que devem acessar gerida Instância (por exemplo, máquinas virtuais do Azure). Pode remover **predefinido** sub-rede, se não precisa dela.

3. Configure o ambiente de rede. No formulário seguinte, é possível configurar parâmetros do seu ambiente de rede:

![Configurar a rede do azure](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

Pode alterar os nomes de VNet e sub-redes e ajustar os intervalos IP associados aos recursos da sua rede. Assim que pressionar o botão "Adquirir", esse formulário irá criar e configurar o seu ambiente. Se não precisar de duas sub-redes pode eliminar predefinido. 

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Modificar uma rede virtual existente para as instâncias geridas 

As perguntas e respostas nesta secção mostram-lhe como adicionar uma instância gerida a rede virtual existente. 

**Está a utilizar modelo de implementação clássico ou do Resource Manager para a rede virtual existente?** 

Só pode criar uma instância gerida em redes virtuais do Resource Manager. 

**Gostaria de criar a nova sub-rede da instância gerida de SQL ou utilizar um já existente?**

Se gostaria de criar um novo: 

- Calcular o tamanho da sub-rede, seguindo as diretrizes a [determinar o tamanho da sub-rede para instâncias geridas](#determine-the-size-of-subnet-for-managed-instances) secção.
- Siga os passos em [adicionar, alterar ou eliminar uma sub-rede de rede virtual](../virtual-network/virtual-network-manage-subnet.md). 
- Criar uma tabela de rota que contém uma entrada única **0.0.0.0/0**, como o próximo salto de Internet e associá-la com a sub-rede para a instância gerida.  

Se quiser criar uma instância gerida dentro de uma sub-rede existente, recomendamos o seguinte script do PowerShell para preparar a sub-rede.
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
Preparação de sub-rede é feita em três passos simples:

- Validar - netwok virtual selecionado e a sub-rede são validadas para a instância gerida, requisitos de rede
- Confirmar - utilizador é apresentado um conjunto de alterações que precisam ser feitas para preparar a sub-rede para a implementação de instância gerida e solicitado consentimento
- Preparar - rede Virtual e sub-rede estão configurados corretamente

**Tem o servidor DNS personalizado configurado?** 

Se Sim, veja [configurando um DNS personalizado](sql-database-managed-instance-custom-dns.md). 

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md)
- Para obter um tutorial que mostra como criar uma VNet, criar uma instância gerida e restaurar uma base de dados a partir de uma cópia de segurança da base de dados, consulte [criar um Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Para problemas DNS, consulte [configurar um DNS personalizado](sql-database-managed-instance-custom-dns.md)
