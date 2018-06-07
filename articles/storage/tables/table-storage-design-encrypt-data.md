---
title: Encriptar dados da tabela de armazenamento do Azure | Microsoft Docs
description: Saiba mais sobre a encriptação de dados de tabela no armazenamento do Azure.
services: storage
documentationcenter: na
author: MarkMcGeeAtAquent
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/11/2018
ms.author: sngun
ms.openlocfilehash: 082e8a54cc8625a4bbdea2157f73874dbc86fde2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661039"
---
# <a name="encrypt-table-data"></a>Encriptar dados da tabela
A biblioteca de clientes do Storage de Azure .NET suporta a encriptação das propriedades de entidade de cadeia para inserção e substitua operações. As cadeias de encriptados são armazenadas no serviço como propriedades binárias e estes são convertidos para cadeias depois de desencriptação.    

Para as tabelas, além da política de encriptação, os utilizadores tem de especificar as propriedades sejam encriptados. Pode fazê-lo especificando a um atributo [EncryptProperty] (para entidades POCO que derivem de TableEntity) ou um resolvedor de encriptação nas opções de pedido. Um resolvedor de encriptação é um delegado que utiliza uma chave de partição, chave de linha e nome de propriedade e devolve um valor boleano que indica se essa propriedade deve ser encriptada. Durante a encriptação, a biblioteca de cliente utiliza estas informações para decidir se pretende encriptar uma propriedade ao escrever a transmissão. Também fornece o delegado para a possibilidade de lógica em torno da forma como as propriedades são encriptadas. (Por exemplo, se X, então encriptar propriedade um; caso contrário, encriptar propriedades A e B.) Não é necessário fornecer estas informações ao ler ou consultar entidades.

## <a name="merge-support"></a>Suporte de intercalação

Intercalação não é atualmente suportada. Porque um subconjunto de propriedades pode ter sido encriptado anteriormente com uma chave diferente, basta intercalar as propriedades de novo e atualizar os metadados resulta numa perda de dados. Intercalar o requer efetuar chamadas de serviço adicional para a entidade pré-existente de leitura do serviço ou utilizar uma nova chave por propriedade, que não são adequadas por motivos de desempenho.     

Para obter informações sobre a encriptação de dados da tabela, consulte [encriptação do lado do cliente e o Cofre de chaves do Azure para armazenamento do Microsoft Azure](../common/storage-client-side-encryption.md).  

## <a name="next-steps"></a>Passos Seguintes

- [Padrões de conceção da tabela](table-storage-design-patterns.md)
- [Relações de modelação](table-storage-design-modeling.md)
- [Relações de modelação](table-storage-design-modeling.md)
- [Estrutura de modificação de dados](table-storage-design-for-modification.md)