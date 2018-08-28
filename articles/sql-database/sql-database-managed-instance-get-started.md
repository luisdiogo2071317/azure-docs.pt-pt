---
title: 'Portal do Azure: Criar uma Instância Gerida do SQL | Microsoft Docs'
description: Crie uma Instância Gerida do SQL, um ambiente de rede e uma VM cliente para acesso.
keywords: tutorial de base de dados sql, criar uma instância gerida de sql
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: quickstart
ms.date: 08/13/2018
ms.author: jovanpop-msft
ms.openlocfilehash: cb378c2d2773096992ef688653fd77b2625f8754
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42022948"
---
# <a name="create-an-azure-sql-managed-instance"></a>Criar uma Instância Gerida do SQL do Azure

Este início rápido orienta-o ao longo da criação de uma Instância Gerida do SQL no Azure. A Instância Gerida da Base de Dados SQL do Azure é uma Instância de Motor de Bases de Dados do SQL Server de Plataforma como Serviço (PaaS) que lhe permite executar e dimensionar bases de dados de elevada disponibilidade do SQL Server na cloud do Azure. Este início rápido mostra-lhe como começar mediante a criação de uma Instância Gerida do SQL.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="prepare-network-environment"></a>Preparar o ambiente de rede

A Instância Gerida do SQL é um serviço seguro colocado na sua própria Rede Virtual (VNet) do Azure. Para criar uma Instância Gerida, tem de preparar o ambiente de rede do Azure, que inclui:
 - A VNet do Azure na qual a Instância Gerida vai ser colocada.
 - A sub-rede na VNet do Azure na qual a Instância Gerida vai ser colocada.
 - A rota definida pelo utilizador que vai permitir à Instância Gerida comunicar com os serviços do Azure que controlam e gerem a instância.

A sub-rede é dedicada à Instância Gerida e não pode criar outros recursos (por exemplo, Máquinas Virtuais do Azure) na mesma. Neste início rápido, vão ser criadas duas sub-redes na sua VNet do Azure, pelo que pode colocar a Instância Gerida na sub-rede dedicada à mesma e os outros recursos na sub-rede predefinida.

1. Clique no botão abaixo para implementar o ambiente de rede do Azure preparado para a Instância Gerida da Base de Dados SQL do Azure:

    <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener"> <img src="http://azuredeploy.net/deploybutton.png"> </a>

    Este botão abre um formulário no portal do Azure, onde pode configurar o ambiente de rede antes de o implementar.

