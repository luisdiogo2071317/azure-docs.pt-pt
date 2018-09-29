---
title: Mover recursos do Azure para a nova subscrição ou grupo de recursos | Documentos da Microsoft
description: Utilize o Azure Resource Manager para mover recursos para um novo grupo de recursos ou subscrição.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: tomfitz
ms.openlocfilehash: 33d5560f2bfef04678cf7a2236fd920385d68aac
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452161"
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Mover recursos para um novo grupo de recursos ou subscrição

Este artigo mostra como mover recursos para uma nova subscrição ou um grupo de recursos na mesma subscrição. Pode utilizar o portal, PowerShell, CLI do Azure ou a API REST para mover o recurso. As operações de movimentação neste artigo estão disponíveis para sem nenhuma assistência do suporte do Azure.

Quando mover os recursos, o grupo de origem e o grupo de destino estão bloqueados durante a operação. Escrever e eliminar operações os grupos de recursos não é permitido enquanto a migração for concluída. Esse bloqueio significa que não é possível adicionar, atualizar ou eliminar recursos nos grupos de recursos, mas isso não significa que os recursos são interrompidos. Por exemplo, se mover um servidor de SQL e a respetiva base de dados para um novo grupo de recursos, uma aplicação que utiliza a base de dados experiências sem períodos de indisponibilidade. Pode ainda ler e escrever na base de dados.

Não é possível alterar a localização do recurso. Mover um recurso apenas move-o para um novo grupo de recursos. Novo grupo de recursos pode ter uma localização diferente, mas que não altera a localização do recurso.

