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
ms.openlocfilehash: efbd4f227880875c11e2c43c84716dfc49c5717d
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Migração de período de indisponibilidade mínimo para a base de dados do Azure para PostgreSQL
Pode migrar a base de dados PostgreSQL existente à base de dados do Azure para PostgreSQL utilizando Attunity replicar para Migrations da Microsoft. Replicar Attunity é uma oferta conjunta Attunity e Microsoft. Juntamente com o serviço de migração de base de dados do Azure, é incluída sem custos adicionais para os clientes da Microsoft. 

Replicar Attunity ajuda a minimizar o período de indisponibilidade durante as migrações de base de dados e mantém a base de dados de origem operacional em todo o processo.

Replicar Attunity é uma ferramenta de replicação de dados que permite a sincronização de dados entre uma variedade de origens e destinos. Esta propaga o script de criação de esquema e dados associados a cada tabela de base de dados. Replicar Attunity não propagar quaisquer outros objetos (por exemplo, SP, acionadores, funções e assim sucessivamente) ou converter, por exemplo, o código de LP/SQL que esteja alojado nesses artefactos para T-SQL.

> [!NOTE]
> Embora Attunity replicar suporta um conjunto abrangente de cenários de migração, concentra-se no suporte para um subconjunto específico de pares de origem/destino.

Uma descrição geral do processo para efetuar uma migração de período de indisponibilidade mínimo inclui:

* **Migrar o esquema de origem PostgreSQL** para uma base de dados do Azure para a base de dados PostgreSQL utilizando o [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) comando com o parâmetro - n e, em seguida, utilizar o [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) comando.

* **Configurar a carga inicial e a sincronização de dados contínua da base de dados de origem para a base de dados de destino** utilizando Attunity replicar para Migrations da Microsoft. Se o fizer, minimiza o tempo que a base de dados de origem tem de ser definida como só de leitura ao preparar-se para mudar as suas aplicações para a base de dados do destino PostgreSQL no Azure.

Para obter mais informações sobre a replicar Attunity para Migrations Microsoft oferta, consulte os seguintes recursos:
 - Vá para o [Attunity replicar para Microsoft Migrations](https://aka.ms/attunity-replicate) página Web.
 - Transferir [Attunity replicar para migrações Microsoft](http://discover.attunity.com/download-replicate-microsoft-lp6657.html).
 - Vá para o [Attunity replicar Comunidade](https://aka.ms/attunity-community/) para um guia rápido, tutoriais e suporte.
 - Para obter orientações passo a passo sobre como utilizar Attunity replicar para migrar de PostgreSQL à base de dados do Azure para PostgreSQL, consulte o [guia de migração de base de dados](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).