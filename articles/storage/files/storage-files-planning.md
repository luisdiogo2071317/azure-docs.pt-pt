---
title: Planear uma implementação de ficheiros do Azure | Documentos da Microsoft
description: Saiba o que considerar quando planear uma implementação de ficheiros do Azure.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: a9c37258d7c9631c6e5fe13007b78c4205a1c249
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55473889"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planear uma implementação dos Ficheiros do Azure
[Os ficheiros do Azure](storage-files-introduction.md) oferece totalmente geridos partilhas de ficheiros na cloud que estão acessíveis através do protocolo SMB padrão do setor. Porque os ficheiros do Azure totalmente gerido, implantá-lo em cenários de produção é muito mais fácil do que implementar e gerir um servidor de ficheiros ou um dispositivo. Este artigo aborda os tópicos a ter em consideração quando implementar uma partilha de ficheiros do Azure para utilização em produção na sua organização.

## <a name="management-concepts"></a>Conceitos de gestão
 O diagrama seguinte ilustra as construções de gestão de ficheiros do Azure:

![Estrutura de Ficheiros](./media/storage-files-introduction/files-concepts.png)

* **Conta de armazenamento**: Todos os acessos ao armazenamento do Azure é feito através de uma conta de armazenamento. Veja [Metas de Escalabilidade e Desempenho](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para obter detalhes sobre a capacidade das contas de armazenamento.

* **Partilha**: Uma partilha de armazenamento de ficheiros é uma partilha de ficheiros SMB no Azure. Todos os ficheiros e diretórios têm de ser criados numa partilha principal. Uma conta pode conter um número ilimitado de partilhas e uma partilha pode armazenar um número ilimitado de ficheiros, até a capacidade de total de 5 TiB de partilha de ficheiros.

* **Diretório**: Uma hierarquia opcional de diretórios.

* **Ficheiro**: Um ficheiro na partilha. Um ficheiro pode ter até 1 TiB de tamanho.

* **Formato de URL**: Para os pedidos para uma partilha de ficheiros do Azure feitas com o protocolo de REST de ficheiros, os ficheiros são endereçáveis através do formato de URL seguinte:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/directories>/<file>
    ```

## <a name="data-access-method"></a>Método de acesso de dados
Ficheiros do Azure oferecem duas, incorporados e convenientes de acesso a dados métodos que pode utilizar em separado, ou em combinação entre si, para aceder aos seus dados:

1. **Cloud acesso direto**: Qualquer partilha de ficheiros do Azure pode ser montada por [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md), e/ou [Linux](storage-how-to-use-files-linux.md) com o protocolo de bloco de mensagem de servidor (SMB) padrão do setor ou através da API de REST de ficheiros. Com o SMB, leituras e escritas de ficheiros na partilha são feitas diretamente na partilha de ficheiros no Azure. Para montar por uma VM no Azure, o cliente SMB no SO tem de suportar, pelo menos, SMB 2.1. Montar no local, como na estação de trabalho de um utilizador, o cliente do SMB suportado pela estação de trabalho tem de suportar, pelo menos, SMB 3.0 (com criptografia). Além de SMB, novos aplicativos ou serviços podem aceder diretamente a partilha de ficheiros através de REST de ficheiros, que fornece uma interface de programação de aplicativo de fácil e escalonável para o desenvolvimento de software.
2. **Azure File Sync**: Com o Azure File Sync, as partilhas podem ser replicadas para servidores do Windows no local ou no Azure. Os utilizadores seriam aceder a partilha de ficheiros através do Windows Server, tal como através de uma partilha SMB ou NFS. Isto é útil para cenários em que dados serão acedidos e modificados distantes de um datacenter do Azure, tal como num cenário de filiais. Dados podem ser replicados entre vários pontos de extremidade do Windows Server, tal como entre várias filiais. Por fim, dados podem ser colocado em camadas para ficheiros do Azure, que todos os dados são continua acessível através do servidor, mas o servidor não tem uma cópia completa dos dados. Em vez disso, dados de forma totalmente integrada são recuperados quando aberto pelo seu utilizador.

A tabela a seguir ilustra como os utilizadores e as aplicações podem aceder a partilha de ficheiros do Azure:

| | Acesso direto à cloud | Azure File Sync |
|------------------------|------------|-----------------|
| Quais protocolos precisar de utilizar? | Os ficheiros do Azure suporta o SMB 2.1 e API REST de ficheiros SMB 3.0. | Aceder à sua partilha de ficheiros do Azure através de qualquer protocolo suportado no Windows Server (SMB, NFS, FTPS, etc.) |  
| Onde está a executar a sua carga de trabalho? | **No Azure**: Os ficheiros do Azure oferece acesso direto aos seus dados. | **No local com rede lenta**: Clientes Windows, Linux e macOS podem montar uma partilha de ficheiros do Windows no local como um cache rápido da sua partilha de ficheiros do Azure. |
| O nível de ACLs precisa? | Nível de partilha e o ficheiro. | Nível de partilha de ficheiros e utilizador. |

## <a name="data-security"></a>Segurança de dados
Os ficheiros do Azure tem várias opções incorporadas para garantir a segurança de dados:

* Suporte para a encriptação em ambos os protocolos de over-the-wire: Encriptação SMB 3.0 e REST de ficheiros através de HTTPS. Por predefinição: 
    * Os clientes que suportam encriptação SMB 3.0 enviarem e recebem dados através de um canal criptografado.
    * Os clientes que não suportam SMB 3.0 com a encriptação podem comunicar intra-datacenter através de SMB 2.1 ou SMB 3.0 sem encriptação. Os clientes do SMB não são permitidos para comunicar o Centro de dados inter através de SMB 2.1 ou SMB 3.0 sem encriptação.
    * Os clientes podem comunicar através do REST de ficheiros com HTTP ou HTTPS.
* Encriptação em repouso ([encriptação do serviço de armazenamento do Azure](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): Encriptação de serviço de armazenamento (SSE) está ativado para todas as contas de armazenamento. Dados Inativos são encriptados com chaves geridas pelo totalmente. Encriptação em repouso não aumentar os custos de armazenamento ou reduzir o desempenho. 
* Requisito opcional de dados encriptados em trânsito: quando selecionada, ficheiros do Azure rejeita o acesso os dados através de canais não encriptadas. Especificamente, são permitidas apenas HTTPS e SMB 3.0 com ligações de encriptação. 

    > [!Important]  
    > Exigir que a transferência segura de dados fará com que os clientes SMB antigos não capazes de comunicar com o SMB 3.0 com a encriptação efetuar a ativação. Para obter mais informações, consulte [montar no Windows](storage-how-to-use-files-windows.md), [montar no Linux](storage-how-to-use-files-linux.md), e [montar em macOS](storage-how-to-use-files-mac.md).

Para segurança máxima, é altamente recomendável sempre ativar ambas as encriptação inativa e ativar a encriptação de dados em trânsito, sempre que estiver a utilizar clientes modernos para aceder aos seus dados. Por exemplo, se precisa montar uma partilha numa VM Windows Server 2008 R2, que só suporta o SMB 2.1, terá de permitir o tráfego não criptografado para a sua conta de armazenamento, uma vez que o SMB 2.1 não suporta a encriptação.

Se estiver a utilizar o Azure File Sync para aceder a partilha de ficheiros do Azure, sempre Utilizamos HTTPS e SMB 3.0 com a encriptação para sincronizar os dados para os seus servidores do Windows, independentemente de se necessita de encriptação de dados em repouso.

## <a name="file-share-performance-tiers"></a>Escalões de desempenho de partilha de ficheiros
Ficheiros do Azure suportam dois escalões de desempenho: standard e premium.

* **Partilhas de ficheiros padrão** são apoiados por rotação unidades de disco de rígido (HDDs) que apresentam um desempenho fiável para cargas de trabalho de e/s que são menos sensíveis à variabilidade de desempenho, como partilhas de ficheiros para fins gerais e ambientes de desenvolvimento/teste. Partilhas de ficheiros padrão só estão disponíveis num modelo de faturação pay as you go.
* **As partilhas de ficheiros (pré-visualização) Premium** são apoiados por discos Estado sólidos (SSDs) que fornecem consistente de alto desempenho e baixa latência, em milissegundos de dígito na maioria das operações de e/s, para mais cargas de trabalho de e/s intensiva. Isso as torna adequadas para uma grande variedade de cargas de trabalho, como bases de dados, alojamento de web sites, ambientes de desenvolvimento, etc. Partilhas de ficheiros de Premium só estão disponíveis num modelo de faturação aprovisionado.

### <a name="provisioned-shares"></a>Partilhas aprovisionadas
Partilhas de ficheiros Premium são aprovisionadas com base numa proporção de GiB/IOPS/débito fixa. Para cada GiB aprovisionado, a partilha será emitida um IOPS e o débito de MiB/s de 0,1 até aos limites máximos por partilha. O mínimo de aprovisionamento são 100 GiB com o mínimo de IOPS/débito. Tamanho da partilha pode ser aumentado em qualquer altura e diminui a qualquer momento, mas pode ser reduzido a cada 24 horas desde o último aumento.

Na base de melhor esforço, todas as partilhas podem ultrapassar os limites até três IOPS por GiB de armazenamento aprovisionado durante 60 minutos ou mais consoante o tamanho da partilha. Novas partilhas de começam com o crédito de rajada completo com base na capacidade aprovisionada.

| Capacidade aprovisionada | 100 GiB | 500 GiB | 1 TiB | 5 TiB | 
|----------------------|---------|---------|-------|-------|
| Linha de base de IOPS | 100 | 500 | 1,024 | 5,120 | 
| Limite de rajada | 300 | 1,500 | 3,072 | 15,360 | 
| Débito | 110 MiB/seg | 150 MiB/seg | 202 MiB/seg | 612 MiB/seg |

## <a name="file-share-redundancy"></a>Redundância de partilha de ficheiros
Os ficheiros do Azure suporta três opções de redundância de dados: armazenamento localmente redundante (LRS), o armazenamento com redundância de zona (ZRS) e o armazenamento georredundante (GRS). As secções seguintes descrevem as diferenças entre as opções de redundância diferentes:

### <a name="locally-redundant-storage"></a>Armazenamento localmente redundante
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Armazenamento com redundância de zona
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Armazenamento georredundante
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Padrão de crescimento de dados
Hoje em dia, o tamanho máximo para uma partilha de ficheiros do Azure é de 5 TiB. Por causa da limitação atual, deve considerar o crescimento de dados esperado durante a implantação de uma partilha de ficheiros do Azure. 

É possível sincronizar as partilhas de ficheiros do Azure vários para um único servidor de ficheiros do Windows com o Azure File Sync. Isto permite-lhe garantir que as partilhas de ficheiros de grande, mais antigos que podem ter no local podem ser colocadas em Azure File Sync. Para obter mais informações, consulte [planear uma implementação de sincronização de ficheiros do Azure](storage-files-planning.md).

## <a name="data-transfer-method"></a>Método de transferência de dados
Existem muitas opções fácil em massa de transferência de dados a partir de um ficheiro existente partilharem, tais como uma partilha de ficheiros no local, para ficheiros do Azure. Algumas aplicações populares incluem (lista parcial):

* **Azure File Sync**: Como parte da primeira sincronização entre uma partilha de ficheiros do Azure (uma "ponto final da Cloud") e um espaço de nomes de diretório Windows (um "servidor de ponto final"), o Azure File Sync irá replicar todos os dados da partilha de ficheiros existentes para ficheiros do Azure.
* **[Importar/exportar do Azure](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: O serviço importar/exportar do Azure permite-lhe transferir de forma segura grandes quantidades de dados para uma partilha de ficheiros do Azure envie unidades de disco rígido num Datacenter do Azure. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: O Robocopy é uma ferramenta de cópia bem conhecido que acompanha o Windows e Windows Server. Robocopy pode ser utilizado para transferir dados para ficheiros do Azure ao montar a partilha de ficheiros localmente e, em seguida, utilizar a localização de montado como o destino no comando Robocopy.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**: O AzCopy é um utilitário de linha de comandos concebido para copiar dados de e para ficheiros do Azure, bem como armazenamento de Blobs do Azure, utilizando os comandos simples com um desempenho ideal. O AzCopy é disponível para Windows e Linux.

## <a name="next-steps"></a>Passos Seguintes
* [Planear uma implementação de sincronização de ficheiros do Azure](storage-sync-files-planning.md)
* [Implementar os ficheiros do Azure](storage-files-deployment-guide.md)
* [Implementar a sincronização de ficheiros do Azure](storage-sync-files-deployment-guide.md)
