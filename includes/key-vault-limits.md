---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: ed0c387f9785336fbf18b3fd3c0cd9a7b09df633
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279862"
---
Transações da chave (transações máx. permitidas dentro de 10 segundos, por cofre por região<sup>1</sup>):

|Tipo de chave|Chave de HSM<br>Criar chave|HSM-key<br>Todas as outras transações|Chave de software<br>Criar chave|Chave de software<br>Todas as outras transações|
|:---|---:|---:|---:|---:|
|RSA de 2048 bits|5|1000|10|2000|
|RSA de 3072 bits|5|250|10|500|
|RSA 4096 bits|5|125|10|250|
|P-256 ECC|5|1000|10|2000|
|P-384 ECC|5|1000|10|2000|
|P-521 ECC|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

Segredos, chaves de conta de armazenamento gerida e transações de cofre:
| Tipo de transações | Transações de máx. permitidas dentro de 10 segundos, por cofre por região<sup>1</sup> |
| --- | --- |
| Todas as transações |2000 |
|

Ver [orientações para a limitação do Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md) para obter informações sobre como resolver problemas de limitação quando estes limites são ultrapassados.

<sup>1</sup> existe um limite de toda a subscrição para todos os tipos de transação, o que é de 5 vezes por limite de Cofre de chaves. Por exemplo, HSM - outras transações por subscrição estão limitadas a 5000 transações dentro de 10 segundos por subscrição.
