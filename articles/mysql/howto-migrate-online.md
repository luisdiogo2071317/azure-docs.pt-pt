---
title: Migração de período de indisponibilidade mínimo para a base de dados do Azure para MySQL
description: Este artigo descreve como efetuar uma migração de período de indisponibilidade mínimo de uma base de dados MySQL à base de dados do Azure para MySQL, utilizando o serviço de migração de base de dados do Azure.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: ecbd35bd45bd11292bbe4a032329d704858d4c77
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293926"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migração de período de indisponibilidade mínimo para a base de dados do Azure para MySQL
Pode efetuar migrações MySQL à base de dados do Azure para MySQL com o período de indisponibilidade mínimo utilizando recentemente introduzidas **capacidade de sincronização contínua** para o [serviço de migração de base de dados do Azure](https://aka.ms/get-dms) (DMS). Esta funcionalidade limita a quantidade de tempo de inatividade é incorrida pela aplicação.

## <a name="overview"></a>Descrição geral
O DMS executa uma carga inicial no local à base de dados do Azure para MySQL e, em seguida, sincronizar continuamente quaisquer novas transações para o Azure enquanto a aplicação permanece em execução. Depois de intercete os dados de cópia de segurança no Azure do lado do destino, pare a aplicação para um momento breve (período de indisponibilidade mínimo), aguarde que o último lote de dados (desde o momento em que pare a aplicação até que a aplicação é efetivamente indisponível para efetuar qualquer novo tráfego) para detetar configurar no destino e, em seguida, atualizar a cadeia de ligação para apontar para o Azure. Quando tiver terminado, a aplicação será em direto no Azure!

![Sincronização contínua com o serviço de migração de base de dados do Azure](./media/howto-migrate-online/ContinuousSync.png)

Migração de DMS das origens de MySQL está atualmente em pré-visualização. Se pretender experimentar o serviço para migrar as cargas de trabalho do MySQL, inscreva-se através do Azure o DMS [página pré-visualização](https://aka.ms/dms-preview) para expressar seu interesse. Os seus comentários são valiosas em ajudar a melhorar ainda mais o serviço.

## <a name="next-steps"></a>Passos Seguintes
- Veja o vídeo [migre facilmente MySQL/PostgreSQL aplicações para o Azure geridas serviço](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), que contém uma demonstração que mostra como migrar aplicações de MySQL à base de dados do Azure para MySQL.
- Inscrever-se para pré-visualização limitada de migrações de período de indisponibilidade mínimo de MySQL à base de dados do Azure para MySQL através do Azure o DMS [página pré-visualização](https://aka.ms/dms-preview).
