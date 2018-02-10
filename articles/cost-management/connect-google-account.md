---
title: "Ligar uma conta do Google Cloud Platform a gestão de custo do Azure | Microsoft Docs"
description: "Ligar a uma conta do Google Cloud Platform para ver o custo e repots de dados de utilização na gestão de custo."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 02/05/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 81a38f471ee1f2f8064a956eca121fd0e6feb235
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="connect-a-google-cloud-platform-account"></a>Ligar a uma conta do Google Cloud Platform

Pode ligar a sua conta do Google Cloud Platform existente para o Azure custo Management. Depois de ligar a sua conta para gestão de custo, custo e dados de utilização estão disponíveis em relatórios de gestão de custo. Este artigo ajuda-o a configurar e ligar a sua conta da Google com a gestão de custo.

## <a name="collect-project-information"></a>Recolher informações do projeto

Iniciar ao recolher informações sobre o seu projeto.

1. Inicie sessão na consola do Google Cloud Platform no [https://console.cloud.google.com](https://console.cloud.google.com).
2. Reveja as informações de projeto que pretende integrar a gestão de custos e tenha em atenção que o **nome do projeto** e **ID de projeto**. Manter as informações útil para os passos seguintes.  
    ![Consola do Google Cloud Platform](./media/connect-google-account/gcp-console01.png)
3. Se a faturação não está ativada e ligada ao seu projeto, crie uma conta de faturação. Para obter mais informações, consulte [criar uma nova conta de faturação](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create\_a\_new\_billing\_account).

## <a name="enable-storage-bucket-billing-export"></a>Ativar a exportação de faturação de registo de armazenamento

Custo de gestão obtém o Google dados a partir de um registo de armazenamento de faturação. Manter o **nome do registo** e **prefixo relatório** informações à mão para utilização posterior durante o registo de custo de gestão.

Utilizar o armazenamento do Google na nuvem para armazenar os relatórios de utilização incorreu taxas mínimas. Para obter mais informações, consulte [preços de armazenamento de nuvem](https://cloud.google.com/storage/pricing).

1. Se não tiver ativado a exportação de faturação para um ficheiro, siga as instruções apresentadas em [como ativar a exportação de faturação para um ficheiro](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file). Pode utilizar o formato de exportação de faturação JSON ou CSV.
2. Caso contrário, na consola do Google Cloud Platform, navegue para **faturação** > **exportação de faturação**. Tenha em atenção a faturação **nome do registo** e **prefixo relatório**.  
    ![Exportação de faturação](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Ative as APIs do Google Cloud Platform

Para recolher informações de recursos e de utilização, a gestão de custos tem seguintes Google Cloud Platform APIs ativada:

- BigQuery API
- Google Cloud SQL
- Arquivo de dados do Google Cloud API
- Armazenamento na nuvem do Google
- Google Cloud Storage JSON API
- API do motor de computação do Google

### <a name="enable-or-verify-apis"></a>Ativar ou certifique-se de APIs

1. Na consola do Google Cloud Platform, selecione o projeto que pretende registar com o custo de gestão.
2. Navegue para **APIs e serviços** > **biblioteca**.
3. Utilize a procura para localizar a que cada listado anteriormente API.
4. Para cada API, certifique-se de que **API ativada** é apresentado. Caso contrário, clique em **ATIVAR**.

## <a name="add-a-google-cloud-account-to-cost-management"></a>Adicione uma conta do Google Cloud a gestão de custos

1. Abra o portal de Cloudyn do portal do Azure ou navegue até ao [https://azure.cloudyn.com](https://azure.cloudyn.com/) e iniciar sessão.
2. Clique em **definições** (symbol roda dentada) e, em seguida, selecione **contas na nuvem**.
3. No **gestão de contas**, selecione o **contas Google** separador e, em seguida, clique em **adicionar novo +**.
4. No **nome da conta Google**, introduza o endereço de e-mail da conta de faturação, em seguida, clique em **seguinte**.
5. Na caixa de diálogo de autenticação de Google, selecione ou introduza uma conta do Google e, em seguida, **permitir** cloudyn.com acesso à sua conta.
6. Adicione as informações do projeto de pedido que tinha anterior indicado. Incluem **ID de projeto**, **projeto** nome, **faturação** nome do registo, e **ficheiro faturação** reportar prefixo, em seguida, clique em  **Guardar**.  
    ![Adicione o projeto do Google](./media/connect-google-account/add-project.png)

A conta do Google é apresentada na lista de contas e deverá indicar **autenticado**. Sob a mesma, o seu nome de projeto Google e o ID devem aparecer e ter um símbolo de marca de verificação verde. Estado da conta deverá indicar **concluído**.

Dentro de algumas horas, os relatórios de gestão de custo apresentam informações de custo e a utilização de Google.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre o Azure custo Management Cloudyn, avance para o [rever os custos de utilização e](./tutorial-review-usage.md) tutorial para a gestão de custo.
