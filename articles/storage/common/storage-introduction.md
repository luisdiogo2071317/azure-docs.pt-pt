---
title: Introdução ao Azure Storage | Microsoft Docs
description: Introdução ao Armazenamento do Azure, o armazenamento de dados da Microsoft na cloud.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: get-started-article
ms.date: 03/06/2018
ms.author: tamram
ms.openlocfilehash: 799636d0a702407be06bbe8cebae552b34d860db
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="introduction-to-microsoft-azure-storage"></a>Introdução ao Storage do Microsoft Azure

O Armazenamento do Microsoft Azure é um serviço cloud gerido da Microsoft que oferece armazenamento de elevada disponibilidade, seguro, duradouro, dimensionável e redundante. A Microsoft trata da manutenção e lida com os problemas críticos por si.

O Armazenamento do Azure consiste em três serviços de dados: Armazenamento de Blobs, Armazenamento de Ficheiros e Armazenamento de Filas. O Armazenamento de blobs suporta o armazenamento standard e premium, sendo que o último só utiliza SSDs para o desempenho mias rápido possível. Outra funcionalidade é o armazenamento de acesso esporádico, que lhe permite armazenar grandes quantidades de dados raramente acedidos a um custo mais baixo.

Neste artigo, vai ficar a saber o seguinte:
* Os serviços do Armazenamento do Azure
* Os tipos de contas de armazenamento
* Aceder a blobs, filas e ficheiros
* Encriptação
* Replicação
* Transferir dados para dentro ou fora do armazenamento
* As várias bibliotecas de clientes de armazenamento disponíveis.

Para começar a trabalhar rapidamente com o Armazenamento do Azure, veja [Criar uma conta de armazenamento](storage-quickstart-create-account.md).

## <a name="introducing-the-azure-storage-services"></a>Apresentação dos serviços de Armazenamento do Azure

Para utilizar qualquer um dos serviços fornecidos pelo Armazenamento do Azure --- Armazenamento de blobs, Armazenamento de ficheiros e Armazenamento de filas --, crie primeiro uma conta de armazenamento e, em seguida, pode transferir dados de/para um serviço específico nessa conta.

## <a name="blob-storage"></a>Armazenamento de blobs

Os blobs são, essencialmente, ficheiros semelhantes aos que armazena no seu computador (ou tablet, dispositivo móvel, etc.). Podem ser imagens, ficheiros do Microsoft Excel, ficheiros HTML, discos rígidos virtuais (VHDs), macrodados, como registos, cópias de segurança de bases de dados -- podem ser qualquer coisa, no fundo. Os blobs são armazenados em contentores, que são semelhantes a pastas.

Depois de armazenar ficheiros no Armazenamento de blobs, pode aceder aos mesmos em qualquer parte do mundo através de URLs, da interface REST ou de uma das bibliotecas de cliente de armazenamento do SDK do Azure. As bibliotecas de clientes de armazenamento estão disponíveis para muitas linguagens, incluindo Node.js, Java, PHP, Ruby, Python e .NET.

Existem três tipos de blobs - blobs de blocos, blobs de páginas (utilizados para ficheiros VHD) e blobs de acréscimo.

* Os blobs de blocos são utilizados para armazenar ficheiros comuns com um máximo de 4,7 TB.
* Os blobs de páginas são utilizados para armazenar ficheiros de acesso aleatório com até 8 TB de tamanho. São utilizados para os ficheiros VHD que suportam a VMs.
* Os blobs de acréscimo são compostos por blocos, de forma semelhante aos blobs de blocos, mas estão otimizados para operações de acréscimo. São utilizados para ações como registar informações no mesmo blob provenientes de várias VMs.

Para conjuntos de dados muito grandes e em que as limitações de rede fazem com que carregar ou transferir dados para o Armazenamento de blobs de forma automática seja irrealista, pode enviar um conjunto de discos rígidos à Microsoft para importar ou exportar dados diretamente do datacenter. Consulte o artigo [Utilizar o Serviço de Importação/Exportação do Microsoft Azure para Transferir Dados para o Blob Storage](../storage-import-export-service.md).

