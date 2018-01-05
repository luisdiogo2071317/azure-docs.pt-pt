---
title: Gerir a capacidade de armazenamento na pilha do Azure | Microsoft Docs
description: "Monitorizar e gerir o espaço de armazenamento disponível para a pilha do Azure."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: b0e694e4-3575-424c-afda-7d48c2025a62
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: brenduns
ms.reviewer: jiahan
ms.openlocfilehash: dce4252846732ca5161018103438df1f9ff6146d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="manage-storage-capacity-for-azure-stack"></a>Gerir a capacidade de armazenamento para a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

As informações neste artigo ajuda-o monitor de operador da nuvem de pilha do Azure e gerir a capacidade de armazenamento da implementação deles pilha do Azure. A infraestrutura de armazenamento de pilha do Azure aloca um subconjunto da capacidade de armazenamento total da implementação da pilha do Azure, a ser utilizada para **dos serviços de armazenamento**. Os serviços do storage armazenam dados de um inquilino em partilhas em volumes que correspondem a nós da implementação.

Como um operador da nuvem, tem uma quantidade limitada de armazenamento para trabalhar com. A quantidade de armazenamento é definida pela solução de que implementar. A solução é fornecida pelo seu fornecedor de OEM quando utiliza uma solução de vários nós ou de hardware no qual instala o Kit de desenvolvimento de pilha do Azure.

