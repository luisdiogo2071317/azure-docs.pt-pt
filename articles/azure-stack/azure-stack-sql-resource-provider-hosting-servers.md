---
title: SQL Server que aloja os servidores na pilha do Azure | Microsoft Docs
description: Como adicionar as instâncias do SQL Server para o aprovisionamento através do fornecedor de recursos do adaptador de SQL
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
ms.date: 05/18/2018
ms.author: jeffgilb
ms.openlocfilehash: e08c0bfd3cbed64f5042e469801e20c913c2f70e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359429"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Adicionar servidores de alojamento para o fornecedor de recursos SQL
Pode utilizar instâncias do SQL Server em VMs dentro do seu [Azure pilha](azure-stack-poc.md), ou uma instância fora do seu ambiente de pilha do Azure, fornecido o fornecedor de recursos pode ligar ao mesmo. Os requisitos gerais são:

* A instância do SQL Server têm de estar dedicada para utilização por cargas de trabalho RP e o utilizador. Não é possível utilizar uma instância do SQL Server que está a ser utilizada por outros consumidores, incluindo os serviços de aplicação.
* O fornecedor de recursos VM do SQL Server não está associado a um domínio e só pode ser ligados utilizando a autenticação SQL.
* Tem de configurar uma conta com privilégios adequados para utilização pelo fornecedor de recursos.
* O fornecedor de recursos e utilizadores, tais como Web Apps, utilizam a rede de utilizador, pelo que não é necessária conectividade para a instância do SQL Server nesta rede. Este requisito, normalmente, significa que o IP para as instâncias do SQL Server tem de estar numa rede pública.
* Gestão de instâncias do SQL Server e respetivos anfitriões é até que; o fornecedor de recursos não executar a aplicação de patches, cópia de segurança, credencial de rotação, etc.
* SKUs podem ser utilizados para criar diferentes classes de capacidades SQL, tais como desempenho, sempre, etc.

Um número de imagens da máquina virtual IaaS do SQL Server está disponível através da funcionalidade de gestão do Marketplace. Certifique-se de que sempre transferir a versão mais recente do **SQL IaaS extensão** antes de implementar uma VM com um item do Marketplace. As imagens do SQL Server são as mesmas que as VMs de SQL que estão disponíveis no Azure. Para as VMs de SQL criadas a partir destas imagens, a extensão de IaaS e as correspondentes melhoramentos portais fornecer funcionalidades como a aplicação de patches automática e as capacidades de cópia de segurança.

