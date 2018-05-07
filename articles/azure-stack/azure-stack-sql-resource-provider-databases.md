---
title: Utilizar bases de dados fornecidas pelo RP de adaptador de SQL na pilha do Azure | Microsoft Docs
description: Como criar e gerir bases de dados SQL aprovisionados utilizando o fornecedor de recursos do adaptador de SQL
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 2808847642639069e60102b195ac97957c8593f0
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="create-sql-databases"></a>Criar bases de dados SQL
Bases de dados self-service são fornecidos através da experiência do portal de utilizador. O utilizador precisa de uma subscrição que tenha uma oferta, que contém o serviço de base de dados.

1. Iniciar sessão para o [Azure pilha](azure-stack-poc.md) portal de utilizador (administradores de serviço também podem utilizar o portal de administração).

2. Clique em **+ nova** &gt; **dados + armazenamento "** &gt; **base de dados do SQL Server** &gt; **adicionar**.

3. Preencha o formulário com detalhes de base de dados, incluindo um **nome de base de dados**, **tamanho máximo**e alterar os outros parâmetros conforme necessário. É-lhe pedido para escolher um SKU para a base de dados. Servidores de alojamento são adicionadas, foram atribuídos um SKU. Bases de dados são criadas nesse agrupamento de servidores que compõem o SKU de alojamento.

  ![Nova base de dados](./media/azure-stack-sql-rp-deploy/newsqldb.png)

  >[!NOTE]
  > O tamanho de base de dados tem de ser, pelo menos, a 64 MB. Podem ser aumentado com as definições.

4. Preencha as definições de início de sessão: **início de sessão da base de dados**, e **palavra-passe**. Estas definições estão a credencial de autenticação do SQL Server que é criada para o acesso ao apenas esta base de dados. O nome de utilizador de início de sessão tem de ser globalmente exclusivo. Crie uma nova definição de início de sessão ou selecione um existente. Pode reutilizar definições de início de sessão para outras bases de dados utilizando o SKU do mesmo.

    ![Criar um novo início de sessão de base de dados](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Submeter o formulário e aguarde a conclusão da implementação.

    No painel resultante, tenha em atenção o campo de "Cadeia de ligação". Pode utilizar essa cadeia em qualquer aplicação que necessita de acesso do SQL Server (por exemplo, uma aplicação web) na sua pilha do Azure.

    ![Obter a cadeia de ligação](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-alwayson-databases"></a>Eliminar as bases de dados do AlwaysOn de SQL
Quando uma base de dados do AlwaysOn de SQL é eliminado do fornecedor de recursos, este é eliminada com êxito da primário e de Disponibilidade AlwaysOn grupo, mas por conceção, SQL Server AG coloca a base de dados em estado de cada réplica de restauro e não remover a base de dados, a menos que acionado. Se uma base de dados não for removido, as réplicas secundárias vai para sincronizar não Estado. Adicionar uma nova base de dados novamente para o AG com o mesmo através de RP ainda funciona.

## <a name="verify-sql-alwayson-databases"></a>Certifique-se de bases de dados do AlwaysOn de SQL
Bases de dados do AlwaysOn devem aparecer como sincronizadas e estão disponíveis em todas as instâncias e no grupo de disponibilidade. Após a ativação pós-falha, a base de dados deve ligar de forma totalmente integrada. Pode utilizar o SQL Server Management Studio para verificar que uma base de dados está a sincronizar:

![Verifique se AlwaysOn](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)


## <a name="next-steps"></a>Passos Seguintes

[Manter o fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-maintain.md)
