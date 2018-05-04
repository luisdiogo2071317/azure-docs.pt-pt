---
title: Funcionalidades exclusivas de blobs de página do Azure | Microsoft Docs
description: Uma descrição geral de blobs de páginas do Azure e os respetivos vantagens, incluindo casos de utilização com scripts de exemplo.
services: storage
author: anasouma
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 04/30/2018
ms.author: wielriac
ms.openlocfilehash: 79590e1987ee29ca06f9fb103f548518b2c1c57e
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="unique-features-of-azure-page-blobs"></a>Funcionalidades exclusivas de blobs de página do Azure

Storage do Azure oferece três tipos de armazenamento de BLOBs: Blobs de blocos, os Blobs de acréscimo e blobs de páginas. Os blobs de blocos são compostos de blocos e são ideais para armazenar o texto ou ficheiros binários e para carregar ficheiros grandes de forma eficiente. Acrescentar blobs são também constituídos por blocos, mas são otimizados para operações de acréscimo, torná-los ideais para cenários de registo. Os blobs de páginas são constituídos por páginas de 512 bytes cópias de segurança para 8 TB de tamanho total e estão concebidos para operações de leitura/escrita aleatórias frequentes. Os blobs de páginas são a base de discos de IaaS do Azure. Este artigo incida no explicar as funcionalidades e vantagens de blobs de páginas.

Os blobs de páginas são uma coleção de páginas de 512 bytes, que fornecem a capacidade de leitura/escrita arbitrários intervalos de bytes. Por conseguinte, os blobs de páginas são ideais para armazenar as estruturas de dados baseada no índice e dispersos como SO e discos de dados para máquinas virtuais e bases de dados. Por exemplo, BD SQL do Azure utiliza os blobs de páginas como o armazenamento persistente subjacente para as suas bases de dados. Além disso, os blobs de páginas também, muitas vezes, são utilizados para os ficheiros com atualizações com base no intervalo.  

As principais funcionalidades de blobs de páginas do Azure são a interface REST, as características de durabilidade do armazenamento subjacente e as capacidades de migração totalmente integrada para o Azure. Estas funcionalidades são abordadas mais detalhadamente na secção seguinte. Além disso, os blobs de páginas do Azure atualmente são suportados em dois tipos de armazenamento: padrão de armazenamento e de armazenamento Premium. Armazenamento Premium foi concebido especificamente para cargas de trabalho que necessitam de elevado desempenho consistente e a latência baixa efetuar blobs de páginas de premium ideal para bases de dados de armazenamento de dados de performant elevada.  Armazenamento Standard é mais económico para executar cargas de trabalho insensível à latência.

## <a name="sample-use-cases"></a>Casos de utilização de exemplo