## <a name="azure-files"></a>Ficheiros do Azure
O serviço [Ficheiros do Azure](../files/storage-files-introduction.md) permite configurar partilhas de ficheiros de rede de elevada disponibilidade que podem ser acedidas através do protocolo SMB (Server Message Block) padrão. Isto significa que múltiplas VMs podem partilhar os mesmos ficheiros com acesso de leitura e de escrita. Também pode ler os ficheiros através da interface REST ou das bibliotecas de cliente de armazenamento.

A única coisa que distingue os Ficheiros do Azure dos ficheiros numa partilha de ficheiros empresarial é o facto de o utilizador poder aceder aos ficheiros a partir de qualquer parte do mundo através de um URL que aponta para o ficheiro e inclui um token de assinatura de acesso partilhado (SAS). Pode gerar tokens SAS; estes permitem o acesso específico a um recurso privado durante um período de tempo específico.

As partilhas de ficheiros podem ser utilizadas para inúmeros cenários comuns:

* Muitas aplicações no local utilizam partilhas de ficheiros. Esta funcionalidade facilita a migração dessas aplicações que partilham dados no Azure. Se montar a partilha de ficheiros na mesma letra de unidade utilizada pela aplicação no local, a parte da sua aplicação que acede à partilha de ficheiros deve funcionar com alterações mínimas, se existirem.

* Os ficheiros de configuração podem ser armazenados numa partilha de ficheiros e acedidos a partir de múltiplas VMs. As ferramentas e os utilitários utilizados pelos múltiplos programadores num grupo podem ser armazenados numa partilha de ficheiros, o que garante que todos podem encontrá-los e que utilizam a mesma versão.

* Os registos de diagnóstico, métricas e informações de falha são apenas três exemplos de dados que podem ser escritos numa partilha de ficheiros e processados ou analisados mais tarde.

Neste momento, a autenticação baseada no Active Directory e as listas de controlo de acesso (ACLs) não são suportadas, mas vão ser futuramente. As credenciais da conta de armazenamento são utilizadas para fornecer à autenticação o acesso à partilha de ficheiros. Isto significa que qualquer pessoa com a partilha montada terá total acesso de leitura/escrita à partilha.

## <a name="queue-storage"></a>Armazenamento de filas

O serviço Filas do Azure é utilizado para armazenar e obter mensagens. As mensagens das filas podem ter até 64 KB de tamanho, ao passo que as filas podem conter milhões de mensagens. Geralmente, as filas são utilizadas para armazenar listas de mensagens que vão ser processadas de forma assíncrona.

Por exemplo, imaginemos que quer dar aos seus clientes a capacidade de carregar imagens e que quer igualmente criar miniaturas de cada uma delas. Pode fazer com que os clientes esperem até criar as miniaturas enquanto carregam as imagens. Uma alternativa é utilizar uma fila. Quando o cliente concluir o carregamento, escreva uma mensagem para a fila. Em seguida, crie um função das Funções do Azure para obter a mensagem da fila e criar as miniaturas. Cada uma das partes envolvidas neste processamento pode ser dimensionada à parte, dando-lhe mais controlo para a otimizar para a sua utilização.

## <a name="table-storage"></a>Table Storage

