---
title: Atualizar para uma conta de armazenamento para fins gerais v2 - armazenamento do Azure | Documentos da Microsoft
description: Atualizar para contas de armazenamento para fins gerais v2.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/11/2018
ms.author: tamram
ms.openlocfilehash: 8a0b823a12178df56417b05de28c2125ec155829
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45740818"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Atualizar para uma conta de armazenamento para fins gerais v2

Contas de armazenamento para fins gerais v2 de suportar as funcionalidades de armazenamento do Azure mais recente e incorporam toda a funcionalidade de fins gerais v1 e contas de armazenamento de Blobs. Contas para fins gerais v2 são recomendadas para a maioria dos cenários de armazenamento. Contas para fins gerais v2 fornecem o mais baixo por gigabyte os preços de capacidade do armazenamento do Azure, bem como competitiva em termos da indústria para transações.

Atualizar para uma conta de armazenamento para fins gerais v2 de sua para fins gerais v1 ou contas de armazenamento de BLOBs é simples. Pode atualizar com o portal do Azure, PowerShell ou da CLI do Azure. Atualizar uma conta não pode ser invertido e poderá resultar em despesas de faturação.

## <a name="upgrade-using-the-azure-portal"></a>Atualizar com o portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Navegue até à sua conta de armazenamento.
3. Na **configurações** secção, clique em **configuração**.
4. Em **Tipo de conta**, clique em **Atualizar**.
5. Em **Confirmar atualização**, introduza o nome da sua conta. 
6. Clique em **atualizar** na parte inferior do painel.

## <a name="upgrade-with-powershell"></a>Atualizar com o PowerShell

Para atualizar uma conta para fins gerais v1 para uma conta de fins gerais v2 com o PowerShell, primeiro Atualize o PowerShell para utilizar a versão mais recente do **azurerm. Storage** módulo. Veja [Instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) para obter informações sobre a instalação do PowerShell. 

Em seguida, chame o seguinte comando para atualizar a conta, substituindo o nome do grupo de recursos e a conta de armazenamento:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>Atualizar com a CLI do Azure

Para atualizar uma conta para fins gerais v1 para uma conta de fins gerais v2 com a CLI do Azure, instale primeiro a versão mais recente da CLI do Azure. Veja [Install the Azure CLI 2.0 ](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) (Instalar a CLI 2.0 do Azure) para obter informações sobre a instalação da CLI. 

Em seguida, chame o seguinte comando para atualizar a conta, substituindo o nome do grupo de recursos e a conta de armazenamento:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
``` 

## <a name="specify-an-access-tier-for-blob-data"></a>Especificar um escalão de acesso para os dados de blob

As contas de fins gerais v2 suportam todos os serviços de armazenamento do Azure e objetos de dados, mas o escalão de acesso está disponível apenas para blobs de blocos no armazenamento de Blobs. Quando atualizar para uma conta de armazenamento para fins gerais v2, pode especificar um escalão de acesso para os seus dados de Blobs. 

Escalões de acesso permitem-lhe escolher o armazenamento mais económico com base nos seus padrões de utilização antecipada. Os blobs de blocos podem ser armazenados num acesso frequente, esporádico ou arquivo escalão. Para obter mais informações sobre as camadas de acesso, consulte [armazenamento de Blobs do Azure: frequente, esporádico e de camadas de armazenamento de arquivo](../blobs/storage-blob-storage-tiers.md).

Por predefinição, é criada uma nova conta de armazenamento no escalão acesso frequente e uma conta de armazenamento para fins gerais v1 é atualizada para o escalão de acesso frequente. Se está a explorar a camada de acesso a utilizar para a sua pós-atualização de dados, considere o seu cenário. Existem dois cenários de usuário comum para a migração para uma conta para fins gerais v2:

* Tem uma conta de armazenamento para fins gerais v1 existente e pretende avaliar uma alteração para uma conta de armazenamento para fins gerais v2, com a camada de armazenamento correta para dados de Blobs.
* Decidiu utilizar uma conta de armazenamento para fins gerais v2 ou já tiver uma e pretende avaliar se deve utilizar a camada de armazenamento frequente ou esporádico para dados de Blobs.

Em ambos os casos, a primeira prioridade é estimar os custos de armazenamento, acesso e a funcionar nos seus dados armazenados numa conta de armazenamento para fins gerais v2 e compará-los com seus custos atuais.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Estimar os custos para os padrões de utilização atual

Para estimar os custos de armazenar e aceder aos dados de BLOBs numa conta de armazenamento para fins gerais v2 numa camada específica, avaliar o seu padrão de utilização existente ou fazer uma aproximação do padrão de utilização esperado. Em geral, precisa de saber o seguinte:

* O consumo de armazenamento Blob, em gigabytes, incluindo:
    - Que quantidade de dados está a ser armazenada na conta de armazenamento?
    - De que forma o volume de dados é alterado mensalmente: os novos dados substituem constantemente os dados antigos?
* O padrão de acesso primária para os seus dados de armazenamento de BLOBs, incluindo:
    - A quantidade de dados está a ser lida da e escrita para a conta de armazenamento? 
    - Quantas operações de leitura versus escrever operações ocorrem nos dados na conta de armazenamento?

Para decidir qual a camada de acesso melhor às suas necessidades, pode ser útil para determinar quanta capacidade seus dados de Blobs está atualmente a utilizar e como esses dados está a ser utilizados. 

Para recolher dados de utilização para a sua conta de armazenamento antes da migração, pode monitorizar a conta de armazenamento através de [do Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). O Azure Monitor efetua o registo e fornece dados de métricas para os serviços do Azure, incluindo o armazenamento do Azure. 

Para monitorizar os dados de consumo para os blobs na sua conta de armazenamento, ative métricas de capacidade no Azure Monitor. Métricas de capacidade gravam dados sobre a quantidade de armazenamento os blobs na sua conta estiver a utilizar numa base diária. Métricas de capacidade podem ser utilizadas para calcular o custo do armazenamento de dados na conta de armazenamento. Para saber como a capacidade de armazenamento de BLOBs para é cobrada para cada tipo de conta, veja [preços do blob de bloco](https://azure.microsoft.com/pricing/details/storage/blobs/).

Para monitorizar os padrões de acesso de dados para armazenamento de BLOBs, ative métricas de transação no Azure Monitor. Pode filtrar diferentes operações de armazenamento do Azure para estimar a frequência com que cada um é chamado. Para saber como diferentes tipos de transações têm um preço para o bloco e de acréscimo para cada tipo de conta, consulte [preços do blob de bloco](https://azure.microsoft.com/pricing/details/storage/blobs/).  

Para obter mais informações sobre a coleta de métricas do Azure Monitor, consulte [métricas de armazenamento do Azure no Azure Monitor](storage-metrics-in-azure-monitor.md).

## <a name="next-steps"></a>Passos Seguintes

- [Criar uma conta de armazenamento](storage-quickstart-create-account.md)
- [Gerir contas de armazenamento do Azure](storage-account-manage.md)