Vamos discutir sobre alguns casos de utilização de blobs de páginas iniciar com discos de IaaS do Azure. Os blobs de páginas do Azure são a estrutura principal da plataforma de discos virtuais de IaaS do Azure. Azure SO e discos de dados são implementados como discos virtuais, onde os dados são forma durável persistente na plataforma de armazenamento do Azure e, em seguida, entregar às máquinas virtuais para um desempenho máximo. Discos do Azure são mantidos no Hyper-V [formato VHD](https://technet.microsoft.com/library/dd979539.aspx) e armazenada como um [BLOBs de páginas](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) no armazenamento do Azure. Além de utilizar discos virtuais para VMs IaaS do Azure, os blobs de páginas também ativar cenários de PaaS e DBaaS, tais como o serviço de BD SQL do Azure, que utiliza atualmente blobs de páginas para armazenar dados SQL, ativar as operações de leitura e escrita aleatórias rápida para a base de dados. Outro exemplo seria se tiver um serviço de PaaS para acesso de suporte de dados partilhado para aplicações de edição de vídeos colaboração, blobs de páginas ativar um acesso rápido às localizações aleatórios no suporte de dados. Também permite rápidos e eficientes editar e intercalação de suporte de dados do mesmo por vários utilizadores. 

Primeiro Microsoft Serviços de terceiros, como o Azure Site Recovery, cópia de segurança do Azure, bem como muitas terceiros aos programadores implementar inovações líder da indústria, utilizando a interface REST do blob de páginas. Seguem-se alguns dos cenários exclusivos implementados no Azure: 
* Gestão de aplicações direcionadas para instantâneo incremental: as aplicações podem tirar partido instantâneos do blob de página e REST APIs para guardar os pontos de verificação da aplicação sem incorrer em dispendiosa duplicação de dados. Storage do Azure suporta instantâneos locais para blobs de páginas, que não necessitam de copiar o blob de todo. Estes instantâneos pública APIs também ativar o acesso e copiar das diferenças entre os instantâneos.
* Migração em direto de aplicações e dados do local para a nuvem: copiar os dados no local e utilizar REST APIs para escrever diretamente para um blob de página do Azure, enquanto a VM no local continua a ser executado. Depois do destino detetou cópias de segurança, pode rapidamente ativação pós-falha para a VM do Azure com os dados. Desta forma, pode migrar as suas VMs e discos virtuais do local para a nuvem com o período de indisponibilidade mínimo, uma vez que a migração de dados ocorre em segundo plano enquanto continua a utilizar a VM e o período de indisponibilidade necessário para a ativação pós-falha será curtos (em minutos).
* [Baseado em SAS](../common/storage-dotnet-shared-access-signature-part-1.md) acesso, o que permite cenários, como leitores de vários e escritor único com suporte para o controlo de simultaneidade partilhado.

## <a name="page-blob-features"></a>Funcionalidades de blob de página

### <a name="rest-api"></a>API REST
Consulte o documento seguinte para obter uma introdução [desenvolvimento com os blobs de páginas](storage-dotnet-how-to-use-blobs.md). Por exemplo, observe como aceder a blobs de página utilizando a biblioteca de clientes de armazenamento para .NET. 

O diagrama a seguir descreve as relações entre a conta, contentores e blobs de página gerais.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-specified-size"></a>Criar um blob de página vazia de um tamanho especificado
Para criar um blob de página, vamos primeiro de criar um **CloudBlobClient** objeto, com o URI base para aceder ao armazenamento de BLOBs para a sua conta de armazenamento (*pbaccount* na figura 1) juntamente com o  **StorageCredentialsAccountAndKey** objeto, conforme mostrado no exemplo seguinte. O exemplo mostra, em seguida, criar uma referência a um **CloudBlobContainer** de objeto e, em seguida, criar o contentor (*testvhds*) se já não existir. Em seguida, utilizar o **CloudBlobContainer** objeto, criar uma referência a um **CloudPageBlob** objeto especificando o nome do blob de página (os4.vhd) para o acesso. Para criar o blob de página, chame [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) transmitir o tamanho máximo para o blob criar. O *blobSize* tem de ser um múltiplo de 512 bytes.

```csharp
using Microsoft.WindowsAzure.StorageClient;
long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### <a name="resizing-a-page-blob"></a>Redimensionar um blob de página
Para redimensionar um blob de página após a criação, utilize o [redimensionar](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) API. O tamanho pedido deve ser um múltiplo de 512 bytes.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### <a name="writing-pages-to-a-page-blob"></a>Páginas de escrita para um blob de página
Para escrever as páginas, utilize o [CloudPageBlob.WritePages](/library/microsoft.windowsazure.storageclient.cloudpageblob.writepages.aspx) método.  Isto permite-lhe escrever um conjunto de páginas até 4MBs sequencial. Tem de começar o deslocamento a ser escrito num limite de 512 bytes (startingOffset % 512 = = 0) e de fim num limite 512 - 1.  Exemplo de código seguinte mostra como chamar **WritePages** para um blob:

```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Assim que um pedido de escrita de um conjunto de páginas sequencial tem êxito no serviço blob e é replicado para durabilidade e a resiliência, a operação de escrita consolidou e é devolvido com êxito para o cliente.  

O abaixo mostra diagrama operações de escrita de 2 separado:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Uma operação de escrita que começa no desvio 0 de comprimento de 1024 bytes 
2.  Uma operação de escrita que começa no desvio 4096 de comprimento 1024 

#### <a name="reading-pages-from-a-page-blob"></a>Páginas de ler a partir de um blob de página
Para ler páginas, utilize o [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) método para um intervalo de bytes de leitura de BLOBs de páginas. Isto permite-lhe transferir o intervalo de bytes a partir de qualquer desfasamento no blob ou um blob completa. Durante a leitura, o desvio não tem de iniciar um múltiplo de 512. Ao ler os bytes de uma página NUL, o serviço devolve zero bytes.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
A figura seguinte mostra uma operação de leitura com BlobOffSet de 256 e rangeSize de 4352. Dados devolvidos são realçados cor de laranja. Zeros são devolvidos para páginas NUL

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Se tiver um blob sparsely povoado, pode querer transferir apenas as regiões de página válida para evitar pagar egressing de zero bytes e para reduzir a latência de transferência.  Para determinar quais as páginas são apoiadas por dados, utilize [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). Em seguida, pode enumerar os intervalos de devolvido e transferir os dados em cada intervalo. 
```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the rangeSize
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // TODO: use the buffer for the page range just read
} 
```

#### <a name="leasing-a-page-blob"></a>Leasing de um blob de página
A operação de concessão Blob estabelece e gere um bloqueio de um blob para escrita e operações de eliminação. Esta operação é útil em cenários onde um blob de página estiver a ser acedido a partir de vários clientes para garantir que apenas um cliente pode escrever para o blob de cada vez. Discos do Azure, por exemplo, utiliza este leasing mecanismo para garantir que o disco apenas é gerido por uma única VM. A duração do bloqueio pode ser 15 a 60 segundos, ou pode ser infinita. Consulte a documentação [aqui](/rest/api/storageservices/lease-blob) para obter mais detalhes.

> Utilize a seguinte hiperligação para obter [exemplos de código](/resources/samples/?service=storage&term=blob&sort=0 ) para muitos outros cenários de aplicação. 

Para além das APIs REST avançadas, blobs de páginas também fornecem acesso partilhado, características de durabilidade e a segurança avançada. Abordaremos as vantagens em maior detalhe nos parágrafos seguintes. 

### <a name="concurrent-access"></a>Acesso simultâneo
REST API de blobs de página e o mecanismo de locação permite que as aplicações para o blob de página de acesso de vários clientes. Por exemplo, digamos que necessita para criar um serviço de nuvem distribuída que partilhe objetos de armazenamento com vários utilizadores. Isto pode dever uma aplicação web que serve uma coleção grande de imagens para vários utilizadores. Uma opção para implementar esta é a utilizar uma VM com discos ligados. Downsides de incluem, (i) a restrição que um disco só pode ser anexado a uma única VM, por conseguinte, limitar a escalabilidade, flexibilidade e aumentar a riscos. Se existir um problema com a VM ou o serviço em execução na VM, em seguida, devido a concessão, a imagem está inacessível até que a concessão expira ou está danificada; e o custo adicional de ter uma VM do IaaS (ii). 

Uma opção alternativa consiste em utilizar blobs de páginas diretamente através de APIs de REST de armazenamento do Azure. Esta opção elimina a necessidade de VMs de IaaS dispendiosos, oferece total flexibilidade de acesso direto a partir de vários clientes, simplifica o modelo de implementação clássica ao eliminar a necessidade de expor/anular exposição discos e elimina o risco de problemas na VM. E, fornece o mesmo nível de desempenho para operações de leitura/escrita aleatórias como um disco

### <a name="durability-and-high-availability"></a>Características de durabilidade e elevada disponibilidade
Storage Standard e premium são durável armazenamento onde os dados de blob de página estão sempre replicados para garantir durabilidade e elevada disponibilidade. Para obter mais informações sobre a redundância de armazenamento do Azure, consulte este [documentação](../common/storage-redundancy.md). Azure consistentemente tem entregar durabilidade de nível empresarial para o IaaS discos e blobs, com um líder da indústria de página ZERO % [Annualized taxa de falhas](https://en.wikipedia.org/wiki/Annualized_failure_rate). Ou seja, Azure nunca perdeu a dados de blob de página do cliente. 

### <a name="seamless-migration-to-azure"></a>Migração totalmente integrada para o azure
Para os clientes e os programadores que estão interessados em implementar a sua própria solução de cópia de segurança personalizada, o Azure oferece também incrementais instantâneos que contêm apenas as diferenças. Esta funcionalidade evita o custo da cópia completa inicial, o que reduz significativamente o custo de cópia de segurança. Juntamente com a capacidade de dados diferenciais forma eficiente e cópia de leitura, este é outra poderosa que permite inovações ainda mais do que os programadores, originando uma experiência de recuperação (DR) de cópia de segurança e desastre melhor na classe no Azure. Pode configurar a suas próprias cópia de segurança ou de soluções de DR para as suas VMs no Azure com [instantâneos do Blob](/rest/api/storageservices/snapshot-blob) juntamente com o [Get página intervalos](/rest/api/storageservices/get-page-ranges) API e o [BLOBs de cópia Incremental](/rest/api/storageservices/incremental-copy-blob) API, que pode Utilize para copiar facilmente os dados incrementais para DR. 

Além disso, muitas empresas têm cargas de trabalho críticas já em execução nos centros de dados no local. Para migrar a carga de trabalho para a nuvem, uma das principais preocupações seria o período de indisponibilidade necessário para copiar os dados e o risco de problemas imprevistos após o switchover. Em muitos casos, o período de indisponibilidade pode ser um showstopper para migração para a nuvem. Utilizando a página blobs a REST API, Azure resolve este problema ao ativar a migração para a nuvem com mínima interrupção para cargas de trabalho críticas. 

Para obter exemplos sobre como criar um instantâneo e como restaurar um blob de página a partir de um instantâneo, consulte o [programa de configuração de um processo de cópia de segurança utilizando instantâneos de incrementais](../../virtual-machines/windows/incremental-snapshots.md) artigo.
