---
title: Mover grandes quantidades de dados de/para o armazenamento na cloud no Azure | Documentos da Microsoft
description: Uma visão geral dos métodos diferentes para mover dados para e do armazenamento do Azure.
services: storage
author: JarrettRenshaw
ms.service: storage
ms.topic: article
ms.date: 01/30/2017
ms.author: jarrettr
ms.component: common
ms.openlocfilehash: 81d7b5cf03e56ecc54db71b09af335d6cb794806
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525540"
---
# <a name="moving-data-to-and-from-azure-storage"></a>Mover dados de e para o Storage do Azure
Se pretender mover os dados no local para o armazenamento do Azure (ou vice-versa), existem diversas maneiras de fazer isso. A abordagem que funciona melhor para si dependerá do seu cenário. Este artigo fornece uma visão geral de diferentes cenários e ofertas apropriadas para cada um deles.

## <a name="building-applications"></a>Criando aplicativos
Se estiver criando um aplicativo, a programação com a API REST ou um dos nossos muitas bibliotecas de cliente é uma ótima maneira de mover dados para e do armazenamento do Azure.

Armazenamento do Azure fornece bibliotecas de cliente avançadas para .NET, iOS, Java, Android, plataforma Universal do Windows (UWP), Xamarin, C++, node. js, PHP, Ruby e Python. As bibliotecas de cliente oferecem funcionalidades avançadas, tais como lógica de repetição, registo e carregamentos paralelos. Também pode desenvolver diretamente na API REST, que pode ser chamada por qualquer linguagem que efetue pedidos HTTP/HTTPS.

Ver [introdução ao armazenamento de Blobs do Azure](../blobs/storage-dotnet-how-to-use-blobs.md) para saber mais.

Além disso, também Oferecemos os [biblioteca de movimento de dados de armazenamento do Azure](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) que é uma biblioteca concebida para elevado desempenho cópia de dados para e do Azure. Consulte a nossa biblioteca de movimento de dados [documentação](https://github.com/Azure/azure-storage-net-data-movement) para saber mais. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Quickly viewing/interacting with your data (Ver/interagir rapidamente com os seus dados)
Se pretender que uma forma fácil de ver os seus dados de armazenamento do Azure enquanto também tem a capacidade de carregar e transferir os seus dados, considere utilizar um Explorador de armazenamento do Azure.

Consulte nossa lista de [exploradores de armazenamento do Azure](../storage-explorers.md) para saber mais.

## <a name="system-administration"></a>Administração do sistema
Se exige ou esteja mais confortável com um utilitário de linha de comandos (por exemplo, os administradores de sistema), aqui estão algumas opções que deve considerar:

### <a name="azcopy"></a>AzCopy
O AzCopy é um utilitário de linha de comandos concebido para elevado desempenho cópia de dados para e do armazenamento do Azure. Também pode copiar dados numa conta de armazenamento, ou entre contas de armazenamento diferentes. O AzCopy é disponível no [Windows](storage-use-azcopy.md) e, no [Linux](storage-use-azcopy-linux.md).

Ver [transferir dados com o utilitário de linha de comandos do AzCopy](storage-use-azcopy.md) ou [transferir dados com AzCopy no Linux](storage-use-azcopy-linux.md) para saber mais.

### <a name="azure-powershell"></a>Azure PowerShell
O Azure PowerShell é um módulo que oferece cmdlets para gerir serviços no Azure. É uma shell de linha de comandos e linguagem de script baseada em tarefas concebida especialmente para a administração de sistemas.

Ver [utilizar o Azure PowerShell com armazenamento do Azure](storage-powershell-guide-full.md) para saber mais.

### <a name="azure-cli"></a>CLI do Azure
CLI do Azure fornece um conjunto de código-fonte aberto, os comandos para várias plataformas para trabalhar com os serviços do Azure. CLI do Azure está disponível no Windows, OSX e Linux.

Ver [utilizar a CLI do Azure com o armazenamento de Azure](../storage-azure-cli.md) para saber mais.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Mover grandes quantidades de dados com uma rede lenta
Um dos maiores desafios associados a mover grandes quantidades de dados é o tempo de transferência. Se quiser obter dados de/para armazenamento do Azure sem se preocupar sobre os custos de redes ou escrever código, em seguida, importar/exportar do Azure é uma solução apropriada.

Ver [importar/exportar do Azure](../storage-import-export-service.md) para saber mais.

## <a name="backing-up-your-data"></a>Backup de seus dados
Se precisar simplesmente de seus dados ao armazenamento do Azure de cópia de segurança, cópia de segurança do Azure é a melhor opção. Esta é uma solução poderosa para fazer backup de dados no local e VMs do Azure.

Ver [Azure Backup](../../backup/backup-introduction-to-azure-backup.md) para saber mais.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Aceder a dados no local e a partir da cloud
Se precisar de uma solução para aceder aos seus dados no local e a partir da cloud, em seguida, deve considerar a utilização solução de armazenamento de cloud híbrida do Azure, StorSimple. Esta solução consiste num dispositivo físico do StorSimple que inteligentemente frequentemente arquivos de dados que são utilizados no SSDs, ocasionalmente utilizados dados em HDDs e inativos/cópia de segurança/dados de arquivo no armazenamento do Azure.

Ver [StorSimple](../../storsimple/storsimple-overview.md) para saber mais.

## <a name="recovering-your-data"></a>Recuperar seus dados
Quando tiver aplicações e cargas de trabalho no local, terá uma solução que permite que sua empresa continue a executar em caso de desastre. O Azure Site Recovery processa a replicação, ativação pós-falha e recuperação de máquinas virtuais e servidores físicos. Os dados replicados são armazenados no armazenamento do Azure, permitindo-lhe eliminar a necessidade de um datacenter no local secundário.

Ver [do Azure Site Recovery](../../site-recovery/site-recovery-overview.md) para saber mais.
### <a name="moving-data-faq"></a>Mover dados FAQ:
## <a name="can-i-migrate-vhds-from-one-region-to-another-without-copying"></a>Posso migrar VHDs de uma região para outro sem copiar?
A única forma de copiar VHDs entre região é copiar os dados entre as contas de armazenamento em cada região. Pode utilizar o AZCopy para isso. Ver dados de transferência com o utilitário de linha de comandos de AzCopy para saber mais. Para grandes quantidades de dados, também pode importar/exportar do Azure. Ver [importar/exportar do Azure](https://docs.microsoft.com/azure/storage/storage-import-export-service) para saber mais.