Porque a pilha do Azure não suporta a expansão da capacidade de armazenamento, é importante [monitor](#monitor-shares) o armazenamento disponível para garantir que as operações eficiente são mantidas.  

Quando a capacidade livre restante de uma partilha fica limitada, planeie [gerir espaço](#manage-available-space) para impedir que as partilhas em execução sem capacidade.

As opções para gerir a capacidade incluem:
- Recuperar a capacidade
- Migrar um contentor

Quando é uma partilha de 100% utilizados, o armazenamento já não serviço funções para que a partilha. Para obter assistência em operações para a partilha de restauro, contacte o suporte da Microsoft.

## <a name="understand-volumes-and-shares-containers-and-disks"></a>Compreender os volumes e partilhas, contentores e discos
### <a name="volumes-and-shares"></a>Volumes e partilhas
O *serviço de armazenamento* partições o armazenamento disponível em separado e iguais volumes que estão atribuídas para armazenar dados de inquilino. O número de volumes é igual ao número de nós na implementação de pilha do Azure:

- Numa implementação de quatro nós, existem quatro volumes. Cada volume possui uma única partilha. Numa implementação com vários nós, o número de partilhas não é reduzido, se um nó é removido ou malfunctioning.
- Se utilizar o Kit de programação de pilha do Azure, não há um único volume com uma única partilha.

Dado que as partilhas de serviço de armazenamento para a utilização exclusiva do serviços de armazenamento, deve não diretamente modificar, adicionar ou remover todos os ficheiros nas partilhas. Apenas os serviços de armazenamento devem funcionar nos ficheiros armazenados nestes volumes.

Partilhas em volumes armazena dados de inquilino. Dados de inquilino incluem os blobs de páginas, blobs de blocos, acrescentar blobs, tabelas, filas, bases de dados e relacionadas com arquivos de metadados. Porque os objetos de armazenamento (blobs, etc.) individualmente estão contidos dentro de uma única partilha, o tamanho máximo de cada objeto não pode exceder o tamanho de uma partilha. O tamanho máximo dos novos objetos depende de capacidade que permanece numa partilha, como o espaço não utilizado quando esse novo objeto é criado.

Quando é uma partilha baixa no espaço livre e as ações para [recuperar](#reclaim-capacity) espaço não são efetuadas com êxito ou disponível, o operador da nuvem do Azure pilha pode [migrar](#migrate-a-container-between) os contentores de BLOBs de uma partilha para outro.

- Para obter mais informações sobre contentores e blobs, consulte [armazenamento de BLOBs](azure-stack-key-features.md#blob-storage) em funcionalidades de chave e conceitos na pilha do Azure.
- Para obter informações sobre como funcionam os utilizadores do inquilino com o blob storage na pilha do Azure, consulte [dos serviços de armazenamento do Azure pilha](/azure/azure-stack/user/azure-stack-storage-overview#azure-stack-storage-services).


### <a name="containers"></a>Contentores
Utilizadores de inquilino criam contentores, em seguida, são utilizados para armazenar dados de Blobs. Enquanto o utilizador decide qual contentor colocar blobs, o serviço de armazenamento utiliza um algoritmo para determinar qual volume para colocar o contentor. O algoritmo normalmente escolhe o volume com o espaço máximo disponível.  

Depois de um blob é colocado num contentor, pode aumentar esse blob utilizar mais espaço. À medida que adiciona novos blobs e existente blobs aumentar, o espaço disponível no volume que contém esse contentor diminui.  

Contentores não estão limitados a uma única partilha. Quando os dados de blob combinados num contentor que cresce utilize 80% ou mais do espaço disponível, o contentor entra *capacidade excedida* modo. No modo de capacidade excedida, novos blobs que são criados no contentor são atribuídos para um volume diferente que tenha espaço suficiente. Ao longo do tempo, um contentor no modo de capacidade excedida pode ter blobs que estão distribuídos por vários volumes.

Quando é utilizada a 80% e 90% de espaço disponível no volume, o sistema gera alertas no portal de administrador da pilha do Azure. Os operadores da nuvem deverão consultar a capacidade de armazenamento disponível e planeie a rebalancear o conteúdo. O serviço de armazenamento deixa de funcionar quando um disco é 100% utilizado e não existem alertas adicionais.

### <a name="disks"></a>Discos
Discos VM são adicionados a contentores pelos inquilinos e incluem um disco de sistema operativo. As VMs podem também ter um ou mais discos de dados. Ambos os tipos de discos são armazenados como blobs de páginas. As orientações para inquilinos é colocar todos os discos para um contentor separado para melhorar o desempenho da VM.
- Cada contentor que retém um disco (BLOBs de páginas) de uma VM é considerado um contentor ligado à VM que detém o disco.
- Um contentor que não contém qualquer disco de uma VM é considerado um contentor livre.

As opções para libertar espaço num contentor anexado [estão limitadas](#move-vm-disks).
> [!TIP]  
> Os operadores da nuvem não gere diretamente discos, que estão anexados às máquinas virtuais (VMs) que os inquilinos podem adicionar a um contentor. No entanto, quando planear gerir o espaço em partilhas de armazenamento, pode ser de utilização para compreender a inter-relação entre discos a contentores e partilhas.

## <a name="monitor-shares"></a>Partilhas de monitor
Utilize o PowerShell ou o portal de administração para monitorizar as partilhas para que possa compreender quando o espaço livre é limitado. Quando utilizar o portal, receber alertas sobre partilhas que são baixas num espaço.    

### <a name="use-powershell"></a>Utilizar o PowerShell
Como um operador da nuvem, pode monitorizar a capacidade de armazenamento de uma partilha utilizando o PowerShell **Get-AzsStorageShare** cmdlet. O cmdlet Get-AzsStorageShare devolve o espaço livre, alocado e total em bytes em cada uma das partilhas.   
![Exemplo: Espaço livre para partilhas de retorno](media/azure-stack-manage-storage-shares/free-space.png)

- **Total de capacidade** é o espaço total em bytes, que estão disponíveis na partilha. Este espaço é utilizado para dados e metadados que é mantido pelos serviços de armazenamento.
- **Utilizado capacidade** é a quantidade de dados em bytes, que é utilizado pelas todas as extensões dos ficheiros que armazenam os dados de inquilino e metadados associados.

### <a name="use-the-administrator-portal"></a>Utilizar o portal de administrador
Como um operador da nuvem, pode utilizar o portal de administração para ver a capacidade de todas as partilhas de armazenamento. **Aceda ao armazenamento** > **partilhas de ficheiros** para abrir a lista de partilha de ficheiros onde pode ver as informações de utilização.
![Exemplo: Partilhas de ficheiros de armazenamento](media/azure-stack-manage-storage-shares/storage-file-shares.png)
- **TOTAL** é o espaço total em bytes, que estão disponíveis na partilha. Este espaço é utilizado para dados e metadados que é mantido pelos serviços de armazenamento.
- **UTILIZADO** é a quantidade de dados em bytes, que é utilizado pelas todas as extensões dos ficheiros que armazenam os dados de inquilino e metadados associados.

### <a name="storage-space-alerts"></a>Alertas de espaço de armazenamento
Quando utiliza o portal de administração, receber alertas sobre partilhas que são baixas num espaço.

> [!IMPORTANT]
> Como um operador da nuvem, manter partilhas atingir a utilização total. Quando é uma partilha de 100% utilizados, o armazenamento já não serviço funções para que a partilha. Para recuperar o espaço livre e restaurar as operações numa partilha que é 100% utilizados, tem de contactar o suporte da Microsoft.

**Aviso**: quando uma partilha de ficheiros é superior a 80% utilizados, receberá um *aviso* alerta no portal de administração: ![exemplo: alerta de aviso](media/azure-stack-manage-storage-shares/alert-warning.png)


**Crítico**: quando uma partilha de ficheiros é superior a 90% utilizados, receberá um *críticos* alerta no portal de administração: ![exemplo: alerta crítico](media/azure-stack-manage-storage-shares/alert-critical.png)

**Ver detalhes**: no portal de administração podem abrir os detalhes de um alerta para ver as opções de atenuação: ![exemplo: ver detalhes do alerta](media/azure-stack-manage-storage-shares/alert-details.png)


## <a name="manage-available-space"></a>Gerir o espaço disponível
Quando é necessário espaço livre numa partilha, utilize os métodos menos INVASIVO primeiro. Por exemplo, tente recuperar espaço antes de optar por migrar um contentor.  

### <a name="reclaim-capacity"></a>Recuperar a capacidade
*Esta opção aplica-se para implementações de vários nós e o Kit de desenvolvimento de pilha do Azure.*

Pode recuperar a capacidade utilizada pelas contas de inquilino que tenham sido eliminadas. Esta capacidade é automaticamente recuperada quando os dados [período de retenção](azure-stack-manage-storage-accounts.md#set-the-retention-period) for atingido, ou pode agir para recuperá-lo imediatamente.

Para obter mais informações, consulte [recuperar a capacidade](azure-stack-manage-storage-accounts.md#reclaim) gerir recursos de armazenamento.

### <a name="migrate-a-container-between-volumes"></a>Migrar um contentor entre volumes
*Esta opção só se aplica a implementações de vários nós.*

Devido a padrões de utilização do inquilino, algumas partilhas inquilino utilizam mais espaço do que outros. O resultado pode ser uma partilha que executa baixa num espaço antes de outras partilhas que estão relativamente não utilizados.

Pode tentar libertar espaço numa partilha overused migrando manualmente alguns contentores de BLOBs para outra partilha. Pode migrar vários contentores mais pequenos para uma única partilha que tenha capacidade para conter todos eles. Pode utilizar a migração para mover *livre* contentores. Contentores livres são contentores que não contém um disco para uma VM.   

Migração consolida todas as um blob de contentores na partilha de novo.

- Se um contentor entrou no modo de capacidade excedida e colocou blobs em volumes adicionais, a nova partilha tem de ter capacidade suficiente para conter todos os blobs para o contentor que migrar. Isto inclui os blobs que se encontram em partilhas adicionais.

- O cmdlet do PowerShell *Get-AzsStorageContainer* identifica apenas o espaço utilizado no volume inicial para um contentor. O cmdlet não identifica espaço utilizado por blobs colocados em volumes adicionais. Por conseguinte, o respetivo tamanho total de um contentor pode não estar evidente. É possível que a consolidação de um contentor numa nova partilha pode enviar essa nova partilha numa condição excedida onde coloca dados nas partilhas adicionais. Como resultado, poderá ter de rebalancear partilhas novamente.

- Se a falta de permissões para um grupo de recursos e não é possível utilizar o PowerShell para consultar os volumes adicionais de dados de capacidade excedida, trabalhar com o proprietário desses grupos de recursos e contentores para compreender o tamanho total dos dados de migração antes de migrar os dados.  

> [!IMPORTANT]
> Migração de blobs para um contentor é uma operação offline que requer a utilização do PowerShell. Até concluir a migração, todos os blobs para o contentor que está a migrar permanecerem offline e não podem ser utilizados.

#### <a name="to-migrate-containers-using-powershell"></a>Para migrar os contentores utilizando o PowerShell
1. Confirme que tem [Azure PowerShell instalada e configurada](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Para obter mais informações, veja [Using Azure PowerShell with Azure Resource Manager (Utilizar o Azure PowerShell com o Azure Resource Manager)](http://go.microsoft.com/fwlink/?LinkId=394767).
2.  Examine o contentor para compreender que dados são a partilha de que pretende migrar. Para identificar os contentores de candidatos melhor para a migração de um volume, utilize o **Get-AzsStorageContainer** cmdlet:

    ```
    $shares = Get-AzsStorageShare
    $containers = Get-AzsStorageContainer -ShareName $shares[0].ShareName -Intent Migration
    ```
    Em seguida, examine $containers:
    ```
    $containers
    ```
    ![Exemplo: $Containers](media/azure-stack-manage-storage-shares/containers.png)

3.  Identificar as melhores partilhas de destino para armazenar o contentor que migrar:
    ```
    $destinationshares = Get-AzsStorageShare -SourceShareName
    $shares[0].ShareName -Intent ContainerMigration
    ```
    Em seguida, examine $destinationshares:
    ```
    $destinationshares
    ```    
    ![Exemplo: $destination partilhas](media/azure-stack-manage-storage-shares/examine-destinationshares.png)

4. Começar a migração para um contentor. A migração é assíncrona. Se começar a migração de contentores adicionais antes de concluir a migração primeiro, utilize o id da tarefa para controlar o estado de cada.
  ```
  $jobId = Start-AzsStorageContainerMigration -ContainerToMigrate $containers[1] -DestinationShareUncPath $destinationshares[0].UncPath
  ```
  Em seguida, examine $jobId. No exemplo seguinte, substitua *d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0* com o id da tarefa que pretende examinar:
  ```
  $jobId
  d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0
  ```
5. Utilize o id da tarefa para verificar o estado da tarefa de migração. Quando a migração de contentor estiver concluída, **MigrationStatus** está definido como **concluída**.
  ```
  Get-AzsStorageContainerMigrationStatus -JobId $jobId
  ```
  ![Exemplo: Estado de migração](media/azure-stack-manage-storage-shares/migration-status1.png)

6.  Pode cancelar uma tarefa de migração em curso. Cancelar as tarefas são processadas de forma assíncrona de migração. Pode controlar o cancelamento utilizando $jobid:

  ```
  Stop-AzsStorageContainerMigration -JobId $jobId
  ```
  ![Exemplo: Estado de reversão](media/azure-stack-manage-storage-shares/rollback.png)

7. Pode executar o comando no passo 6 novamente, até que o estado confirma que a tarefa de migração é **cancelado**:  
    ![Exemplo: Estado cancelado](media/azure-stack-manage-storage-shares/cancelled.png)

### <a name="move-vm-disks"></a>Mover os discos VM
*Esta opção só se aplica a implementações de vários nós.*

O método mais extremos para gerir o espaço envolve a mudança de discos da máquina virtual. Como mover um contentor anexado (um que contém um disco da VM) é complexa, contacte a Microsoft Support para realizar esta ação.

## <a name="next-steps"></a>Próximos Passos
Saiba mais sobre [oferta de máquinas virtuais para os utilizadores](azure-stack-tutorial-tenant-vm.md).
