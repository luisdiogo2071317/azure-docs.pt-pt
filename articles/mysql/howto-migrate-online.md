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
ms.openlocfilehash: d23628fd8446f6e7e0e5ed14b98da13c09b2d592
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migração de período de indisponibilidade mínimo para a base de dados do Azure para MySQL
Pode migrar a base de dados MySQL existente à base de dados do Azure para MySQL utilizando Attunity replicar para Migrations da Microsoft. Replicar Attunity é uma oferta conjunta Attunity e Microsoft. Juntamente com o serviço de migração de base de dados do Azure, é incluída sem custos adicionais para os clientes da Microsoft. 

Replicar Attunity ajuda a minimizar o período de indisponibilidade durante as migrações de base de dados e mantém a base de dados de origem operacional em todo o processo.

Replicar Attunity é uma ferramenta de replicação de dados que permite a sincronização de dados entre uma variedade de origens e destinos. Esta propaga o script de criação de esquema e dados associados a cada tabela de base de dados. Replicar Attunity não propagar quaisquer outros objetos (por exemplo, SP, acionadores, funções e assim sucessivamente) ou converter, por exemplo, o código de LP/SQL que esteja alojado nesses artefactos para T-SQL.

> [!NOTE]
> Embora Attunity replicar suporta um conjunto abrangente de cenários de migração, concentra-se no suporte para um subconjunto específico de pares de origem/destino.

Uma descrição geral do processo para efetuar uma migração de período de indisponibilidade mínimo inclui:

* **Migrar o esquema de origem MySQL** para uma base de dados do Azure para MySQL geridos serviço base de dados com [MySQL Workbench](https://www.mysql.com/products/workbench/).

* **Configurar a carga inicial e a sincronização de dados contínua da base de dados de origem para a base de dados de destino** utilizando Attunity replicar para Migrations da Microsoft. Se o fizer, minimiza o tempo que a base de dados de origem tem de ser definida como só de leitura ao preparar-se para mudar as suas aplicações para a base de dados do destino MySQL no Azure.

Para obter mais informações sobre a replicar Attunity para Migrations Microsoft oferta, consulte os seguintes recursos:
 - Vá para o [Attunity replicar para Microsoft Migrations](https://aka.ms/attunity-replicate) página Web.
 - Transferir [Attunity replicar para migrações Microsoft](http://discover.attunity.com/download-replicate-microsoft-lp6657.html).
 - Vá para o [Attunity replicar Comunidade](https://aka.ms/attunity-community) para um guia rápido, tutoriais e suporte.
 - Para obter orientações passo a passo sobre como utilizar Attunity replicar para migrar a base de dados MySQL à base de dados do Azure para MySQL, consulte o [guia de migração de base de dados](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).