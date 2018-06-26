---
title: MySQL alojar servidores na pilha do Azure | Microsoft Docs
description: Como adicionar instâncias MySQL para aprovisionamento através do fornecedor de recursos do adaptador de MySQL
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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 5522eb1b8b0398aeb6f1b0dd8578b906880b4e89
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938454"
---
# <a name="add-hosting-servers-for-the-mysql-resource-provider"></a>Adicionar servidores de alojamento para o fornecedor de recursos de MySQL

Pode alojar uma instância de MySQL numa máquina virtual (VM) no [Azure pilha](azure-stack-poc.md), ou numa VM fora do seu ambiente de pilha do Azure, desde que o fornecedor de recursos de MySQL pode ligar à instância.

## <a name="connect-to-a-mysql-hosting-server"></a>Ligar a um servidor de alojamento MySQL

Certifique-se de que tem as credenciais para uma conta com privilégios de administrador de sistema. Para adicionar um servidor de alojamento, siga estes passos:

1. Inicie sessão no portal do operador de pilha do Azure como um administrador de serviço.
2. Selecione **mais serviços**.
3. Selecione **recursos administrativos** > **MySQL servidores de alojamento** > **+ adicionar**. Esta ação abre o **adicionar um servidor de alojamento de MySQL** caixa de diálogo, mostrada na captura de ecrã seguinte.

   ![Configurar um servidor de alojamento](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

4. Forneça os detalhes de ligação da sua instância do servidor de MySQL.

   * Para **nome do servidor de alojamento MySQL**, forneça o nome de domínio completamente qualificado (FQDN) ou um endereço IPv4 válido. Não utilize o nome abreviado de VM.
   * Uma instância de MySQL predefinida não foi fornecida, pelo que terá de especificar o **tamanho de alojamento de servidor em GB**. Introduza um tamanho próximo da capacidade do servidor da base de dados.
   * Mantenha a predefinição para **subscrição**.
   * Para **grupo de recursos**, crie uma nova ou utilize um grupo existente.

   > [!NOTE]
   > Se a instância de MySQL pode ser acedida por inquilino e o administrador do Azure Resource Manager, pode colocá-la sob o controlo do fornecedor de recursos. No entanto, a instância de MySQL **tem** atribuída exclusivamente para o fornecedor de recursos.

5. Selecione **SKUs** para abrir o **criar SKU** caixa de diálogo.

   ![Criar um SKU de MySQL](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)

   O SKU **nome** deve refletir as propriedades do SKU, para que os utilizadores podem implementar as bases de dados a SKU adequado.

   >[!IMPORTANT]
   >Os carateres especiais, incluindo espaços e períodos, não são suportados no **nome** ou **camada** quando cria um SKU para o fornecedor de recursos de MySQL.

6. Selecione **OK** para criar o SKU.
7. Em **adicionar um servidor de alojamento de MySQL**, selecione **criar**.

Como adicionar servidores, atribua-lhes para um SKU de novo ou existente para diferenciar as ofertas de serviços. Por exemplo, pode ter uma instância de enterprise MySQL que fornece a base de dados maior e cópias de segurança automáticas. Pode reservar este servidor de elevado desempenho para diversos departamentos da sua organização.

## <a name="increase-backend-database-capacity"></a>Aumentar a capacidade de base de dados de back-end

Pode aumentar a capacidade de base de dados de back-end ao implementar mais servidores MySQL no portal do Azure pilha. Adicione estes servidores para um SKU de novo ou existente. Se adicionar um servidor para um SKU existente, certifique-se de que as características de servidor são os mesmos que os outros servidores no SKU.

## <a name="make-mysql-database-servers-available-to-your-users"></a>Que servidores de base de dados MySQL fique disponível para os seus utilizadores

Crie planos e as ofertas para que os servidores de base de dados MySQL disponíveis aos utilizadores. Adicione o serviço de Microsoft.MySqlAdapter para o plano e, em seguida, adicione a predefinição de Quota ou criar uma Quota de novo.

![Criar planos e as ofertas para bases de dados](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="next-steps"></a>Passos Seguintes

[Criar uma base de dados MySQL](azure-stack-mysql-resource-provider-databases.md)