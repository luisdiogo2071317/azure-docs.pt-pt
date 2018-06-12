---
title: Como restaurar um servidor na base de dados do Azure para MySQL
description: Este artigo descreve como restaurar um servidor na base de dados do Azure para o MySQL no portal do Azure.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 04/01/2018
ms.openlocfilehash: 6a34bbb5eefac117775c9876f3e4a25d3dade736
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266276"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-portal"></a>Como criar cópias de segurança e restaurar um servidor na base de dados do Azure para o MySQL no portal do Azure

## <a name="backup-happens-automatically"></a>Cópia de segurança ocorre automaticamente
Base de dados do Azure para servidores MySQL são cópias de segurança periodicamente para ativar funcionalidades de restauro. Utilizar esta funcionalidade pode restaurar o servidor e todas as suas bases de dados para um anterior ponto no tempo, num servidor novo.

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de procedimentos, tem de:
- Um [base de dados do Azure para o servidor de MySQL e base de dados](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Configuração do conjunto de cópia de segurança

Tornar a escolha entre configurar o servidor para cópias de segurança localmente redundantes ou cópias de segurança georredundante durante a criação do servidor, no **escalão de preço** janela.

> [!NOTE]
> Depois de criar um servidor, o tipo de redundância de que possui, não pode ser mudado vs georredundante localmente redundantes.
>

Durante a criação de um servidor através do portal do Azure, o **escalão de preço** janela é onde poderá selecionar um **localmente redundante** ou **Georredundante** cópias de segurança para o servidor. Esta janela é também onde poderá selecionar a **período de retenção de cópia de segurança** - quanto (num número de dias) que pretende que as cópias de segurança do servidor para.

   ![Preços camada - escolha redundância de cópia de segurança](./media/howto-restore-server-portal/pricing-tier.png)

Para obter mais informações sobre como definir estes valores durante a criação, consulte o [base de dados do Azure para o guia de introdução do MySQL servidor](quickstart-create-mysql-server-database-using-azure-portal.md).

O período de retenção de cópias de segurança pode ser alterado num servidor os seguintes passos:
1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
2. Selecione a base de dados do Azure para o servidor de MySQL. Esta ação abre o **descrição geral** página.
3. Selecione **escalão de preço** no menu, sob **definições**. Utilizar o controlo de deslize pode alterar o **período de retenção de cópia de segurança** a sua preferência entre 7 e 35 dias.
Na captura de ecrã abaixo, foi aumentado para 34 dias.
![Aumento do período de retenção de cópias de segurança](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Clique em **OK** para confirmar a alteração.

O período de retenção de cópias de segurança é regida pelas até que ponto anterior no tempo que pode ser obtido um restauro de ponto no tempo, uma vez que o se basear em cópias de segurança disponíveis. Restauro de ponto no tempo é ainda mais descrito na secção seguinte. 

## <a name="point-in-time-restore"></a>Restauro para um ponto anterior no tempo
Base de dados do Azure para MySQL permite-lhe restaurar o servidor novamente para um ponto no tempo e em para uma nova cópia do servidor. Pode utilizar este servidor novo para recuperar os dados, ou ter as suas aplicações de cliente para este novo servidor do ponto.

Por exemplo, se uma tabela estava acidentalmente removido hoje em dia, ao meio-dia foi possível restaurar para a hora imediatamente antes ao meio-dia e obter a tabela em falta e os dados a partir dessa nova cópia do servidor. Restauro de ponto no tempo no servidor ao nível, não é a nível da base de dados.

Os seguintes passos restaurar o servidor de exemplo para um ponto no tempo:
1. No portal do Azure, selecione a base de dados do Azure para o servidor de MySQL. 

2. Na barra de ferramentas do servidor do **descrição geral** página, selecione **restaurar**.

   ![Base de dados do Azure para restauro MySQL - descrição geral - botão](./media/howto-restore-server-portal/2-server.png)

3. Preencha o formulário de restauro com as informações necessárias:

   ![Base de dados do Azure para MySQL - informações de restauro ](./media/howto-restore-server-portal/3-restore.png)
  - **Ponto de restauro**: selecione o ponto no tempo que pretende restaurar.
  - **Servidor de destino**: forneça um nome para o novo servidor.
  - **Localização**: não é possível selecionar a região. Por predefinição é igual ao servidor de origem.
  - **Escalão de preço**: não é possível alterar estes parâmetros ao efetuar um restauro de ponto no tempo. É igual ao servidor de origem. 

4. Clique em **OK** para restaurar o servidor para restaurar para um ponto no tempo. 

5. Depois do restauro concluir, localize o novo servidor que é criado para verificar que os dados foram restaurados conforme esperado.

>[!Note]
>Tenha em atenção que o novo servidor criado pelo restauro de ponto no tempo tem o mesmo nome de início de sessão do administrador do servidor e palavra-passe que foi válido para o servidor existente ao ponto no tempo escolhido. Pode alterar a palavra-passe a partir do novo servidor **descrição geral** página.

## <a name="geo-restore"></a>Georreplicação restauro
Se tiver configurado o servidor para cópias de segurança georredundante, um novo servidor pode ser criado da cópia de segurança de que o servidor existente. Este novo servidor pode ser criado em qualquer região que a base de dados do Azure para MySQL está disponível.  

1. Selecione o botão (+) **Criar um recurso**, no canto superior esquerdo do portal. Selecione **Bases de Dados** > **Base de Dados do Azure para MySQL**.

   ![A opção "Azure base de dados para MySQL"](./media/howto-restore-server-portal/2_navigate-to-mysql.png)

2. O formato **selecionar origem** lista pendente, escolha **cópia de segurança**. Esta ação carrega uma lista de servidores que têm cópias redundantes georreplicação ativadas. Selecione um destas cópias de segurança para ser a origem do novo servidor.
   ![Selecionar origem: Cópia de segurança e a lista de cópias de segurança redundante georreplicação](./media/howto-restore-server-portal/2-georestore.png)

   > [!NOTE]
   > Quando um servidor é criado pela primeira vez pode não ser imediatamente disponível para o restauro de georreplicação. Poderá demorar algumas horas para os metadados necessários ser preenchido.
   >

3. Preencha o resto do formulário com as suas preferências. Pode selecionar qualquer **localização**. Depois de selecionar a localização, pode selecionar **escalão de preço**. Por predefinição, são apresentados os parâmetros para o servidor existente que está a restaurar. Pode clicar em **OK** sem efetuar alterações para herdar essas definições. Ou pode alterar **computação geração** (se disponível na região que escolheu), número de **vCores**, **período de retenção de cópia de segurança**, e **cópia de segurança Opção de redundância**. A alteração **escalão de preço** (básica, fins gerais ou com otimização de memória) ou **armazenamento** tamanho durante o restauro não é suportado.

>[!Note]
>O novo servidor criado pelo Restauro georreplicação tem o mesmo nome de início de sessão de administrador de servidor e palavra-passe que foi válido para o servidor existente no momento que do restauro foi iniciado. A palavra-passe pode ser alterada a partir do novo servidor **descrição geral** página.


## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre o serviço [cópias de segurança](concepts-backup.md).
- Saiba mais sobre [continuidade do negócio](concepts-business-continuity.md) opções.
