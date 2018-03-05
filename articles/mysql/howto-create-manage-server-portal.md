---
title: Criar e gerir a base de dados do Azure para o servidor de MySQL utilizando o portal do Azure
description: "Este artigo descreve como pode rapidamente criar uma nova base de dados do Azure para o servidor de MySQL e gerir o servidor através do Portal do Azure."
services: mysql
author: ajlam
ms.author: nolanwu
editor: jasonwhowell
manager: kfile
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 0e274c0ada3de5e9000ae41516e5b9b67ef1490b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Criar e gerir a base de dados do Azure para o servidor de MySQL utilizando o portal do Azure
Este tópico descreve como pode criar rapidamente uma nova base de dados do Azure para o servidor de MySQL. Também inclui informações sobre como gerir o servidor utilizando o portal do Azure. Gestão de servidor inclui ver detalhes do servidor e bases de dados, a repor a palavra-passe, dimensionar recursos e a eliminação do servidor.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [Portal do Azure](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Criar uma Base de Dados do Azure para o servidor MySQL
Siga estes passos para criar uma base de dados do Azure para o servidor de MySQL com o nome "mydemoserver."

1. Clique em de **crie um recurso** botão localizada no canto superior esquerdo do portal do Azure.

2. Na nova página, selecione **bases de dados**e, em seguida, na página de bases de dados, selecione **base de dados do Azure para MySQL**.

    > É criada uma base de dados do Azure para o servidor de MySQL com um conjunto definido de [computação e armazenamento](./concepts-pricing-tiers.md) recursos. A base de dados é criado dentro de um grupo de recursos do Azure e numa base de dados do Azure para o servidor de MySQL.

   ![create-new-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. Preencha o formulário de MySQL da base de dados do Azure utilizando as seguintes informações:

    | **Campo do Formulário** | **Descrição do Campo** |
    |----------------|-----------------------|
    | *Nome do servidor* | mydemoserver (nome do servidor é exclusivo global) |
    | *Subscrição* | mysubscription (selecione no menu pendente) |
    | *Grupo de recursos* | myresourcegroup (criar um novo grupo de recursos ou utilize uma já existente) |
    | *Selecionar origem* | Em branco (criar um servidor de MySQL em branco) |
    | *Início de sessão de administrador do servidor* | myadmin (configure o nome da conta de administração) |
    | *Palavra-passe* | palavra-passe de conta do conjunto de admin |
    | *Confirmar palavra-passe* | confirme a palavra-passe da conta de administrador |
    | *Localização* | Sudeste Asiático (selecione entre Europa do Norte e EUA oeste) |
    | *Versão* | 5.7 (escolher a base de dados do Azure para a versão do servidor MySQL) |

4. Clique em **escalão de preço** para especificar o nível de desempenho e o escalão de serviço para o novo servidor. Selecione o **fins gerais** separador. *Gen 4*, *2 vCores*, *5 GB*, e *7 dias* são os valores predefinidos para **computação geração**, **vCore** , **Armazenamento**, e **período de retenção de cópia de segurança**. Pode deixar os controlos de deslize como está. Para ativar as cópias de segurança do servidor no armazenamento georredundante selecione **Georredundante** do **opções de redundância de cópia de segurança**.

   ![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Clique em **Criar** para aprovisionar o servidor. O aprovisionamento demora alguns minutos.

    > Selecione o **afixar ao dashboard** opção para permitir controlo fácil das implementações.

## <a name="update-an-azure-database-for-mysql-server"></a>Atualizar uma base de dados do Azure para o servidor de MySQL
Depois do novo servidor tiver sido aprovisionado, o utilizador tem várias opções para configurar o servidor existente, incluindo a repor a palavra-passe de administrador e dimensionamento do servidor ou reduzir verticalmente alterando vCore ou de armazenamento.

### <a name="change-the-administrator-user-password"></a>Alterar a palavra-passe de utilizador do administrador
1. Do servidor **descrição geral**, clique em **Repor palavra-passe** para mostrar a janela de repor a palavra-passe.

   ![descrição geral](./media/howto-create-manage-server-portal/overview.png)

2. Introduza uma palavra-passe nova e confirme a palavra-passe na janela, conforme mostrado:

   ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)

3. Clique em **OK** para guardar a nova palavra-passe.

### <a name="scale-vcores-updown"></a>Escala vCores para cima/para baixo

1. Clique em **escalão de preço**, localizado em **definições**.

2. Alterar o **vCore** definição ao mover o controlo de deslize para o valor pretendido.

    ![scale-compute](./media/howto-create-manage-server-portal/scale-compute.png)

3. Clique em **OK** para guardar as alterações.

### <a name="scale-storage-up"></a>Armazenamento de escala cópias de segurança

1. Clique em **escalão de preço**, localizado em **definições**.

2. Alterar o **armazenamento** definição ao mover o controlo de deslize para o valor pretendido.

    ![armazenamento à escala](./media/howto-create-manage-server-portal/scale-storage.png)

3. Clique em **OK** para guardar as alterações.

## <a name="delete-an-azure-database-for-mysql-server"></a>Eliminar uma base de dados do Azure para o servidor de MySQL

1. Do servidor **descrição geral**, clique em de **eliminar** botão para abrir o pedido de confirmação de eliminação.

    ![eliminar](./media/howto-create-manage-server-portal/delete.png)

2. Escreva o nome do servidor na caixa de entrada para confirmação dupla.

    ![confirm-delete](./media/howto-create-manage-server-portal/confirm.png)

3. Clique em de **eliminar** botão para confirmar a eliminação do servidor. Aguarde até o pop "servidor de MySQL eliminada com êxito" aparecem na barra de notificação.

## <a name="list-the-azure-database-for-mysql-databases"></a>Lista a base de dados do Azure para bases de dados MySQL
Do servidor **descrição geral**, desloque para baixo até verá o mosaico na parte inferior da base de dados. Todas as bases de dados no servidor estão listados na tabela.

   ![show-databases](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Mostrar detalhes de uma base de dados do Azure para o servidor de MySQL
Clique em **propriedades**, localizado em **definições** para ver informações detalhadas sobre o servidor.

![propriedades](./media/howto-create-manage-server-portal/properties.png)

## <a name="next-steps"></a>Passos Seguintes

[Início rápido: Criar a base de dados do Azure para o servidor de MySQL utilizando o portal do Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)