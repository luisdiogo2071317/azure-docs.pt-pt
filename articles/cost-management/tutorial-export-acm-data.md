---
title: Tutorial - criar e gerir dados exportados do Azure Cost Management | Microsoft Docs
description: Este artigo mostra como pode criar e gerir dados exportados do Azure Cost Management para utilizá-los em sistemas externos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 12/07/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 4614a1417213ed8b4d57c3b7ab21ac7424d75949
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087938"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: criar e gerir dados exportados

Se leu o tutorial de Análise de Custos, está familiarizado com a transferência manual dos dados do Cost Management. No entanto, pode criar uma tarefa periódica que exporta automaticamente os seus dados de gestão de custos para o armazenamento do Azure numa base diária, semanal ou mensal. Os dados exportados estão no formato CSV e contêm todas as informações recolhidas pelo Cost Management. Pode utilizar os dados exportados no armazenamento do Azure com sistemas externos e combiná-los com os seus dados personalizados. E pode utilizar os dados exportados num sistema externo, como um dashboard ou outro sistema financeiro.

Os exemplos neste tutorial orientam-no na exportação dos dados de gestão de custos e, em seguida, verificam se os dados foram exportados com êxito.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma exportação diária
> * Verificar se os dados são recolhidos

## <a name="prerequisites"></a>Pré-requisitos

A exportação de dados está disponível para todos os clientes do [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/). As seguintes permissões do Azure são suportadas por subscrição para a exportação de dados por utilizador e grupo:

- Proprietário – pode criar, modificar ou eliminar exportações agendadas de uma subscrição.
- Contribuinte – pode criar, modificar ou eliminar as respetivas exportações agendadas. Pode modificar o nome de exportações agendadas criadas por outros utilizadores.
- Leitor – pode agendar exportações para as quais tem permissões.

Para contas de Armazenamento do Azure:
- São necessárias permissões de escrita para alterar a conta de armazenamento configurada, independentemente das permissões na exportação.
- A sua conta de armazenamento do Azure tem de estar configurada para o armazenamento de blobs ou ficheiros.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com/).

## <a name="create-a-daily-export"></a>Criar uma exportação diária

Cost Management + Faturação &gt; Cost Management &gt; selecione um subscrição ou grupo de recursos numa subscrição &gt; Exportação &gt;  **Adicionar**.

Escreva um nome para a exportação e selecione a opção "Diariamente exportar dos custos de mês até à data". Clique em **Seguinte**.

![Novo de exportação de exemplo que mostra o tipo de exportação](./media/tutorial-export-acm-data/basics_exports.png)

Especifique a subscrição para a sua conta de armazenamento do Azure, em seguida, selecione a sua conta de armazenamento.  Especifique o contentor de armazenamento e o caminho do diretório que pretende que o ficheiro de exportação para ir para.  Clique em **Seguinte**.

![Novo de exportação de exemplo que mostra os detalhes da conta de armazenamento](./media/tutorial-export-acm-data/storage_exports.png)

Reveja os detalhes de exportação e clique em **criar**.

A nova exportação é apresentada na lista de exportações. Por predefinição, são ativadas exportações de novo. Se pretender desativar ou eliminar uma exportação agendada, clique em qualquer item na lista e, em seguida, clique em **Desativar** ou **Eliminar**.

Inicialmente, pode demorar uma ou duas horas para que a exportação seja executada. No entanto, pode demorar até quatro horas antes de os dados serem apresentados nos ficheiros exportados.

## <a name="verify-that-data-is-collected"></a>Verificar se os dados são recolhidos

Pode verificar facilmente se os seus dados do Cost Management estão a ser recolhidos e ver o ficheiro CSV exportado com o Explorador de Armazenamento do Azure.

Na lista de exportação, clique no nome da conta de armazenamento. Na página da conta de armazenamento, clique em Abrir no Explorador. Se vir uma caixa de confirmação, clique em **Sim** para abrir o ficheiro no Explorador de Armazenamento do Microsoft Azure.

![Página de conta de armazenamento que mostra informações de exemplo e a ligação para o abrir no Explorador](./media/tutorial-export-acm-data/storage-account-page.png)

No Explorador de Armazenamento, navegue para o contentor que pretende abrir e selecione a pasta correspondente ao mês atual. É apresentada uma lista de ficheiros CSV. Selecione um e, em seguida, clique em **Abrir**.

![Informações de exemplo mostradas no Explorador de armazenamento](./media/tutorial-export-acm-data/storage-explorer.png)

O ficheiro abre com o programa ou a aplicação que definiu para abrir as extensões de ficheiro CSV. Eis um exemplo do Excel.

![Exemplo exportados dados CSV mostrados no Excel](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>Aceder aos dados exportados a partir de outros sistemas

Um dos objetivos de exportar os dados do Cost Management é aceder aos dados a partir de sistemas externos. Pode utilizar um sistema de dashboard ou outro sistema financeiro. Estes sistemas variam bastante, de modo que mostrar um exemplo seria impraticável.  No entanto, pode começar a perceber como aceder aos seus dados de aplicações em [Introdução ao Armazenamento do Azure](../storage/common/storage-introduction.md).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma exportação diária
> * Verificar se os dados são recolhidos

Avance para o tutorial seguinte para otimizar e melhorar a eficiência, ao identificar os recursos inativos e subutilizados.

> [!div class="nextstepaction"]
> [Analisar e tomar medidas relacionadas com recomendações de otimização](tutorial-acm-opt-recommendations.md)
