---
title: Ligar o Gestor de configuração ao Log Analytics | Microsoft Docs
description: Este artigo mostra os passos para ligar o Configuration Manager para análise de registos e iniciar a análise de dados.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 6bb59e4b63933500bc8571dca2422eec6c3456ee
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129762"
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Ligar o Gestor de configuração ao Log Analytics
Pode ligar o seu ambiente do System Center Configuration Manager ao Log Analytics do Azure para dados de coleção de dispositivos de sincronização e fazem referência a estas coleções na análise de registos e a automatização do Azure.  

## <a name="prerequisites"></a>Pré-requisitos

Análise de registos suporta ramo atual do System Center Configuration Manager, versão 1606 e planos superior.  

## <a name="configuration-overview"></a>Descrição geral de configuração
Os passos seguintes resumem os passos para configurar a integração do Configuration Manager com a análise de registos.  

1. No portal do Azure, registe o Configuration Manager como uma aplicação de aplicação Web e/ou Web API e certifique-se de que tem o ID de cliente e a chave secreta do cliente do registo do Azure Active Directory. Consulte [portal de utilização para criar do Active Directory principal de serviço e aplicação que pode aceder aos recursos](../azure-resource-manager/resource-group-create-service-principal-portal.md) para obter informações detalhadas sobre como efetuar este passo.
2. No portal do Azure, [conceder o Configuration Manager (a aplicação web registado) com permissão para aceder à análise de registos](#grant-configuration-manager-with-permissions-to-log-analytics).
3. No Configuration Manager, [adicionar uma ligação utilizando o Assistente para adicionar ligação OMS](#add-an-oms-connection-to-configuration-manager).
4. No Configuration Manager, [atualizar as propriedades de ligação](#update-oms-connection-properties) se a chave secreta do cliente ou a palavra-passe nunca expira ou se tenha perdida.
5. [Transfira e instale o Microsoft Monitoring Agent](#download-and-install-the-agent) no computador que executa a função de sistema de sites de ponto do Configuration Manager service ligação. O agente envia dados do Configuration Manager para a área de trabalho de análise de registos.
6. Na análise de registos, [importar coleções do Configuration Manager](#import-collections) como grupos de computadores.
7. Na análise de registos, ver dados do Configuration Manager como [grupos de computadores](log-analytics-computer-groups.md).

Pode ler mais sobre a ligação do Configuration Manager à OMS no [sincronizar os dados do Configuration Manager para o Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="grant-configuration-manager-with-permissions-to-log-analytics"></a>Conceda o Configuration Manager com permissões para análise de registos
No procedimento seguinte, pode conceder a *contribuinte* função na sua área de trabalho de análise de registos para a aplicação AD e um principal de serviço que criou anteriormente para o Configuration Manager.  Se ainda não tiver uma área de trabalho, consulte [criar uma área de trabalho no Log Analytics do Azure](log-analytics-quick-create-workspace.md) antes de continuar.  Isto permite que o Configuration Manager para autenticar e ligar à sua área de trabalho de análise de registos.  

> [!NOTE]
> Tem de especificar permissões na análise de registos do Configuration Manager. Caso contrário, receberá uma mensagem de erro quando utiliza o Assistente de configuração no Configuration Manager.
>

1. No portal do Azure, clique em **Todos os serviços**, que se encontra no canto superior esquerdo. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.<br><br> ![Portal do Azure](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Na lista de áreas de trabalho de análise de registos, selecione a área de trabalho para modificar.
3. No painel esquerdo, selecione **controlo de acesso (IAM)**.
4. Na página de controlo de acesso, clique em **adicionar** e **adicionar permissões** painel aparece.
5. No **adicionar permissões** painel, no **função** selecione na lista pendente a **contribuinte** função.  
6. Sob o **atribuir acesso** na lista pendente, selecione a aplicação do Configuration Manager que criou anteriormente no AD e, em seguida, clique em **OK**.  

## <a name="download-and-install-the-agent"></a>Transfira e instale o agente
Consulte o artigo [computadores Windows ligar para o serviço de análise de registos do Azure](log-analytics-agent-windows.md) para compreender os métodos disponíveis para instalar o Microsoft Monitoring Agent no computador que aloja o serviço do Configuration Manager função de sistema de sites de ponto de ligação.  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Adicionar uma ligação de OMS ao Configuration Manager
Para adicionar uma ligação do OMS, ambiente do Configuration Manager tem de ter um [ponto de ligação de serviço](https://technet.microsoft.com/library/mt627781.aspx) configurados para o modo online.

1. No **administração** área de trabalho do Configuration Manager, selecione **OMS conector**. Esta ação abre o **Adicionar Assistente de ligação do OMS**. Selecione **Seguinte**.
2. No **geral** ecrã, confirme que efetuou as seguintes ações e que que tenha os detalhes para cada item, em seguida, selecione **seguinte**.

   1. No portal do Azure, tiver registado do Configuration Manager como uma aplicação de aplicação Web e/ou Web API e de que tem o [ID de cliente a partir do registo](../active-directory/active-directory-integrating-applications.md).
   2. No portal do Azure, criou uma chave de segredo de aplicação para a aplicação registada no Azure Active Directory.  
   3. No portal do Azure, que forneceu a aplicação web registado com permissão para aceder à OMS.  
      ![Ligação à página geral de assistente do OMS](./media/log-analytics-sccm/sccm-console-general01.png)
3. No **do Azure Active Directory** ecrã, configure as definições de ligação à análise de registos, fornecendo o **inquilino**, **ID de cliente**, e **cliente A chave secreta**, em seguida, selecione **seguinte**.  
   ![Ligação à página OMS Assistente do Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Se lhe conseguido todos os outros procedimentos com êxito, em seguida, as informações no **configuração da ligação OMS** ecrã serão apresentadas automaticamente nesta página. As informações para as definições de ligação devem aparecer para sua **subscrição do Azure**, **grupo de recursos do Azure**, e **área de trabalho do Operations Management Suite**.  
   ![Ligação à página de ligação do OMS assistente OMS](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. O assistente liga ao serviço de análise de registos, utilizando as informações que tenha de entrada. Selecione as coleções de dispositivos que pretende sincronizar com o serviço e, em seguida, clique em **adicionar**.  
   ![Selecione as coleções](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Verifique as definições de ligação no **resumo** ecrã, em seguida, selecione **seguinte**. O **progresso** ecrã mostra o estado da ligação, em seguida, deve **concluída**.

> [!NOTE]
> Tem de ligar o site de nível superior da hierarquia para análise de registos. Se ligar um site primário autónomo para análise de registos e, em seguida, adicionar um site de administração central ao seu ambiente, tem de eliminar e recriar a ligação dentro da nova hierarquia.
>
>

Depois de ligação do Configuration Manager a análise de registos, pode adicionar ou remover coleções e ver as propriedades da ligação.

## <a name="update-log-analytics-connection-properties"></a>Atualizar propriedades da ligação de análise de registos
Se uma chave secreta de cliente ou a palavra-passe nunca expira ou se tenha perdida, terá de atualizar manualmente as propriedades de ligação de análise de registos.

1. No Configuration Manager, navegue para **serviços em nuvem**, em seguida, selecione **OMS conector** para abrir o **propriedades de ligação do OMS** página.
2. Nesta página, clique em de **do Azure Active Directory** separador para ver o **inquilino**, **ID de cliente**, **expiração chave secreta de cliente**. **Certifique-se** sua **chave secreta do cliente** se tiver expirado.

## <a name="import-collections"></a>Importar coleções
Depois de ter uma ligação de OMS foi adicionada para o Configuration Manager e o agente instalado no computador com a ligação de serviço do Configuration Manager função do sistema de sites do ponto, o passo seguinte é para importar coleções do Configuration Manager na análise de registos como grupos de computadores.

Depois de concluir a configuração inicial para importar coleções de dispositivos da sua hierarquia, as informações de associação da coleção são obtidas todos os 3 horas para manter a associação actual. Pode optar por desativar este em qualquer altura.

1. No portal do Azure, clique em **Todos os serviços**, que se encontra no canto superior esquerdo. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. Na lista de áreas de trabalho de análise de registos, selecione a área de trabalho do Configuration Manager está registado.  
3. Selecione **Definições avançadas**.<br><br> ![Definições Avançadas do Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecione **grupos de computadores** e, em seguida, selecione **SCCM**.  
5. Selecione **associações de coleção do Gestor de configuração da importação** e, em seguida, clique em **guardar**.  
   ![Grupos de computadores - separador SCCM](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Ver dados do Configuration Manager
Depois de ter uma ligação de OMS foi adicionada para o Configuration Manager e o agente foi instalado no computador com a função de sistema de sites de ponto do Configuration Manager service ligação, dados do agente são enviados para análise de registos. Na análise de registos, as coleções do Configuration Manager aparecem como [grupos de computadores](log-analytics-computer-groups.md). Pode ver os grupos do **do Configuration Manager** página em **Settings\Computer grupos**.

Depois das coleções são importadas, pode ver que foram detetados como quantos computadores com associações de coleção. Também pode ver o número de coleções que tenham sido importados.

![Grupos de computadores - separador SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Ao clicar em qualquer um, abre-se procurar, apresentar o todas os grupos importados ou todos os computadores que pertencem a cada grupo. Utilizar [pesquisa registo](log-analytics-log-searches.md), pode iniciar uma análise aprofundada de dados do Configuration Manager.

## <a name="next-steps"></a>Passos Seguintes
* Utilize [pesquisa registo](log-analytics-log-searches.md) para ver informações detalhadas sobre os dados do Configuration Manager.