> [!NOTE]
> Este artigo descreve como mover a oferta de contas de recursos dentro do Azure. Se quiser alterar a sua conta do Azure da oferta (por exemplo, atualizar do pay as you go para pré-pagamento) enquanto continua a trabalhar com os recursos existentes, consulte [mudar a sua subscrição do Azure para outra oferta](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Lista de verificação antes de mover recursos

Antes de mover um recurso, é necessário realizar alguns passos importantes. Ao confirmar estas condições, pode evitar erros.

1. As subscrições de origem e de destino tem de existir no mesmo [inquilino do Azure Active Directory](../active-directory/develop/quickstart-create-new-tenant.md). Para verificar-se de que ambas as subscrições têm o mesmo ID de inquilino, utilize o Azure PowerShell ou a CLI do Azure.

  Para o Azure PowerShell, utilize:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Para a CLI do Azure, utilize:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Se os IDs de inquilino para as subscrições de origem e de destino não são da mesma, utilize os seguintes métodos para reconciliar os IDs de inquilino:

  * [Transferir a propriedade de uma subscrição do Azure para outra conta](../billing/billing-subscription-transfer.md)
  * [Como associar ou adicionar uma subscrição do Azure ao Azure Active Directory](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. A subscrição de destino tem de estar registada no fornecedor de recursos do recurso a ser movido. Se não, receberá um erro a indicar que o **subscrição não está registada para um tipo de recurso**. Poderá encontrar este problema ao mover um recurso para uma nova subscrição, mas em que esta nunca foi utilizada com esse tipo de recurso.

  Para o PowerShell, utilize os seguintes comandos para obter o estado do registo:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Para registar um fornecedor de recursos, utilize:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
  ```

  Para a CLI do Azure, utilize os seguintes comandos para obter o estado do registo:

  ```azurecli-interactive
  az account set -s <destination-subscription-name-or-id>
  az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
  ```

  Para registar um fornecedor de recursos, utilize:

  ```azurecli-interactive
  az provider register --namespace Microsoft.Batch
  ```

1. A conta de mover os recursos tem de ter, pelo menos, as seguintes permissões:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** no grupo de recursos de origem.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** no grupo de recursos de destino.

1. Antes de mover os recursos, verifique as quotas de subscrição para a subscrição que estiver a mover os recursos para. Se mover os recursos significa que a subscrição irá exceder os limites, terá de rever se podem pedir um aumento na quota. Para obter uma lista de limites e como pedir um aumento, consulte [subscrição do Azure e limites do serviço, quotas e restrições](../azure-subscription-service-limits.md).

1. Sempre que possível, quebra de grandes passa para operações de movimentação separado. Gestor de recursos falha imediatamente as tentativas de mover os recursos mais de 800 numa única operação. No entanto, o mover os recursos de menos de 800 também poderá falhar por tempo limite.

1. O serviço tem de permitir a capacidade de mover recursos. Para determinar se a mudança será bem sucedido [validar o seu pedido de movimentação](#validate-move). Consulte as secções abaixo neste artigo, dos quais [serviços permitem mover recursos](#services-that-can-be-moved) e em que [serviços não permitem mover recursos](#services-that-cannot-be-moved).

## <a name="when-to-call-support"></a>Quando deve contactar o suporte

Pode mover a maioria dos recursos por meio de operações de gestão personalizada mostrados neste artigo. Utilize as operações de gestão personalizada para:

* Mover recursos do Resource Manager.
* Mover recursos clássicos de acordo com o [limitações da implementação clássica](#classic-deployment-limitations).

Contacte [suportar](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) quando precisa de:

* Mover os recursos para uma nova conta do Azure (e de inquilino do Azure Active Directory) e precisar de ajuda com as instruções na secção anterior.
* Mover recursos clássicos, mas estiver a ter problemas com as limitações.

## <a name="validate-move"></a>Validar a movimentação

O [validar a operação de movimentação](/rest/api/resources/resources/validatemoveresources) permite testar o seu cenário de movimentação sem, na verdade, mover os recursos. Esta operação é utilizada para determinar se a mudança será bem sucedida. Para executar esta operação, precisa de:

* nome do grupo de recursos de origem
* ID de recurso do grupo de recursos de destino
* ID de recurso de cada recurso para mover
* o [token de acesso](/rest/api/azure/#acquire-an-access-token) para a sua conta

Envie o pedido seguinte:

```
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2018-02-01
Authorization: Bearer <access-token>
Content-type: application/json
```

Com um corpo de pedido:

```json
{
 "resources": ['<resource-id-1>', '<resource-id-2>'],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Se o pedido está formatado corretamente, a operação retornar:

```
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

O código de 202 estado indica foi aceite o pedido de validação, mas ele ainda não ainda por determinar se a operação de movimentação terá êxito. O `location` valor contém um URL que utilizar para verificar o estado da operação de longa execução.  

Para verificar o estado, envie o pedido seguinte:

```
GET <location-url>
Authorization: Bearer <access-token>
```

Enquanto a operação ainda está em execução, continua a receber o código de 202 estado. Aguardar o número de segundos, indicados a `retry-after` valor antes de tentar novamente. Se a operação de movimentação for validado com êxito, receberá o código de 204 estado. Se a validação de movimentação falhar, recebe uma mensagem de erro, tal como:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="services-that-can-be-moved"></a>Serviços que podem ser movidos

A lista seguinte fornece um resumo geral dos serviços do Azure que podem ser movidos para um novo grupo de recursos e subscrição. Para mais detalhes, consulte [mover o suporte de operação para recursos](move-support-resources.md).

* Analysis Services
* Gestão de API
* Aplicações de serviço de aplicações (aplicações web) – consulte [limitações do serviço de aplicações](#app-service-limitations)
* Certificados de Serviço de Aplicações
* Application Insights
* Automatização
* Azure Active Directory B2C
* Azure Cosmos DB
* Base de Dados do Azure para MySQL
* Base de Dados do Azure para PostgreSQL
* DevOps do Azure - as organizações de DevOps do Azure com a extensão de não-Microsoft compras obrigatória [cancelar as suas compras](https://go.microsoft.com/fwlink/?linkid=871160) antes de poder avançar a conta em várias subscrições.
* Azure Maps
* Reencaminhamento do Azure
* O Azure Stack - registos
* Batch
* Serviços BizTalk
* Serviço de Bot
* CDN
* Serviços cloud - veja [limitações da implementação clássica](#classic-deployment-limitations)
* Serviços Cognitivos
* Registo de Contentor
* Content Moderator
* Gestão de Custos
* Informações do Cliente
* Catálogo de Dados
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Event Grid
* Hubs de Eventos
* Porta de entrada
* Clusters do HDInsight - veja [limitações do HDInsight](#hdinsight-limitations)
* Centro de IoT
* Hubs IoT
* Cofre de Chaves
* Balanceadores de carga - veja [limitações de Balanceador de carga](#lb-limitations)
* Log Analytics
* Aplicações Lógicas
* Machine Learning - Machine Learning Studio, serviços web podem ser movidos para um grupo de recursos na mesma subscrição, mas não uma subscrição diferente. Outros recursos de aprendizagem automática podem ser movidos entre subscrições.
* Managed Disks – consulte [limitações de máquinas virtuais para restrições](#virtual-machines-limitations)
* Identidade gerida - atribuído ao utilizador
* Serviços de Multimédia
* Hubs de Notificação
* Informações Operacionais
* Gestão de Operações
* Dashboards de portais
* Power BI - tanto o Power BI Embedded e robustez com a coleção de área de trabalho de BI
* IP público - veja [limitações de IP público](#pip-limitations)
* Cache de redis - se a instância de Cache de Redis está configurada com uma rede virtual, a instância não pode ser movida para uma subscrição diferente. Ver [limitações de redes virtuais](#virtual-networks-limitations).
* Scheduler
* Pesquisa
* Service Bus
* Service Fabric
* Malha de recursos de infraestrutura do serviço
* Serviço SignalR
* Não é possível mover o armazenamento - contas de armazenamento em regiões diferentes na mesma operação. Em vez disso, a utilizar operações separadas para cada região.
* Armazenamento (clássica) – consulte [limitações da implementação clássica](#classic-deployment-limitations)
* Estado do Stream Analytics - Analytics Stream tarefas não podem ser movidas quando em execução.
* Servidor de base de dados SQL - base de dados e servidor têm de residir no mesmo grupo de recursos. Quando move um SQL server, todas as suas bases de dados também são movidas. Este comportamento aplica-se às bases de dados do Azure SQL Database e o Azure SQL Data Warehouse.
* Time Series Insights
* Gestor de Tráfego
* Máquinas virtuais - para VMs com discos geridos, consulte [limitações de máquinas virtuais](#virtual-machines-limitations)
* Máquinas virtuais (clássico) - consulte [limitações da implementação clássica](#classic-deployment-limitations)
* Veja conjuntos de dimensionamento de máquinas virtuais - [limitações de máquinas virtuais](#virtual-machines-limitations)
* Redes virtuais - veja [limitações de redes virtuais](#virtual-networks-limitations)
* Gateway de VPN

## <a name="services-that-cannot-be-moved"></a>Serviços que não não possível mover

A lista seguinte fornece um resumo geral dos serviços do Azure que não pode ser movido para um novo grupo de recursos e subscrição. Para mais detalhes, consulte [mover o suporte de operação para recursos](move-support-resources.md).

* Serviços de domínio do AD
* Serviço de estado de funcionamento do AD híbrido
* Gateway de Aplicação
* Migração de base de dados do Azure
* Azure Databricks
* Azure Migrate
* Batch AI
* Certificados - certificados de serviço de aplicações podem ser movidos, mas tem de certificados carregados [limitações](#app-service-limitations).
* Container Instances
* Serviço de Contentor
* Data Box
* Espaços de desenvolvimento
* Dynamics LCS
* ExpressRoute
* Serviço Kubernetes
* Serviços de laboratório - mudança para o novo grupo de recursos na mesma subscrição está ativada, mas a movimentação entre subscrições não está ativada.
* Balanceadores de carga - veja [limitações de Balanceador de carga](#lb-limitations)
* Aplicações Geridas
* Microsoft Genomics
* NetApp
* IP público - veja [limitações de IP público](#pip-limitations)
* Cofre de serviços de recuperação - também não mover os recursos de computação, rede e armazenamento associados ao Cofre de serviços de recuperação, consulte [limitações dos serviços de recuperação](#recovery-services-limitations).
* SAP HANA no Azure
* Segurança
* Site Recovery
* O StorSimple Device Manager
* Redes virtuais (clássico) - veja [limitações da implementação clássica](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Limitações de máquinas virtuais

Discos geridos são suportados para migração a partir de 24 de Setembro de 2018. 

1. Terá de se registar para ativar esta funcionalidade.

  ```azurepowershell-interactive
  Register-AzureRmProviderFeature -FeatureName ManagedResourcesMove -ProviderNamespace Microsoft.Compute
  ```

  ```azurecli-interactive
  az feature register --namespace Microsoft.Compute --name ManagedResourcesMove
  ```

1. O pedido de registo inicialmente devolve um Estado de `Registering`. Pode verificar o estado atual com:

  ```azurepowershell-interactive
  Get-AzureRmProviderFeature -FeatureName ManagedResourcesMove -ProviderNamespace Microsoft.Compute
  ```

  ```azurecli-interactive
  az feature show --namespace Microsoft.Compute --name ManagedResourcesMove
  ```

1. Aguarde alguns minutos até que o estado alterar para `Registered`.

1. Depois do recurso está registado, registe o `Microsoft.Compute` fornecedor de recursos. Efetue este passo, mesmo que o fornecedor de recursos foi registado anteriormente.

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
  ```

  ```azurecli-interactive
  az provider register --namespace Microsoft.Compute
  ```

Esse suporte significa que pode também mover:

* Máquinas virtuais com os discos geridos
* Imagens gerenciadas
* Instantâneos geridos
* Conjuntos de disponibilidade com máquinas virtuais com discos geridos

Aqui estão as restrições que ainda não são suportadas:

* Máquinas virtuais com o certificado armazenadas no Key Vault pode ser movidas para um novo grupo de recursos na mesma subscrição, mas não em várias subscrições.
* Máquinas de virtuais configuradas com o Azure Backup. Utilize a abaixo da solução para mover estas máquinas virtuais
  * Localize a localização da sua máquina Virtual.
  * Localize um grupo de recursos com o seguinte padrão de nomenclatura: `AzureBackupRG_<location of your VM>_1` AzureBackupRG_westus2_1 por exemplo,
  * Se no portal do Azure, em seguida, verificação "Mostrar tipos ocultos"
  * Se, no PowerShell, utilize o `Get-AzureRmResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet
  * Se, na CLI, utilize o `az resource list -g AzureBackupRG_<location of your VM>_1`
  * Agora, localize o recurso com o tipo `Microsoft.Compute/restorePointCollections` que tem o padrão de nomenclatura `AzureBackup_<name of your VM that you're trying to move>_###########`
  * Eliminar este recurso
  * Após a eliminação estiver concluída, poderá mover a Máquina Virtual
* Não é possível mover a conjuntos de dimensionamento de máquina virtual com o Balanceador de carga de SKU Standard ou IP público de SKU Standard
* Máquinas virtuais criadas a partir dos recursos de mercado com planos ligados não pode ser movidas entre grupos de recursos ou subscrições. Desaprovisionar a máquina virtual na subscrição atual e implemente novamente na subscrição nova.


## <a name="virtual-networks-limitations"></a>Limitações de redes virtuais

Ao mover uma rede virtual, também tem de mover os recursos dependentes. Para Gateways de VPN, tem de mover os endereços IP, gateways de rede virtual e todos os recursos de ligação associada. Gateways de rede local podem estar num grupo de recursos diferente.

Para mover uma rede virtual em modo de peering, tem primeiro de desativar o peering de rede virtual. Uma vez desativada, pode mover a rede virtual. Após a mudança, reativar o peering de rede virtual.

Não é possível mover uma rede virtual para uma subscrição diferente se a rede virtual contém uma sub-rede com ligações de navegação de recursos. Por exemplo, se um recurso de Cache de Redis é implementado numa sub-rede, o que a sub-rede tem uma ligação de navegação de recursos.

## <a name="app-service-limitations"></a>Limitações do serviço de aplicações

As limitações para mover os recursos de serviço de aplicações são diferentes com base no se estiver a mover os recursos numa subscrição ou para uma nova subscrição.

As limitações descritas nestas secções aplicam-se para certificados carregados, não certificados de serviço de aplicações. Pode mover os certificados de serviço de aplicações para um novo grupo de recursos ou subscrição, sem limitações. Se tiver várias aplicações web que utilizam o mesmo certificado de serviço de aplicações, primeiro mover todas as aplicações web, em seguida, mova o certificado.

### <a name="moving-within-the-same-subscription"></a>Mover dentro da mesma subscrição

Ao mover uma aplicação Web _dentro da mesma subscrição_, não é possível mover os certificados SSL carregados. No entanto, pode mover uma aplicação Web para o novo grupo de recursos sem mover o seu certificado SSL carregado e funcionalidade SSL de seu aplicativo ainda funciona.

Se pretender mover o certificado SSL com a aplicação Web, siga estes passos:

1.  Elimine o certificado carregado a partir da aplicação Web.
2.  Mova a aplicação Web.
3.  Carregue o certificado para a aplicação Web movido.

### <a name="moving-across-subscriptions"></a>Mover entre subscrições

Ao mover uma aplicação Web _entre subscrições_, as seguintes limitações aplicam-se:

- O grupo de recursos de destino não deve ter quaisquer recursos existentes do serviço de aplicações. Os recursos de serviço de aplicações incluem:
    - Aplicações Web
    - Planos do Serviço de Aplicações
    - Certificados SSL carregados ou importados
    - Ambientes do App Service
- Todos os recursos de serviço de aplicações no grupo de recursos tem de ser movidos em conjunto.
- Recursos de serviço de aplicações só podem ser movidos do grupo de recursos em que foram originalmente criados. Se um recurso de serviço de aplicações não se encontra no respetivo grupo de recursos original, ele tem ser movido para o grupo de recursos original primeiro e, em seguida, podem ser movida entre subscrições.

## <a name="classic-deployment-limitations"></a>Limitações da implementação clássica

As opções para mover recursos implementados por meio do modelo clássico diferem com base no se estiver a mover os recursos numa subscrição ou para uma nova subscrição.

### <a name="same-subscription"></a>Mesma subscrição

Ao mover recursos do grupo de recursos para outro grupo de recursos dentro da mesma subscrição, aplicam-se as seguintes restrições:

* Não não possível mover a redes virtuais (clássicas).
* Máquinas virtuais (clássicas) têm de ser movidas com o serviço cloud.
* Só pode ser movido serviço em nuvem quando a mudança inclui todas as suas máquinas virtuais.
* Apenas um serviço cloud pode ser movido por vez.
* Apenas uma conta de armazenamento (clássica) pode ser movida de cada vez.
* Não é possível mover a conta de armazenamento (clássica) na mesma operação com uma máquina virtual ou um serviço em nuvem.

Para mover recursos clássicos para um novo grupo de recursos dentro da mesma subscrição, utilize as operações de movimentação padrão através da [portal](#use-portal), [Azure PowerShell](#use-powershell), [da CLI do Azure](#use-azure-cli), ou [REST API](#use-rest-api). Utilize as mesmas operações que utiliza para mover os recursos do Resource Manager.

### <a name="new-subscription"></a>Nova subscrição

Quando mover recursos para uma nova subscrição, aplicam-se as seguintes restrições:

* Todos os recursos clássicos na subscrição têm de ser movidos na mesma operação.
* A subscrição de destino não pode conter quaisquer outros recursos clássicos.
* A mudança só pode ser pedida através da API REST separado para move clássico. Os comandos de movimentação do Gestor de recursos padrão não funcionam quando mover recursos clássicos para uma nova subscrição.

Para mover recursos clássicos para uma nova subscrição, utilize as operações REST que são específicas para recursos clássicos. Para utilizar o REST, execute os seguintes passos:

1. Verifique se a subscrição de origem pode participar de uma movimentação entre subscrições. Utilize a seguinte operação:

  ```HTTP
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     No corpo do pedido, incluem:

  ```json
  {
    "role": "source"
  }
  ```

     A resposta para a operação de validação é o seguinte formato:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Verifique se a subscrição de destino pode participar de uma movimentação entre subscrições. Utilize a seguinte operação:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     No corpo do pedido, incluem:

  ```json
  {
    "role": "target"
  }
  ```

     A resposta está no mesmo formato que a validação de subscrição de origem.
3. Se ambas as subscrições passam na validação, mova todos os recursos clássicos de uma subscrição para outra subscrição com a seguinte operação:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    No corpo do pedido, incluem:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

A operação pode ser executada durante vários minutos.

## <a name="recovery-services-limitations"></a>Limitações de serviços de recuperação

Movimentação não está ativada para os recursos de armazenamento, rede ou computação utilizados para configurar a recuperação após desastre com o Azure Site Recovery.

Por exemplo, suponha que configurou a replicação de máquinas no local para uma conta de armazenamento (Storage1) e deseja que a máquina protegida para surgir após a ativação pós-falha para o Azure como uma máquina virtual (VM1) ligada a uma rede virtual (Network1). Não é possível mover qualquer um desses recursos do Azure - Storage1 VM1 e Network1 - entre grupos de recursos dentro da mesma subscrição ou em várias subscrições.

Para mover uma VM inscrita no **cópia de segurança do Azure** entre grupos de recursos:
 1. Temporariamente parar cópia de segurança e manter os dados de cópia de segurança
 2. Mova a VM para o grupo de recursos de destino
 3. Volte de a protegê-lo sob o mesmo/novo cofre, que os utilizadores podem restaurar a partir dos pontos de restauro disponíveis criados antes da operação de movimentação.
Se o utilizador muda a VM de cópia de segurança em várias subscrições, passo 1 e o passo 2 continuam as mesmas. Passo 3, o utilizador tem de proteger a VM num novo cofre presente / criado na subscrição de destino. Cofre dos Recovery Services não suporta cópias de segurança entre subscrições.

## <a name="hdinsight-limitations"></a>Limitações do HDInsight

Pode mover clusters do HDInsight para uma nova subscrição ou grupo de recursos. No entanto, não é possível mover entre subscrições, os recursos de rede ligados ao cluster do HDInsight (por exemplo, a rede virtual, uma NIC ou um balanceador de carga). Além disso, não é possível mover a um novo grupo de recursos um NIC que está ligado a uma máquina virtual para o cluster.

Ao migrar um cluster do HDInsight para uma nova subscrição, primeiro mova outros recursos (como a conta de armazenamento). Em seguida, mova o cluster do HDInsight por si só.

## <a name="search-limitations"></a>Limitações de pesquisa

Não é possível mover vários recursos de pesquisa colocados em regiões diferentes ao mesmo tempo.
Nesse caso, terá de movê-los separadamente.

## <a name="lb-limitations"></a> Limitações de Balanceador de carga

Podem ser movido Balanceador de carga de SKU básico.
Não é possível mover o Balanceador de carga de SKU Standard.

## <a name="pip-limitations"></a> Limitações de IP públicas

Podem ser movido IP público de SKU básico.
Não é possível mover o IP público de SKU Standard.

## <a name="use-portal"></a>Utilizar o portal

Para mover os recursos, selecione o grupo de recursos que contém os recursos e, em seguida, selecione o **mover** botão.

![mover recursos](./media/resource-group-move-resources/select-move.png)

Selecione se estiver a mover os recursos para um novo grupo de recursos ou uma nova subscrição.

Selecione os recursos necessários para mover e o grupo de recursos de destino. Confirmar que tem de atualizar os scripts para estes recursos e selecione **OK**. Se o ícone de edição de subscrição que selecionou no passo anterior, também tem de selecionar a subscrição de destino.

![Selecionar destino](./media/resource-group-move-resources/select-destination.png)

Na **notificações**, verá que a operação de movimentação está em execução.

![Mostrar o estado de movimentação](./media/resource-group-move-resources/show-status.png)

Quando tiver concluído, foi notificado do resultado.

![Mostrar o resultado de movimentação](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Utilizar o PowerShell

Para mover os recursos existentes para outro grupo de recursos ou subscrição, utilize o [Move-AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) comando. O exemplo seguinte mostra como mover vários recursos para um novo grupo de recursos.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Para mover para uma nova subscrição, incluem um valor para o `DestinationSubscriptionId` parâmetro.

## <a name="use-azure-cli"></a>Utilizar a CLI do Azure

Para mover os recursos existentes para outro grupo de recursos ou subscrição, utilize o [movimentação do recurso de az](/cli/azure/resource?view=azure-cli-latest#az-resource-move) comando. Forneça os IDs dos recursos para mover dos recursos. O exemplo seguinte mostra como mover vários recursos para um novo grupo de recursos. Na `--ids` parâmetro, fornecer uma lista separada por espaço do recurso IDs para mover.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Para mover para uma nova subscrição, forneça o `--destination-subscription-id` parâmetro.

## <a name="use-rest-api"></a>Utilizar a API REST

Para mover os recursos existentes para outro grupo de recursos ou subscrição, execute:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

No corpo do pedido, especifique o grupo de recursos de destino e os recursos para mover. Para obter mais informações sobre a operação de REST de movimentação, consulte [mover recursos](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os cmdlets do PowerShell para gerir a sua subscrição, veja [utilizar o Azure PowerShell com o Resource Manager](powershell-azure-resource-manager.md).
* Para saber mais sobre os comandos da CLI do Azure para gerir a sua subscrição, veja [com a CLI do Azure com o Resource Manager](xplat-cli-azure-resource-manager.md).
* Para saber mais sobre recursos de portal de gestão da subscrição, veja [utilizar o portal do Azure para gerir recursos](resource-group-portal.md).
* Para saber mais sobre como aplicar uma organização lógica aos seus recursos, veja [utilizar etiquetas para organizar os recursos](resource-group-using-tags.md).
