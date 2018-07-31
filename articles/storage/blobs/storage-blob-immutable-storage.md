---
title: Armazenamento imutável para armazenamento de Blobs do Azure (pré-visualização) | Documentos da Microsoft
description: O armazenamento do Azure oferece suporte WORM (escrita uma vez, muitos de leitura) para armazenamento de Blob (objeto) que permite aos utilizadores armazenar dados num Estado não apagável e não modificável para um intervalo especificado.
services: storage
author: sangsinh
ms.service: storage
ms.topic: article
ms.date: 05/29/2018
ms.author: sangsinh
ms.component: blobs
ms.openlocfilehash: 723496c2f2bd29d60a19232616bda756a7c94ed3
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39345807"
---
# <a name="store-business-critical-data-in-azure-blob-storage-preview"></a>Store dados críticos da empresa no armazenamento de Blobs do Azure (pré-visualização)

Armazenamento imutável para armazenamento de Blobs do Azure (objeto) permite aos utilizadores armazenar dados críticos da empresa num Estado WORM (escrita uma vez, muitos de leitura). Este estado não apagável e não modificável os dados faz para um intervalo especificado pelo utilizador. BLOBs podem ser criadas e ler, mas não modificados ou eliminados, durante o intervalo de retenção.

## <a name="overview"></a>Descrição geral

Armazenamento imutável ajuda as instituições financeiras e setores relacionados, particularmente, organizações de Mediador dealer – para armazenar dados de forma segura.

As aplicações típicas incluem:

- **Conformidade a normas**: armazenamento imutável para armazenamento de Blobs do Azure ajuda as organizações endereço seg 17a-4(f), 1.31(d) CFTC, FINRA e outros regulamentos.

- **Proteger a retenção de documentos**: armazenamento de BLOBs garante que os dados não podem ser modificados ou eliminados por nenhum utilizador, incluindo utilizadores com privilégios administrativos da conta.

- **Suspensão legal**: armazenamento imutável para armazenamento de Blobs do Azure permite aos utilizadores armazenar informações confidenciais que é essenciais para litígio ou uma investigação criminal num Estado de prova de adulteração durante o período pretendido.

Armazenamento imutável permite:

- **Suporte de política de retenção com base no tempo**: os utilizadores definir políticas para armazenar dados de um intervalo especificado.

- **Suporte de política de retenção legal**: quando o intervalo de retenção não é conhecido, os usuários podem configurar retenções jurídicas para armazenar dados immutably até que a suspensão legal está desmarcada.  Quando uma retenção legal está definida, os blobs podem ser criados e lidos, mas não podem ser modificados nem eliminados. Cada suspensão legal está associado uma marca de alfanumérica definidas pelo utilizador que é utilizada como uma cadeia de caracteres de identificador (por exemplo, um ID de incidente).

- **O suporte para todas as camadas de BLOBs**: políticas WORM são independentes da camada de armazenamento de Blobs do Azure e aplicam-se a todas as camadas: frequente, esporádico e de arquivo. Os utilizadores podem armazenar os dados na camada mais com otimização de custos para as cargas de trabalho, mantendo a imutabilidade de dados.

- **Configuração de nível de contêiner**: os utilizadores podem configurar políticas de retenção com base no tempo e retenção legal etiquetas ao nível do contentor. Ao utilizar as definições de nível de contêiner simples, os utilizadores podem criar e políticas de retenção com base no tempo de bloqueio expandir a intervalos de retenção definir e desmarque retenções jurídicas; e muito mais. Estas políticas se aplicam a todos os blobs no contentor, novo e existente.

- **Suporte de registo de auditoria**: cada contêiner inclui um registo de auditoria. Ele apresenta os comandos de retenção com base no tempo até cinco para políticas de retenção com base no tempo bloqueado, com um máximo de três logs para extensões de intervalo de retenção. Para uma retenção com base no tempo, o registo contém o ID de utilizador, tipo de comando, carimbos de data / hora e intervalo de retenção. Para retenções jurídicas, o registo contém o ID de utilizador, tipo de comando, carimbos de data / hora e as etiquetas de retenção legal. Este registo é retido durante o tempo de vida do contentor, de acordo com as diretrizes de normas de 17a-4(f) seg. O [registo de atividades do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) mostra um registo mais abrangente de todos os o controle de atividades de plano. É responsabilidade do usuário para armazenar os registos de forma permanente, como poderá ser necessário por motivos de regulamentação ou outros.

Armazenamento imutável está ativado em todas as regiões públicas do Azure.

## <a name="how-it-works"></a>Como funciona

