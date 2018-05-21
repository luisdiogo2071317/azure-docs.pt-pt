---
title: Serviço de aplicações no notas de versão 1 de atualização de pilha do Azure | Microsoft Docs
description: Saiba mais sobre as novidades na atualização de um para o serviço de aplicações na pilha do Azure, os problemas conhecidos e onde pode transferir a atualização.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 80bd865b7a08d9488c0fb6a1a5b60445b9c6eaaa
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="app-service-on-azure-stack-update-1-release-notes"></a>Serviço de aplicações no notas de versão 1 de atualização de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Estas notas de versão descrevem as melhorias e correções no App Service do Azure no Azure pilha Update 1 e a quaisquer problemas conhecidos. Problemas conhecidos são divididos em problemas diretamente relacionada com a implementação, o processo de atualização e a problemas com a compilação (pós-instalação).

> [!IMPORTANT]
> Aplicar a atualização 1802 ao seu sistema de pilha do Azure integrado ou implementar o kit de desenvolvimento de pilha do Azure mais recente antes de implementar o serviço de aplicações do Azure.
>
>

## <a name="build-reference"></a>Referência de compilação

O serviço de aplicação no número de compilação de 1 de atualização de pilha do Azure é **69.0.13698.9**

### <a name="prerequisites"></a>Pré-requisitos

