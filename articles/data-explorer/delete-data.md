---
title: Eliminar dados a partir do Explorador de dados do Azure
description: Este artigo descreve os cenários de eliminação em massa no Azure explorar dados, incluindo a remoção e eliminações de retenção com base.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 571a005dd3f50690f291a7ffa3c1174ea15cb0ed
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048045"
---
# <a name="delete-data-from-azure-data-explorer"></a>Eliminar dados a partir do Explorador de dados do Azure

O Explorador de dados do Azure suporta várias abordagens de eliminação em massa, que abordamos neste artigo. Ele não dá suporte por registo eliminação em tempo real, porque está otimizado para rápido acesso de leitura.

* Se a base de dados já não for necessário, elimine-o com o comando de banco de dados de lista.

    ```Kusto
    .drop database <DatabaseName>
    ```

* Se já não necessitar de uma ou mais tabelas, eliminá-los através da tabela de lista ou remover o comando de tabelas.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Se os dados antigos já não for necessário, elimine-o ao alterar o período de retenção ao nível da base de dados ou tabela.

    Considere uma base de dados ou tabela que está definida para 90 dias de retenção. Negócios tem de alterar, agora é necessário apenas de 60 dias de dados. Neste caso, elimine os dados mais antigos em uma das seguintes formas.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Para obter mais informações, consulte [política de retenção](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

* Pode eliminar registos individuais utilizando o *limpar* operação, com base num tipo de predicado `where CustomerName == 'contoso'`. Dito isso, uma remoção é uma eliminação em massa que não é projetada para eliminação em tempo real. O exemplo seguinte mostra uma limpeza.

    ```Kusto
    .purge table Customer records
    | where CustomerName =='contoso'
    ```

Se precisar de assistência com problemas de eliminação de dados, abra um pedido de suporte no [portal do Azure](https://portal.azure.com).