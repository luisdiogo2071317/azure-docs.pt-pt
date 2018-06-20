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
ms.date: 06/18/2018
ms.author: jeffgilb
ms.openlocfilehash: 183d9479ae18e557b00d0867cad79600145da7bd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265233"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Adicionar servidores de alojamento para o fornecedor de recursos SQL

Pode alojar uma instância do SQL Server numa máquina virtual (VM) no [Azure pilha](azure-stack-poc.md), ou numa VM fora do seu ambiente de pilha do Azure, desde que o fornecedor de recursos do SQL Server pode ligar à instância.

## <a name="overview"></a>Descrição geral

Os requisitos gerais para servidores de alojamento do SQL Server são:

* A instância do SQL Server têm de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância do SQL Server que está a ser utilizada por quaisquer outros consumidores. Esta restrição aplica-se também nos serviços de aplicação.
* O fornecedor de recursos VM do SQL Server não está associado a um domínio e só pode ser ligados utilizando a autenticação SQL.
* Tem de configurar uma conta com privilégios adequados para utilização pelo fornecedor de recursos.
* O fornecedor de recursos e utilizadores, tais como Web Apps, utilizam a rede de utilizador, pelo que não é necessária conectividade para a instância do SQL Server nesta rede. Este requisito, normalmente, significa que o IP para as instâncias do SQL Server tem de estar numa rede pública.
* Gestão de instâncias do SQL Server e respetivos anfitriões é até que. Por exemplo, o fornecedor de recursos não aplicar as atualizações, processar as cópias de segurança ou processar rotação de credenciais.
* Pode utilizar SKUs que suportam diferentes classes de capacidades SQL, tais como desempenho e elevada disponibilidade com AlwaysOn.

### <a name="sql-server-virtual-machine-images"></a>Imagens da máquina virtual do SQL Server

Imagens da máquina virtual IaaS do SQL Server estão disponíveis através da funcionalidade de gestão do Marketplace. Estas imagens são os mesmos que as VMs de SQL que estão disponíveis no Azure.

Certifique-se de que sempre transferir a versão mais recente do **SQL IaaS extensão** antes de implementar uma VM com um item do Marketplace.  A extensão de IaaS e o portal correspondente melhorias fornecem funcionalidades adicionais, tais como a aplicação de patches automática e cópia de segurança.

Existem outras opções para implementar as VMs do SQL Server, incluindo modelos no [Galeria de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Quaisquer servidores de alojamento instalado uma pilha do Azure com vários nós têm de ser criados a partir de uma subscrição de utilizador. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de utilizador ou a partir de uma sessão do PowerShell com um início de sessão adequado. Todos os servidores de alojamento são VMs sujeito a faturação e tem de ter licenças adequadas do SQL Server. O administrador de serviço _pode_ ser o proprietário dessa subscrição.

### <a name="required-privileges"></a>Privilégios necessários

Criar um utilizador administrativo com privilégios mais baixos que um sysadmin do SQL Server. O utilizador necessita apenas de permissões para as seguintes operações:

- Base de dados: Criar, Alter, com Containment (para sempre no apenas), remover, cópia de segurança
- Grupo de disponibilidade: Alter, associação, adicionar/remover base de dados
- Início de sessão: Criar, selecione, Alter, Drop, revogar
- Selecionadas operações: \[mestre\].\[ SYS\].\[ availability_group_listeners\] (AlwaysOn), sys. availability_replicas (AlwaysOn), Databases, \[mestre\].\[ SYS\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[mestre\].\[ SYS\].\[ availability_groups\] (AlwaysOn), sys. master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Fornecem a capacidade de estabelecendo ligação à autónoma alojar o SQL server

Pode utilizar autónomo (não-HA) os servidores SQL através de qualquer edição do SQL Server 2014 ou SQL Server 2016. Certifique-se de que tem as credenciais para uma conta com privilégios de administrador do sistema.

Para adicionar um servidor de alojamento autónomo já estiver configurado, siga estes passos:

1. Inicie sessão no portal do operador de pilha do Azure como um administrador de serviço.

2. Selecione **procurar** &gt; **recursos administrativos** &gt; **SQL que aloja servidores**.

   ![Servidores de alojamento do SQL Server](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Em **SQL que aloja servidores**, pode ligar o fornecedor de recursos do SQL Server para instâncias do SQL Server que servem de back-end do fornecedor de recursos.

   ![Dashboard de adaptador de SQL](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Preencha o formulário com os detalhes de ligação da sua instância do SQL Server.

   ![Adicionar um SQL Server que aloja o servidor](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Pode, opcionalmente, incluem um nome de instância e especifique um número de porta, se a instância não está atribuída à porta predefinida 1433.

   > [!NOTE]
   > A instância do SQL Server pode ser acedida pelo utilizador e administrador do Azure Resource Manager, desde que pode ser colocado sob o controlo do fornecedor de recursos. A instância do SQL __tem__ atribuída exclusivamente para o fornecedor de recursos.

4. Como adicionar servidores, tem de atribuir-lhes um SKU de novo ou existente para diferenciar as ofertas de serviços. Por exemplo, pode ter uma instância do SQL Server Enterprise que fornece:
  
   - capacidade de base de dados
   - cópia de segurança automática
   - servidores de elevado desempenho para departamentos individuais de reserva

   Todos os servidores de alojamento um SKU devem ter as mesmas capacidades. O **nome** deve refletir as propriedades do SKU, para que os utilizadores podem implementar as bases de dados a SKU adequado.

   > [!IMPORTANT]
   > Os carateres especiais, incluindo espaços e períodos, não são suportados no **família** ou **camada** nomes quando cria um SKU para os fornecedores de recursos do SQL Server e o MySQL.

   Por exemplo:

   ![Criar SKU](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

   >[!NOTE]
   > SKUs podem demorar até uma hora ser visível no portal. Os utilizadores não é possível criar uma base de dados até que o SKU não ter sido criada.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Fornecer elevada disponibilidade utilizando sempre em grupos de disponibilidade SQL

Configurar o SQL Always On instâncias é necessários passos adicionais e requer um mínimo de três VMs (ou máquinas físicas) Este artigo pressupõe que já tem uma compreensão sólida dos grupos de disponibilidade Always On. Para obter mais informações, consulte:

* [Introdução ao SQL Server Always On nos grupos de disponibilidade em máquinas virtuais do Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Always On nos grupos de disponibilidade (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> O fornecedor de recursos do SQL Server adaptador _apenas_ suporta SP1 Enterprise do SQL Server 2016 ou posterior instâncias Always On. Esta configuração de adaptador requer novas funcionalidades do SQL Server, tais como o seeding automático.

Além da lista de requisitos anterior, tem de ativar [Seeding automático](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) em cada grupo de disponibilidade para cada instância do SQL Server.

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

### <a name="to-add-sql-always-on-hosting-servers"></a>Para adicionar SQL Always On nos servidores de alojamento

1. Inicie sessão no portal de administração de pilha do Azure como um administrador de serviço.

2. Selecione **procurar** &gt; **recursos administrativos** &gt; **SQL que aloja servidores** &gt; **+ adicionar**.

   Em **SQL que aloja servidores** , pode ligar o fornecedor de recursos do servidor de SQL reais instâncias do SQL Server que servem de back-end do fornecedor de recursos.

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