> [!IMPORTANT]
> Novas implementações do serviço de aplicações do Azure na pilha do Azure agora requerem um [certificado de caráter universal de três requerente](azure-stack-app-service-before-you-get-started.md#get-certificates) devido a melhorias na forma no qual o SSO para Kudu agora é processada no App Service do Azure. O requerente nova é  **\*. sso.appservice.\< região\>.\< DomainName\>.\< extensão\>**
>
>

Consulte o [documentação antes de começar a utilizar](azure-stack-app-service-before-you-get-started.md) antes de iniciar a implementação.

### <a name="new-features-and-fixes"></a>Novas funcionalidades e correções

App Service do Azure no Azure pilha Update 1 inclui as seguintes melhorias e correções:

- **Elevada disponibilidade do App Service do Azure** -cargas de trabalho do 1802 de pilha do Azure a atualização ativada para ser implementado em domínios de falha. Por conseguinte, a infraestrutura de serviço de aplicações é capaz de ser tolerante a falhas, será implementado em vários domínios de falhas. Por predefinição todas as novas implementações do App Service do Azure tem esta capacidade no entanto, para implementações concluídas antes do Azure pilha 1802 atualização ser aplicada consulte o [documentação de domínio de falhas do serviço de aplicações](azure-stack-app-service-fault-domain-update.md)

- **Implementar na rede virtual existente** -os clientes podem agora implementar o serviço de aplicações na pilha do Azure dentro de uma rede virtual existente. Implementação de uma rede virtual existente permite aos clientes ligar ao SQL Server e do servidor de ficheiros, necessário para o App Service do Azure, através das portas privadas. Durante a implementação, os clientes podem selecionar para implementar uma rede virtual existente, no entanto [tem de criar sub-redes para utilização pelo App Service](azure-stack-app-service-before-you-get-started.md#virtual-network) antes da implementação.

- Atualizações **inquilino de serviço de aplicações, administrador, portais de funções e as ferramentas do Kudu**. Consistente com a versão do SDK de Portal de pilha do Azure.

- **Atualizações para as seguinte estruturas de aplicações e ferramentas**:
    - Adicionar **.Net Core 2.0** suportar
    - Adicionar **Node.JS** versões:
        - 6.11.2
        - 6.11.5
        - 7.10.1
        - 8.0.0
        - 8.1.4
        - 8.4.0
        - 8.5.0
        - 8.7.0
        - 8.8.1
        - 8.9.0
    - Adicionar **NPM** versões:
        - 3.10.10
        - 4.2.0
        - 5.0.0
        - 5.0.3
        - 5.3.0
        - 5.4.2
        - 5.5.1
    - Adicionar **PHP** atualizações:
        - 5.6.32
        - 7.0.26 (x86 e x64)
        - 7.1.12 (x86 e x64)
    - Atualizado **Git para Windows** v 2.14.1
    - Atualizado **Mercurial** para v4.5.0

  - Foi adicionado suporte para **apenas HTTPS** funcionalidade na funcionalidade do domínio personalizado no Portal de inquilino do serviço de aplicações. 

  - Foram adicionada validação da ligação de armazenamento no selecionador de armazenamento personalizada para as funções do Azure 

#### <a name="fixes"></a>Correções

- Ao criar um pacote de implementação offline, os clientes deixará de receber um acesso negado a mensagem de erro ao abrir a pasta do instalador do serviço de aplicações

- Resolver problemas ao trabalhar na funcionalidade de domínios personalizados no Portal de inquilino do serviço de aplicações.

- Impedir que os clientes utilizando nomes de administrador reservado durante a configuração

- Ativar a implementação de serviço de aplicações com **associado a um domínio** servidor de ficheiros

- Obtenção melhorada de raiz de pilha do Azure no script de certificado e agora validar o certificado de raiz no instalador do serviço de aplicações.

- Estado incorreto fixo que está a ser devolvido para o Azure Resource Manager quando uma subscrição é eliminado que recursos contida no espaço de nomes ' Microsoft. Web.

### <a name="known-issues-with-the-deployment-process"></a>Problemas conhecidos relacionados com o processo de implementação

- Erros de validação de certificado

Alguns clientes tem encontrados problemas quando fornecer certificados para o instalador do serviço de aplicações quando implementar um sistema integrada, devido a validação excessivamente restritiva no instalador do. O instalador do serviço de aplicação foi lançado novamente, os clientes devem [transferir o instalador atualizado](https://aka.ms/appsvconmasinstaller). Se continuar a ter problemas de validação de certificados com o instalador atualizado, contacte o suporte.

- Problema ao obter o certificado de raiz do Azure pilha do sistema integrado.

Um erro no Get-AzureStackRootCert.ps1 causou uma falha ao obter o certificado de raiz de pilha do Azure ao executar o script num computador que não tenha o certificado de raiz instalado os clientes. O script também foi novamente lançado, resolver este problema e os clientes de pedido [transferir os scripts de programa auxiliar atualizado](https://aka.ms/appsvconmashelpers). Se continuar a ter problemas ao obter o certificado de raiz com o script de atualizados, contacte o suporte.

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos relacionados com o processo de atualização

- Não existem não existem problemas conhecidos para a atualização do serviço de aplicações do Azure no Azure pilha Update 1.

### <a name="known-issues-post-installation"></a>Problemas conhecidos (pós-instalação)

- Troca de ranhura não funcionar

Troca de ranhura de site é dividida nesta versão. Para restaurar a funcionalidade, conclua estes passos:

1. Modifique o grupo de segurança de rede ControllersNSG para **permitir** ligações de ambiente de trabalho remotas para as instâncias de controlador do serviço de aplicações. Substitua o nome do grupo de recursos que implementou o serviço de aplicações no AppService.local.

    ```powershell
      Add-AzureRmAccount -EnvironmentName AzureStackAdmin

      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

2. Navegue para o **CN0 VM** em máquinas virtuais no portal do administrador de pilha do Azure e **clicar em ligar** para abrir uma sessão de ambiente de trabalho remoto com a instância de controlador. Utilize as credenciais especificadas durante a implementação do serviço de aplicações.
3. Iniciar **PowerShell como administrador** e execute o seguinte script

    ```powershell
        Import-Module appservice

        $sm = new-object Microsoft.Web.Hosting.SiteManager

        if($sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$true)
        {
          $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus=$false
        #  'Slot swap mode reverted'
        }
        
        # Confirm new setting is false
        $sm.HostingConfiguration.SlotsPollWorkerForChangeNotificationStatus
        
        # Commit Changes
        $sm.CommitChanges()

        Get-AppServiceServer -ServerType ManagementServer | ForEach-Object Repair-AppServiceServer
        
    ```

4. Feche a sessão de ambiente de trabalho remota.
5. Reverter o grupo de segurança de rede ControllersNSG para **negar** ligações de ambiente de trabalho remotas para as instâncias de controlador do serviço de aplicações. Substitua o nome do grupo de recursos que implementou o serviço de aplicações no AppService.local.

    ```powershell

        Add-AzureRmAccount -EnvironmentName AzureStackAdmin

        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
- Trabalhadores não conseguem alcançar o servidor de ficheiros quando o serviço de aplicações está implementado na rede virtual existente e o servidor de ficheiros só está disponível na rede privada.
 
Se optar por implementar uma rede virtual existente e um endereço IP para ligar ao seu servidor de ficheiros, tem de adicionar uma regra de segurança de saída, permitindo o tráfego entre a sub-rede de trabalho e o servidor de ficheiros SMB. Para fazê-lo, aceda a WorkersNsg no Portal de administração e adicionar uma regra de segurança de saída com as seguintes propriedades:
 * Origem: nenhuma
 * Intervalo de portas de origem: *
 * Destino: Endereços IP
 * Intervalo de endereços IP de destino: intervalo de IPs para o servidor de ficheiros
 * Intervalo de portas de destino: 445
 * Protocolo: TCP
 * Ação: permitir
 * Prioridade: 700
 * Nome: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Problemas conhecidos para administradores de nuvem funcionamento do serviço de aplicações do Azure na pilha do Azure

Consulte a documentação do [notas de versão 1802 de pilha do Azure](azure-stack-update-1802.md)

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do App Service do Azure, consulte [do serviço de aplicações do Azure na descrição geral do Azure pilha](azure-stack-app-service-overview.md).
- Para obter mais informações sobre como preparar a implementação do serviço de aplicações na pilha do Azure, consulte [antes de começar com o serviço de aplicações na pilha de Azure](azure-stack-app-service-before-you-get-started.md).
