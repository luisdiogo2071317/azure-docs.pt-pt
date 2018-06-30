---
title: Gerir áreas de trabalho no Log Analytics do Azure e no portal do OMS | Microsoft Docs
description: Pode gerir áreas de trabalho no Log Analytics do Azure e no Portal do OMS com uma variedade de tarefas administrativas em utilizadores, contas, áreas de trabalho e contas do Azure.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 3b4e0f978cc7d23d0157b78fd2dff27096d2768b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133018"
---
# <a name="manage-workspaces"></a>Gerir áreas de trabalho

Para gerir o acesso ao Log Analytics, tem de realizar diversas tarefas administrativas relacionadas com áreas de trabalho. Este artigo fornece conselhos sobre as melhores práticas e procedimentos para gerir áreas de trabalho. Uma área de trabalho é essencialmente um contentor que inclui informações da conta e informações de configuração simples para a conta. O utilizador ou outros membros da sua organização podem utilizar várias áreas de trabalho para gerir diferentes conjuntos de dados recolhidos da totalidade ou de partes da sua infraestrutura de TI.

Para criar uma área de trabalho, terá de:

1. Ter uma subscrição do Azure.
2. Escolher um nome de área de trabalho.
3. Associe área de trabalho com uma das suas subscrições e grupos de recursos.
4. Escolher uma localização geográfica.

## <a name="determine-the-number-of-workspaces-you-need"></a>Determinar o número de áreas de trabalho necessárias
Uma área de trabalho é um recurso do Azure e é um contentor onde os dados são recolhidos, agregados, analisados e apresentados no portal do Azure.

Pode ter múltiplas áreas de trabalho por subscrição do Azure e ter acesso a mais de uma área de trabalho, com capacidade para consultá-las facilmente. Esta secção descreve quando pode ser útil criar mais do que uma área de trabalho.

Atualmente, uma área de trabalho fornece:

* Uma localização geográfica para armazenamento de dados
* Isolamento de dados para definir os direitos de acesso de utilizador diferente
* Âmbito para a configuração das definições como a retenção e dados capping

Do consumo ponto de vista, recomendamos que criam áreas de trabalho como alguns quanto possível. Torna a administração e as experiências de consulta mais fácil e mais rápida. No entanto, com base nas características anteriores, pode querer criar várias áreas de trabalho se:

* For uma empresa global e precisar de dados armazenados em regiões específicas por motivos de soberania ou conformidade dos dados.
* Estiver a utilizar o Azure e pretender evitar custos de transferência de dados de saída ao ter uma área de trabalho na mesma região dos recursos do Azure que gere.
* Pretender atribuir custos a diferentes departamentos ou grupos empresariais com base na respetiva utilização. Quando criar uma área de trabalho para cada departamento ou grupo empresarial na sua própria subscrição do Azure.
* É um fornecedor de serviços geridos e necessita de manter os dados do Log Analytics de cada cliente gerido isolado em relação aos de outros clientes.
* Gerir vários clientes e pretender que cada cliente, departamento ou grupo empresarial veja os seus próprios dados, mas não os dados de outros.

Quando utilizar agentes do Windows para recolher dados, pode [configurar cada agente para comunicar a uma ou mais áreas de trabalho](log-analytics-windows-agents.md).

Se estiver a utilizar o System Center Operations Manager, cada grupo de gestão do Operations Manager só pode ser ligado a uma área de trabalho. Pode instalar o Microsoft Monitoring Agent em computadores geridos pelo Operations Manager e fazer com que o agente reporte ao Operations Manager e a uma área de trabalho do Log Analytics diferente.

### <a name="workspace-information"></a>Informações da área de trabalho

Pode ver detalhes sobre a sua área de trabalho no portal do Azure. 

#### <a name="view-workspace-information-in-the-azure-portal"></a>Ver informações da área de trabalho no portal do Azure

