---
title: 'Portal do Azure: Criar uma instância gerida de SQL | Documentos da Microsoft'
description: Crie uma Instância Gerida do SQL, um ambiente de rede e uma VM cliente para acesso.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: Carlrab
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: 201ba431a4382741815536db2bb4d08f0068be80
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54329546"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Início rápido: Criar uma Instância Gerida de Base de Dados SQL do Azure

Este guia de início rápido orienta-o ao longo da criação de uma [Instância Gerida](sql-database-managed-instance.md) de Base de Dados SQL do Azure no portal do Azure.

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-managed-instance"></a>Criar uma Instância Gerida

Os passos seguintes mostram como criar uma Instância Gerida.

1. Selecione **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize **Instância Gerida** e, em seguida, selecione **Instância Gerida do Azure SQL**.
3. Selecione **Criar**.

   ![Criar instância gerida](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Preencha os **instância gerida** formulário com as informações pedidas, utilizando as informações na tabela a seguir:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   | **Subscrição** | A sua subscrição | Uma subscrição em que tenha permissão para criar novos recursos |
   |**Nome da instância gerida**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Início de sessão de administração da instância gerida**|Qualquer nome de utilizador válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Não utilize "serveradmin", que é uma função ao nível do servidor reservada.|
   |**Palavra-passe**|Qualquer palavra-passe válida|A palavra-passe tem de ter, pelo menos, 16 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Agrupamento**|O agrupamento que pretende utilizar para a sua instância gerida|Para obter informações sobre agrupamentos, veja [agrupamentos](https://docs.microsoft.com/sql/t-sql/statements/collations).|
   |**Localização**|A localização em que pretende criar a Instância Gerida|Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure).|
   |**Rede virtual**|Selecione **criar nova rede virtual** ou uma rede virtual válido e uma sub-rede.| Se uma rede/sub-rede está desativado é deve ser [modificado para satisfazer os requisitos de rede](sql-database-managed-instance-configure-vnet-subnet.md) antes de selecioná-lo como um destino para a nova instância gerida. Para obter informações sobre os requisitos para configurar o ambiente de rede para uma instância gerida, veja [configurar uma VNet para a instância gerida da base de dados SQL do Azure](sql-database-managed-instance-connectivity-architecture.md). |
   |**Grupo de recursos**|Um grupo de recursos novo ou existente|Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições).|

   ![formulário da instância gerida](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Para utilizar a instância gerida como um grupo de ativação pós-falha de instância secundário, selecione o Check-out e especifique a instância gerida DnsAzurePartner. Esta funcionalidade está em pré-visualização e não é mostrado na captura de ecrã que acompanha este artigo.
6. Selecione **escalão de preço** para dimensionar recursos de computação e armazenamento, bem como para rever as opções de escalão de preço. O escalão de preço Fins Gerais com 32 GB de memória e 16 núcleos virtuais é o valor predefinido.
7. Utilize os controlos de deslize ou as caixas de texto para especificar a quantidade de armazenamento e o número de núcleos virtuais.
8. Quando terminar, escolha **aplicar** para guardar a sua seleção.  
9. Selecione **criar** para implementar a instância gerida.
10. Selecione o **notificações** ícone para ver o estado da implementação.

    ![progresso da implementação da instância gerida](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Selecione **implementação em curso** para abrir a janela instância gerida para monitorizar o progresso da implementação ainda mais.

> [!IMPORTANT]
> Para a primeira instância numa sub-rede, o tempo de implementação é normalmente muito mais do que nas instâncias subsequentes. Não cancele a operação de implementação porque dura mais do que o esperado. Criar a segunda Instância Gerida na sub-rede demora apenas alguns minutos.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Analisar os recursos e obter o nome do servidor completamente qualificado

Depois da implementação ser concluída com êxito, reveja os recursos criados e obtenha o nome de servidor completamente qualificado para utilização nos guias de início rápido posteriores.

1. Abra o grupo de recursos da Instância Gerida e veja os respetivos recursos que foram criados no guia de início rápido [Criar uma Instância Gerida](#create-a-managed-instance).

2. Selecione a sua instância gerida.

   ![Recursos da Instância Gerida](./media/sql-database-managed-instance-get-started/resources.png)

3. Sobre o **descrição geral** separador, localize a **anfitrião** propriedade e copie o anfitrião totalmente qualificado de endereços para a instância gerida.

   ![Recursos da Instância Gerida](./media/sql-database-managed-instance-get-started/host-name.png)

   O nome é semelhante à **your_machine_name.a1b2c3d4e5f6.database.windows.net**.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre como ligar a uma Instância Gerida, veja:
  - Para uma descrição geral das opções de ligação para aplicações, veja [Ligar as suas aplicações à Instância Gerida](sql-database-managed-instance-connect-app.md).
  - Para obter um guia de início rápido que mostre como ligar a uma Instância Gerida a partir de uma máquina virtual do Azure, veja [Configurar uma ligação de máquina virtual do Azure](sql-database-managed-instance-configure-vm.md).
  - Para um guia de início rápido que mostre como ligar a uma Instância Gerida a partir de um computador cliente no local através de uma ligação ponto a site, veja [Configurar uma ligação ponto a site](sql-database-managed-instance-configure-p2s.md).
- Para restaurar uma base de dados do SQL Server existente do local para uma Instância Gerida, pode utilizar o [Azure Database Migration Service (DMS) para a migração](../dms/tutorial-sql-server-to-managed-instance.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados ou o [comando T-SQL RESTORE](sql-database-managed-instance-get-started-restore.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados.
- Para avançadas de monitorização de desempenho da base de dados de instância gerida com inteligência incorporada de resolução de problemas, consulte [SQL Database do Azure Monitor, através da análise de SQL do Azure](../azure-monitor/insights/azure-sql.md)