Armazenamento imutável para armazenamento de Blobs do Azure suporta dois tipos de políticas de imutáveis ou WORM: retenção com base no tempo e retenções jurídicas. Para obter detalhes sobre como criar estas políticas imutáveis, consulte a [introdução ao](#Getting-started) secção.

Quando uma política de retenção com base no tempo ou a suspensão legal é aplicada num contêiner, todos os blobs existentes mover para o imutável (escrita e eliminação protegidos) Estado. Todos os novos blobs, que são carregados para o contentor também irão mudar para o estado imutável.

> [!IMPORTANT]
> Tem de ser uma política de retenção com base no tempo *bloqueado* para o blob estar num imutável (escrita e eliminação protegidos) Estado para seg 17a-4(f) e outra a conformidade a normas. Recomendamos que bloqueia a política numa quantidade razoável de tempo, normalmente dentro de 24 horas. Não recomendamos utilizar o *desbloqueado* estado para qualquer finalidade sem ser de curto prazo versões de avaliação do recurso.

Quando é aplicada uma política de retenção com base no tempo num contentor, todos os blobs no contentor permanecerá no Estado imutável durante a *eficaz* período de retenção. O período de retenção efetivo para os blobs existentes é igual à diferença entre a hora de criação dos blobs e o intervalo de retenção especificado pelo utilizador. 

Para novos blobs, o período de retenção efetivo é igual ao intervalo de retenção especificado pelo utilizador. Uma vez que os utilizadores podem alterar o intervalo de retenção, o armazenamento imutável utiliza o valor mais recente do intervalo de retenção especificado pelo utilizador para calcular o período de retenção em vigor.

> [!TIP]
> Exemplo:
> 
> Um utilizador cria uma política de retenção com base no tempo com um intervalo de retenção de cinco anos.
>
> Os BLOBs existentes nesse contentor, testblob1, foi criado um ano atrás. O período de retenção em vigor para testblob1 é quatro anos.
>
> Um novo blob, testblob2, é agora carregado para o contentor. O período de retenção em vigor para este blob novo é cinco anos.

### <a name="legal-holds"></a>Retenções legais

Ao definir uma retenção legal, todos os blobs novos e existentes permanecem no Estado imutável até que a suspensão legal está desmarcada. Para obter mais informações sobre como conjunto e retenções jurídicas clear, consulte a [introdução ao](#Getting-started) secção.

Um contentor pode ter uma retenção legal e uma política de retenção com base no tempo, ao mesmo tempo. Todos os blobs existentes nesse contentor permanecem no Estado imutável até que todos os retenções jurídicas forem verificadas, mesmo que o seu período de retenção em vigor expirou. Por outro lado, um blob permanece num Estado imutável até expira o período de retenção em vigor, apesar de tem sido limpo retenções jurídicas todos os.

A tabela seguinte mostra os tipos de operações de BLOBs que estão desativadas para os diferentes cenários de imutáveis. Para obter mais informações, consulte a [API de serviço de Blobs do Azure](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) documentação.

|Cenário  |Estado de blob  |Operações de BLOBs não permitidas  |
|---------|---------|---------|
|O intervalo de retenção efetivo do blob ainda não expirou e/ou a retenção legal está definida     |Imutável: protegido contra eliminação e escrita         |Eliminar Contentor, Eliminar Blob, Colocar Blob1, Colocar Bloco, Colocar Lista de Blocos, Definir Metadados de Blob, Colocar Página, Definir as Propriedades do Blob, Tirar um Instantâneo do Blob, Blob de Cópia Incremental, Anexar Bloco         |
|O intervalo de retenção efetivo no blob expirou     |Protegido apenas contra escrita (as operações de eliminação são permitidas)         |Colocar Blob, Colocar Bloco, Colocar Lista de Blocos, Definir Metadados de Blob, Colocar Página, Definir as Propriedades do Blob, Tirar um Instantâneo do Blob, Blob de Cópia Incremental, Anexar Bloco         |
|Todos os legais mantém desmarcada e nenhuma política de retenção com base no tempo é definida no contentor     |Mutável         |Nenhuma         |
|Nenhuma política WORM é criada (com base no tempo de retenção ou suspensão legal)     |Mutável         |Nenhuma         |

> [!NOTE]
> O Blob de colocar primeiro e as operações de colocar a lista de bloqueios e colocação de blocos que são necessárias para criar um blob, são permitidas nos dois primeiros cenários da tabela acima. Todas as operações subsequentes não são permitidas.
>
> Armazenamento imutável está disponível apenas nas contas de armazenamento de BLOBs e GPv2. Tem de ser criada por meio [do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

## <a name="pricing"></a>Preços

Não é sem custos adicionais para utilizar esta funcionalidade. Dados imutáveis é o preço da mesma forma como os dados regulares, e mutáveis. Para detalhes de preços, consulte a [armazenamento do Azure, página de preços](https://azure.microsoft.com/pricing/details/storage/blobs/).

### <a name="restrictions"></a>Restrições

As seguintes restrições aplicam-se durante a pré-visualização pública:

- *Não armazene dados críticos ou de produção.*
- Aplicam todas as pré-visualização e restrições de contrato de confidencialidade.

## <a name="getting-started"></a>Introdução

As versões mais recentes do [portal do Azure](http://portal.azure.com), [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), e [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May2018) suporta armazenamento imutável para armazenamento de Blobs do Azure.

### <a name="azure-portal"></a>Portal do Azure

1. Crie um novo contentor ou selecione um contentor existente para armazenar os blobs que precisam de ser mantidos no estado imutável.
 O contentor tem de ser uma conta de armazenamento GPv2.
2. Selecione **política de acesso** nas definições de contentor. Em seguida, selecione **+ Adicionar política** sob **armazenamento de BLOBs imutável**.

    ![Definições de contentor no portal](media/storage-blob-immutable-storage/portal-image-1.png)

3. Para ativar a retenção com base no tempo, selecione **retenção com base no tempo** no menu pendente.

    !["Retenção de com base no tempo" selecionada em "Tipo de política"](media/storage-blob-immutable-storage/portal-image-2.png)

4. Introduza o intervalo de retenção em dias (mínimo é de um dia).

    ![Caixa de "Período de retenção de atualização para"](media/storage-blob-immutable-storage/portal-image-5-retention-interval.png)

    Como pode ver na captura de ecrã, o estado inicial da política está desbloqueado. Pode testar a funcionalidade com um intervalo de retenção mais pequeno e fazer alterações à política antes de bloqueá-lo. Bloqueio é essencial para conformidade com as normas, como SEC 17a-4.

5. A política de bloqueio. Faça duplo clique no botão de reticências (**...** ), e é apresentado o menu seguinte:

    !["Política de bloqueio" no menu](media/storage-blob-immutable-storage/portal-image-4-lock-policy.png)

    Selecione **política de bloqueio**, e o estado de política é apresentado como bloqueado. Depois da política está bloqueada, não é possível eliminá-lo e apenas extensões de intervalo de retenção serão permitidas.

6. Para ativar retenções jurídicas, selecione **+ Adicionar política**. Selecione **suspensão Legal** no menu pendente.

    !["De retenção legal" no menu em "Tipo de política"](media/storage-blob-immutable-storage/portal-image-legal-hold-selection-7.png)

7. Crie uma retenção legal com uma ou mais etiquetas.

    ![Caixa de "Nome de etiqueta" sob o tipo de política](media/storage-blob-immutable-storage/portal-image-set-legal-hold-tags.png)

### <a name="azure-cli-20"></a>CLI 2.0 do Azure

Instalar o [extensão de CLI do Azure](http://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) utilizando `az extension add -n storage-preview`.

Se já tiver a extensão instalada, utilize o seguinte comando para ativar o armazenamento de imutável: `az extension update -n storage-preview`.

A funcionalidade está incluída nos seguintes grupos de comandos: `az storage container immutability-policy` e `az storage container legal-hold`. Executar `-h` nos mesmos para ver os comandos.

### <a name="powershell"></a>PowerShell

[PowerShell versão 4.4.0-preview](https://github.com/Azure/azure-powershell/releases/tag/Azure.Storage.v4.4.0-preview-May20180) suporta armazenamento imutável.
Para ativar a funcionalidade, siga estes passos:

1. Certifique-se de que tem a versão mais recente do PowerShellGet instalado: `Install-Module PowerShellGet –Repository PSGallery –Force`.
2. Remova qualquer instalação anterior do Azure PowerShell.
3. Instalação AzureRM: `Install-Module AzureRM –Repository PSGallery –AllowClobber`. Azure pode ser instalado da mesma forma deste repositório.
4. Instale a versão de pré-visualização dos cmdlets do plano de gestão de armazenamento: `Install-Module -Name AzureRM.Storage -AllowPrerelease -Repository PSGallery -AllowClobber`.

O [código do PowerShell de exemplo](#sample-powershell-code) seção mais adiante neste artigo ilustra a utilização da funcionalidade.

## <a name="client-libraries"></a>Bibliotecas de cliente

As seguintes bibliotecas de cliente suportam armazenamento imutável para armazenamento de Blobs do Azure:

- [Biblioteca de cliente .NET versão 7.2.0-preview e posterior](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage/7.2.0-preview)
- [Biblioteca de cliente do node. js versão 4.0.0 e posterior](https://www.npmjs.com/package/azure-arm-storage)
- [Biblioteca de cliente Python versão 2.0.0 Release Candidate 2 e posterior](https://pypi.org/project/azure-mgmt-storage/2.0.0rc1/)

## <a name="supported-values"></a>Valores suportados

- O intervalo de retenção mínimo é um dia. O máximo são 400 anos.
- Para uma conta de armazenamento, o número máximo de contentores com políticas de imutáveis bloqueadas é 1000.
- Para uma conta de armazenamento, o número máximo de contentores com uma definição de suspensão legal é 1000.
- Para um contentor, o número máximo de etiquetas de suspensão legal é 10.
- O comprimento máximo de uma etiqueta de suspensão legal é 23 carateres alfanuméricos. O comprimento mínimo é de três carateres.
- Para um contentor, o número máximo de extensões de intervalo de retenção permitido para políticas de imutáveis bloqueadas é três.
- Para um contentor com uma política de imutável bloqueado, um máximo de cinco registos da política de retenção com base no tempo e um máximo de 10 legais armazenar os registos são mantidos durante a duração do contentor de política.

## <a name="faq"></a>FAQ

**A funcionalidade aplicam-se para apenas blobs de blocos, ou a página e de acréscimo também?**

Armazenamento imutável pode ser utilizado com qualquer tipo de blob.  Contudo, recomendamos que a usá-lo principalmente para blobs de blocos. Ao contrário dos blobs de blocos, página blobs de acréscimo a necessidade de blobs para ser criado fora de um contentor WORM e, em seguida, copiados em. Depois de copiar nestes blobs num contentor WORM, não mais *acrescenta* para um acréscimo são permitidas alterações para um blob de página ou blob.

**É necessário criar sempre uma nova conta de armazenamento para utilizar esta funcionalidade?**

Pode utilizar o armazenamento imutável com as contas GPv2 existentes ou em novas contas de armazenamento se o tipo de conta GPv2. Esta funcionalidade está disponível apenas com armazenamento de Blobs.

**O que acontece se tentar eliminar um contentor com uma política de retenção baseada no tempo ou de retenção legal *bloqueada*?**

A operação eliminar contentor irá falhar se for, pelo menos, um blob com uma política de retenção com base no tempo bloqueado ou uma retenção legal. Isso é verdadeiro, mesmo se os dados forem [eliminado de forma recuperável](storage-blob-soft-delete.md). A operação Eliminar Contentor terá êxito se não houver nenhum blob com um intervalo de retenção ativo e nenhumas retenções legais. Tem de eliminar os blobs antes de poder eliminar o contentor. 

**O que acontece se tentar eliminar uma conta de armazenamento com um contentor WORM que tenha uma política de retenção baseada no tempo ou de retenção legal *bloqueada*?**

A eliminação da conta de armazenamento falhará se existir, pelo menos, um contentor WORM com uma retenção legal ou um blob com um intervalo de retenção ativo.  Tem de eliminar todos os contentores WORM antes de poder eliminar a conta de armazenamento. Para obter informações sobre a eliminação do contentor, consulte a pergunta anterior.

**Posso mover os dados entre camadas de blob diferentes (frequente, esporádico, progressivo) quando o blob está no estado imutável?**

Sim, pode utilizar o comando Definir Camada de Blob para mover dados entre as camadas de blob, mantendo os dados no estado imutável. Armazenamento imutável é suportado nos escalões de BLOBs de acesso frequente, esporádico e esporádica.

**O que acontece se eu não conseguir pagar e o meu intervalo de retenção não tiver expirado?**

No caso de não-pagamento, políticas de retenção de dados normais aplica como estipulado nos termos e condições do seu contrato com a Microsoft.

**Oferecem um período de avaliação gratuita ou de tolerância para experimentar a funcionalidade?**

Sim. Quando uma política de retenção com base no tempo é criada, ele está numa *desbloqueado* estado. Neste estado, pode fazer qualquer alteração desejada para o intervalo de retenção, por exemplo, aumento ou diminuição e até mesmo eliminar a política. Depois de bloqueada a política, ela fica bloqueada para sempre, impedir a eliminação. Além disso, o intervalo de retenção já não pode ser diminuído quando a política está bloqueada. Recomendamos vivamente que utilize o *desbloqueado* apenas para fins de avaliação de estado e a política de bloqueio num período de 24 horas. Essas práticas ajudarão a estar em conformidade com seg 17a-4(f) e outros regulamentos.

**A funcionalidade está disponível em clouds nacionais e do governo?**

Armazenamento imutável está atualmente disponível apenas nas regiões públicas do Azure. Se estiver interessado numa cloud nacional específica, envie um e-mail azurestoragefeedback@microsoft.com.

## <a name="sample-powershell-code"></a>Código do PowerShell de exemplo

É o seguinte script do PowerShell de exemplo para referência. Este script cria uma nova conta de armazenamento e um contentor. Em seguida, mostra como definir e limpar retenções jurídicas, criar e bloquear uma política de retenção com base no tempo (também conhecido como uma política de imutabilidade) e expandir o intervalo de retenção.

```powershell
$ResourceGroup = "<Enter your resource group>”
$StorageAccount = "<Enter your storage account name>"
$container = "<Enter your container name>"
$container2 = "<Enter another container name>”
$location = "<Enter the storage account location>"

# Log in to the Azure Resource Manager account
Login-AzureRMAccount
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Storage"

# Create your Azure resource group
New-AzureRmResourceGroup -Name $ResourceGroup -Location $location

# Create your Azure storage account
New-AzureRmStorageAccount -ResourceGroupName $ResourceGroup -StorageAccountName `
    $StorageAccount -SkuName Standard_LRS -Location $location -Kind Storage

# Create a new container
New-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Create Container 2 with a storage account object
$accountObject = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount
New-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Get a container
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container

# Get a container with an account object
$containerObject = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container

# List containers
Get-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount

# Remove a container (add -Force to dismiss the prompt)
Remove-AzureRmStorageContainer -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container2

# Remove a container with an account object
Remove-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2

# Remove a container with a container object
$containerObject2 = Get-AzureRmStorageContainer -StorageAccount $accountObject -Name $container2
Remove-AzureRmStorageContainer -InputObject $containerObject2

# Set a legal hold
Add-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag tag1,tag2

# Set a legal hold with an account object
Add-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag tag3

# Set a legal hold with a container object
Add-AzureRmStorageContainerLegalHold -Container $containerObject -Tag tag4,tag5

# Clear a legal hold
Remove-AzureRmStorageContainerLegalHold -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -Name $container -Tag tag2

# Clear a legal hold with an account object
Remove-AzureRmStorageContainerLegalHold -StorageAccount $accountObject -Name $container -Tag tag3,tag5

# Clear a legal hold with a container object
Remove-AzureRmStorageContainerLegalHold -Container $containerObject -Tag tag4

# Create or update an immutability policy
## with an account name or container name

Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container -ImmutabilityPeriod 10

## with an account object
Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -ImmutabilityPeriod 1 -Etag $policy.Etag

## with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 7

## with an immutability policy object
Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -ImmutabilityPeriod 5

# Get an immutability policy
Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName $ResourceGroup `
    -StorageAccountName $StorageAccount -ContainerName $container

# Get an immutability policy with an account object
Get-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container

# Get an immutability policy with a container object
Get-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject

# Lock an immutability policy (add -Force to dismiss the prompt)
## with an immutability policy object

$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy -force

## with an account name or container name
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

## with an account object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -Etag $policy.Etag

## with a container object
$policy = Lock-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -Etag $policy.Etag -force

# Extend an immutability policy
## with an immutability policy object

$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container

$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy `
    $policy -ImmutabilityPeriod 11 -ExtendPolicy

## with an account name or container name
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -ImmutabilityPeriod 11 -Etag $policy.Etag -ExtendPolicy

## with an account object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -StorageAccount `
    $accountObject -ContainerName $container -ImmutabilityPeriod 12 -Etag `
    $policy.Etag -ExtendPolicy

## with a container object
$policy = Set-AzureRmStorageContainerImmutabilityPolicy -Container `
    $containerObject -ImmutabilityPeriod 13 -Etag $policy.Etag -ExtendPolicy

# Remove an immutability policy (add -Force to dismiss the prompt)
## with an immutability policy object
$policy = Get-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container
Remove-AzureRmStorageContainerImmutabilityPolicy -ImmutabilityPolicy $policy

## with an account name or container name
Remove-AzureRmStorageContainerImmutabilityPolicy -ResourceGroupName `
    $ResourceGroup -StorageAccountName $StorageAccount -ContainerName $container `
    -Etag $policy.Etag

## with an account object
Remove-AzureRmStorageContainerImmutabilityPolicy -StorageAccount $accountObject `
    -ContainerName $container -Etag $policy.Etag

## with a container object
Remove-AzureRmStorageContainerImmutabilityPolicy -Container $containerObject `
    -Etag $policy.Etag
    
```