1. Se ainda não o fez, inicie sessão no [portal do Azure](https://portal.azure.com) através da sua subscrição do Azure.
2. No menu **Hub**, clique em **Mais serviços** e, na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Clique em **Log Analytics**.  
    ![Hub do Azure](./media/log-analytics-manage-access/hub.png)  
3. No painel de subscrições do Log Analytics, selecione uma área de trabalho.
4. O painel da área de trabalho apresenta detalhes sobre a área de trabalho e ligações para informações adicionais.  
    ![detalhes da área de trabalho](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Gerir contas e utilizadores
Cada área de trabalho pode ter várias contas associadas e cada conta pode ter acesso a várias áreas de trabalho. Acesso é gerido através de [acesso baseado em funções do Azure](../active-directory/role-based-access-control-configure.md). Estes direitos de acesso aplica-se no portal do Azure e o acesso através da API.


As atividades seguintes também necessitam de permissões do Azure:

| Ação                                                          | Permissões do Azure Necessárias | Notas |
|-----------------------------------------------------------------|--------------------------|-------|
| Adicionar e remover soluções de gestão                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Estas permissões têm de ser concedidas ao nível do grupo de recursos ou da subscrição. |
| Alterar o escalão de preço                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Ver dados nos mosaicos de solução *Backup* e *Site Recovery* | Administrador/Coadministrador | Acede aos recursos implementados com o modelo de implementação clássica |
| Criar um espaço de trabalho no portal do Azure                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Gerir o acesso ao Log Analytics com permissões do Azure
Para conceder acesso à área de trabalho do Log Analytics com permissões do Azure, siga os passos em [Utilize atribuições de funções para gerir o acesso aos recursos de subscrição do Azure](../active-directory/role-based-access-control-configure.md).

O Azure tem duas funções de utilizador incorporadas para o Log Analytics:
- Leitor do Log Analytics
- Contribuidor do Log Analytics

Os membros da função *Leitor do Log Analytics* podem:
- Ver e procurar todos os dados de monitorização 
- Ver e monitorizar as definições, incluindo ver a configuração dos diagnósticos do Azure em todos os recursos do Azure.

A função de leitor de análise do registo inclui as seguintes ações do Azure:

| Tipo    | Permissão | Descrição |
| ------- | ---------- | ----------- |
| Ação | `*/read`   | Capacidade para ver todos os recursos do Azure e a configuração do recurso. Inclui ver: <br> o estado da extensão da máquina virtual <br> a configuração dos diagnósticos do Azure nos recursos <br> todas as propriedades e definições de todos os recursos |
| Ação | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Capacidade para executar consultas Log Search v2 |
| Ação | `Microsoft.OperationalInsights/workspaces/search/action` | Capacidade para executar consultas Log Search v1 |
| Ação | `Microsoft.Support/*` | Capacidade para abrir pedidos de suporte |
|Ação Não | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Impede a leitura da área de trabalho chave necessária para utilizar a API de recolha de dados e para instalar agentes. Isto impede que o utilizador a adição de novos recursos para a área de trabalho |


Os membros da função *Contribuidor do Log Analytics* podem:
- Ler todos os dados de monitorização, como pode de leitor de análise do registo  
- Criar e configurar contas de Automatização  
- Adicionar e remover soluções de gestão    
    > [!NOTE] 
    > Para efetuar com êxito as últimas duas ações, esta permissão tem de ser concedidas ao nível de grupo ou de subscrição do recurso.  

- Ler as chaves das contas de armazenamento   
- Configurar a recolha de registos a partir do Armazenamento do Azure  
- Editar as definições de monitorização para recursos do Azure, incluindo
  - Adicionar a extensão de VM a VMs
  - Configurar os diagnósticos do Azure em todos os recursos do Azure

> [!NOTE] 
> Pode utilizar a capacidade de adicionar uma extensão de máquina virtual a máquinas virtuais para obter o controlo total das mesmas.

A função de contribuinte de análise do registo inclui as seguintes ações do Azure:

| Permissão | Descrição |
| ---------- | ----------- |
| `*/read`     | Capacidade para ver todos os recursos e configuração dos mesmos. Inclui ver: <br> o estado da extensão da máquina virtual <br> a configuração dos diagnósticos do Azure nos recursos <br> todas as propriedades e definições de todos os recursos |
| `Microsoft.Automation/automationAccounts/*` | Capacidade para criar, e configurar contas de Automatização do Azure, incluindo adicionar e editar runbooks |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Adicionar, atualizar e remover extensões de máquinas virtuais, incluindo a extensão Microsoft Monitoring Agent e o Agente do OMS para a extensão do Linux |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Ver a chave da conta de armazenamento. Necessária para configurar o Log Analytics para ler registos a partir das contas de Armazenamento do Azure |
| `Microsoft.Insights/alertRules/*` | Adicionar, atualizar e remover regras de alertas |
| `Microsoft.Insights/diagnosticSettings/*` | Adicionar, atualizar e remover as definições de diagnósticos em recursos do Azure |
| `Microsoft.OperationalInsights/*` | Adicionar, atualizar e remover a configuração de áreas de trabalho do Log Analytics |
| `Microsoft.OperationsManagement/*` | Adicionar e remover soluções de gestão |
| `Microsoft.Resources/deployments/*` | Criar e eliminar as implementações. Necessário para adicionar e remover soluções, áreas de trabalho e contas de automatização |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Criar e eliminar as implementações. Necessário para adicionar e remover soluções, áreas de trabalho e contas de automatização |

Para adicionar e remover utilizadores de uma função de utilizador, é necessário ter as permissões `Microsoft.Authorization/*/Delete` e `Microsoft.Authorization/*/Write`.

Utilize estas funções para conceder aos utilizadores acesso em âmbitos diferentes:
- Subscrição - acesso a todas as áreas de trabalho da subscrição
- Grupo de Recursos - acesso a todas as áreas de trabalho no grupo de recursos
- Recurso - acesso apenas à área de trabalho especificada

Recomendamos que realiza as atribuições ao nível do recurso (área de trabalho), para garantir o controlo de acesso correto.  Utilize [funções personalizadas](../active-directory/role-based-access-control-custom-roles.md) para criar funções com as permissões específicas necessárias.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Ligar uma área de trabalho existente a uma subscrição do Azure
Todas as áreas de trabalho criadas depois de 26 de setembro de 2016 têm de ser associadas a uma subscrição do Azure no momento da criação. As áreas de trabalho criadas antes desta data têm de ser associadas a uma área de trabalho quando iniciar sessão. Ao criar a área de trabalho a partir do portal do Azure ou ao ligar a sua área de trabalho a uma subscrição do Azure, o Azure Active Directory é ligado como a sua conta da organização.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Para ligar uma área de trabalho a uma subscrição do Azure no portal do Azure
1. Inicie sessão no [Portal do Azure](http://portal.azure.com).
2. Procure o **Log Analytics** e selecione-o.
3. Verá a lista de áreas de trabalho existentes. Clique em **Adicionar**.  
   ![lista de áreas de trabalho](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. Em **Área de Trabalho do OMS**, clique em **Ou ligação existente**.  
   ![ligação existente](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Clique em **Configurar definições necessárias**.  
   ![configurar definições necessárias](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Verá a lista de áreas de trabalho que ainda não estão ligadas à sua conta do Azure. Selecione uma área de trabalho.  
   ![selecionar áreas de trabalho](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Se for necessário, pode alterar os valores dos seguintes itens:
   * Subscrição
   * Grupo de recursos
   * Localização
   * Escalão de preço  
     ![alterar valores](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Clique em **OK**. A área de trabalho está agora ligada à sua conta do Azure.

> [!NOTE]
> Se não visualizar a área de trabalho que pretende ligar, a sua subscrição do Azure não tem acesso à área de trabalho que criou com o portal do OMS.  Para conceder acesso a esta conta a partir do portal do OMS, consulte [Add a user to an existing workspace (Adicionar um utilizador a uma área de trabalho existente)](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Atualizar uma área de trabalho para um plano pago
Existem três tipos de planos de área de trabalho para o OMS: **Gratuito**, **Standard** e **OMS**.  Se estiver no plano *Gratuito*, existe um limite de 500 MB de dados por dia enviados para o Log Analytics.  Se exceder este montante, terá de alterar a sua área de trabalho para um plano pago para evitar deixar de recolher dados para lá deste limite. Pode alterar o tipo de plano em qualquer momento.  Para obter mais informações sobre os preços do OMS, veja [Detalhes dos Preços](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Utilizar a elegibilidade de uma subscrição do OMS
Para utilizar a elegibilidade incluída na compra de OMS E1, OMS E2 OMS ou OMS Add-On for System Center, escolha o plano *OMS* do OMS Log Analytics.

Quando compra uma subscrição do OMS, as elegibilidades são adicionadas ao seu Contrato Enterprise. Qualquer subscrição do Azure criada ao abrigo deste contrato pode utilizar as elegibilidades. Todas as áreas de trabalho nestas subscrições utilizam as elegibilidades do OMS.

Para garantir que a utilização de uma área de trabalho é aplicada às suas elegibilidades da subscrição do OMS, tem de:

1. Criar a área de trabalho numa subscrição do Azure que faça parte do Contrato Enterprise que inclui a subscrição do OMS
2. Selecionar o plano *OMS* para a área de trabalho

> [!NOTE]
> Se a área de trabalho tiver sido criada antes de 26 de setembro de 2016 e o seu plano de preços do Log Analytics for o *Premium*, esta utiliza as elegibilidades do OMS Add-On for System Center. Também pode alterar para o escalão de preços *OMS* para utilizar as elegibilidades.
>
>

As elegibilidades da subscrição do OMS não estão visíveis no portal do Azure nem do OMS. Pode ver as elegibilidades e a utilização no Portal da Empresa.  

Se precisar de alterar a subscrição do Azure à qual está ligada a área de trabalho, pode utilizar o cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) do Azure PowerShell.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Utilizar o Azure Commitment a partir de um Contrato Enterprise
Se não tiver uma subscrição do OMS, paga cada componente do OMS em separado e a utilização aparece na fatura do Azure.

Se tiver uma alocação monetária do Azure na inscrição empresarial à qual as subscrições do Azure estão associadas, qualquer utilização do Log Analytics será debitada automaticamente na alocação monetária restante.

Se precisar de alterar a subscrição do Azure à qual está ligada a área de trabalho, pode utilizar o cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) do Azure PowerShell.  

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-azure-portal"></a>Alterar uma área de trabalho para um escalão de preço pago no portal do Azure
1. Inicie sessão no [Portal do Azure](http://portal.azure.com).
2. Procure o **Log Analytics** e selecione-o.
3. Verá a lista de áreas de trabalho existentes. Selecione uma área de trabalho.  
4. No painel da área de trabalho, em **Geral**, clique em **Escalão de preço**.  
5. Em **Escalão de preço**, selecione um escalão de preço e, em seguida, clique em **Selecionar**.  
    ![selecionar plano](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Quando atualizar a vista no portal do Azure, verá o **Escalão de preço** atualizado com o escalão que selecionou.  
    ![plano atualizado](./media/log-analytics-manage-access/manage-access-change-plan04.png)

> [!NOTE]
> Se a sua área de trabalho estiver ligada a uma conta de Automatização, antes poder selecionar o escalão de preço *Autónomo (Por GB)*, tem de eliminar quaisquer soluções de **Automatização e Controlo** e desassociar a conta de Automatização. No painel da área de trabalho, em **Geral**, clique em **Soluções** para ver e eliminar soluções. Para desassociar a Conta de automatização, clique no nome da Conta de automatização no painel **Escalão de preços**.
>
>

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-oms-portal"></a>Alterar uma área de trabalho para um escalão de preço pago no portal do OMS

Para alterar o escalão de preço através do portal do OMS, tem de ter uma subscrição do Azure.

1. No portal do OMS, clique no mosaico **Definições**.
2. Clique no separador **Contas** e, em seguida, clique no separador **Subscrição e Plano de Dados do Azure**.
3. Clique no escalão de preço que quer utilizar.
4. Clique em **Guardar**.  
   ![subscrição e planos de dados](./media/log-analytics-manage-access/subscription-tab.png)

O novo plano de dados é apresentado no friso do portal do OMS na parte superior da página Web.

![Friso do OMS](./media/log-analytics-manage-access/data-plan-changed.png)


## <a name="change-how-long-log-analytics-stores-data"></a>Alterar o tempo durante o qual o Log Analytics armazena dados

No escalão de preço Gratuito, o Log Analytics disponibiliza os últimos sete dias de dados.
No escalão de preço Standard, o Log Analytics disponibiliza os últimos 30 dias de dados.
No escalão de preço Premium, o Log Analytics disponibiliza os últimos 365 dias de dados.
Nos escalões de preço Autónomo e OMS, por predefinição, o Log Analytics disponibiliza os últimos 31 dias de dados.

Quando utiliza os escalões de preço Autónomo e OMS, pode manter um máximo de dois anos de dados (730 dias). Os dados armazenados por mais tempo do que a predefinição de 31 dias incorrem numa cobrança de retenção de dados. Para obter mais informações sobre preços, veja o artigo [Custos da utilização excessiva](https://azure.microsoft.com/pricing/details/log-analytics/).

Para alterar o período de retenção de dados, consulte [Gerir custos ao controlar o volume de dados e a retenção no Log Analytics](log-analytics-manage-cost-storage.md).


## <a name="delete-a-log-analytics-workspace"></a>Eliminar uma área de trabalho do Log Analytics
Ao eliminar uma área de trabalho do Log Analytics, todos os dados relacionados com a mesma são eliminados do serviço do Log Analytics num prazo de 30 dias.

Se for um administrador e existirem vários utilizadores associados à área de trabalho, a associação entre esses utilizadores e a área de trabalho é interrompida. Se os utilizadores estiverem associados a outras áreas de trabalho, podem continuar a utilizar o Log Analytics com essas áreas de trabalho. No entanto, se não estiverem associados a outras áreas de trabalho, terão de criar uma área de trabalho para utilizar o serviço. Para eliminar uma área de trabalho, consulte [Eliminar uma área de trabalho do Log Analytics do Azure](log-analytics-manage-del-workspace.md)

## <a name="next-steps"></a>Passos Seguintes
* Consulte [Recolher dados de computadores no seu ambiente com o Log Analytics](log-analytics-concept-hybrid.md) para recolher dados de computadores no seu centro de dados ou outro ambiente de cloud.
* Consulte [Recolher dados sobre Máquinas Virtuais do Azure](log-analytics-quick-collect-azurevm.md) para configurar a recolha de dados a partir de VMs do Azure.  
* [Adicionar soluções do Log Analytics a partir da Galeria de Soluções](log-analytics-add-solutions.md) para adicionar funcionalidade e recolher dados.

