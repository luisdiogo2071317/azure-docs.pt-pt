---
title: Monitorizar o estado de funcionamento e alertas no Azure Stack | Documentos da Microsoft
description: Saiba como monitorizar o estado de funcionamento e alertas no Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/10/2018
ms.author: mabrigg
ms.openlocfilehash: 69ed08e8f6c820790c432bfa25988e038fd0efbd
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379694"
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Monitorizar o estado de funcionamento e alertas no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

O Azure Stack inclui capacidades que permitem-lhe ver o estado de funcionamento e alertas para uma região do Azure Stack de monitorização da infraestrutura. O **gestão da região** fixação, por predefinição no portal do administrador para a subscrição do fornecedor padrão, apresenta uma lista de todas as regiões do Azure Stack implementadas. O mosaico mostra o número de alertas críticos e de aviso ativos para cada região e é o ponto de entrada para o estado de funcionamento e a funcionalidade de alerta do Azure Stack.

 ![O mosaico de gestão da região](media/azure-stack-monitor-health/image1.png)

 ## <a name="understand-health-in-azure-stack"></a>Compreender o estado de funcionamento no Azure Stack

 Estado de funcionamento e alertas são geridos pelo fornecedor de recursos de estado de funcionamento. Componentes de infraestrutura do Azure Stack registe-se com o fornecedor de estado de funcionamento de recursos durante a implementação do Azure Stack e a configuração. Este registo permite a exibição do Estado de funcionamento e alertas para cada componente. Estado de funcionamento no Azure Stack é um conceito simple. Se existirem alertas para uma instância registrado de um componente, o estado de funcionamento desse componente reflete a gravidade do alerta ativada pior; aviso ou crítico.

## <a name="alert-severity-definition"></a>Definição de gravidade do alerta

No Azure Stack os alertas são gerados com apenas dois gravidades: **aviso** e **críticos**.

- **Aviso**  
  Um operador pode resolver o alerta de aviso de forma agendada. O alerta, normalmente, não afeta as cargas de trabalho do utilizador.

- **Crítico**  
  Um operador deve resolver o alerta crítico com urgência. Estes são problemas que atualmente afetam ou em breve irão afetar a utilizadores do Azure Stack. 

 
 ## <a name="view-and-manage-component-health-state"></a>Ver e gerir o estado de funcionamento do componente
 
 Como um operador do Azure Stack, pode ver o estado de funcionamento dos componentes no portal do administrador e através da REST API e PowerShell.
 
Para ver o estado de funcionamento no portal, clique a região que deseja ver no **gestão da região** mosaico. Pode ver o estado de funcionamento das funções de infraestrutura e de fornecedores de recursos.

![Lista de funções de infraestrutura](media/azure-stack-monitor-health/image2.png)

Pode clicar num fornecedor de recursos ou uma função de infraestrutura para ver informações mais detalhadas.

> [!WARNING]  
> Se clique numa função de infraestrutura e, em seguida, clique na instância de função, existem opções para iniciar, reiniciar ou encerrar. Não utilize estas ações ao aplicar as atualizações para um sistema integrado. Além disso, execute **não** utilizar estas opções num ambiente do Kit de desenvolvimento do Azure Stack. Estas opções destinam-se apenas para um ambiente de sistemas integrados, onde há mais de uma instância de função por função de infraestrutura. Reiniciar uma instância de função (especialmente AzS-Xrp01) do Kit de desenvolvimento, faz com que instabilidade do sistema. Para obter assistência de resolução de problemas, publique o seu problema para o [fórum do Azure Stack](https://aka.ms/azurestackforum).
>
 
## <a name="view-alerts"></a>Ver alertas

A lista de alertas ativos para cada região do Azure Stack está disponível diretamente a partir da **gestão da região** painel. O primeiro mosaico na configuração padrão é o **alertas** mosaico, que apresenta um resumo do críticos e alertas de aviso para a região. Pode afixar o mosaico alertas, como qualquer outro mosaico neste painel, ao dashboard para acesso rápido.   

![Mosaico que mostra um aviso de alertas](media/azure-stack-monitor-health/image3.png)

Ao selecionar a parte superior dos **alertas** mosaico, navegue para a lista de todos os alertas ativos para a região. Se selecione o **crítico** ou **aviso** item de linha do quadrado, navegue para uma lista filtrada de alertas (crítico ou de aviso). 

![Alertas de aviso filtrado](media/azure-stack-monitor-health/image4.png)
  
O **alertas** painel suporta a capacidade de filtrar no Estado (ativo ou fechado) e a gravidade a (crítico ou de aviso). A vista predefinida apresenta todos os alertas ativos. Todos os alertas fechados são removidos do sistema após sete dias.

![Painel de filtro para filtrar por críticas ou de estado de aviso](media/azure-stack-monitor-health/image5.png)

O **vista de API** ação apresenta a API de REST que foi utilizado para gerar a vista de lista. Esta ação fornece uma forma rápida de se familiarizar com a sintaxe de REST API que pode utilizar a alertas de consulta. Pode utilizar esta API na automatização ou para integração com o seu datacenter existente de monitorização, relatórios e soluções de emissão de permissões. 

![A opção de API de modo de exibição que mostra a API REST](media/azure-stack-monitor-health/image6.png)

Pode clicar num alerta específico para ver os detalhes do alerta. Os detalhes do alerta mostram todos os campos que estão associados com o alerta e habilitar a navegação rápida para o componente afetado e a origem do alerta. Por exemplo, o alerta seguinte ocorre se uma das instâncias de função de infraestrutura fica offline ou não está acessível.  

![O painel de detalhes do alerta](media/azure-stack-monitor-health/image7.png)

Depois da instância de função de infraestrutura esteja novamente online, este alerta fecha automaticamente. Alerta de muitos, mas nem todos os fecha automaticamente quando o problema subjacente for resolvido. Recomendamos que seleciona **fechar alerta** depois de efetuar os passos de remediação. Se o problema persistir, o Azure Stack gera um novo alerta. Se resolver o problema, o alerta continua fechado e não requer nenhuma ação adicional.

## <a name="next-steps"></a>Passos Seguintes

[Gerir atualizações no Azure Stack](azure-stack-updates.md)

[Gestão da região no Azure Stack](azure-stack-region-management.md)
