---
title: Limitações na base de dados do Azure para PostgreSQL
description: Este artigo descreve as limitações na base de dados do Azure para PostgreSQL, como o número de ligação e opções de motor de armazenamento.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/30/2018
ms.openlocfilehash: f24f15134bf189097f20f75ff0b23b72a3e48363
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51299611"
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Limitações na base de dados do Azure para PostgreSQL
As secções seguintes descrevem a capacidade e limites funcionais no serviço de base de dados.

## <a name="maximum-connections"></a>Número máximo de ligações
Seguem-se o número máximo de ligações por vCores e escalão de preço: 

|**Escalão de Preço**| **vCore(s)**| **Máx. ligações** |
|---|---|---|
|Básica| 1| 50 |
|Básica| 2| 100 |
|Fins Gerais| 2| 150|
|Fins Gerais| 4| 250|
|Fins Gerais| 8| 480|
|Fins Gerais| 16| 950|
|Fins Gerais| 32| 1500|
|Memória Otimizada| 2| 300|
|Memória Otimizada| 4| 500|
|Memória Otimizada| 8| 960|
|Memória Otimizada| 16| 1900|

Quando as ligações excederem o limite, poderá receber o erro seguinte:
> FATAL: Desculpe, muitos clientes já

O sistema do Azure requer cinco ligações para monitorizar a base de dados do Azure para o servidor PostgreSQL. 

## <a name="functional-limitations"></a>Limitações funcionais
### <a name="scale-operations"></a>Operações de dimensionamento
- Dimensionamento dinâmico de e para os escalões de preços básicos não é atualmente suportada.
- Diminuir o tamanho de armazenamento do servidor não é atualmente suportada.

### <a name="server-version-upgrades"></a>Atualização de versão do servidor
- Migração automatizada entre as versões do motor de base de dados principal não é atualmente suportada. Se gostaria de atualizar para a próxima versão principal, dê uma [cópia de segurança e restaurar](./howto-migrate-using-dump-and-restore.md) -lo para um servidor que foi criado com a nova versão do motor.

### <a name="vnet-service-endpoints"></a>Pontos finais de serviço de VNet
- Suporte para pontos finais de serviço da VNet é apenas para fins gerais e memória otimizada de servidores.

### <a name="restoring-a-server"></a>Restaurar um servidor
- Quando utilizar a funcionalidade PITR, é criado o novo servidor com as mesmas configurações de escalão de preço que o servidor que baseia-se.
- O novo servidor criado durante um restauro não tem as regras de firewall que existiam no servidor original. Regras de firewall tem de ser configurado separadamente para este servidor novo.
- Não é suportado restaurar um servidor foi eliminado.

## <a name="next-steps"></a>Passos Seguintes
- Compreender [o que está disponível em cada escalão de preço](concepts-pricing-tiers.md)
- Saiba mais sobre [base de dados PostgreSQL versões suportadas](concepts-supported-versions.md)
- Revisão [como fazer cópias de segurança e restaurar um servidor na base de dados do Azure para PostgreSQL com o portal do Azure](howto-restore-server-portal.md)
