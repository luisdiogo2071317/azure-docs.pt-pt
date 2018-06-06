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
ms.date: 06/04/2018
ms.openlocfilehash: 5cd829236d8d8a58e68f7bf766790aa3f0cb656e
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757421"
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Limitações na base de dados do Azure para PostgreSQL
As secções seguintes descrevem a capacidade e limites funcionais no serviço de base de dados.

## <a name="maximum-connections"></a>Número máximo de ligações
O número máximo de ligações por vCores e escalão de preço é os seguintes: 

|**Escalão de Preço**| **vCore(s)**| **Máx. ligações** |
|---|---|---|
|Básica| 1| 50 |
|Básica| 2| 100 |
|Fins Gerais| 2| 150|
|Fins Gerais| 4| 250|
|Fins Gerais| 8| 480|
|Fins Gerais| 16| 950|
|Fins Gerais| 32| 1500|
|Memória Otimizada| 2| 150|
|Memória Otimizada| 4| 250|
|Memória Otimizada| 8| 480|
|Memória Otimizada| 16| 950|

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