2. Opcionalmente, altere os nomes da VNet e das sub-redes e ajuste os intervalos de IP associados aos seus recursos de rede. Em seguida, prima o botão “Comprar” para criar e configurar o ambiente:

    ![criar ambiente de instância gerida](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

    > [!Note]
    > Esta implementação do Azure Resource Manager vai criar duas sub-redes na VNet, uma para a Instância Gerida, chamada **ManagedInstances**, e outra, denominada **Default**, para os outros recursos, como a máquina virtual cliente, que pode ser utilizada para ligar à Instância Gerida. Se não precisar de duas sub-redes, pode eliminar a predefinida (“Default”) mais tarde; no entanto, nesse caso, não poderá concluir o passo 3 deste guia de início rápido - [preparar a máquina cliente](#prepare-client-machine).

    > [!Note]
    > Se alterar o nome da VNet e das sub-redes, certifique-se de que se lembra dos nomes novos, pois vão ser precisos nas secções seguintes. No resto do tutorial, partir-se-á do princípio de que criou a VNet com o nome **MyNewVNet**, a sub-rede **ManagedInstances** para a Instância Gerida do SQL e a sub-rede **Default** para máquinas virtuais e outros recursos.

## <a name="create-a-managed-instance"></a>Criar uma Instância Gerida

Os passos seguintes mostram como criar a sua Instância Gerida depois de a pré-visualização ter sido aprovada.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize **Instância Gerida** e, em seguida, selecione **MI (pré-visualização)**.
3. Clique em **Criar**.

   ![Criar instância gerida](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

4. Selecione a sua subscrição e certifique-se de que os termos de pré-visualização são apresentados como **Aceites**.

   ![pré-visualização de instância gerida aceite](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

5. Preencha o formulário da Instância Gerida com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome da instância gerida**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Início de sessão de administração da instância gerida**|Qualquer nome de utilizador válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Não utilize “serveradmin”, que é uma função reservada a nível de servidor.| 
   |**Palavra-passe**|Qualquer palavra-passe válida|A palavra-passe tem de ter, pelo menos, 16 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Grupo de Recursos**|O grupo de recursos que criou anteriormente||
   |**Localização**|A localização que selecionou anteriormente|Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure).|
   |**Rede virtual**|A rede virtual que criou anteriormente| Escolha o item **MyNewVNet/ManagedInstances**, caso não tenha alterado os nomes no passo anterior. Caso contrário, escolha o nome da VNet e a sub-rede da instância gerida que introduziu na secção anterior. **Não utilize a sub-rede predefinida, porque a mesma não está configurada para alojar Instâncias Geridas**. |

   ![formulário de criação de instância gerida](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

6. Clique em **Escalão de preço** para saber o tamanho dos recursos de computação e armazenamento, bem como para rever as opções de escalão de preço. Por predefinição, a sua instância obtém 32 GB de espaço de armazenamento livre de encargos, **o que poderá não ser suficiente para as suas aplicações**.
7. Utilize os controlos de deslize ou as caixas de texto para especificar a quantidade de armazenamento e o número de núcleos virtuais. 
   ![escalão de preço de instâncias geridas](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

8. Quando terminar, clique em **Aplicar** para guardar a sua seleção.  
9. Clique em **Criar** para implementar a Instância Gerida.
10. Clique no ícone **Notificações** para ver o estado da implementação.
 
   ![progresso da implementação](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

11. Clique em **Implementação em curso** para abrir a janela Instância Gerida, para monitorizar ainda mais o progresso da implementação.
 
   ![progresso da implementação 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Enquanto ocorre a implementação, avance para o procedimento seguinte.

> [!IMPORTANT]
> Para a primeira instância numa sub-rede, o tempo de implementação é normalmente muito superior do que nas instâncias subsequentes - por vezes, demora mais de 24 horas a concluir. Não cancele a operação de implementação por estar a demorar mais do que o esperado. Este período de tempo para implementar a sua primeira instância é uma situação temporária. É de esperar uma redução significativa do tempo de implementação pouco tempo após o início da pré-visualização pública. Criar a segunda Instância Gerida na sub-rede demora alguns minutos.

## <a name="prepare-client-machine"></a>Preparar a máquina cliente

Uma vez que a Instância Gerida do SQL é colocada na sua Rede Virtual privada, tem de criar uma VM do Azure com uma ferramenta de cliente do SQL instalada, como o SQL Server Management Studio ou o SQL Operations Studio, para ligar à Instância Gerida e executar consultas.

> [!Note]
> Em vez da máquina virtual do Azure cliente, pode configurar uma ligação [Ponto a Site](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) e ligar à Instância Gerida a partir do seu computador local.

A forma mais fácil de criar uma máquina virtual cliente com todas as ferramentas necessárias é utilizar os modelos do Azure Resource Manager.

1. Clique no botão seguinte (confirme que tem sessão iniciada no portal do Azure noutro separador do browser):

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. No formulário que se abre, introduza o nome da máquina virtual, o nome de utilizador do administrador e a palavra-passe que vai utilizar para se ligar à VM.

    ![criar a VM cliente](./media/sql-database-managed-instance-get-started/create-client-sql-vm.png)

    Se não tiver alterado o nome da VNet e a sub-rede predefinida, não é preciso alterar os dois últimos parâmetros; caso contrário, deve alterá-los para os valores que introduziu quando configurou o ambiente de rede.

3. Clique no botão “Comprar” e a VM do Azure é implementada na rede que preparou.

4. Utilize a ligação de Ambiente de Trabalho Remoto para ligar à VM e localize o SQL Server Management Studio ou o SQL Operation Studio que são instalados automaticamente na VM.

5. Abra o SSMS e introduza o **nome do anfitrião** da sua Instância Gerida, na caixa **Nome do servidor**, selecione **Autenticação do SQL Server**, indique o início de sessão e a palavra-passe na caixa de diálogo **Ligar ao Servidor** e clique em **Ligar**.

    ![ligação SSMS](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Depois de ligar, pode ver as bases de dados do sistema e dos utilizadores no nó de bases de dados e vários objetos em nós de Segurança, Objetos de Servidor, Replicação, Gestão, SQL Server Agent e Gerador de Perfis de XEvent.

## <a name="next-steps"></a>Passos seguintes

 - [Connect your applications to Managed Instance](sql-database-managed-instance-connect-app.md) (Ligar as suas aplicações a uma Instância Gerida).
 - [Migrate your databases from on-premises to Managed Instance](sql-database-managed-instance-migrate.md) (Migrar as suas bases de dados do local para uma Instância Gerida).


