---
title: Avaliar as opções para migrar os dados do StorSimple 5000-7000 série | Documentos da Microsoft
description: Fornece uma descrição geral das opções para migrar dados de séries StorSimple 5000 a 7000.
services: storsimple
documentationcenter: NA
author: alkohli
manager: twooley
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2018
ms.author: alkohli
ms.openlocfilehash: a1cdf3235674ae651c30fa4f13622b80212abc7d
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "40099938"
---
# <a name="options-to-migrate-data-from-storsimple-5000-7000-series"></a>Opções de migração de dados do StorSimple 5000-7000 série 
StorSimple 5000-7000 série está a atingir [fim do suporte](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) em Julho de 2019. Os clientes que tenham o StorSimple 5000-7000 série tem uma opção para atualizar para o outro Azure primeiro os serviços híbridos de terceiros. Este artigo descreve as opções de híbrido do Azure disponíveis para migrar os dados. 

## <a name="migration-options"></a>Opções de migração

Os clientes utilizam o StorSimple 5000-7000 série têm as seguintes duas opções principais:

- **Atualizar para o StorSimple série 8000** – atualizar para o StorSimple 8000 série e, assim, continuar com a plataforma do StorSimple.  Este caminho de atualização exige que os clientes substituir os seus dispositivos das séries 5000 a 7000 com uma série 8000. Os dados são migrados do dispositivo de série 5000-7000 utilizando a ferramenta de migração. Depois da migração foi concluída com êxito, os dispositivos da série StorSimple 8000 irão continuar a dados de camada de armazenamento de Blobs do Azure. 

    Para obter mais informações sobre como migrar dados usando uma série StorSimple 8000, aceda a [migrar dados do StorSimple 5000-7000 série para o dispositivo da 8000 série](storsimple-8000-migrate-from-5000-7000.md).

- **Migrar para o Azure File Sync** – neste totalmente nova opção de migração permite aos clientes armazenar as partilhas de ficheiros da sua organização nos ficheiros do Azure. Estas partilhas de ficheiros, em seguida, estão centralizadas para acesso no local com o Azure File Sync (AFS). AFS pode ser implementada num anfitrião Windows Server. A migração de dados real, em seguida, é executada como um anfitrião cópia ou a ferramenta de migração.

    Para obter mais informações sobre como migrar dados para sincronização de ficheiros do Azure, aceda a [migrar dados do StorSimple 5000-7000 série para o Azure File Sync](https://aka.ms/StorSimpleMigrationAFS).

## <a name="migration---frequently-asked-questions"></a>Migração - perguntas mais frequentes

### <a name="q-when-do-the-storsimple-5000-and-7000-series-devices-reach-end-of-service"></a>P. Ao o StorSimple 5000 e os dispositivos das 7000 séries alcançar final do serviço? 

A. StorSimple 5000-7000 série alcançar [final do serviço](https://support.microsoft.com/lifecycle/search?alpha=StorSimple%205000%2F7000%20Series) em Julho de 2019. O final do serviço implica que Microsoft já não será capaz de fornecer suporte para hardware e software destes dispositivos depois de Julho de 2019. É altamente recomendável que começar a formular um plano para migrar os dados dos seus dispositivos agora.

### <a name="q-what-happens-to-the-data-i-have-stored-in-azure"></a>P. O que acontece com os dados que tem armazenados no Azure?  

A. Pode continuar a utilizar os dados no Azure, uma vez que fizer a migração para um serviço mais recente. 


### <a name="q--what-happens-to-the-data-i-have-stored-locally-on-my-storsimple-device"></a>P.  O que acontece com os dados que tem armazenados localmente no meu dispositivo do StorSimple? 

A. Os dados no dispositivo local podem ser copiados para o serviço mais recente, conforme descrito nos documentos de migração.

### <a name="what-happens-if-i-want-to-keep-my-storsimple-50007000-series-appliance"></a>O que acontece se eu quiser manter a minha aplicação de série StorSimple 5000/7000? 

A. Enquanto os serviços poderão continuar a funcionar, a Microsoft já não será capaz de fornecer suporte de hardware e software. Migração é vivamente recomendada para continuidade do negócio.

### <a name="q-what-options-are-available-to-migrate-data-from-storsimple-5000-7000-series-devices"></a>P. Que opções estão disponíveis para migrar os dados do StorSimple dispositivos das séries 5000 a 7000? 

A. Dependendo do seu cenário, os utilizadores de séries StorSimple 5000 a 7000 tem as seguintes opções de migração. 

 - **Atualizar para a 8000 série**: Utilize esta opção se pretende continuar na plataforma do StorSimple. 
 - **Migrar para o Azure File Sync**: Utilize esta opção se desejar alternar para o formato nativo do Azure. Pode utilizar o Azure File Sync para o gerenciamento centralizado de partilhas de ficheiros. 

Pode contactar o Support da Microsoft para discutir as opções de migração não listadas aqui.

### <a name="q-is-migration-to-other-storage-solutions-supported"></a>P. É a migração para outras soluções de armazenamento suportadas?

A. Sim. Migração para outras soluções de armazenamento através da cópia de anfitrião dos dados é suportada.

### <a name="q-is-migration-supported-by-microsoft"></a>P. A migração é suportada pela Microsoft? 

A. Migrar de série de 5000 ou 7000 é uma operação totalmente suportada. Na verdade, a Microsoft recomenda Contatando os de suporte antes de começar a migração. A migração é atualmente uma operação assistida. Se pretende migrar dados do StorSimple de dispositivo da série 5000-7000 [abra um pedido de suporte](storsimple-8000-contact-microsoft-support.md).

### <a name="q-how-does-the-cost-compare-for-the-two-listed-migrations-to-azure-hybrid-services"></a>P. Como o compara o custo para duas migrações diretas para os serviços híbridos do Azure que se listados? 

A. Custo de migração varia consoante a opção que escolher. Enquanto a migração em si é gratuita, se optar por atualizar para uma série StorSimple 8000, haverá o custo do dispositivo de hardware. Da mesma forma, ao utilizar o Azure File Sync, podem ser aplicadas taxas de subscrição para o serviço. Em cada caso, os clientes também terão de pagar os custos de armazenamento em curso. Consulte a [Calculadora de preços da Microsoft para os respetivos serviços](https://azure.microsoft.com/pricing/#product-picker) para uma estimativa.  

### <a name="q--how-long-does-it-take-to-complete-a-migration"></a>P.  Quanto tempo demora a concluir uma migração?

A. O tempo para migrar os dados depende da quantidade de dados e a opção de atualização selecionado. 

## <a name="next-steps"></a>Passos Seguintes
 - [Migrar dados do StorSimple 5000-7000 série para um dispositivo da 8000 série](storsimple-8000-migrate-from-5000-7000.md).
 - [Migrar dados de uma série de 5000 a 7000 do StorSimple para o Azure File Sync](storsimple-5000-7000-afs-migration.md)
