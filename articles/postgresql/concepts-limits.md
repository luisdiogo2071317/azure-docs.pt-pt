---
title: Limitações na base de dados do Azure para PostgreSQL
description: Este artigo descreve limitações na base de dados do Azure para PostgreSQL, tais como o número de ligação e as opções do motor de armazenamento.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: 7e06cdba7c9c9f7e5c1d621e7421a18c342c0fdb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Limitações na base de dados do Azure para PostgreSQL
As secções seguintes descrevem a capacidade e limites funcionais no serviço de base de dados.

## <a name="pricing-tier-maximums"></a>Valores máximos de escalão de preço
Base de dados do Azure para PostgreSQL tem vários escalões de preços, que pode escolher durante a criação de um servidor. Para obter mais informações, consulte [escalões de preço na base de dados do Azure para PostgreSQL](concepts-pricing-tiers.md).  

Há um número máximo de ligações, unidades de computação e armazenamento em cada escalão de preço, da seguinte forma: 

|Escalão de Preço| Geração de computação| vCore(s)| Máx. ligações |
|---|---|---|---|
|Básica| Geração 4| 1| 50 |
|Básica| Geração 4| 2| 100 |
|Básica| Geração 5| 1| 50 |
|Básica| Geração 5| 2| 100 |
|Fins Gerais| Geração 4| 2| 150|
|Fins Gerais| Geração 4| 4| 250|
|Fins Gerais| Geração 4| 8| 480|
|Fins Gerais| Geração 4| 16| 950|
|Fins Gerais| Geração 4| 32| 1500|
|Fins Gerais| Geração 5| 2| 150|
|Fins Gerais| Geração 5| 4| 250|
|Fins Gerais| Geração 5| 8| 480|
|Fins Gerais| Geração 5| 16| 950|
|Fins Gerais| Geração 5| 32| 1500|
|Memória Otimizada| Geração 5| 2| 150|
|Memória Otimizada| Geração 5| 4| 250|
|Memória Otimizada| Geração 5| 8| 480|
|Memória Otimizada| Geração 5| 16| 950|

Quando as ligações excedem o limite, poderá receber o erro seguinte:
> FATAL: Lamentamos, mas já demasiados clientes

O sistema do Azure requer ligações de cinco para monitorizar a base de dados do Azure para o servidor de PostgreSQL. 

## <a name="functional-limitations"></a>Limitações funcionais
### <a name="scale-operations"></a>Operações de dimensionamento
1.  Dimensionamento dinâmico de servidores em escalões de preço não é atualmente suportada. Ou seja, alternar entre escalões básico, fins gerais ou com otimização de memória.
2.  Diminuir o tamanho de armazenamento do servidor não é atualmente suportada.

### <a name="server-version-upgrades"></a>Atualização de versão do servidor
- Automatizar a migração entre versões do motor de base de dados principal não é atualmente suportada.

### <a name="subscription-management"></a>Gestão de subscrições
- Mover dinamicamente servidores entre subscrições e grupos de recursos não é atualmente suportada.

### <a name="point-in-time-restore-pitr"></a>Ponto-na-tempo-restauro (PITR)
1.  Quando utilizar a funcionalidade PITR, é criado o novo servidor com as configurações que o servidor que se baseia no mesmas.
2.  Não é suportado restaurar um servidor eliminado.

## <a name="next-steps"></a>Passos Seguintes
- Compreender [que está disponível em cada escalão de preço](concepts-pricing-tiers.md)
- Saiba mais sobre [versões de base de dados PostgreSQL](concepts-supported-versions.md)
- Reveja [como fazer cópias de segurança e restaurar um servidor na base de dados do Azure para PostgreSQL no portal do Azure](howto-restore-server-portal.md)
