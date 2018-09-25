---
title: O que fazer em caso de interrupção de armazenamento do Azure | Documentos da Microsoft
description: O que fazer em caso de interrupção de armazenamento do Azure
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 09/13/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 20db515e99f3e7535ba7b60bbd84f050e33b7acb
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033928"
---
# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>O que fazer se ocorrer uma falha do Armazenamento do Azure
Na Microsoft, trabalhamos arduamente para se certificar de que os nossos serviços estão sempre disponíveis. Às vezes, força além do nosso impacto de controle-nos de forma a provocar interrupções de serviço não planeada num ou mais regiões. Para ajudar a lidar com essas ocorrências raras, fornecemos as seguintes orientações de alto nível dos serviços de armazenamento do Azure.

## <a name="how-to-prepare"></a>Como preparar
É fundamental para cada cliente preparar o seu próprio plano de recuperação após desastre. O esforço necessário para recuperar a partir de uma falha no armazenamento normalmente envolve o pessoal de operações e procedimentos automatizados para reativar as suas aplicações num Estado de funcionamento. Veja a documentação do Azure abaixo para criar seu próprio plano de recuperação após desastre:

* [Lista de verificação de disponibilidade](https://docs.microsoft.com/azure/architecture/checklist/availability)
* [Conceber aplicações resilientes para o Azure](https://docs.microsoft.com/azure/architecture/resiliency/)
* [Serviço de recuperação de sites do Azure](https://azure.microsoft.com/services/site-recovery/)
* [Replicação do Armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Serviço de cópia de segurança do Azure](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Como detetar
A forma recomendada para determinar o estado do serviço do Azure é subscrever o [Dashboard de estado de funcionamento do serviço de Azure](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>O que fazer se ocorrer uma falha de armazenamento
Se um ou mais serviços de armazenamento estão temporariamente indisponíveis numa ou mais regiões, existem duas opções para a ser considerada. Se desejar ter acesso imediato aos seus dados, considere a opção 2.

### <a name="option-1-wait-for-recovery"></a>Opção 1: Fim da espera para recuperação
Neste caso, nenhuma ação da sua parte é necessária. Estamos a trabalhar diligentemente para restaurar a disponibilidade do serviço do Azure. Pode monitorizar o estado do serviço no [Dashboard de estado de funcionamento do serviço de Azure](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>Opção 2: Copiar dados a partir do secundário
Se escolheu [armazenamento georredundante com acesso de leitura (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (recomendado) para as suas contas de armazenamento, terá acesso de leitura aos seus dados da região secundária. Pode usar ferramentas como [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)e o [biblioteca de movimento de dados do Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) para copiar dados da região secundária para outra conta de armazenamento num região unimpacted e, em seguida, ponto seus aplicativos para que o armazenamento de conta para ambos leitura e escrita de disponibilidade.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>O que esperar se ocorre uma ativação pós-falha de armazenamento
Se escolheu [armazenamento georredundante (GRS)](storage-redundancy-grs.md) ou [armazenamento georredundante com acesso de leitura (RA-GRS)](storage-redundancy-grs.md#read-access-geo-redundant-storage) (recomendado), armazenamento do Azure irá manter os seus dados duráveis em duas regiões (primário e secundário). Em ambas as regiões, o armazenamento do Azure mantém constantemente várias réplicas dos seus dados.

Quando um desastre regional afeta a região primária, tentaremos primeiro restaurar o serviço nessa região para fornece a melhor combinação das RTO e RPO. Depende da natureza do desastre e impacto, em algumas ocasiões raras, poderá não conseguir restaurar a região primária. Nesse ponto, iremos efetuar uma ativação pós-falha geográfica. Replicação de dados entre regiões é um processo assíncrono que envolve um atraso, portanto, é possível que as alterações que ainda não tem sido replicadas para a região secundária podem ser perdidas.

Alguns pontos sobre a experiência de ativação pós-falha geográfica de armazenamento:

* Armazenamento geo-ativação pós-falha será acionada apenas pela equipa do armazenamento do Azure – não é necessária nenhuma ação do cliente. A ativação pós-falha é acionada quando a equipe de armazenamento do Azure esgotou todas as opções de restauração de dados na mesma região, que fornece a melhor combinação das RTO e RPO.
* Sua existente armazenamento pontos finais de serviço para blobs, tabelas, filas e ficheiros permanecem inalterados após a ativação pós-falha; a entrada DNS fornecida pelo Microsoft terá de ser atualizado para mudar da região primária para a região secundária. A Microsoft realiza essa atualização automaticamente como parte do processo de ativação pós-falha geográfica.
* Antes e durante a ativação pós-falha geográfica, não terá acesso de escrita para a sua conta de armazenamento devido ao impacto do desastre, mas pode continuar a ler da secundária se a sua conta de armazenamento tenha sido configurada como RA-GRS.
* Quando a ativação pós-falha geográfica foi concluída e as alterações DNS propagadas, leitura e de acesso de escrita para a sua conta de armazenamento são restaurados se tiver o GRS ou RA-GRS. O ponto final que tinha anteriormente o ponto final secundário se torna o ponto final primário. 
* Pode verificar o estado da localização primária e consulta a última hora de ativação pós-falha geográfica para a sua conta de armazenamento. Para obter mais informações, consulte [contas de armazenamento - obter propriedades](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/getproperties).
* Após a ativação pós-falha, a conta de armazenamento será totalmente esteja a funcionar, mas num estado "degradado", como ele está hospedada numa região autónoma não possível de georreplicação. Para mitigar este risco, iremos restaurar a região primária original e, em seguida, fazer uma reativação pós-falha geo para restaurar o estado original. Se a região primária original é irrecuperável, podemos irá alocar noutra região secundária.

## <a name="best-practices-for-protecting-your-data"></a>Melhores práticas para proteger os dados
Há algumas abordagens recomendadas para fazer backup dos dados de armazenamento em intervalos regulares.

* Utilizar discos de VM – a [serviço de cópia de segurança do Azure](https://azure.microsoft.com/services/backup/) para criar cópias de segurança os discos VM utilizados pelas máquinas virtuais do Azure.
* Os blobs de blocos – ative [eliminação de forma recuperável](../blobs/storage-blob-soft-delete.md) para proteger contra as eliminações ao nível do objeto e substitui ou copiar os blobs para outra conta de armazenamento na região outra que use [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), ou o [biblioteca de movimento de dados do Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Tabelas – utilizam [AzCopy](storage-use-azcopy.md) para exportar os dados da tabela para outra conta de armazenamento noutra região.
* Ficheiros – utilizam [AzCopy](storage-use-azcopy.md) ou [Azure PowerShell](storage-powershell-guide-full.md) para copiar os ficheiros para outra conta de armazenamento noutra região.

Para obter informações sobre a criação de aplicações que tiram partido da funcionalidade de RA-GRS, consulte [criação altamente disponíveis de aplicativos com o armazenamento RA-GRS](../storage-designing-ha-apps-with-ragrs.md)
