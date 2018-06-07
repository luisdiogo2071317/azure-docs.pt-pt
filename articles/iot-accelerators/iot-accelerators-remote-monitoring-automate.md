---
title: Detetar problemas de dispositivos na solução de monitorização remota - Azure | Microsoft Docs
description: Este tutorial mostra como utilizar as regras e ações para detetar automaticamente problemas baseadas em limiares dispositivos na solução de monitorização remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-suite
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: df1ba7909c64e8ccc24bcf3584bd28b2629f49ff
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627318"
---
# <a name="detect-issues-using-threshold-based-rules"></a>Detetar problemas através de regras baseadas em limiares

Este tutorial mostra as capacidades do motor de regras de na solução de monitorização remota. Para apresentar estas capacidades, o tutorial utiliza um cenário na aplicação Contoso IoT.

Contoso tem uma regra que gera um alerta crítico quando o pressão comunicadas por um **Chiller** dispositivo excede 250 PSI. Como um operador, pretende identificar **Chiller** inicial de dispositivos que poderão ter sensores problemáticas procurando picos de pressão. Para identificar estes dispositivos, crie uma regra para gerar um aviso quando a pressão excede 150 PSI.

Tem também foi disse que um alerta crítico tem de ser acionado quando a humidade média do **Chiller** dispositivo nos últimos cinco minutos for superior a 80% e a temperatura do **Chiller** dispositivo no é maior do que 75 graus fahrenheit últimos 5 minutos.

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Ver as regras na sua solução
> * Criar uma nova regra
> * Criar uma nova regra com várias condições
> * Editar uma regra existente
> * Eliminar uma regra

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este tutorial, precisa de uma instância implementada a solução de monitorização remota na sua subscrição do Azure.

Se ainda não implementado a solução de monitorização remota ainda, deve efetuar o [implementar o acelerador de solução de monitorização remota](iot-accelerators-remote-monitoring-deploy.md) tutorial.

## <a name="view-the-rules-in-your-solution"></a>Ver as regras na sua solução

O **regras** página na solução mostra uma lista de todas as regras de atuais:

![Página de regras e ações](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2.png)

Para ver apenas as regras que se aplicam ao **Chiller** dispositivos, aplicar um filtro:

![Filtrar a lista de regras](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2.png)

Pode ver mais informações sobre uma regra e editá-lo ao selecionar na lista:

![Ver detalhes da regra](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2.png)

Para desativar, ativar ou eliminar uma ou mais regras, selecione várias regras na lista:

![Selecione várias regras](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2.png)

## <a name="create-a-new-rule"></a>Criar uma nova regra

Para adicionar uma nova regra que gera um aviso quando a pressão num **Chiller** dispositivo excede 150 PSI, escolha **nova regra**:

![Criar regra](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2.png)

Utilize os seguintes valores para criar a regra:

| Definição          | Valor                                 |
| ---------------- | ------------------------------------- |
| Nome da regra        | Aviso de chiller                       |
| Descrição      | Pressão chiller excedeu 150 PSI |
| Grupo de dispositivos     | **Chillers** grupo de dispositivos             |
| Cálculo      | Instantâneas                               |
| Campo de condição 1| pressure                              |
| Operador de condição 1 | Mais do que                      |
| Valor da condição 1    | 150                               |
| Nível de Serverity  | Aviso                               |

Para guardar a nova regra, escolha **aplicar**.

Pode ver quando a regra é acionada no **regras** página ou no **Dashboard** página.

## <a name="create-a-new-rule-with-multiple-conditions"></a>Criar uma nova regra com várias condições

Para criar uma nova regra com várias condições que gera um crítico de alertas quando a humidade média do **Chiller** dispositivo nos últimos cinco minutos for superior a 80% e a temperatura do **Chiller** dispositivo nos últimos cinco minutos é maior do que 75 graus fahrenheit, escolha **nova regra**:

![Criar regra de mult](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2.png)

Utilize os seguintes valores para criar a regra:

| Definição          | Valor                                 |
| ---------------- | ------------------------------------- |
| Nome da regra        | Humidade chiller e temp crítico    |
| Descrição      | Humidade e a temperatura são críticas |
| Grupo de dispositivos     | **Chillers** grupo de dispositivos             |
| Cálculo      | Média                               |
| Período de tempo      | 5                                     |
| Campo de condição 1| humidade                              |
| Operador de condição 1 | Mais do que                      |
| Valor da condição 1    | 80                               |
| Nível de Serverity  | Crítica                              |

Para adicionar a segunda condição, clique em "+ Adicionar condição".

![Criar a condição 2](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2.png)

Utilize os seguintes valores na nova condição:

| Definição          | Valor                                 |
| ---------------- | ------------------------------------- |
| Campo de condição 2| temperatura                           |
| Operador de condição 2 | Mais do que                      |
| Valor da condição 2    | 75                                |

Para guardar a nova regra, escolha **aplicar**.

Pode ver quando a regra é acionada no **regras** página ou no **Dashboard** página.

## <a name="edit-an-existing-rule"></a>Editar uma regra existente

Para efetuar uma alteração a uma regra existente, selecione-o na lista de regras.

![Editar regra](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2.png)

<!--## Disable a rule

To temporarily switch off a rule, you can disable it in the list of rules. Choose the rule to disable, and then choose **Disable**. The **Status** of the rule in the list changes to indicate the rule is now disabled. You can re-enable a rule that you previously disabled using the same procedure.

![Disable rule](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable.png)

You can enable and disable multiple rules at the same time if you select multiple rules in the list.-->

<!--## Delete a rule

To permanently delete a rule, choose the rule in the list of rules and then choose **Delete**.

You can delete multiple rules at the same time if you select multiple rules in the list.-->

## <a name="next-steps"></a>Passos Seguintes

Este tutorial mostrou, como para:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Ver as regras na sua solução
> * Criar uma nova regra
> * Editar uma regra existente
> * Eliminar uma regra

Agora que aprendeu como detetar problemas através de regras baseadas em limiares, os passos sugeridos são saber como:

* [Gerir e configurar os seus dispositivos](iot-accelerators-remote-monitoring-manage.md).
* [Resolver problemas e resolver problemas de dispositivo](iot-accelerators-remote-monitoring-maintain.md).
* [Testar a sua solução com dispositivos simulados](iot-accelerators-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->