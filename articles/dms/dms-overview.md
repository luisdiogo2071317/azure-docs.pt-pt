---
title: Descrição geral de pré-visualização do serviço de migração de base de dados do Azure | Microsoft Docs
description: Descrição geral do serviço de migração de base de dados do Azure, que fornece migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.topic: article
ms.date: 04/25/2018
ms.openlocfilehash: b28ea5606e4fae849a2906b0d81a9ed07f265ebf
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="what-is-the-azure-database-migration-service-preview"></a>O que é a pré-visualização de serviço de migração de base de dados do Azure?
O serviço de migração de base de dados do Azure é um serviço completamente gerido concebido para permitir migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure com o período de indisponibilidade mínimo. O serviço está atualmente em pré-visualização pública, com esforços de desenvolvimento concentra-se em:

- Fiabilidade e desempenho.
- Iterativa adição de pares de destino de origem.
- Contínuo investimento em migrações friction-livre.

## <a name="use-familiar-tools"></a>Utilize ferramentas familiares
O serviço de migração de base de dados do Azure integra-se algumas das funcionalidades das nossas existente ferramentas e serviços. Fornece aos clientes com uma solução abrangente e altamente disponível. O serviço utiliza o [Assistente de migração de dados](http://aka.ms/dma) para gerar relatórios de avaliação que fornecem recomendações para ajudá-lo durante as alterações necessárias antes de efetuar uma migração. Está a funcionar para efetuar qualquer correção necessária. Quando estiver pronto para iniciar o processo de migração, o serviço de migração de base de dados do Azure efetua todos os passos associados. Pode acionados e se esquecer da sua projetos de migração com tranquilidade atenção saber que o processo tira partido das melhores práticas, conforme determinado pela Microsoft.

## <a name="regional-availability-during-public-preview"></a>Disponibilidade regional durante pré-visualização pública
A versão de pré-visualização pública do serviço de migração de base de dados do Azure está atualmente disponível nas seguintes regiões:
- EUA Leste
- EUA Central
- EUA Centro-Sul
- EUA Oeste
- Canadá Central
- Sul do Brasil
- Europa Ocidental
- Europa do Norte
- Sudeste Asiático
- Índia Ocidental

## <a name="next-steps"></a>Passos Seguintes
- [Criar uma instância do serviço de migração de base de dados do Azure utilizando o portal do Azure](quickstart-create-data-migration-service-portal.md).
- [Migrar o servidor SQL para a base de dados SQL do Azure](tutorial-sql-server-to-azure-sql.md).
- [Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure](pre-reqs.md).
- [FAQ sobre como utilizar o serviço de migração de base de dados do Azure](faq.md).
