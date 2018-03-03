---
title: Como restaurar um servidor na base de dados do Azure para PostgreSQL
description: Este artigo descreve como restaurar um servidor na base de dados do Azure para PostgreSQL no portal do Azure.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 7607a3e60eec39de61c785b8ff75a9f11fa02d0c
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-using-the-azure-portal"></a>Como criar cópias de segurança e restaurar um servidor na base de dados do Azure para PostgreSQL no portal do Azure

## <a name="backup-happens-automatically"></a>Cópia de segurança ocorre automaticamente
Base de dados do Azure para servidores de PostgreSQL são cópias de segurança periodicamente para ativar funcionalidades de restauro. Utilizar esta funcionalidade pode restaurar o servidor e todas as suas bases de dados para um anterior ponto no tempo, num servidor novo.

## <a name="set-backup-configuration"></a>Configuração do conjunto de cópia de segurança

Tornar a escolha entre configurar o servidor para cópias de segurança localmente redundantes ou cópias de segurança georredundante durante a criação do servidor, no **escalão de preço** janela.

> [!NOTE]
> Depois de criar um servidor, o tipo de redundância de que possui, não pode ser mudado vs georredundante localmente redundantes.
>

Durante a criação de um servidor através do portal do Azure, o **escalão de preço** janela é onde poderá selecionar um **localmente redundante** ou **Georredundante** cópias de segurança para o servidor. Esta janela é também onde poderá selecionar a **período de retenção de cópia de segurança** - quanto (num número de dias) que pretende que as cópias de segurança do servidor para.

   ![Preços camada - escolha redundância de cópia de segurança](./media/howto-restore-server-portal/pricing-tier.png)

Para obter mais informações sobre como definir estes valores durante a criação, consulte o [base de dados do Azure para o guia de introdução do PostgreSQL servidor](quickstart-create-server-database-portal.md).

O período de retenção de cópias de segurança de um servidor pode ser alterado os seguintes passos:
1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
2. Selecione a base de dados do Azure para o servidor de PostgreSQL. Esta ação abre o **descrição geral** página.
3. Selecione **escalão de preço** no menu, sob **definições**. Utilizar o controlo de deslize pode alterar o **período de retenção de cópia de segurança** a sua preferência entre 7 e 35 dias.
Na captura de ecrã abaixo, foi aumentado para 34 dias.
![Aumento do período de retenção de cópias de segurança](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Clique em **OK** para confirmar a alteração.

O período de retenção de cópias de segurança é regida pelas até que ponto anterior no tempo que pode ser obtido um restauro de ponto no tempo, uma vez que o se basear em cópias de segurança disponíveis. Restauro de ponto no tempo é ainda mais descrito na secção seguinte. 

## <a name="point-in-time-restore-in-the-azure-portal"></a>Restauro de ponto no tempo no portal do Azure
Base de dados do Azure para PostgreSQL permite-lhe restaurar o servidor novamente para um ponto no tempo e em para uma nova cópia do servidor. Pode utilizar este servidor novo para recuperar os dados, ou ter as suas aplicações de cliente para este novo servidor do ponto.

Por exemplo, se uma tabela estava acidentalmente removido hoje em dia, ao meio-dia foi possível restaurar para a hora imediatamente antes ao meio-dia e obter a tabela em falta e os dados a partir dessa nova cópia do servidor. Restauro de ponto no tempo no servidor ao nível, não é a nível da base de dados.

Os seguintes passos restaurar o servidor de exemplo para um ponto no tempo:
1. No portal do Azure, selecione a base de dados do Azure para o servidor de PostgreSQL. 

2. Na barra de ferramentas do servidor do **descrição geral** página, selecione **restaurar**.

   ![Base de dados do Azure para restauro PostgreSQL - descrição geral - botão](./media/howto-restore-server-portal/2-server.png)

3. Preencha o formulário de restauro com as informações necessárias:

   ![Base de dados do Azure para PostgreSQL - informações de restauro ](./media/howto-restore-server-portal/3-restore.png)
  - **Ponto de restauro**: selecione o ponto no tempo que pretende restaurar.
  - **Servidor de destino**: forneça um nome para o novo servidor.
  - **Localização**: não é possível selecionar a região. Por predefinição é igual ao servidor de origem.
  - **Escalão de preço**: não é possível alterar estes parâmetros ao efetuar um restauro de ponto no tempo. É igual ao servidor de origem. 

4. Clique em **OK** para restaurar o servidor para restaurar para um ponto no tempo. 

5. Depois do restauro concluir, localize o novo servidor que é criado para verificar que os dados foram restaurados conforme esperado.

>[!Note]
>O novo servidor criado pelo restauro de ponto no tempo tem o mesmo nome de início de sessão do administrador do servidor e palavra-passe que foi válido para o servidor existente ao ponto no tempo escolhido. Pode alterar a palavra-passe a partir do novo servidor **descrição geral** página.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre o serviço [cópias de segurança](concepts-backup.md).
- Saiba mais sobre [continuidade do negócio](concepts-business-continuity.md) opções.
