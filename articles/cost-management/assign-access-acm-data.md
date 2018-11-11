---
title: Atribuir acesso a dados do Azure Cost Management | Documentos da Microsoft
description: Este artigo descreve apesar de atribuir permissões a dados do Azure Cost Management para vários âmbitos de acesso.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/09/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: a62ecd0201d6b29686abc186ee69f3d26dc5ca4e
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/10/2018
ms.locfileid: "51516295"
---
# <a name="assign-access-to-cost-management-data"></a>Atribuir acesso a dados de gestão de custos

Para a maioria dos usuários, uma combinação de permissões concedidas no portal do Azure e o portal Enterprise (EA) definem o nível de um utilizador de acesso aos dados do Azure Cost Management. Este artigo orienta-embora a atribuir acesso a dados de gestão de custos. Após a combinação de permissões é atribuída, os dados de vistas de utilizador no Cost Management com a base do âmbito que têm acesso a e no âmbito que selecionarem no portal do Azure.

O âmbito do que um usuário selecionar é utilizado em toda a gestão de custos para fornecer a consolidação de dados e para controlar o acesso a informações de custo. Quando utilizar âmbitos, os utilizadores não seleção múltipla-los. Em vez disso, eles selecionam um âmbito maior que âmbitos subordinados até implementar e, em seguida, eles filtro pendente para o que quer ver. Consolidação de dados é importante compreender porque algumas pessoas não devem ter acesso a um âmbito principal que âmbitos subordinados até implementar.

## <a name="cost-management-scopes"></a>Âmbitos de gestão de custos

Para ver dados de custo, um utilizador tem de ter, pelo menos, acesso de leitura a um ou mais dos seguintes âmbitos.

