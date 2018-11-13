---
title: 'Portal do Azure: Criar uma Instância Gerida do SQL | Microsoft Docs'
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
ms.date: 11/01/2018
ms.openlocfilehash: 3eadc2d233fd1716716c323f4c7087ee8363c67c
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912327"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Início Rápido: Criar uma Instância Gerida de Base de Dados SQL do Azure

Este guia de início rápido orienta-o ao longo da criação de uma [Instância Gerida](sql-database-managed-instance.md) de Base de Dados SQL do Azure no portal do Azure. 

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-managed-instance"></a>Criar uma Instância Gerida

Os passos seguintes mostram como criar uma Instância Gerida.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize **Instância Gerida** e, em seguida, selecione **Instância Gerida do Azure SQL**.
3. Clique em **Criar**.

   ![Criar instância gerida](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Preencha o formulário da Instância Gerida com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   | **Subscrição** | A sua subscrição | Uma subscrição em que tenha permissão para criar novos recursos |
   |**Nome da instância gerida**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Início de sessão de administração da instância gerida**|Qualquer nome de utilizador válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Não utilize “serveradmin”, que é uma função reservada a nível de servidor.| 
   |**Palavra-passe**|Qualquer palavra-passe válida|A palavra-passe tem de ter, pelo menos, 16 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Grupo de Recursos**|Um grupo de recursos novo ou existente|Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições).|
   |**Localização**|A localização em que pretende criar a Instância Gerida|Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure).|
   |**Rede virtual**|Selecione **Criar nova rede virtual** ou uma rede virtual criada anteriormente no grupo de recursos que forneceu anteriormente neste formulário| Para configurar uma rede virtual para uma Instância Gerida com definições personalizadas, veja [Configure SQL Managed Instance virtual network environment template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) (Configurar modelo de ambiente de rede virtual de Instância Gerida SQL) no Github. Para obter informações sobre os requisitos para configurar o ambiente de rede para uma Instância Gerida, veja [Configurar uma VNet para a Instância Gerida de Base de Dados SQL do Azure](sql-database-managed-instance-vnet-configuration.md) |

   ![formulário da instância gerida](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Clique em **Escalão de preço** para saber o tamanho dos recursos de computação e armazenamento, bem como para rever as opções de escalão de preço. O escalão de preço Fins Gerais com 32 GB de memória e 16 núcleos virtuais é o valor predefinido.
6. Utilize os controlos de deslize ou as caixas de texto para especificar a quantidade de armazenamento e o número de núcleos virtuais. 
7. Quando terminar, clique em **Aplicar** para guardar a sua seleção.  
8. Clique em **Criar** para implementar a Instância Gerida.
9. Clique no ícone **Notificações** para ver o estado da implementação.

    ![progresso da implementação da instância gerida](./media/sql-database-managed-instance-get-started/deployment-progress.png)

10. Clique em **Implementação em curso** para abrir a janela Instância Gerida, para monitorizar ainda mais o progresso da implementação. 

> [!IMPORTANT]
> Para a primeira instância numa sub-rede, o tempo de implementação é normalmente muito superior do que nas instâncias subsequentes. Não cancele a operação de implementação por estar a demorar mais do que o esperado. Criar a segunda Instância Gerida na sub-rede demora apenas alguns minutos.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Rever os recursos e obter o nome de servidor completamente qualificado

Depois da implementação ser concluída com êxito, reveja os recursos criados e obtenha o nome de servidor completamente qualificado para utilização nos guias de início rápido posteriores.

1. Abra o grupo de recursos da Instância Gerida e veja os respetivos recursos que foram criados no guia de início rápido [Criar uma Instância Gerida](sql-database-managed-instance-get-started.md).

   ![Recursos da Instância Gerida](./media/sql-database-managed-instance-get-started/resources.png)Abra o recurso da Instância Gerida no portal do Azure.

2. Clique na Instância Gerida.
3. No separador **Descrição geral**, localize a propriedade **Anfitrião** e copie endereço de anfitrião completamente qualificado para a Instância Gerida.


   ![Recursos da Instância Gerida](./media/sql-database-managed-instance-get-started/host-name.png)

   O nome é semelhante ao seguinte: **nome_do_computador.neu15011648751ff.database.windows.net**.

## <a name="next-steps"></a>Passos seguintes

- Para saber mais sobre como ligar a uma Instância Gerida, veja:
  - Para uma descrição geral das opções de ligação para aplicações, veja [Ligar as suas aplicações à Instância Gerida](sql-database-managed-instance-connect-app.md).
  - Para obter um guia de início rápido que mostre como ligar a uma Instância Gerida a partir de uma máquina virtual do Azure, veja [Configurar uma ligação de máquina virtual do Azure](sql-database-managed-instance-configure-vm.md).
  - Para um guia de início rápido que mostre como ligar a uma Instância Gerida a partir de um computador cliente no local através de uma ligação ponto a site, veja [Configurar uma ligação ponto a site](sql-database-managed-instance-configure-p2s.md).
- Para restaurar uma base de dados do SQL Server existente do local para uma Instância Gerida, pode utilizar o [Azure Database Migration Service (DMS) para a migração](../dms/tutorial-sql-server-to-managed-instance.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados ou o [comando T-SQL RESTORE](sql-database-managed-instance-get-started-restore.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados.
- Para a monitorização avançada do desempenho de base de dados de Instância Gerida com informações de resolução de problemas incorporadas, veja [Monitorizar a Base de Dados SQL do Azure com a Análise de SQL do Azure](../log-analytics/log-analytics-azure-sql.md)
