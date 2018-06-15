---
title: Azure metas de desempenho e escalabilidade de ficheiros | Microsoft Docs
description: Saiba mais sobre as metas de desempenho e escalabilidade para ficheiros do Azure, incluindo a capacidade, a taxa de pedidos e limites de largura de banda de entrada e saída.
services: storage
documentationcenter: na
author: wmgries
manager: aungoo
editor: tamram
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: beb3e5caf8c8dce9b2ea06bbd0a2ea5a4e05a714
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738079"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Azure metas de desempenho e escalabilidade de ficheiros
[Ficheiros do Azure](storage-files-introduction.md) oferece completamente geridos partilhas de ficheiros na nuvem que estão acessíveis através do protocolo SMB padrão da indústria. Este artigo aborda os destinos de desempenho e escalabilidade para ficheiros do Azure e sincronização de ficheiros do Azure (pré-visualização).

Os destinos de escalabilidade e desempenho aqui listados são destinos de gama alta de classe, mas podem ser afetados por outras variáveis na sua implementação. Por exemplo, o débito para um ficheiro poderá também ser limitado pela sua largura de banda disponível, não apenas os servidores que alojam o serviço de ficheiros do Azure. Recomendamos vivamente que teste o seu padrão de utilização para determinar se a escalabilidade e o desempenho dos ficheiros de Azure satisfazer os seus requisitos. Podemos também empenhados em aumentar estes limites ao longo do tempo. . Não hesitam em fornecer-nos comentários, ou nos comentários abaixo ou no [Azure ficheiros UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files), sobre os limites gostaria de ver-na aumentar.

## <a name="azure-storage-account-scale-targets"></a>Destinos de escala de conta de armazenamento do Azure
O recurso principal para uma partilha de ficheiros do Azure é uma conta de armazenamento do Azure. Uma conta de armazenamento representa um agrupamento de armazenamento no Azure que pode ser utilizado por vários serviços de armazenamento, incluindo ficheiros do Azure, para armazenar dados. Outros serviços que armazenam dados em contas do storage são Blob storage do Azure, o armazenamento de filas do Azure e o Table storage do Azure. Os seguintes destinos de aplicam a todos os serviços de armazenamento armazenar dados numa conta do storage:

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Utilização da conta de armazenamento de outros serviços de armazenamento afeta as partilhas de ficheiros do Azure na sua conta de armazenamento. Por exemplo, se atingir a capacidade de conta de armazenamento máximo Blob Storage do Azure, não será possível criar novos ficheiros na partilha de ficheiros do Azure, mesmo que a partilha de ficheiros do Azure está abaixo do tamanho máximo de partilha.

