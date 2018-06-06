---
title: Gerir servidores registados com sincronização de ficheiros do Azure (pré-visualização) | Microsoft Docs
description: Saiba como registar e anular o registo do Windows Server com um serviço de sincronização de armazenamento do Azure ficheiro sincronização.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: wgries
ms.openlocfilehash: 7385e8b84668facf8bf44f569a611e7dcdba9a1e
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738297"
---
# <a name="manage-registered-servers-with-azure-file-sync-preview"></a>Gerir servidores registados com sincronização de ficheiros do Azure (pré-visualização)
O Azure File Sync (pré-visualização) permite-lhe centralizar as partilhas de ficheiros da sua organização nos Ficheiros do Azure sem abdicar da flexibilidade, do desempenho e da compatibilidade de um servidor de ficheiros no local. Fazê-lo por transformar os seus servidores do Windows para uma cache rápida da Azure da partilha de ficheiros. Pode utilizar qualquer protocolo disponível no Windows Server para aceder aos seus dados localmente (incluindo SMB, NFS e FTPS) e pode ter o número de caches que precisar em todo o mundo.

O seguinte artigo ilustra como registar e gerir um servidor com um serviço de sincronização de armazenamento. Consulte [como implementar a sincronização de ficheiros do Azure (pré-visualização)](storage-sync-files-deployment-guide.md) para obter informações sobre como implementar a sincronização de ficheiros do Azure ponto-a-ponto.

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Registar/anular o registo de um servidor com o serviço de sincronização de armazenamento
Registar um servidor de sincronização de ficheiros do Azure estabelece uma relação de confiança entre o Windows Server e do Azure. Esta relação pode ser utilizada para criar *pontos finais do* no servidor, que representa a pastas específicas que devem ser sincronizadas com uma partilha de ficheiros do Azure (também conhecido como um *ponto final da nuvem*). 

### <a name="prerequisites"></a>Pré-requisitos
Para registar um servidor com um serviço de sincronização de armazenamento, primeiro tem de preparar o seu servidor com os pré-requisitos necessários:

