---
title: Configurar alertas de métricas da base de dados do Azure para MySQL no portal do Azure
description: Este artigo descreve como configurar e alertas de métricas de acesso da base de dados do Azure para MySQL do portal do Azure.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 999b1d03ad8cb0b27de10ff6457c0e6cc9112ee7
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548736"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-mysql"></a>Utilizar o portal do Azure para configurar alertas em métricas da base de dados do Azure para MySQL 

Este artigo mostra-lhe como configurar a base de dados do Azure para MySQL alertas ao utilizar o portal do Azure. Pode receber um alerta com base nas métricas para os seus serviços do Azure de monitorização.

A alerta é acionada quando o valor de uma métrica especificado ultrapassar um limiar que atribui. Os acionadores alerta, quando a condição é a primeira cumpridos e, em seguida, em seguida quando condição que já não é a ser cumprida. 

Pode configurar um alerta para fazer as seguintes ações quando é acionado:
* enviar notificações por e-mail para o administrador de serviços e coadministradores
* Envie e-mail para e-mails adicionais que especificar.
* Chamar um webhook

Pode configurar e obter informações sobre regras de alerta com:
* [Portal do Azure](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../azure-monitor/platform/alerts-classic-portal.md)
* [Interface de linha de comandos (CLI)](../azure-monitor/platform/alerts-classic-portal.md)
* [API de REST do Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Criar uma regra de alerta numa métrica do portal do Azure
1. Na [portal do Azure](https://portal.azure.com/), selecione a base de dados do Azure para o servidor MySQL que pretende monitorizar.

2. Sob o **monitorização** secção a barra lateral, selecione **regras de alerta** conforme mostrado:

   ![Selecionar regras de alerta](./media/howto-alert-on-metric/1-alert-rules.png)

3. Selecione **Adicionar alerta de métrica** (ícone +). 

4. O **Adicionar regra** é aberta a página conforme mostrado abaixo.  Preencha as informações necessárias:

   ![Adicionar formulário de alerta de métrica](./media/howto-alert-on-metric/2-add-rule-form.png)

   | Definição | Descrição  |
   |---------|---------|
   | Nome | Forneça um nome para a regra de alerta. Este valor é enviado o e-mail de notificação de alerta. |
   | Descrição | Forneça uma breve descrição da regra de alerta. Este valor é enviado o e-mail de notificação de alerta. |
   | Alerta referente a | Escolher **métricas** para esse tipo de alerta. |
   | Subscrição | Este campo está pré-preenchido com a subscrição que aloja a base de dados do Azure para MySQL. |
   | Grupo de recursos | Este campo está pré-preenchido com o grupo de recursos da base de dados do Azure para MySQL. |
   | Recurso | Este campo está pré-preenchido com o nome da base de dados do Azure para MySQL. |
   | Métrica | Selecione a métrica que pretende emitir um alerta para. Por exemplo, **percentagem de armazenamento**. |
   | Condição | Escolha a condição para a métrica a ser comparada ao. Por exemplo, **superior a**. |
   | Limiar | Valor de limiar para métrica, por exemplo 85 (percentagem). |
   | Período | O período de tempo que a regra de métrica deve ser satisfeita antes dos acionadores de alerta. Por exemplo, **nos últimos 30 minutos**. |

   Com base no exemplo, o alerta procura por percentagem de armazenamento superior a 85% durante um período de 30 minutos. Esse alerta for acionado quando a percentagem média de armazenamento foi superior a 85% durante 30 minutos. Depois de ocorre o primeiro acionador, novamente aciona quando a percentagem média de armazenamento é inferior a 85% mais de 30 minutos.

5. Escolha o método de notificação que pretende para a regra de alerta. 

   Verifique **proprietários, contribuidores e leitores do E-Mail** opção se pretender que os administradores de subscrição e os coadministradores para ser enviado por e-mail quando o alerta é acionado.

   Se pretender que os e-mails adicionais para receber uma notificação quando o alerta é acionado, adicione-os na **adicionais do administrador email(s)** campo. Separe várias mensagens de correio eletrónico com ponto e vírgula -  *email@contoso.com;email2@contoso.com*

   Opcionalmente, forneça um URI válido no **Webhook** campo se desejar que ele chamado quando o alerta é acionado.

6. Selecione **OK** para criar o alerta.

   Em poucos minutos, o alerta está ativo e aciona conforme descrito anteriormente.

## <a name="manage-your-alerts"></a>Gerir os alertas
Assim que tiver criado um alerta, pode selecioná-lo e fazer as seguintes ações:

* Ver um gráfico que mostra o limiar de métrica e os valores reais do dia anterior relevantes para este alerta.
* **Editar** ou **eliminar** a regra de alerta.
* **Desativar** ou **ativar** o alerta, se quiser temporariamente parar ou retomar a receção das notificações.


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [configurar webhooks nos alertas](../azure-monitor/platform/alerts-webhooks.md).
* Obter um [descrição geral da coleção de métricas](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) para se certificar de que o seu serviço está disponível e reativo.
