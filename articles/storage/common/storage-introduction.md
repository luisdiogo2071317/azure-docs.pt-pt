---
title: Introdução ao Armazenamento do Azure - Armazenamento da cloud no Azure | Microsoft Docs
description: O Armazenamento do Azure é a solução de armazenamento da cloud da Microsoft. O Armazenamento do Azure oferece armazenamento de objetos de dados altamente disponível, seguro, durável, extremamente dimensionável e redundante.
services: storage
author: tamram
ms.service: storage
ms.topic: get-started-article
ms.date: 07/11/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 5e9d105e0824eac6052f4ea3f99a09c19a668294
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525574"
---
# <a name="introduction-to-azure-storage"></a>Introdução ao Armazenamento do Azure

O Armazenamento do Azure é a solução de armazenamento da cloud da Microsoft para cenários de armazenamento de dados modernos. O Armazenamento do Azure oferece um arquivo de objetos extremamente dimensionável para objetos de dados, um serviço de sistema de ficheiros para a cloud, um arquivo de mensagens para mensagens fiáveis e um arquivo de NoSQL. O Armazenamento do Azure é:

- **Durável e de elevada disponibilidade.** A redundância garante que os dados estão seguros em caso de falhas de hardware transitórias. Também pode optar por replicar dados em centros de dados ou regiões geográficas para obter proteção adicional contra catástrofes locais ou desastres naturais. Os dados replicados desta forma permanecem altamente disponíveis no caso de uma falha inesperada. 
- **Proteger.** Todos os dados escritos no Armazenamento do Azure são encriptado pelo serviço. O Armazenamento do Azure oferece-lhe controlo detalhado sobre quem tem acesso aos seus dados.
- **Escalável.** O Armazenamento do Azure foi criado para ser extremamente dimensionável para satisfazer as necessidades de armazenamento e desempenho de dados das aplicações atuais. 
- **Gerido.** O Microsoft Azure trata da manutenção e lida com os problemas críticos por si.
- **Acessível.** Os dados no Armazenamento do Azure são acessíveis a partir de qualquer local no mundo através de HTTP ou HTTPS. A Microsoft disponibiliza SDKs para o Armazenamento do Azure numa variedade de linguagens – .NET, Java, Node.js, Python, PHP, Ruby, Go e outros utilizadores – bem como uma API REST madura. O Armazenamento do Azure suporta scripting no Azure PowerShell ou na CLI do Azure. E o portal do Azure e o Explorador de Armazenamento do Azure oferecem soluções visuais simples para trabalhar com os seus dados.  

## <a name="azure-storage-services"></a>Serviços do Armazenamento do Azure

O Armazenamento do Azure inclui estes serviços de dados: 

- [Blobs do Azure](../blobs/storage-blobs-introduction.md): um arquivo de objetos extremamente dimensionável para texto e dados binários.
- [Ficheiros do Azure](../files/storage-files-introduction.md): partilhas de ficheiros geridos para a cloud ou implementações locais.
- [Filas do Azure](../queues/storage-queues-introduction.md): arquivo de mensagens para mensagens fiáveis entre componentes da aplicação. 
- [Tabelas do Azure](../tables/table-storage-overview.md): um arquivo do NoSQL para armazenamento sem esquemas de dados estruturados.

Cada serviço é acedido através de uma conta de armazenamento. Para começar a utilizar, veja [Criar uma conta de armazenamento](storage-quickstart-create-account.md).

## <a name="blob-storage"></a>Armazenamento de blobs

O Armazenamento de Blobs do Azure é a solução de armazenamento de objetos da Microsoft para a cloud. O Armazenamento de blobs está otimizado para armazenar grandes quantidades de dados não estruturados, como dados de texto ou binários. 

O armazenamento de blobs é ideal para:

* Entrega de imagens ou documentos diretamente a um browser.
* Armazenamento de ficheiros para acesso distribuído.
* Transmissão de áudio e vídeo.
* Armazenamento de dados de cópia de segurança e restauro, recuperação após desastre e arquivo.
* Armazenamento de dados para análise por um serviço no local ou alojado no Azure.

