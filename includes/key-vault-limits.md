Transações da chave (transações máxima permitidas dentro de 10 segundos, por cofre por região<sup>1</sup>):

|Tipo de chave|HSM-Key<br>CRIAR a chave|Chave HSM<br>Todas as outras transações|Chave de software<br>CRIAR a chave|Chave de software<br>Todas as outras transações|
|:---|---:|---:|---:|---:|
|RSA 2048 bits|5|1000|10|2000|
|RSA 3072-bit|5|250|10|500|
|RSA 4096 bits|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

Segredos, chaves de conta de armazenamento gerida e transações do cofre:
| Tipo de transações | Transações de máxima permitidas dentro de 10 segundos, por cofre por região<sup>1</sup> |
| --- | --- |
| Todas as transações |2000 |
|

Consulte [Cofre de chaves do Azure limitação orientações](../articles/key-vault/key-vault-ovw-throttling.md) para obter informações sobre como lidar com limitação quando estes limites são excedidos.

<sup>1</sup> há um limite de toda a subscrição para todos os tipos de transação, o que é x 5 por limite de Cofre de chaves. Por exemplo, HSM - outras transações por subscrição estão limitadas a 5000 transações dentro de 10 segundos por subscrição.