O armazenamento de Tabelas do Azure faz agora parte do Azure Cosmos DB. Para ver a documentação do armazenamento de Tabelas do Azure, veja a [Descrição Geral do Armazenamento de Tabelas do Azure](../../cosmos-db/table-storage-overview.md). Para além do serviço de armazenamento de Tabelas do Azure já existente, há uma nova oferta de API de Tabela do Azure Cosmos DB que disponibiliza tabelas otimizadas para débito, distribuição global e índices secundários automáticos. Para saber mais e experimentar a nova experiência premium, veja [API de Tabelas do Azure Cosmos DB](https://aka.ms/premiumtables).

## <a name="disk-storage"></a>Armazenamento em disco

O Armazenamento do Azure também inclui capacidades de discos geridos e não geridos que as máquinas virtuais utilizam. Para obter mais informações sobre estas funcionalidades, veja a [documentação do Serviço de Computação](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).

## <a name="types-of-storage-accounts"></a>Tipos de contas de armazenamento

Esta tabela mostra os vários tipos de contas de armazenamento e quais os objetos que podem ser utilizados com cada uma.

|**Tipo de conta de armazenamento**|**Standard para fins gerais**|**Premium para fins gerais**|**Armazenamento de blobs, camadas de acesso frequente e esporádico**|
|-----|-----|-----|-----|
|**Serviços suportados**| Serviços de Blob, Ficheiro e Fila | Serviço Blob | Serviço Blob|
|**Tipos de blobs suportados**|Blobs de blocos, blobs de páginas e blobs de acréscimo | Blobs de páginas | Blobs de blocos e blobs de acréscimo|

### <a name="general-purpose-storage-accounts"></a>Contas de armazenamento para fins gerais

Existem dois tipos de contas de armazenamento para fins gerais.

#### <a name="standard-storage"></a>Armazenamento Standard

As contas de armazenamento mais amplamente utilizadas são as contas de armazenamento standard, que podem ser utilizadas para todos os tipos de dados. As contas de armazenamento standard armazenam os dados num suporte de dados magnético.

#### <a name="premium-storage"></a>Armazenamento Premium

O Armazenamento Premium disponibiliza armazenamento de elevado desempenho para blobs de páginas, que são essencialmente utilizados para ficheiros VHD. As contas deste tipo de armazenamento utilizam o SSD para armazenar os dados. A Microsoft recomenda utilizar o Armazenamento Premium em todas as suas VMs.

### <a name="blob-storage-accounts"></a>Contas de Armazenamento de blobs

A conta de Armazenamento de blobs é uma conta de armazenamento especializada utilizada para armazenar blobs de blocos e blobos de acréscimo. Não pode armazenar blobs de páginas nestas contas, pelo que não pode armazenar ficheiros VHD. Estas contas permitem-lhe definir uma camada de acesso Frequente ou Esporádico, pode ser alterada em qualquer altura.

A camada de acesso frequente é utilizada para os ficheiros que são acedidos com frequência – paga um custo mais elevado para o armazenamento, mas o custo de aceder aos blobs é muito inferior. Quanto a blobs armazenados na camada de acesso esporádico, o acesso aos blobs é mais caro, mas o custo do armazenamento é muito mais baixo.

## <a name="accessing-your-blobs-files-and-queues"></a>Aceder a blobs, ficheiros e filas

Cada conta de armazenamento tem duas chaves de autenticação, sendo que pode ser utilizada qualquer uma destas para realizar qualquer operação. Existem duas chaves, pelo que pode implementá-las ocasionalmente para melhorar a segurança. É fundamental que estas chaves estejam guardadas em segurança, porque estar na posso da mesma, juntamente com o nome de utilizador, permite acesso ilimitado a todos os dados da conta de armazenamento.

Esta secção mostra duas formas de proteger a conta de armazenamento e os respetivos dados. Para obter informações detalhadas sobre como proteger a sua conta de armazenamento e os seus dados, veja [manual de segurança do Armazenamento do Azure](storage-security-guide.md).

### <a name="securing-access-to-storage-accounts-using-azure-ad"></a>Utilizar o Azure AD para proteger o acesso às contas de armazenamento

Uma forma de proteger o acesso aos seus dados de armazenamento é controlar o acesso às chaves da conta de armazenamento. Com o Controlo de Acesso Baseado em Funções (RBAC) do Resource Manager, pode atribuir funções a utilizadores, grupos ou aplicações. Estas funções estão associadas a um conjunto específico de ações que são permitidas ou negadas. Utilizar o RBAC para conceder acesso a uma conta de armazenamento processa apenas as operações de gestão dessa conta, tal como alterar a camada de acesso. Não pode utilizar o RBAC para conceder acesso a objetos de dados, como um contentor ou uma partilha de ficheiros específica. No entanto, pode utilizar o RBAC para conceder acesso às chaves da conta de armazenamento, que podem, depois, ser utilizadas para ler os objetos de dados.

### <a name="securing-access-using-shared-access-signatures"></a>Utilizar assinaturas de acesso partilhado para proteger o acesso

Pode utilizar assinaturas de acesso partilhado e políticas de acesso armazenadas para proteger os seus objetos de dados. As assinaturas de acesso partilhado (SAS) são uma cadeia que contém um token de segurança que pode ser ligado ao URI de um recurso que lhe permite delegar acesso a objetos de armazenamento específicos e especificar limitações, como permissões e o intervalo de data/hora do acesso. Esta funcionalidade tem extensas capacidades. Para obter informações detalhadas, veja [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) (Utilizar Assinaturas de Acesso Partilhado [SAS]).

### <a name="public-access-to-blobs"></a>Acesso público a blobs

O Serviço Blob permite-lhe fornecer acesso público a um contentor e respetivos blobs ou a um blob específico. Quando indicar que um contentor ou um blob é público, todas as pessoas podem lê-lo anonimamente. Não é necessária autenticação. Um exemplo de quando pode utilizar o acesso público é se tiver um Web site que utiliza imagens, vídeos ou documentos provenientes do Armazenamento de blobs. Para obter mais informações, veja [Manage anonymous read access to containers and blobs](../blobs/storage-manage-access-to-resources.md) (Gerir o acesso de leitura anónima a contentores e blobs).

## <a name="encryption"></a>Encriptação

Estão disponíveis dois tipos básicos de encriptação para os serviços de Armazenamento. Para obter mais informações sobre segurança e encriptação, veja o [Guia de segurança do Armazenamento do Azure](storage-security-guide.md).

### <a name="encryption-at-rest"></a>Encriptação inativa

A Encriptação do Serviço de Armazenamento do Azure (SEE) ajuda a proteger e salvaguardar os seus dados para cumprir as obrigações de conformidade e segurança da sua organização. Com esta funcionalidade, o Armazenamento do Azure encripta automaticamente os dados antes de continuar a armazenar e desencriptar antes da obtenção. A encriptação, a desencriptação e a gestão de chaves são completamente transparentes para os utilizadores.

O SSE encripta automaticamente dados em todos os escalões de desempenho (Standard e Premium), todos os modelos de implementação (Azure Resource Manager e Clássico) e todos os serviços de Armazenamento do Azure (Blob, Fila, Tabela e Ficheiro). O SSE não afeta o desempenho do Armazenamento do Azure.

Para obter mais informações sobre a encriptação de SSE inativa, veja [Encriptação do Serviço de Armazenamento do Azure para Dados Inativos](storage-service-encryption.md).

### <a name="client-side-encryption"></a>Encriptação do lado do cliente

As bibliotecas de cliente de armazenamento têm métodos que pode chamar para encriptar os dados programaticamente antes de os enviar de forma automática do cliente para o Azure. São armazenados encriptados, o que significa que também estão encriptados quando estão inativos. Ao ler os dados novamente, desencripte as informações depois de as receber.

Para obter mais informações sobre a encriptação do lado do cliente, veja [Encriptação do Lado do Cliente com .NET para o Armazenamento do Microsoft Azure](storage-client-side-encryption.md).

## <a name="replication"></a>Replicação

Para garantir que os seus dados são duráveis, o Armazenamento do Azure manterá (e gerirá) várias cópias dos mesmos. Estas cópias são denominadas “réplicas” ou, por vezes, “redundância”. Quando configurar a sua conta de armazenamento, vai selecionar um tipo de replicação. Na maioria dos casos, esta definição pode ser alterada após a configuração da conta de armazenamento.

**Armazenamento localmente redundante (LRS)**

Armazenamento localmente redundante (LRS), que foi criado para proporcionar, pelo menos, 99,999999999% (11 9 s) de durabilidade de objetos ao longo de um ano. Isto significa que são geridas várias cópias dos seus dados pelo Armazenamento do Azure no datacenter especificado quando a conta de armazenamento foi configurada. Quando as alterações forem consolidadas, todas as cópias são atualizadas antes da devolução com êxito. Isto significa que as réplicas estão sempre sincronizadas. Além disso, as cópias residem em domínios de falha e em domínios de atualização diferentes, o que quer dizer que os seus dados estão disponíveis mesmo que um nó de armazenamento que contenha os seus dados falhe ou seja colocado offline para ser atualizado.

**Armazenamento com redundância de zona (ZRS) (Pré-visualização)**

O armazenamento com redundância de zona (ZRS) foi concebido para simplificar o desenvolvimento de aplicações de elevada disponibilidade. O ZRS fornece durabilidade para objetos de armazenamento de, pelo menos, 99.9999999999% (12 9 s) ao longo de um ano específico. O ZRS replica os dados de forma síncrona em várias zonas de disponibilidade. Considere o ZRS para cenários, como aplicações transacionais onde o período de indisponibilidade não é aceitável. O ZRS permite aos clientes ler e escrever dados, mesmo se uma única zona está indisponível ou irrecuperável. As inserções e atualizações para os dados são efetuadas de forma síncrona e são vivamente consistentes.    

A capacidade ZRS anterior agora é referida como ZRS Classic. As contas ZRS Classic estão disponíveis apenas para blobs de blocos em contas de armazenamento para fins gerais V1. O ZRS Classic replica os dados no modo assíncrono entre datacenters dentro de uma ou duas regiões. Uma réplica pode não estar disponível, a menos que a Microsoft inicie a ativação pós-falha para o secundário. Uma conta ZRS Classic não pode ser convertida para ou do LRS ou GRS e não tem as métricas de capacidade de inicio de sessão.

**Armazenamento georredundante (GRS)**

O armazenamento georredundante (GRS) foi criado para proporcionar 99,99999999999999% (16 9 s) de durabilidade de objetos ao longo de um ano, ao manter as cópias locais dos seus dados numa região primária e outro conjunto de cópias numa região secundária, a centenas de quilómetros da primária. Se ocorrer uma falha na região primária, o Armazenamento do Azure fará uma ativação pós-falha para a região secundária.

**Armazenamento georredundante com acesso de leitura (RA-GRS)**

O armazenamento georredundante com acesso de leitura é exatamente igual ao GRS, com a diferença de que obtém acesso de leitura aos dados na localização secundária. Se o datacenter primário ficar temporariamente indisponível, pode continuar a ler os dados na localização secundária. Isto pode ser muito útil. Por exemplo, pode ter uma aplicação Web que muda para o modo só de leitura e aponta para a cópia secundária, permitindo algum acesso mesmo apesar de as atualizações não estarem disponíveis.

> [!IMPORTANT]
> Pode alterar como os dados são replicados depois de criar a sua conta do Storage. No entanto, pode ser cobrado um custo único adicional relativo à transferência de dados se mudar do LRS ou ZRS para o GRS ou o RA-GRS.
>

Para obter mais informações sobre as opções de replicação, veja [Azure Storage replication](storage-redundancy.md) (Replicação do Armazenamento do Azure).

Para obter informações sobre a recuperação após desastre, veja [What to do if an Azure Storage outage occurs](storage-disaster-recovery-guidance.md) (O que fazer em caso de falha do Armazenamento do Azure).

Para obter um exemplo de como tirar partido do armazenamento RA-GRS para garantir a elevada disponibilidade, veja [Designing Highly Available Applications using RA-GRS](storage-designing-ha-apps-with-ragrs.md) (Conceber Aplicações de Elevada Disponibilidade com RA-GRS).

## <a name="transferring-data-to-and-from-azure-storage"></a>Transferir dados de e para o Armazenamento do Azure

Pode utilizar o utilitário da linha de comandos AzCopy para copiar os dados de blobs e de ficheiros na sua conta ou em várias contas de armazenamento. Veja um dos artigos seguintes para obter ajuda:

* [Transfer data with AzCopy for Windows](storage-use-azcopy.md) (Transferir dados com AzCopy para Windows)
* [Transfer data with AzCopy for Linux](storage-use-azcopy-linux.md) (Transferir dados com AzCopy para Linux)

O AzCopy é criado sobre a [Biblioteca de Movimento de Dados do Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), que está atualmente disponível na pré-visualização.

Pode ser utilizado o serviço Importar/Exportar do Azure para importar ou exportar grandes quantidades de dados de blobs de ou para a sua conta de armazenamento. Para tal, prepare e envie por correio vários discos rígidos para um datacenter do Azure, onde os dados são transferidos do/para os esses discos, que, depois, lhe são devolvidos. Para obter mais informações sobre o serviço de Importação/Exportação, consulte o artigo [Utilizar o Serviço de Importação/Exportação do Microsoft Azure para Transferir Dados para o Blob Storage](../storage-import-export-service.md).

## <a name="pricing"></a>Preços

Para obter informações detalhadas sobre os preços do Armazenamento do Azure, veja a [página de preços](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>APIs de armazenamento, bibliotecas e ferramentas
É possível aceder aos recursos do Storage do Azure por qualquer idioma que consiga efetuar pedidos de HTTP/HTTPS. Além disso, o Storage do Azure oferece bibliotecas de programação para vários idiomas populares. Estas bibliotecas simplificam muitos aspetos do trabalho com o Storage do Azure ao processar detalhes como a invocação síncrona e assíncrona, a criação de batches de operações, a gestão de exceções, as tentativas automáticas, o comportamento operacional, etc. Atualmente, as bibliotecas estão disponíveis para os seguintes idiomas e plataformas, com outros no pipeline:

### <a name="azure-storage-data-services"></a>Serviços de dados do Armazenamento do Azure
* [API REST dos Serviços de Armazenamento](/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1) (Biblioteca de Clientes de Armazenamento para .NET)
* [Biblioteca de Clientes de Armazenamento para C++](https://github.com/Azure/azure-storage-cpp)
* [Biblioteca de Clientes de Armazenamento do Azure para Java/Android](https://azure.microsoft.com/develop/java/)
* [Biblioteca de Clientes de Armazenamento para Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Biblioteca de Clientes de Armazenamento para PHP](https://azure.microsoft.com/develop/php/)
* [Biblioteca de Clientes de Armazenamento para Python](https://azure.microsoft.com/develop/python/)
* [Biblioteca de Clientes de Armazenamento para Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage Cmdlets for PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0) (Cmdlets do Armazenamento para PowerShell)
* [Storage Commands for CLI 2.0](/cli/azure/storage) (Comandos do Armazenamento para a CLI 2.0)

## <a name="next-steps"></a>Passos seguintes

* [Learn more about Blob storage](../blobs/storage-blobs-introduction.md) (Saiba mais sobre o Armazenamento de blobs)
* [Learn more about File storage](../storage-files-introduction.md) (Saiba mais sobre o Armazenamento de ficheiros)
* [Learn more about Queue storage](../queues/storage-queues-introduction.md) (Saiba mais sobre o Armazenamento de filas)

Para começar a trabalhar rapidamente com o Armazenamento do Azure, veja [Criar uma conta de armazenamento](storage-quickstart-create-account.md).

<!-- FIGURE OUT WHAT TO DO WITH ALL THESE LINKS.

Azure Storage resources can be accessed by any language that can make HTTP/HTTPS requests. Additionally, Azure Storage offers programming libraries for several popular languages. These libraries simplify many aspects of working with Azure Storage by handling details such as synchronous and asynchronous invocation, batching of operations, exception management, automatic retries, operational behavior and so forth. Libraries are currently available for the following languages and platforms, with others in the pipeline:

### Azure Storage data services
* [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp)
* [Storage Client Library for Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage Client Library for Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage Client Library for PHP](https://azure.microsoft.com/develop/php/)
* [Storage Client Library for Python](https://azure.microsoft.com/develop/python/)
* [Storage Client Library for Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage Cmdlets for PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)

### Azure Storage management services
* [Storage Resource Provider REST API Reference](/rest/api/storagerp/)
* [Storage Resource Provider Client Library for .NET](/dotnet/api/microsoft.azure.management.storage)
* [Storage Resource Provider Cmdlets for PowerShell 1.0](/powershell/module/azure.storage)
* [Storage Service Management REST API (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure Storage data movement services
* [Storage Import/Export Service REST API](../storage-import-export-service.md)
* [Storage Data Movement Client Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Tools and utilities
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.
* [Azure Storage Client Tools](../storage-explorers.md)
* [Azure SDKs and Tools](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [AzCopy Command-Line Utility](http://aka.ms/downloadazcopy)

## Next steps
To learn more about Azure Storage, explore these resources:

### Documentation
* [Azure Storage Documentation](https://azure.microsoft.com/documentation/services/storage/)
* [Create a storage account](../storage-create-storage-account.md)

-->

### <a name="for-administrators"></a>Para administradores
* [Using Azure PowerShell with Azure Storage (Utilizar o Azure PowerShell com o Armazenamento do Azure)](storage-powershell-guide-full.md)
* [Utilizar a CLI do Azure com o Armazenamento do Azure](../storage-azure-cli.md)

### <a name="for-net-developers"></a>Para programadores do .NET
* [Introdução ao armazenamento de Blobs do Azure através do .NET](../blobs/storage-dotnet-how-to-use-blobs.md)
* [Programar para os Ficheiros do Azure com .NET](../files/storage-dotnet-how-to-use-files.md)
* [Introdução ao armazenamento de Tabelas do Azure através do .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Introdução ao Armazenamento de filas do Azure através do .NET](../storage-dotnet-how-to-use-queues.md)

### <a name="for-javaandroid-developers"></a>Para orogramadores de Java/Android
* [Como utilizar o Armazenamento de blobs do Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Programar para os Ficheiros do Azure com Java](../files/storage-java-how-to-use-file-storage.md)
* [Como utilizar o Armazenamento de tabelas do Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Como utilizar o Armazenamento de filas do Java](../storage-java-how-to-use-queue-storage.md)

### <a name="for-nodejs-developers"></a>Para programadores de Node.js
* [Como utilizar o Armazenamento de blobs do Node.js](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Como utilizar o Armazenamento de tabelas do Node.js](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Como utilizar o Armazenamento de filas do Node.js](../storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Para programadores de PHP
* [Como utilizar o Armazenamento de blobs do PHP](../blobs/storage-php-how-to-use-blobs.md)
* [Como utilizar o Armazenamento de tabelas do PHP](../../cosmos-db/table-storage-how-to-use-php.md)
* [Como utilizar o Armazenamento de filas do PHP](../storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Para programadores de Ruby
* [Como utilizar o Armazenamento de blobs do Ruby](../blobs/storage-ruby-how-to-use-blob-storage.md)
* [Como utilizar o Armazenamento de tabelas do Ruby](../../cosmos-db/table-storage-how-to-use-ruby.md)
* [Como utilizar o Armazenamento de filas do Ruby](../storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Para programadores de Python
* [Como utilizar o Armazenamento de blobs do Python](../blobs/storage-python-how-to-use-blob-storage.md)
* [Programar para os Ficheiros do Azure com Python](../files/storage-python-how-to-use-file-storage.md)
* [Como utilizar o Armazenamento de tabelas do Python](../../cosmos-db/table-storage-how-to-use-python.md)
* [Como utilizar o Armazenamento de filas do Python](../storage-python-how-to-use-queue-storage.md)
