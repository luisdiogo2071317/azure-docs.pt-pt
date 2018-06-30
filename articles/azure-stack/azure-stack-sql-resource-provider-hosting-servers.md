---
title: SQL Server que aloja os servidores na pilha do Azure | Microsoft Docs
description: Como adicionar as instâncias do SQL Server para o aprovisionamento através do fornecedor de recursos de adaptador de SQL.
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
ms.date: 06/29/2018
ms.author: jeffgilb
ms.openlocfilehash: 74d888ffe28e5428b47bfc73122518c22d0f0918
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128712"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Adicionar servidores de alojamento para o fornecedor de recursos SQL

Pode alojar uma instância do SQL Server numa máquina virtual (VM) no [Azure pilha](azure-stack-poc.md), ou numa VM fora do seu ambiente de pilha do Azure, desde que o fornecedor de recursos do SQL Server pode ligar à instância.

## <a name="overview"></a>Descrição geral

Antes de adicionar um servidor de alojamento de SQL, consulte os seguintes requisitos obrigatórios e gerais.

**Requisitos obrigatórios**

* Ative a autenticação do SQL Server na instância do SQL Server. Porque o fornecedor de recursos VM do SQL Server não está associado a um domínio, apenas poderá ligar a um servidor de alojamento utilizando a autenticação SQL.
* Configure os endereços IP para as instâncias do SQL Server como pública. O fornecedor de recursos e utilizadores, tais como Web Apps, comunicam através da rede de utilizador, pelo que não é necessária conectividade para a instância do SQL Server nesta rede.

**Requisitos gerais**

* Dedique a instância do SQL Server para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância do SQL Server que está a ser utilizada por quaisquer outros consumidores. Esta restrição aplica-se também nos serviços de aplicação.
* Configure uma conta com os níveis de privilégio adequado para o fornecedor de recursos.
* Está é responsável por gerir as instâncias do SQL Server e respetivos anfitriões.  Por exemplo, o fornecedor de recursos não aplicar as atualizações, processar as cópias de segurança ou processar rotação de credenciais.

### <a name="sql-server-virtual-machine-images"></a>Imagens da máquina virtual do SQL Server

Imagens da máquina virtual IaaS do SQL Server estão disponíveis através da funcionalidade de gestão do Marketplace. Estas imagens são os mesmos que as VMs de SQL que estão disponíveis no Azure.

