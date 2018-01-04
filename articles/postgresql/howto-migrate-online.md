---
title: "Migração de período de indisponibilidade mínimo para a base de dados do Azure para PostgreSQL | Microsoft Docs"
description: "Este artigo descreve como efetuar uma migração de período de indisponibilidade mínimo extrair uma base de dados PostgreSQL para um ficheiro de informação, restaurar a base de dados PostgreSQL a partir de um ficheiro de arquivo criado pelo pg_dump na base de dados do Azure para PostgreSQL e configurar a carga inicial e sincronização de dados contínuos da base de dados de origem para a base de dados de destino utilizando Attunity replicar para Migrations da Microsoft."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 8c98c58042e7f1d1726eaad6ee03d1531b6c910e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Migração de período de indisponibilidade mínimo para a base de dados do Azure para PostgreSQL
É possível migrar a base de dados PostgreSQL existente à base de dados do Azure para PostgreSQL utilizando Attunity replicar para Microsoft Migrations, uma oferta conjuntamente patrocinada, conjunta da Attunity e da Microsoft que é fornecida juntamente com o serviço de migração de base de dados do Azure sem custos adicionais para os clientes da Microsoft. Replicar Attunity para Microsoft Migrations também permite que o período de indisponibilidade mínimo migração da base de dados e a base de dados de origem continua a ser operacional durante o processo de migração.

Replicar Attunity é uma ferramenta de replicação de dados que permite a sincronização de dados entre uma variedade de origens e destinos, propagar o script de criação de esquema e dados associados a cada tabela de base de dados. Replicar Attunity não propagar quaisquer outros objetos (por exemplo, SP, acionadores, funções, etc.) ou converter, por exemplo, PL/SQL. código alojado nesses artefactos, com T-SQL.

> [!NOTE]
> Enquanto Attunity replicar suporta um conjunto abrangente de cenários de migração, Attunity replicar para Microsoft Migrations concentra-se no suporte para um subconjunto específico de pares de origem/destino.

Uma descrição geral do processo para efetuar uma migração de período de indisponibilidade mínimo inclui:

1. **Migrar o esquema de origem PostgreSQL** para uma base de dados do Azure para a base de dados PostgreSQL utilizando o [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) comando com o parâmetro - n e, em seguida, utilizar o [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) comando.

2. **Configurar a carga inicial e a sincronização de dados contínua da base de dados de origem para a base de dados de destino** utilizando Attunity replicar para Migrations da Microsoft. Se o fizer, minimiza o tempo que a base de dados de origem tem de ser definida como só de leitura ao preparar-se para mudar as suas aplicações para a base de dados do destino PostgreSQL no Azure.

Para obter mais informações sobre a replicar Attunity oferta Migrations Microsoft, veja os seguintes recursos:
 - A replicar Attunity para Microsoft Migrations [página web](https://aka.ms/attunity-replicate).
 - [Transferir](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) Attunity replicar para migrações de Microsoft.
 - A replicar Attunity [Comunidade](https://microsoft.attunity.com/), com um guia rápido, tutoriais e suporte.
 - Para obter orientações passo a passo sobre como utilizar Attunity para migrar de PostgreSQL à base de dados do Azure para PostgreSQL, consulte o [guia de migração de base de dados](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).