Os Objetos no Armazenamento de blobs podem ser acedidos em qualquer local no mundo através de HTTP ou HTTPS. Os utilizadores ou aplicações de cliente podem aceder a blobs através de URLs, à [API REST do Armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), ao [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), à [CLI do Azure](https://docs.microsoft.com/cli/azure/storage) ou a uma biblioteca de cliente do Armazenamento do Azure. As bibliotecas de clientes de armazenamento estão disponíveis para muitas linguagens, incluindo [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/en/latest/index.html), [PHP](http://azure.github.io/azure-storage-php/) e [Ruby](http://azure.github.io/azure-storage-ruby).

Para obter mais informações sobre armazenamento de Blobs, veja a [Introdução ao armazenamento de objetos no Azure](../blobs/storage-blobs-introduction.md).

## <a name="azure-files"></a>Ficheiros do Azure
O serviço [Ficheiros do Azure](../files/storage-files-introduction.md) permite configurar partilhas de ficheiros de rede de elevada disponibilidade que podem ser acedidas através do protocolo SMB (Server Message Block) padrão. Isto significa que múltiplas VMs podem partilhar os mesmos ficheiros com acesso de leitura e de escrita. Também pode ler os ficheiros através da interface REST ou das bibliotecas de cliente de armazenamento.

A única coisa que distingue os Ficheiros do Azure dos ficheiros numa partilha de ficheiros empresarial é o facto de o utilizador poder aceder aos ficheiros a partir de qualquer parte do mundo através de um URL que aponta para o ficheiro e inclui um token de assinatura de acesso partilhado (SAS). Pode gerar tokens SAS; estes permitem o acesso específico a um recurso privado durante um período de tempo específico.

As partilhas de ficheiros podem ser utilizadas para inúmeros cenários comuns:

* Muitas aplicações no local utilizam partilhas de ficheiros. Esta funcionalidade facilita a migração dessas aplicações que partilham dados no Azure. Se montar a partilha de ficheiros na mesma letra de unidade utilizada pela aplicação no local, a parte da sua aplicação que acede à partilha de ficheiros deve funcionar com alterações mínimas, se existirem.

* Os ficheiros de configuração podem ser armazenados numa partilha de ficheiros e acedidos a partir de múltiplas VMs. As ferramentas e os utilitários utilizados pelos múltiplos programadores num grupo podem ser armazenados numa partilha de ficheiros, o que garante que todos podem encontrá-los e que utilizam a mesma versão.

* Os registos de diagnóstico, métricas e informações de falha são apenas três exemplos de dados que podem ser escritos numa partilha de ficheiros e processados ou analisados mais tarde.

Neste momento, a autenticação baseada no Active Directory e as listas de controlo de acesso (ACLs) não são suportadas, mas vão ser futuramente. As credenciais da conta de armazenamento são utilizadas para fornecer à autenticação o acesso à partilha de ficheiros. Isto significa que qualquer pessoa com a partilha montada terá total acesso de leitura/escrita à partilha.

Para obter mais informações sobre o serviço Ficheiros do Azure, veja [Introdução ao serviço Ficheiros do Azure](../files/storage-files-introduction.md).

## <a name="queue-storage"></a>Armazenamento de filas

O serviço Filas do Azure é utilizado para armazenar e obter mensagens. As mensagens das filas podem ter até 64 KB de tamanho, ao passo que as filas podem conter milhões de mensagens. Geralmente, as filas são utilizadas para armazenar listas de mensagens que vão ser processadas de forma assíncrona.

Por exemplo, imaginemos que quer dar aos seus clientes a capacidade de carregar imagens e que quer igualmente criar miniaturas de cada uma delas. Pode fazer com que os clientes esperem até criar as miniaturas enquanto carregam as imagens. Uma alternativa é utilizar uma fila. Quando o cliente concluir o carregamento, escreva uma mensagem para a fila. Em seguida, crie um função das Função do Azure para obter a mensagem da fila e criar as miniaturas. Cada uma das partes envolvidas neste processamento pode ser dimensionada à parte, dando-lhe mais controlo para a otimizar para a sua utilização.

Para obter mais informações sobre o serviço Filas do Azure, veja [Introdução ao serviço Filas do Azure](../queues/storage-queues-introduction.md).

## <a name="table-storage"></a>Table Storage

O armazenamento de Tabelas do Azure faz agora parte do Azure Cosmos DB. Para ver a documentação do armazenamento de Tabelas do Azure, veja a [Descrição Geral do Armazenamento de Tabelas do Azure](../tables/table-storage-overview.md). Para além do serviço de armazenamento de Tabelas do Azure já existente, há uma nova oferta de API de Tabela do Azure Cosmos DB que disponibiliza tabelas otimizadas para débito, distribuição global e índices secundários automáticos. Para saber mais e experimentar a nova experiência premium, veja [API de Tabelas do Azure Cosmos DB](https://aka.ms/premiumtables).

Para obter mais informações sobre o Armazenamento de tabelas, veja [Descrição geral do Armazenamento de tabelas do Azure](../tables/table-storage-overview.md).

## <a name="disk-storage"></a>Armazenamento em disco

O Armazenamento do Azure também inclui capacidades de discos geridos e não geridos que as máquinas virtuais utilizam. Para obter mais informações sobre estas funcionalidades, veja a [documentação do Serviço de Computação](https://docs.microsoft.com/en-gb/azure/#pivot=products&panel=Compute).

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

Para garantir que os seus dados são duráveis, o Armazenamento do Azure replica várias cópias dos mesmos. Quando configurar a sua conta de armazenamento, vai selecionar um tipo de replicação. Na maioria dos casos, esta definição pode ser alterada após a criação da conta de armazenamento. 

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Para obter informações sobre a recuperação após desastre, veja [What to do if an Azure Storage outage occurs](storage-disaster-recovery-guidance.md) (O que fazer em caso de falha do Armazenamento do Azure).

## <a name="transferring-data-to-and-from-azure-storage"></a>Transferir dados de e para o Armazenamento do Azure

Pode utilizar o utilitário da linha de comandos AzCopy para copiar os dados de blobs e de ficheiros na sua conta ou em várias contas de armazenamento. Veja um dos artigos seguintes para obter ajuda:

* [Transfer data with AzCopy for Windows](storage-use-azcopy.md) (Transferir dados com AzCopy para Windows)
* [Transfer data with AzCopy for Linux](storage-use-azcopy-linux.md) (Transferir dados com AzCopy para Linux)

O AzCopy é criado sobre a [Biblioteca de Movimento de Dados do Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), que está atualmente disponível na pré-visualização.

Pode ser utilizado o serviço Importar/Exportar do Azure para importar ou exportar grandes quantidades de dados de blobs de ou para a sua conta de armazenamento. Para tal, prepare e envie por correio vários discos rígidos para um datacenter do Azure, onde os dados são transferidos do/para os esses discos, que, depois, lhe são devolvidos. Para obter mais informações sobre o serviço de Importação/Exportação, consulte o artigo [Utilizar o Serviço de Importação/Exportação do Microsoft Azure para Transferir Dados para o Blob Storage](../storage-import-export-service.md).

Para importar grandes quantidades de dados de Blob para a sua conta de armazenamento de uma forma rápida, acessível e fiável, também pode utilizar o disco do Azure Data Box. A Microsoft envia até cinco discos de estado sólido encriptados (SSD) com uma capacidade de 40 TB, para o seu datacenter através de uma operadora regional. Pode configurar rapidamente os discos, copiar dados para discos através de uma ligação de USB e enviar os discos novamente para o Azure. No datacenter do Azure, os seus dados são carregados automaticamente de unidades para a cloud. Para obter mais informações sobre esta solução, aceda a [Descrição geral do disco do Azure Data Box](https://docs.microsoft.com/azure/databox/data-box-disk-overview).

## <a name="pricing"></a>Preços

Para obter informações detalhadas sobre os preços do Armazenamento do Azure, veja a [página de preços](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>APIs de armazenamento, bibliotecas e ferramentas
É possível aceder aos recursos do Storage do Azure por qualquer idioma que consiga efetuar pedidos de HTTP/HTTPS. Além disso, o Storage do Azure oferece bibliotecas de programação para vários idiomas populares. Estas bibliotecas simplificam muitos aspetos do trabalho com o Storage do Azure ao processar detalhes como a invocação síncrona e assíncrona, a criação de batches de operações, a gestão de exceções, as tentativas automáticas, o comportamento operacional, etc. Atualmente, as bibliotecas estão disponíveis para os seguintes idiomas e plataformas, com outros no pipeline:

### <a name="azure-storage-data-api-and-library-references"></a>API de dados do Armazenamento do Azure e referências da biblioteca
* [API REST dos Serviços de Armazenamento](https://docs.microsoft.com/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage) (Biblioteca de Clientes de Armazenamento para .NET)
* [Biblioteca de Clientes de Armazenamento do Azure para Java/Android](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Biblioteca de Clientes de Armazenamento para Node.js](https://docs.microsoft.com/javascript/api/azure-storage)
* [Biblioteca de Clientes de Armazenamento para Python](https://github.com/Azure/azure-storage-python)
* [Biblioteca de Clientes de Armazenamento para PHP](https://github.com/Azure/azure-storage-php)
* [Biblioteca de Clientes de Armazenamento para Ruby](https://github.com/Azure/azure-storage-ruby)
* [Biblioteca de Clientes de Armazenamento para C++](https://github.com/Azure/azure-storage-cpp)

### <a name="azure-storage-management-api-and-library-references"></a>API de gestão do Armazenamento do Azure e referências da biblioteca
* [API REST do Fornecedor de Recursos de Armazenamento](https://docs.microsoft.com/rest/api/storagerp/)
* [Biblioteca de Clientes do Fornecedor de Recursos de Armazenamento para o .NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/management)
* [API REST da Gestão de Serviços de Armazenamento (Clássica)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-api-and-library-references"></a>API de movimentação do Armazenamento do Azure e referências da biblioteca
* [API REST do Serviço de Importação/Exportação de Armazenamento](https://docs.microsoft.com/rest/api/storageimportexport/)
* [Biblioteca de Clientes do Movimento de Dados de Armazenamento para o .NET](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.datamovement)

### <a name="tools-and-utilities"></a>Ferramentas e utilitários
* [Cmdlets do Azure PowerShell para Armazenamento](https://docs.microsoft.com/powershell/module/azure.storage)
* [Cmdlets da CLI do Azure para Armazenamento](https://docs.microsoft.com/cli/azure/storage)
* [Utilitário da Linha de Comandos do AzCopy](http://aka.ms/downloadazcopy)
* O [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) é uma aplicação autónoma e gratuita da Microsoft, que lhe permite trabalhar visualmente com dados do Armazenamento do Azure no Windows, macOS e Linux.
* [Ferramentas de Cliente do Armazenamento do Azure](../storage-explorers.md)
* [Ferramentas de Programação do Azure](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Passos seguintes

Para começar a trabalhar rapidamente com o Armazenamento do Azure, veja [Criar uma conta de armazenamento](storage-quickstart-create-account.md).