Existem outras opções para implementar as VMs do SQL Server, incluindo modelos no [Galeria de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Quaisquer servidores de alojamento instalado uma pilha do Azure com vários nós têm de ser criados a partir de uma subscrição de utilizador. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de utilizador ou a partir de uma sessão do PowerShell com um início de sessão adequado. Todos os servidores de alojamento são cobráveis VMs e tem de ter licenças adequadas do SQL Server. O administrador de serviço _pode_ ser o proprietário dessa subscrição.


### <a name="required-privileges"></a>Privilégios necessários

Pode criar um novo utilizador administrativo com privilégios sysadmin inferior a completo. As operações específicas que precisam de ser permitidos são:

- Base de dados: Criar, Alter, com Containment (sempre no apenas), lista, cópia de segurança
- Grupo de disponibilidade: Alter, associação, adicionar/remover base de dados
- Início de sessão: Criar, selecione, Alter, Drop, revogar
- Selecionadas operações: \[mestre\].\[ SYS\].\[ availability_group_listeners\] (AlwaysOn), sys. availability_replicas (AlwaysOn), Databases, \[mestre\].\[ SYS\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[mestre\].\[ SYS\].\[ availability_groups\] (AlwaysOn), sys. master_files



## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Fornecem a capacidade de estabelecendo ligação à autónoma alojar o SQL server
Pode utilizar autónomo (não-HA) os servidores SQL através de qualquer edição do SQL Server 2014 ou SQL Server 2016. Certifique-se de que tem as credenciais para uma conta com privilégios de administrador de sistema.

Para adicionar uma autónoma que aloja o servidor que já está aprovisionado, siga estes passos:

1. Inicie sessão no portal de administração do Azure pilha como um administrador de serviço

2. Clique em **procurar** &gt; **recursos administrativos** &gt; **SQL que aloja servidores**.

  ![](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

  O **SQL que aloja servidores** painel é onde pode ligar o fornecedor de recursos do servidor de SQL para instâncias reais do SQL Server que servem de back-end do fornecedor de recursos.

  ![Servidores de alojamento](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Preencha o formulário com os detalhes de ligação da sua instância do SQL Server.

  ![Novo servidor de alojamento](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Opcionalmente, pode incluir um nome de instância e um número de porta pode ser fornecido se a instância não está atribuída para a porta predefinida 1433.

  > [!NOTE]
  > A instância do SQL Server pode ser acedida pelo utilizador e administrador do Azure Resource Manager, desde que pode ser colocado sob o controlo do fornecedor de recursos. A instância do SQL __tem__ atribuída exclusivamente para o RP.

4. Como adicionar servidores, tem de atribuir-lhes um SKU de novo ou existente para diferenciar as ofertas de serviços. Por exemplo, pode ter uma instância do SQL Server Enterprise fornecer:
  - capacidade de base de dados
  - cópia de segurança automática
  - servidores de elevado desempenho para departamentos individuais de reserva
  - e assim sucessivamente.

  O nome do SKU deve refletir as propriedades, para que os utilizadores podem colocar as bases de dados adequadamente. Todos os servidores de alojamento de um SKU devem ter as mesmas capacidades.

> [!IMPORTANT]
> Os carateres especiais, incluindo espaços e períodos, não são suportados no **família** ou **camada** nomes quando cria um SKU para os fornecedores de recursos do SQL Server e o MySQL.

Um exemplo:

![SKUs](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
> SKUs podem demorar até uma hora ser visível no portal. Os utilizadores não é possível criar uma base de dados até que o SKU não ter sido criada.

## <a name="provide-capacity-using-sql-always-on-availability-groups"></a>Fornecem a capacidade de utilizar sempre em grupos de disponibilidade SQL
Configurar o SQL Always On instâncias é necessários passos adicionais e envolve a, pelo menos, três VMs (ou máquinas físicas).

> [!NOTE]
> O adaptador SQL RP _apenas_ suporta SP1 Enterprise do SQL Server 2016 ou posteriores instâncias para Always On, porque requer novas funcionalidades do SQL Server, tais como o seeding automático. Além da lista anterior comuns dos requisitos:

Especificamente, tem de ativar [Seeding automático](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) em cada grupo de disponibilidade para cada instância do SQL Server:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

Em instâncias secundárias Utilize estes comandos do SQL Server:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

Para adicionar servidores de alojamento SQL Always On, siga estes passos:

1. Inicie sessão no portal de administração de pilha do Azure como um administrador de serviços

2. Clique em **procurar** &gt; **recursos administrativos** &gt; **SQL que aloja servidores** &gt; **+ adicionar**.

    O **SQL que aloja servidores** painel é onde pode ligar o fornecedor de recursos do servidor de SQL para instâncias reais do SQL Server que servem de back-end do fornecedor de recursos.

3. Preencha o formulário com os detalhes de ligação da sua instância do SQL Server, que está a ser se de que utiliza o endereço FQDN do sempre no serviço de escuta (e o número de porta opcional). Forneça as informações de conta para a conta configurada com privilégios de administrador de sistema.

4. Selecione esta caixa para ativar o suporte para instâncias do SQL sempre no grupo de disponibilidade.

    ![Servidores de alojamento](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Adicione a instância do SQL Always On para um SKU. 

> [!IMPORTANT]
> Não é possível misturar servidores autónomos com Always On instâncias o SKU do mesmo. A tentar combinar tipos após a adição de resultados do servidor de alojamento primeiro num erro.


## <a name="making-sql-databases-available-to-users"></a>Disponibilizar bases de dados SQL para os utilizadores

Crie planos e as ofertas para disponibilizar bases de dados SQL para os utilizadores. Adicione o serviço de Microsoft.SqlAdapter para o plano e adicione uma Quota de existente ou crie um novo. Se criar uma quota, especifique a capacidade para permitir ao utilizador.

![Criar planos e as ofertas para incluir as bases de dados](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)


## <a name="next-steps"></a>Passos Seguintes

[Adicionar bases de dados](azure-stack-sql-resource-provider-databases.md)