Certifique-se de que sempre transferir a versão mais recente do **SQL IaaS extensão** antes de implementar uma VM do SQL Server com um item do Marketplace. A extensão de IaaS e o portal correspondente melhorias fornecem funcionalidades adicionais, tais como a aplicação de patches automática e cópia de segurança. Para obter mais informações sobre esta extensão, consulte [automatizar tarefas de gestão em Virtual Machines do Azure com a extensão de agente do SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Existem outras opções para implementar as VMs do SQL Server, incluindo modelos no [Galeria de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Quaisquer servidores de alojamento instalado uma pilha do Azure com vários nós têm de ser criados a partir de uma subscrição de utilizador. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de utilizador ou a partir de uma sessão do PowerShell com um início de sessão adequado. Todos os servidores de alojamento são VMs sujeito a faturação e tem de ter licenças adequadas do SQL Server. O administrador de serviço _pode_ ser o proprietário dessa subscrição.

### <a name="required-privileges"></a>Privilégios necessários

Pode criar um utilizador administrativo com privilégios mais baixos que um sysadmin do SQL Server. O utilizador necessita apenas de permissões para as seguintes operações:

* Base de dados: Criar, Alter, com Containment (para sempre no apenas), remover, cópia de segurança
* Grupo de disponibilidade: Alter, associação, adicionar/remover base de dados
* Início de sessão: Criar, selecione, Alter, Drop, revogar
* Selecionadas operações: \[mestre\].\[ SYS\].\[ availability_group_listeners\] (AlwaysOn), sys. availability_replicas (AlwaysOn), Databases, \[mestre\].\[ SYS\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[mestre\].\[ SYS\].\[ availability_groups\] (AlwaysOn), sys. master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Fornecem a capacidade de estabelecendo ligação à autónoma alojar o SQL server

Pode utilizar autónomo (não-HA) os servidores SQL através de qualquer edição do SQL Server 2014 ou SQL Server 2016. Certifique-se de que tem as credenciais para uma conta com privilégios de administrador do sistema.

Para adicionar um servidor de alojamento autónomo já estiver configurado, siga estes passos:

1. Inicie sessão no portal do operador de pilha do Azure como um administrador de serviço.

2. Selecione **procurar** &gt; **recursos administrativos** &gt; **SQL que aloja servidores**.

   ![Servidores de alojamento do SQL Server](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Em **SQL que aloja servidores**, pode ligar o fornecedor de recursos do SQL Server para instâncias do SQL Server que servem de back-end do fornecedor de recursos.

   ![Dashboard de adaptador de SQL](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. No **adicionar um servidor de alojamento de SQL**, forneça os detalhes de ligação para a instância do SQL Server.

   ![Adicionar um SQL Server que aloja o servidor](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Opcionalmente, forneça um nome de instância e especifique um número de porta, se a instância não está atribuída à porta predefinida 1433.

   > [!NOTE]
   > A instância do SQL Server pode ser acedida pelo utilizador e administrador do Azure Resource Manager, desde que pode ser colocado sob o controlo do fornecedor de recursos. A instância do SQL __tem__ atribuída exclusivamente para o fornecedor de recursos.

4. Como adicionar servidores, deve atribuí-las a um SKU existente ou criar um SKU de novo. Em **adicionar um servidor que aloja**, selecione **SKUs**.

   * Para utilizar um SKU existente, escolha um SKU disponível e, em seguida, selecione **criar**.
   * Para criar um SKU, selecione **+ criar novo SKU**. No **criar SKU**, introduza as informações necessárias e, em seguida, selecione **OK**.

     > [!IMPORTANT]
     > Os carateres especiais, incluindo espaços e períodos, não são suportados no **nome** campo. Utilize os exemplos na captura de ecrã seguinte para introduzir os valores para o **família**, **camada**, e **edição** campos.

     ![Criar um SKU](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

      SKUs podem demorar até uma hora ser visível no portal. Os utilizadores não é possível criar uma base de dados até que o SKU não ter sido criada.

### <a name="sku-notes"></a>Notas SKU

Pode utilizar os SKUs para diferenciar as ofertas de serviços. Por exemplo, pode ter uma instância do SQL Server Enterprise que tem as seguintes características:
  
* capacidade elevada
* elevado desempenho
* Elevada disponibilidade

Pode criar um SKU do exemplo anterior, limitar o acesso a grupos específicos que necessitam de uma base de dados de elevado desempenho.

>[!TIP]
>Utilize um nome SKU reflete descreve as funcionalidades dos servidores no SKU, tais como a capacidade e o desempenho. O nome serve como um auxiliar para ajudar os utilizadores a implementar as bases de dados para o SKU adequado.

Como melhor prática, todos os servidores de alojamento um SKU devem ter as mesmas características de desempenho e de recursos.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Fornecer elevada disponibilidade utilizando sempre em grupos de disponibilidade SQL

Configurar o SQL Always On instâncias é necessários passos adicionais e requer três VMs (ou máquinas físicas) Este artigo pressupõe que já tem uma compreensão sólida dos grupos de disponibilidade Always On. Para obter mais informações, veja os artigos seguintes:

* [Introdução ao SQL Server Always On nos grupos de disponibilidade em máquinas virtuais do Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Always On nos grupos de disponibilidade (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> O fornecedor de recursos do SQL Server adaptador _apenas_ suporta SP1 Enterprise do SQL Server 2016 ou posterior instâncias Always On. Esta configuração de adaptador requer novas funcionalidades do SQL Server, tais como o seeding automático.

### <a name="automatic-seeding"></a>Automático seeding
Tem de ativar [Seeding automático](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) em cada grupo de disponibilidade para cada instância do SQL Server.

Para ativar o seeding automático em todas as instâncias, editar e, em seguida, execute o seguinte comando SQL para cada instância:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

Nas instâncias do secundárias, editar e, em seguida, execute o seguinte comando SQL para cada instância:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="configure-contained-database-authentication"></a>Configurar a autenticação de base de dados contida
Antes de adicionar uma base de dados contida a um grupo de disponibilidade, certifique-se de que a opção de servidor de autenticação de base de dados contida está definida para 1 em todas as instâncias de servidor que aloja uma réplica de disponibilidade para o grupo de disponibilidade. Para obter mais informações, consulte [incluído a opção de configuração do servidor de autenticação de base de dados](https://docs.microsoft.com/sql/database-engine/configure-windows/contained-database-authentication-server-configuration-option?view=sql-server-2017).

Utilize estes comandos para definir a opção de servidor de autenticação de base de dados contida para cada instância:

  ```
  EXEC sp_configure 'contained database authentication', 1
  GO
  RECONFIGURE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>Para adicionar SQL Always On nos servidores de alojamento

1. Inicie sessão no portal de administração de pilha do Azure como um administrador de serviço.

2. Selecione **procurar** &gt; **recursos administrativos** &gt; **SQL que aloja servidores** &gt; **+ adicionar**.

   Em **SQL que aloja servidores**, pode ligar o fornecedor de recursos do servidor de SQL para instâncias reais do SQL Server que servem de back-end do fornecedor de recursos.

3. Preencha o formulário com os detalhes de ligação para a instância do SQL Server. Certifique-se de que utiliza o endereço FQDN do sempre no serviço de escuta (e o número de porta opcional.) Forneça as informações para a conta configurada com privilégios de administrador do sistema.

4. Selecione a caixa de sempre no grupo de disponibilidade para ativar o suporte para instâncias do SQL sempre no grupo de disponibilidade.

   ![Ativar sempre no](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Adicione a instância do SQL Always On para um SKU.

   > [!IMPORTANT]
   > Não é possível misturar servidores autónomos com Always On instâncias o SKU do mesmo. A tentar combinar tipos após a adição de resultados do servidor de alojamento primeiro num erro.

## <a name="make-the-sql-databases-available-to-users"></a>Fazer com que as bases de dados do SQL Server disponíveis aos utilizadores

Crie planos e as ofertas para disponibilizar bases de dados SQL para os utilizadores. Adicionar o **Microsoft.SqlAdapter** para o plano de serviço e adicionar a predefinição de Quota ou criar uma Quota de novo.

![Criar planos e as ofertas para incluir as bases de dados](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="next-steps"></a>Passos Seguintes

[Adicionar bases de dados](azure-stack-sql-resource-provider-databases.md)
