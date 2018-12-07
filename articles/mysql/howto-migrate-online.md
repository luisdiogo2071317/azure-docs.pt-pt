---
title: Migração de período de indisponibilidade mínimo para a base de dados do Azure para MySQL
description: Este artigo descreve como efetuar uma migração de período de indisponibilidade mínimo de uma base de dados do MySQL para a base de dados do Azure para MySQL com o Azure Database Migration Service.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 267212d8f832b96bf66145b97c3464471e28593d
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014196"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migração de período de indisponibilidade mínimo para a base de dados do Azure para MySQL
Pode efetuar as migrações de MySQL para a base de dados do Azure para MySQL com o período de indisponibilidade mínimo utilizando recém-lançado **capacidade de sincronização contínua** para o [Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Esta funcionalidade limita a quantidade de tempo de inatividade que é incorrido pela aplicação.

## <a name="overview"></a>Descrição geral
O Azure DMS executa uma carga inicial de seu local para a base de dados do Azure para MySQL e, em seguida, sincroniza continuamente nenhuma transação nova para o Azure enquanto o aplicativo permanece em execução. Depois de captura os dados de cópia de segurança no lado do Azure de destino, pare a aplicação por um breve momento (tempo de inatividade mínimo), espera até que o último lote de dados (a partir da hora de parar a aplicação até que o aplicativo é efetivamente indisponível para qualquer novo tráfego) para capturar o destino de cópia de segurança e, em seguida, atualize a cadeia de ligação para apontar para o Azure. Quando tiver terminado, a aplicação ficará em direto no Azure!

![Sincronia contínua com o serviço de migração de base de dados do Azure](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Passos Seguintes
- Ver o vídeo [MySQL/PostgreSQL de migrar facilmente aplicações para o Azure managed service](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), que contém uma demonstração de como migrar aplicações do MySQL para base de dados do Azure para MySQL.
- Veja o tutorial [MySQL migrar a base de dados do Azure para MySQL online com o DMS](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
