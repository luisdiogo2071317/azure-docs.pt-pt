---
title: "Tarefas de gestão do serviço de nuvem comuns | Microsoft Docs"
description: Saiba como gerir Cloud Services no portal do Azure. Estes exemplos utilizam o portal do Azure.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: e60bf5c82e68d49abaa44d80ac9fafba2d8265da
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="manage-cloud-services-in-the-azure-portal"></a>Gerir serviços em nuvem no portal do Azure
No **serviços em nuvem** área do portal do Azure, pode:

* Uma função de serviço ou uma implementação de atualização.
* Promova uma implementação faseada para produção.
* Ligar recursos ao seu serviço de nuvem, para que possa ver as dependências de recursos e dimensionar os recursos em conjunto.
* Elimine um serviço em nuvem ou de uma implementação.

Para obter mais informações sobre como dimensionar o serviço de nuvem, consulte [configurar o dimensionamento automático para um serviço em nuvem no portal do](cloud-services-how-to-scale-portal.md).

## <a name="update-a-cloud-service-role-or-deployment"></a>Uma função de serviço de nuvem ou implementação de atualização
Se precisar de atualizar o código de aplicação do serviço em nuvem, utilize **atualizar** no painel de serviço em nuvem. Pode atualizar uma única função ou todas as funções. Para atualizar, pode carregar um ficheiro de configuração do serviço ou o novo pacote de serviço.

1. No [portal do Azure][Azure portal], selecione o serviço de nuvem que pretende atualizar. Este passo é aberto o painel de instância de serviço de nuvem.

2. No painel, selecione **atualização**.

    ![Botão de atualização](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Atualize a implementação com um novo ficheiro de pacote de serviço (. cspkg) e o ficheiro de configuração de serviço (. cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. Opcionalmente, atualize a conta de armazenamento e a etiqueta de implementação.

5. Se quaisquer funções tem apenas uma instância de função, selecione o **implementar mesmo que uma ou mais funções contenham uma única instância** caixa de verificação para ativar a atualização continuar.

    Azure pode garantir a disponibilidade do serviço apenas por cento de 99,95 durante uma atualização de serviço em nuvem se cada função tem, pelo menos, duas instâncias de função (máquinas virtuais). Com duas instâncias de função, uma máquina virtual processa os pedidos de cliente enquanto o outro está atualizado.

6. Selecione o **iniciar a implementação** caixa de verificação para aplicar a atualização após o carregamento do pacote foi concluído.

7. Selecione **OK** para começar a atualizar o serviço.

## <a name="swap-deployments-to-promote-a-staged-deployment-to-production"></a>Comutação implementações para promover uma implementação faseada para produção
Quando optar por uma nova versão de um serviço em nuvem, a fase de implementar e testar a nova versão no ambiente de teste do serviço na nuvem. Utilize **comutação** mudar os URLs através do qual as duas implementações são tratadas e promover uma nova versão para produção.

Pode trocar entre implementações o **serviços em nuvem** página ou dashboard.

1. No [portal do Azure][Azure portal], selecione o serviço de nuvem que pretende atualizar. Este passo é aberto o painel de instância de serviço de nuvem.

2. No painel, selecione **comutação**.

    ![Botão de comutação de serviços cloud](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Abre o pedido de confirmação seguinte:

    ![Troca de serviços cloud](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Depois de verificar as informações de implementação, selecione **OK** ao trocar as implementações.

    A troca de implementação acontece rapidamente porque a única coisa que altera os endereços IP virtuais (VIP) para as implementações.

    Para guardar os custos de computação, pode eliminar a implementação de teste depois de verificar que a implementação de produção está a funcionar conforme esperado.

### <a name="common-questions-about-swapping-deployments"></a>Perguntas comuns sobre a troca de implementações

**Quais são os pré-requisitos para a troca de implementações?**

Existem dois chaves pré-requisitos para uma troca de implementação efetuada com êxito:

- Se pretender utilizar um endereço IP estático para a ranhura de produção, tem de reservar uma para o bloco de transição. Caso contrário, a troca falha.

- Todas as instâncias das suas funções devem estar em execução antes de efetuar a troca. Pode verificar o estado das suas instâncias no **descrição geral** painel do portal do Azure. Em alternativa, pode utilizar o [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) comando no Windows PowerShell.

Tenha em atenção que as atualizações de SO convidado e serviço autorrecuperação operações também podem fazer com trocas de implementação falhe. Para obter mais informações, consulte [problemas de implementação de serviço de nuvem de resolução de problemas](cloud-services-troubleshoot-deployment-problems.md).

**Uma troca implica o período de indisponibilidade para a minha aplicação? Como posso processá-lo?**

Tal como descrito na secção anterior, uma troca de implementação é rápida, normalmente, porque é apenas uma alteração de configuração de Balanceador de carga do Azure. Em alguns casos, poderá demorar 10 ou mais segundos e o resultado em falhas de ligação transitório. Para limitar o impacto para os seus clientes, considere implementar [lógica de repetição do cliente](../best-practices-retry-general.md).

## <a name="delete-deployments-and-a-cloud-service"></a>Eliminar as implementações e um serviço em nuvem
Antes de poder eliminar um serviço em nuvem, tem de eliminar cada implementação existente.

Para guardar os custos de computação, pode eliminar a implementação de teste depois de verificar que a implementação de produção está a funcionar conforme esperado. É-lhe faturado para custos de computação implementado as instâncias de função que são parados.

Utilize o procedimento seguinte para eliminar uma implementação ou o serviço em nuvem.

1. No [portal do Azure][Azure portal], selecione o serviço de nuvem que pretende eliminar. Este passo é aberto o painel de instância de serviço de nuvem.

2. No painel, selecione **eliminar**.

    ![Botão de eliminar dos serviços cloud](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Para eliminar o serviço de integralmente na nuvem, selecione o **serviço e as respetivas implementações de nuvem** caixa de verificação. Ou pode escolher uma o **implementação de produção** ou **implementação faseada** caixa de verificação.

    ![Eliminação de serviços cloud](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Selecione **eliminar** na parte inferior.

5. Para eliminar o serviço em nuvem, selecione **eliminar o serviço em nuvem**. Em seguida, na linha de confirmação, selecione **Sim**.

> [!NOTE]
> Quando um serviço em nuvem é eliminado e monitorização verboso estiver configurado, terá de eliminar os dados manualmente da sua conta de armazenamento. Para obter informações sobre onde encontrar as tabelas de métricas, consulte [introdução à monitorização do serviço de nuvem](cloud-services-how-to-monitor.md).


## <a name="find-more-information-about-failed-deployments"></a>Localizar mais informações sobre implementações falhadas
O **descrição geral** painel tem uma barra de estado na parte superior. Quando seleciona a barra, um novo painel abre-se e apresenta as informações de erro. Se a implementação não contém quaisquer erros, o painel de informações é em branco.

![Descrição geral dos serviços de nuvem](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Passos Seguintes
* [Configuração geral do seu serviço de nuvem](cloud-services-how-to-configure-portal.md).
* Saiba como [implementar um serviço em nuvem](cloud-services-how-to-create-deploy-portal.md).
* Configurar um [nome de domínio personalizado](cloud-services-custom-domain-name-portal.md).
* Configurar [certificados SSL](cloud-services-configure-ssl-certificate-portal.md).
