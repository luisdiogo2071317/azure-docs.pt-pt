---
title: "Migração de período de indisponibilidade mínimo para a base de dados do Azure para MySQL | Microsoft Docs"
description: "Este artigo descreve como efetuar uma migração de período de indisponibilidade mínimo de uma base de dados MySQL à base de dados do Azure para o MySQL e como configurar carga inicial e a sincronização de dados contínua da base de dados de origem para a base de dados de destino utilizando Attunity replicar para a Microsoft Migrações."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 429f5c9f939a802184a6513a63fb9115abf4b235
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migração de período de indisponibilidade mínimo para a base de dados do Azure para MySQL
É possível migrar a base de dados MySQL existente à base de dados do Azure para MySQL utilizando Attunity replicar para Microsoft Migrations, uma oferta conjuntamente patrocinada, conjunta da Attunity e da Microsoft que é fornecida juntamente com o serviço de migração de base de dados do Azure em não custos adicionais para os clientes da Microsoft. Replicar Attunity para Microsoft Migrations também permite que o período de indisponibilidade mínimo migração da base de dados e a base de dados de origem continua a ser operacional durante o processo de migração.

Replicar Attunity é uma ferramenta de replicação de dados que permite a sincronização de dados entre uma variedade de origens e destinos, propagar o script de criação de esquema e dados associados a cada tabela de base de dados. Replicar Attunity não propagar quaisquer outros objetos (por exemplo, SP, acionadores, funções, etc.) ou converter, por exemplo, PL/SQL. código alojado nesses artefactos, com T-SQL.

> [!NOTE]
> Enquanto Attunity replicar suporta um conjunto abrangente de cenários de migração, Attunity replicar para Microsoft Migrations concentra-se no suporte para um subconjunto específico de pares de origem/destino.

Uma descrição geral do processo para efetuar uma migração de período de indisponibilidade mínimo inclui:

1. **Migrar o esquema de origem MySQL** para uma base de dados do Azure para a base de dados MySQL, utilizando o [MySQL Workbench](https://www.mysql.com/products/workbench/).

2. **Configurar a carga inicial e a sincronização de dados contínua da base de dados de origem para a base de dados de destino** utilizando Attunity replicar para Migrations da Microsoft. Se o fizer, minimiza o tempo que a base de dados de origem tem de ser definida como só de leitura ao preparar-se para mudar as suas aplicações para a base de dados do destino MySQL no Azure.

Para obter mais informações sobre a replicar Attunity oferta Migrations Microsoft, veja os seguintes recursos:
 - A replicar Attunity para Microsoft Migrations [página web](https://aka.ms/attunity-replicate).
 - [Transferir](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Attunity replicar para migrações de Microsoft.
 - A replicar Attunity [Comunidade](https://microsoft.attunity.com/), com um guia rápido, tutoriais e suporte.
 - Para obter orientações passo a passo sobre como utilizar Attunity para migrar de MySQL à base de dados do Azure para MySQL, consulte o [guia de migração de base de dados](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).