* O servidor deve estar em execução uma versão suportada do Windows Server. Para obter mais informações, consulte [versões do Windows Server](storage-sync-files-planning.md#supported-versions-of-windows-server).
* Certifique-se de que o serviço de sincronização de armazenamento foi implementado. Para obter mais informações sobre como implementar um serviço de sincronização de armazenamento, consulte [como implementar a sincronização de ficheiros do Azure (pré-visualização)](storage-sync-files-deployment-guide.md).
* Certifique-se de que o servidor está ligado à internet e de que o Azure está acessível.
* Desative a configuração de segurança avançada do IE para os administradores de com a IU do Gestor de servidor.
    
    ![IU do Gestor de servidor com a configuração avançada do IE segurança realçado](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Certifique-se de que o módulo do AzureRM PowerShell está instalado no seu servidor. Se o servidor for um membro de um Cluster de ativação pós-falha, cada nó do cluster irá exigir o módulo de AzureRM. Obter mais detalhes sobre como instalar o módulo de AzureRM podem ser encontrados no [instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Recomendamos que utilize a versão mais recente do módulo do AzureRM PowerShell para registar/anular o registo do servidor. Se o pacote de AzureRM tiver sido instalado anteriormente neste servidor (e a versão do PowerShell neste servidor é 5.* ou superior), pode utilizar o `Update-Module` cmdlet para este pacote de atualização. 
* Se utilizar um servidor de proxy de rede no seu ambiente, configure as definições de proxy no seu servidor para o agente de sincronização utilizar.
    1. Determinar o número de porta e o endereço IP do proxy
    2. Edite estes dois ficheiros:
        * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
        * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    3. Adicione as linhas na figura 1 (abaixo nesta secção) em /System.ServiceModel em dois ficheiros acima alterar 127.0.0.1:8888 para o endereço IP correto (substituir 127.0.0.1) e o número de porta correto (substituir 8888):
    4. Configurar as definições de proxy de WinHTTP através de linha de comandos:
        * Mostrar a proxy: netsh winhttp Mostrar proxy
        * Definir o proxy: netsh winhttp define 127.0.0.1:8888 de proxy
        * Repor o proxy: netsh winhttp repor proxy
        * Se esta configuração depois do agente está instalado, em seguida, reinicie o nosso agente de sincronização: net stop filesyncsvc
    
```XML
    Figure 1:
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
    </system.net>
```    

### <a name="register-a-server-with-storage-sync-service"></a>Registar um servidor com o serviço de sincronização de armazenamento
Antes de um servidor pode ser utilizado como um *ponto final do servidor* numa sincronização de ficheiros do Azure *grupo de sincronização*, tem de ser registado com um *o serviço de sincronização de armazenamento*. Um servidor só pode ser registado com um serviço de sincronização de armazenamento de cada vez.

#### <a name="install-the-azure-file-sync-agent"></a>Instalar o agente de sincronização de ficheiros do Azure
1. [Transferir o agente de sincronização de ficheiros do Azure](https://go.microsoft.com/fwlink/?linkid=858257).
2. Inicie o instalador do agente de sincronização de ficheiros do Azure.
    
    ![O primeiro painel do instalador do agente de sincronização de ficheiros do Azure](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Lembre-se de que ativa as atualizações para o agente de sincronização de ficheiros do Azure utilizando o Microsoft Update. É importante porque as correções de segurança críticas e melhoramentos de funcionalidades para o pacote de servidor são fornecidos através do Microsoft Update.

    ![Certifique-se de que o Microsoft Update está ativado no painel do Microsoft Update do programa de instalação do agente de sincronização de ficheiros do Azure](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Se o servidor ainda não foi registado, o registo do servidor IU irá aparecer imediatamente após a conclusão da instalação.

> [!Important]  
> Se o servidor for um membro de um Cluster de ativação pós-falha, o agente de sincronização de ficheiros do Azure tem de ser instalado em cada nó no cluster.

#### <a name="register-the-server-using-the-server-registration-ui"></a>Registar o servidor utilizando o registo do servidor da IU
> [!Important]  
> Subscrições de fornecedor de solução (CSP) na nuvem não é possível utilizar o registo do servidor da IU. Em alternativa, utilize o PowerShell (abaixo nesta secção).

1. Se o registo do servidor da IU não foram iniciados imediatamente após a conclusão da instalação do agente de sincronização de ficheiros do Azure, que pode ser iniciado manualmente, executando `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Clique em *início de sessão* para aceder à sua subscrição do Azure. 

    ![Abrir a caixa de diálogo do registo do servidor da IU](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Escolha a subscrição correta, o grupo de recursos e o serviço de sincronização de armazenamento da caixa de diálogo.

    ![Informações do serviço de sincronização de armazenamento](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. Pré-visualização, um mais início de sessão, é necessário para concluir o processo. 

    ![Inicie sessão na caixa de diálogo](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Se o servidor for um membro de um Cluster de ativação pós-falha, cada servidor tem de executar o registo do servidor. Ao visualizar os servidores registados no Portal do Azure, sincronização de ficheiros do Azure automaticamente reconhece cada nó como um membro do mesmo Cluster de ativação pós-falha e agrupa-los em conjunto corretamente.

#### <a name="register-the-server-with-powershell"></a>Registar o servidor com o PowerShell
Também pode efetuar o registo do servidor através do PowerShell. Esta é a única forma de suportados do registo de servidor de subscrições do fornecedor de solução em nuvem (CSP):

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Anular o registo do servidor com o serviço de sincronização de armazenamento
Existem vários passos que são necessárias para anular o registo de um servidor com um serviço de sincronização de armazenamento. Vamos ver como corretamente anular o registo de um servidor.

> [!Warning]  
> Tente resolver problemas com a sincronização, na nuvem em camadas ou qualquer outro aspeto de sincronização de ficheiros do Azure ao anular o registo e registar um servidor, ou a remover e recriar os pontos finais de servidor, a menos que explicitamente instruído para por um engenheiro de Microsoft. Um servidor de anulação de registo e remover pontos finais do servidor são uma operação destrutivas e, em camadas ficheiros em volumes com pontos finais do servidor serão não ser "ligação restabelecidos" para as respetivas localizações na partilha de ficheiros do Azure depois do servidor registado e pontos finais do servidor recriados, que resultará em sincronização erros. Tenha também em atenção, em camadas ficheiros existentes fora de um espaço de nomes de ponto final do servidor podem ser perdidos permanentemente. Os ficheiros em camadas podem existir no servidor nunca foi ativada pontos finais, mesmo quando a criação de camadas de nuvem.

#### <a name="optional-recall-all-tiered-data"></a>(Opcional) Recuperar todos os dados em camadas
Se pretender que os ficheiros que estão atualmente em camadas para estar disponível depois de remover a sincronização de ficheiros do Azure (ou seja, este é uma produção, não um teste, ambiente), recuperar todos os ficheiros em cada volume que contém os pontos finais do servidor. Desativar a criação de camadas para todos os pontos finais de servidor de nuvem e, em seguida, execute o seguinte cmdlet do PowerShell:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <a-volume-with-server-endpoints-on-it>
```

> [!Warning]  
> Se o volume local que aloja o ponto final do servidor não tem espaço livre suficiente para recuperar todos os dados em camadas, o `Invoke-StorageSyncFileRecall` cmdlet irá falhar.  

#### <a name="remove-the-server-from-all-sync-groups"></a>Remova o servidor de todos os grupos de sincronização
Antes de anular o registo do servidor no serviço de sincronização de armazenamento, é necessário remover todos os pontos finais nesse servidor. Isto pode ser feito através do portal do Azure:

1. Navegue para o serviço de sincronização de armazenamento em que o servidor está registado.
2. Remova todos os pontos finais de servidor para este servidor em cada grupo de sincronização no serviço de sincronização de armazenamento. Isto pode ser conseguido clicando com o ponto final do servidor relevante no painel do grupo de sincronização.

    ![Remover um ponto final do servidor de um grupo de sincronização](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Isto também pode ser conseguido com um script do PowerShell simple:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Connect-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

#### <a name="unregister-the-server"></a>Anular o registo do servidor
Agora que tem sido resgatar os todos os dados e o servidor foi removido de todos os grupos de sincronização, o servidor pode ser anulado. 

1. No portal do Azure, navegue para o *registado servidores* secção do serviço de sincronização de armazenamento.
2. Faça duplo clique no servidor que pretende anular o registo e clique em "Anular o registo do servidor".

    ![Anular o registo do servidor](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Sincronização de ficheiros do Azure, garantindo que é um boa vizinho no seu centro de dados 
Uma vez que a sincronização de ficheiros do Azure estará raramente o serviço único em execução no seu centro de dados, pode querer limitar a utilização de rede e armazenamento de sincronização de ficheiros do Azure.

> [!Important]  
> A definição demasiado baixa limites terá impacto no desempenho da sincronização de sincronização de ficheiros do Azure e devolução de chamada.

### <a name="set-azure-file-sync-network-limits"></a>Definir limites de rede de sincronização de ficheiros do Azure
Pode limitar a utilização de rede de sincronização de ficheiros do Azure utilizando o `StorageSyncNetworkLimit` cmdlets. 

Por exemplo, pode criar um novo limite de limitação para se certificar de que a sincronização de ficheiros do Azure não utiliza mais de 10 Mbps entre as 09: 00 e as 17: 00 (17:00 h) durante a semana de trabalho: 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

Pode ver o seu limite utilizando o cmdlet seguinte:

```PowerShell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

Para remover limites de rede, utilize `Remove-StorageSyncNetworkLimit`. Por exemplo, o seguinte comando remove todos os limites de rede:

```PowerShell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>Utilizar o QoS de armazenamento do Windows Server 
Quando a sincronização de ficheiros do Azure está alojada numa máquina virtual em execução no anfitrião de Virtualização do Windows Server, pode utilizar o QoS de armazenamento (qualidade de serviço de armazenamento) regular o consumo de e/s de armazenamento. A política QoS de armazenamento pode ser definida como um máximo (ou limite, como como StorageSyncNetwork limite é imposta acima) ou como um mínimo (ou reservas). Definição mínimo em vez de um máximo permite a sincronização de ficheiros do Azure impulsar para utilizar a largura de banda de armazenamento disponível se outras cargas de trabalho não estão a utilizá-la. Para obter mais informações, consulte [qualidade do serviço de armazenamento](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview).

## <a name="see-also"></a>Consulte também
- [Planear uma implementação de sincronização de ficheiros do Azure (pré-visualização)](storage-sync-files-planning.md)
- [Implementar a sincronização de ficheiros do Azure (pré-visualização)](storage-sync-files-deployment-guide.md) 
- [Resolver problemas de sincronização de ficheiros do Azure (pré-visualização)](storage-sync-files-troubleshoot.md)