## <a name="azure-files-scale-targets"></a>Destinos de escala de ficheiros do Azure
[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Destinos de escala de sincronização de ficheiros do Azure
Sincronização de ficheiros do Azure, ter tentámos quanto possível para a estrutura para utilização ilimitada, no entanto, isto não é sempre possível. A tabela abaixo indica os limites do nosso teste e os destinos são os limites fixos realmente:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Métricas de desempenho de sincronização de ficheiros do Azure
Uma vez que o agente de sincronização de ficheiros do Azure é executado num computador Windows Server que liga-se às partilhas de ficheiros do Azure, o desempenho de sincronização Efetivo depende de um número de fatores na sua infraestrutura: Windows Server e a configuração do disco subjacente, largura de banda de rede entre o servidor e o armazenamento do Azure, tamanho do ficheiro, o tamanho total do conjunto de dados e a atividade no conjunto de dados. Uma vez que a sincronização de ficheiros do Azure funciona no nível do ficheiro, as características de desempenho de uma solução baseada na sincronização de ficheiros do Azure é melhor medido no número de objetos (ficheiros e diretórios) processados por segundo. 
 
Para a sincronização de ficheiros do Azure, o desempenho é fundamental em duas fases:
1. **Aprovisionamento de uma única inicial**: para otimizar o desempenho no aprovisionamento inicial, consulte [integração com sincronização de ficheiros do Azure](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync) para obter os detalhes de implementação ideal.
2. **Sincronização em curso**: depois dos dados inicialmente é pré-propagadas das partilhas de ficheiros do Azure, sincronização de ficheiros do Azure mantém os vários pontos finais sincronizadas.

Para ajudar a planear a implementação para cada um das fases, abaixo estão os resultados observados durante os testes internos num sistema com uma configuração
| Configuração do sistema |  |
|-|-|
| CPU | 64 núcleos virtual com a cache de MiB L3 64 |
| Memória | 128 GiB |
| Disco | Cache de cópia de discos SAS com RAID 10 com bateria |
| Rede | Rede de 1 Gbps |
| Carga de trabalho | Servidor de ficheiros de objetivo geral|

| Aprovisionamento de uma única inicial  |  |
|-|-|
| Número de objetos | 10 milhões de objetos | 
| Tamanho do conjunto de dados| TiB ~ 4 |
| Tamanho médio de ficheiro | KiB ~ 500 (ficheiro maior: 100 GiB) |
| Carregar o débito | 15 objetos por segundo |
| Espaço de nomes transferência débito * | 350 objetos por segundo |
 
* Quando é criado um novo ponto final do servidor, o agente de sincronização de ficheiros do Azure não transferir os conteúdos do ficheiro. -O espaço de nomes completo sincroniza-se primeiro e, em seguida, acionadores em segundo plano para transferir os ficheiros na sua totalidade da devolução de chamada ou na nuvem em camadas, se estiver ativada, a política de camadas na nuvem definida no ponto final de servidor.

| Sincronização em curso  |   |
|-|--|
| Número de objetos sincronizados| 125,000 objetos (~ 1% renovação) | 
| Tamanho do conjunto de dados| 50 giB |
| Tamanho médio de ficheiro | KiB ~ 500 (ficheiro maior: 100 GiB) |
| Carregar o débito | 20 objetos por segundo |
| Débito de transferência completa * | 30 objetos por segundo |
 
Se nuvem camadas é ativada, é provável que observar um melhor desempenho, como apenas alguns do ficheiro de dados são transferidos. Sincronização de ficheiros do Azure transfere apenas os dados de ficheiros em cache quando estes são alterados em qualquer um dos pontos finais. Para todos os ficheiros em camadas ou criados recentemente, o agente não transfere os dados de ficheiro e, em vez disso, apenas sincroniza-se o espaço de nomes a todos os pontos finais do servidor. O agente também suporta parciais transferências de ficheiros em camadas como estes são acedidos pelo utilizador. 
 
> [!Note]  
> Os números acima não são uma indicação de desempenho que irá ocorrer. O desempenho real dependerá vários fatores, conforme descrito no início desta secção.

Como um guia Geral para a sua implementação, deve manter algumas coisas em mente:
- O débito de objeto aproximadamente dimensiona in proportion to o número de grupos de sincronização no servidor. Dividir dados em vários grupos de sincronização num servidor gera a melhorar o débito, que também é limitado pelo servidor e da rede.
- O débito de objeto é inversely proporcional ao MiB por segundo débito. Para os ficheiros mais pequenos, irá ocorrer um maior débito em termos do número de objetos processados por segundo, mas inferior MiB por segundo débito. Por outro lado, para ficheiros maiores, irá obter menos objetos processados por segundo, mas superior MiB por segundo débito. MiB por segundo débito é limitado pelos destinos de escala de ficheiros do Azure. 

## <a name="see-also"></a>Consulte também
- [Planear uma implementação dos Ficheiros do Azure](storage-files-planning.md)
- [Planear uma implementação de sincronização de ficheiros do Azure](storage-sync-files-planning.md)
- [Metas de desempenho e escalabilidade para outros serviços de armazenamento](../common/storage-scalability-targets.md)