| **Âmbito** | **Definido no** | **Acesso necessário para ver os dados** | **Definição de pré-requisitos de EA** | **Consolida os dados para** |
| --- | --- | --- | --- | --- |
| Conta de faturação<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Administrador da Empresa | Nenhuma | Todas as subscrições do contrato enterprise |
| Departamento | [https://ea.azure.com](https://ea.azure.com/) | Administrador do Departamento | **Custos de vista DA** ativada | Todas as subscrições que pertencem a uma conta de inscrição que está associada ao departamento |
| Conta de inscrição<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Proprietário da Conta | **Custos de exibição AO** ativada | Todas as subscrições da conta de inscrição |
| Grupo de gestão | [https://portal.azure.com](https://portal.azure.com/) | Leitor do Cost Management (ou Leitor) | **Custos de exibição AO** ativada | Todas as subscrições abaixo o grupo de gestão |
| Subscrição | [https://portal.azure.com](https://portal.azure.com/) | Leitor do Cost Management (ou Leitor) | **Custos de exibição AO** ativada | Todos os recursos/grupos de recursos na subscrição |
| Grupo de recursos | [https://portal.azure.com](https://portal.azure.com/) | Leitor do Cost Management (ou Leitor) | **Custos de exibição AO** ativada | Todos os recursos no grupo de recursos |

<sup>1</sup> a conta de cobrança também é referida como o Enterprise Agreement ou inscrição.

<sup>2</sup> a conta de inscrição é também referida como o proprietário da conta.

## <a name="enable-access-to-costs-in-the-ea-portal"></a>Ativar o acesso aos custos no portal de EA

Âmbito da conta de faturação requer o **cobranças de vista DA** opção **ativado** no portal do EA. Todos os outros âmbitos de exigem a **cobranças de exibição de pedidos** opção **ativado** no portal do EA.

Para ativar uma opção:

1. Inicie sessão no portal do EA em [ https://ea.azure.com ](https://ea.azure.com) com uma conta de administrador de empresa.
2. Selecione **gerir** no painel esquerdo.
3. Para a gestão de custos âmbitos que deseja fornecer acesso, ative a opção de custos para **cobranças de vista DA** e/ou **cobranças de exibição AO**.  
    ![Guia de inscrição que mostra a vista DA e AO cobra opções](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

Depois de opções de custo de visualização estiverem ativadas, a maioria dos âmbitos também necessitam de configuração de permissão do controlo (RBAC) de acesso baseado em funções no portal do Azure.

## <a name="enterprise-administrator-role"></a>Função de administrador de empresa

Por predefinição, o administrador da empresa tem acesso à conta de faturação (Enterprise Agreement/inscrição) e a todos os outros âmbitos, que são os âmbitos subordinados. O administrador de empresa atribui o acesso a âmbitos para outros utilizadores. Como melhor prática para continuidade do negócio, deve ter sempre dois utilizadores com acesso de administrador de empresa. As secções seguintes são exemplos passo a passo do acesso de atribuição de administrador empresarial a âmbitos para outros utilizadores.

## <a name="assign-billing-account-scope-access"></a>Atribuir acesso de âmbito de conta faturação

Acesso ao âmbito da conta de faturação requer permissão de administrador de empresa no portal do EA. O administrador de empresa tem acesso para ver os custos entre a inscrição de EA toda ou várias inscrições. É necessária nenhuma ação no portal do Azure para o âmbito da conta de faturação.

1. Inicie sessão no portal do EA em [ https://ea.azure.com ](https://ea.azure.com) com uma conta de administrador de empresa.
2. Selecione **gerir** no painel esquerdo.
3. Sobre o **inscrição** separador, selecione a inscrição que pretende gerir.  
    ![Portal EA](./media/assign-access-acm-data/ea-portal.png)
4. Clique em **+ Adicionar administrador**.
5. Na caixa de adicionar administrador, selecione o tipo de autenticação e escreva o endereço de e-mail do utilizador.
6. Se o utilizador deve ter acesso só de leitura aos dados de utilização e custos, em **só de leitura**, selecione **Sim**.  Caso contrário, selecione **não**.
7. Clique em **adicionar** para criar a conta.  
    ![Adicionar a caixa de administrador](./media/assign-access-acm-data/add-admin.png)

Pode demorar até 30 minutos para que o novo utilizador possa aceder a dados no Cost Management.

### <a name="assign-department-scope-access"></a>Atribuir acesso de âmbito do departamento

Acesso ao âmbito departamento requer o acesso de (custos de vista DA) do departamento administrador no portal do EA. O administrador do departamento tem acesso para ver os custos e os dados de utilização associado a um departamento ou para vários departamentos.  Os dados para o departamento de incluem todas as subscrições pertencentes a uma conta de inscrição que estejam ligadas ao departamento. É necessária nenhuma ação no portal do Azure.

1. Inicie sessão no portal do EA em [ https://ea.azure.com ](https://ea.azure.com) com uma conta de administrador de empresa.
2. Selecione **gerir** no painel esquerdo.
3. Sobre o **inscrição** separador, selecione a inscrição que pretende gerir.
4. Clique nas **departamento** separador e, em seguida, clique em **Adicionar administrador**.
5. Na caixa de adicionar administrador de departamento, selecione o tipo de autenticação e, em seguida, escreva o endereço de e-mail do utilizador.
6. Se o utilizador deve ter acesso só de leitura aos dados de utilização e custos, em **só de leitura**, selecione **Sim**.  Caso contrário, selecione **não**.
7. Selecione os departamentos que pretende conceder permissões administrativas do departamento para.
8. Clique em **adicionar** para criar a conta.  
    ![Adicionar a caixa de administrador do departamento](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Atribuir acesso de âmbito de conta de inscrição

Acesso ao âmbito da conta de inscrição requer acesso de (custos de exibição de pedidos) de proprietário de conta no portal de EA. O proprietário da conta pode ver os custos e os dados de utilização associados com uma conta de inscrição. Dados da conta de inscrição incluem todas as subscrições Azure associadas a inscrição. É necessária nenhuma ação no portal do Azure.

1. Inicie sessão no portal do EA em [ https://ea.azure.com ](https://ea.azure.com) com uma conta de administrador de empresa.
2. Selecione **gerir** no painel esquerdo.
3. Sobre o **inscrição** separador, selecione a inscrição que pretende gerir.
4. Clique nas **conta** separador e, em seguida, clique em **adicionar conta**.
5. Na caixa de adicionar conta, selecione o **departamento** associar a conta para ou deixá-la como não atribuídos.
6. Selecione o tipo de autenticação e escreva o nome da conta.
7. Escreva o endereço de e-mail do utilizador e, em seguida, opcionalmente, escreva o Centro de custos.
8. Clique em **adicionar** para criar a conta.  
    ![Adicionar caixa da conta](./media/assign-access-acm-data/add-account.png)

## <a name="assign-management-group-scope-access"></a>Atribuir acesso de âmbito do grupo de gestão

O acesso a um âmbito de grupo de gestão requer, pelo menos, permissão de leitor de gestão de custos (ou leitor). Configurar as permissões de ao grupo de gestão no portal do Azure. Tem de ter, pelo menos, permissão de contribuinte para o grupo de gestão para ativar o acesso para outras pessoas. E, também deve ter ativado a **cobranças de exibição AO** definição no portal do EA.

1. Inicie sessão no Portal do Azure em [http://portal.azure.com](http://portal.azure.com).
2. Selecione **todos os serviços** na barra lateral, procure _grupos de gestão_, em seguida, selecione **grupos de gestão**.
3. Selecione o grupo de gestão na hierarquia.
4. Junto ao nome do seu grupo de gestão, clique em **detalhes**.
5. Selecione **controlo de acesso (IAM)** no painel à esquerda.
6. Clique em **Adicionar**.
7. Sob **função**, selecione **leitor de gestão de custos**.
8. Sob **atribuir acesso aos**, selecione **utilizador do Azure AD, grupo ou aplicação**.
9. Para atribuir acesso, procure e, em seguida, selecione o utilizador.
10. Clique em **Guardar**.  
    ![Adicionar caixa permissões](./media/assign-access-acm-data/add-permissions.png)

## <a name="assign-subscription-scope-access"></a>Atribuir acesso de âmbito da subscrição

Acesso a uma subscrição requer, pelo menos, permissão de leitor de gestão de custos (ou leitor). Configurar as permissões de para uma subscrição no portal do Azure. Tem de ter, pelo menos, permissão de contribuinte à subscrição para ativar o acesso para outras pessoas. E, também deve ter ativado a **cobranças de exibição AO** definição no portal do EA.

1. Inicie sessão no Portal do Azure em [http://portal.azure.com](http://portal.azure.com).
2. Selecione **todos os serviços** na barra lateral, procure _subscrições_, em seguida, selecione **subscrições**.
3. Selecione a sua subscrição.
4. Selecione **controlo de acesso (IAM)** no painel à esquerda.
5. Clique em **Adicionar**.
6. Sob **função**, selecione **leitor de gestão de custos**.
7. Sob **atribuir acesso aos**, selecione **utilizador do Azure AD, grupo ou aplicação**.
8. Para atribuir acesso, procure e, em seguida, selecione o utilizador.
9. Clique em **Guardar**.

## <a name="assign-resource-group-scope-access"></a>Atribuir acesso de âmbito do grupo de recursos

O acesso a um grupo de recursos requer, pelo menos, permissão de leitor de gestão de custos (ou leitor). Configurar a permissão para um grupo de recursos no portal do Azure. Tem de ter, pelo menos, permissão de contribuinte para o grupo de recursos para ativar o acesso para outras pessoas. E, também deve ter ativado a **cobranças de exibição AO** definição no portal do EA.

1. Inicie sessão no Portal do Azure em [http://portal.azure.com](http://portal.azure.com).
2. Selecione **todos os serviços** na barra lateral, procure _grupos de recursos_, em seguida, selecione **grupos de recursos**.
3. Selecione o grupo de recursos.
4. Selecione **controlo de acesso (IAM)** no painel à esquerda.
5. Clique em **Adicionar**.
6. Sob **função**, selecione **leitor de gestão de custos**.
7. Sob **atribuir acesso aos**, selecione **utilizador do Azure AD, grupo ou aplicação**.
8. Para atribuir acesso, procure e, em seguida, selecione o utilizador.
9. Clique em **Guardar**.

## <a name="next-steps"></a>Passos Seguintes

- Se ainda não tiver concluído o primeira guia de introdução do Cost Management, leia-a em [começar a analisar os custos](quick-acm-cost-analysis.md).
