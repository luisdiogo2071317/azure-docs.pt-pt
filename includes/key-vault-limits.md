---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: efa367157a8fd896cdc9680bf2ab6ba6a9e3dbb0
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430022"
---
Transações da chave (transações máx. permitidas dentro de 10 segundos, por cofre por região<sup>1</sup>):

|Tipo de chave|HSM-key<br>Criar chave|HSM-key<br>Todas as outras transações|Chave de software<br>Criar chave|Chave de software<br>Todas as outras transações|
|:---|---:|---:|---:|---:|
|RSA de 2048 bits|5|1000|10|2000|
|RSA de 3072 bits|5|250|10|500|
|RSA 4096 bits|5|125|10|250|
|P-256 ECC|5|1000|10|2000|
|P-384 ECC|5|1000|10|2000|
|P-521 ECC|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

> [!NOTE]
> Se examinar a tabela a seguir, verá que para as chaves de software de segurança, permitimos permitimos 1000 transações por 10 segundos de chaves de segurança de transações de 2000 por 10 segundos e por HSM. A proporção de transações de software de segurança de mensagens em fila para 3072 chaves para chaves de 2048 é 500/2000 ou 0.4. Isso significa que, se um cliente faz 500 3072 chave transações dentro de 10 segundos, atingem o seu limite máximo e não é possível fazer outras operações de chaves. 
   
|Tipo de chave  | Chave de software |HSM-key  |
|---------|---------|---------|
|RSA de 2048 bits     |    2000     |   1000    |
|RSA de 3072 bits     |     500    |    250     |
|RSA 4096 bits     |    125     |    250     |
|P-256 ECC     |    2000     |  1000     |
|P-384 ECC     |    2000     |  1000     |
|P-521 ECC     |    2000     |  1000     |
|ECC SECP256K1     |    2000     |  1000     |


Segredos, chaves de conta de armazenamento gerida e transações de cofre:
| Tipo de transações | Transações de máx. permitidas dentro de 10 segundos, por cofre por região<sup>1</sup> |
| --- | --- |
| Todas as transações |2000 |
|

Ver [orientações para a limitação do Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md) para obter informações sobre como resolver problemas de limitação quando estes limites são ultrapassados.

<sup>1</sup> existe um limite de toda a subscrição para todos os tipos de transação, o que é de 5 vezes por limite de Cofre de chaves. Por exemplo, HSM - outras transações por subscrição estão limitadas a 5000 transações dentro de 10 segundos por subscrição.
