---
title: Migração de período de indisponibilidade mínimo para a base de dados do Azure para PostgreSQL
description: Este artigo descreve como efetuar uma migração de período de indisponibilidade mínimo de uma base de dados do PostgreSQL à base de dados do Azure para PostgreSQL com o Azure Database Migration Service.
services: postgresql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: 80e5d30677735b35d90fda6288d7bf6f2ea4aa1b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093838"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Migração de período de indisponibilidade mínimo para a base de dados do Azure para PostgreSQL
Pode efetuar as migrações de PostgreSQL para o banco de dados do Azure para PostgreSQL com o período de indisponibilidade mínimo utilizando recém-lançado **capacidade de sincronização contínua** para o [Azure Database Migration Service](https://aka.ms/get-dms) (DMS) . Esta funcionalidade limita a quantidade de tempo de inatividade que é incorrido pela aplicação.

## <a name="overview"></a>Descrição geral
O DMS executa uma carga inicial de seu local para a base de dados do Azure para PostgreSQL e, em seguida, sincroniza continuamente nenhuma transação nova para o Azure enquanto o aplicativo permanece em execução. Depois de captura os dados de cópia de segurança no lado do Azure de destino, pare a aplicação por um breve momento (tempo de inatividade mínimo), espera até que o último lote de dados (a partir da hora de parar a aplicação até que o aplicativo é efetivamente indisponível para qualquer novo tráfego) para capturar o destino de cópia de segurança e, em seguida, atualize a cadeia de ligação para apontar para o Azure. Quando tiver terminado, a aplicação ficará em direto no Azure!

![Sincronia contínua com o serviço de migração de base de dados do Azure](./media/howto-migrate-online/ContinuousSync.png)

Migração de DMS das origens de PostgreSQL está atualmente em pré-visualização. Se quiser experimentar o serviço para migrar as cargas de trabalho do PostgreSQL, inscreva-se através do Azure DMS [página de pré-visualização](https://aka.ms/dms-preview) para expressar o seu interesse. Seus comentários são valiosos para ajudar a melhorar ainda mais o serviço.

## <a name="next-steps"></a>Passos Seguintes
- Ver o vídeo [modernização de aplicações com o Microsoft Azure](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), que contém uma demonstração de como migrar aplicações de PostgreSQL para base de dados do Azure para PostgreSQL.
- Veja o tutorial [PostgreSQL migrar a base de dados do Azure para PostgreSQL online com o DMS](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
