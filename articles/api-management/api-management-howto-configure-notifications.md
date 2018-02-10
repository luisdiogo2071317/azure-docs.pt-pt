---
title: "Configurar notificações e modelos de e-mail na API Management do Azure | Microsoft Docs"
description: "Saiba como configurar notificações e modelos de e-mail na API Management do Azure."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: apimpm
ms.openlocfilehash: 228cbb103e13c478bea460bb04de43d6480bc60e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Como configurar notificações e modelos de e-mail na API Management do Azure
API Management fornece a capacidade para configurar notificações para eventos específicos e para configurar os modelos de e-mail que são utilizados para comunicar com os administradores e programadores de uma instância de API Management. Este artigo mostra como configurar notificações para os eventos disponíveis e fornece uma descrição geral de configuração de modelos de e-mail utilizados estes eventos.

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver uma instância de serviço de API Management, concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).

## <a name="publisher-notifications"></a>Configurar notificações

1. Selecione o **API MANAGEMENT** instância.
2. Clique em **notificações** para ver as notificações disponíveis.

    ![Notificações de publicador][api-management-publisher-notifications]

    A lista seguinte de eventos pode ser configurada para notificações.

    * **Pedidos de subscrição (exigir aprovação)** -os destinatários de e-mail especificado e os utilizadores irão receber notificações por e-mail sobre pedidos de subscrição para exigir a aprovação de produtos de API.
    * **Novas subscrições** -os destinatários de e-mail especificado e os utilizadores irão receber notificações por e-mail sobre novas subscrições de produto de API.
    * **Pedidos de Galeria de aplicações** -os destinatários de e-mail especificado e os utilizadores irão receber notificações por e-mail quando novas aplicações são enviadas para a Galeria de aplicações.
    * **BCC** -os destinatários de e-mail especificado e os utilizadores irão receber o e-mail oculta carbono cópias de todas as mensagens de correio eletrónico enviadas para os programadores.
    * **Novo problema ou comentário** - os destinatários de e-mail especificado e os utilizadores irão receber notificações por e-mail quando um novo problema ou comentário é submetido no portal de programador.
    * **Mensagem de fecho conta** -os destinatários de e-mail especificado e os utilizadores irão receber notificações por e-mail quando uma conta for fechada.
    * **Limite de quota de subscrição aproximado** -os seguintes destinatários de e-mail e os utilizadores irão receber notificações por e-mail quando a utilização de subscrição obtém próximo da quota de utilização.

    Para cada evento, pode especificar os destinatários de e-mail utilizando a caixa de texto de endereço de e-mail ou pode selecionar os utilizadores a partir de uma lista.

3. Para especificar os endereços de e-mail para ser notificado, introduzi-las na caixa de texto de endereço de correio eletrónico. Se tiver vários endereços de e-mail, separá-los com vírgulas.

    ![Destinatários das notificações][api-management-email-addresses]
4. Prima **Adicionar**.

## <a name="email-templates"></a>Configurar modelos de notificação
API Management fornece modelos de notificação para as mensagens de e-mail que são enviadas MBS administrar e utilizar o serviço. Os modelos de e-mail seguintes são fornecidos.

* Submissão de Galeria de aplicações aprovada
* Letra de farewell de programador
* Limite de quota de programador aproximar-se de notificação
* Convidar utilizador
* Novo comentário adicionado a um problema
* Novo problema recebido
* Nova subscrição ativada
* Confirmação de subscrição renovada
* Recusar o pedido de subscrição
* Foi recebido um pedido subscrição

Estes modelos podem ser modificados conforme pretendido.

Para ver e configurar os modelos de e-mail para a instância de API Management, clique em **modelos notificações**.

![Modelos de E-mail][api-management-email-templates]

Cada modelo de correio eletrónico tem um requerente em texto simples e uma definição de corpo em formato HTML. Cada item pode ser personalizado conforme pretendido.

![Editor de modelo de correio eletrónico][api-management-email-template]

O **parâmetros** lista contém uma lista de parâmetros, que quando inseridas no assunto ou do corpo, será substituído o valor designado quando o e-mail é enviado. Para introduzir um parâmetro, coloque o cursor onde pretende que o parâmetro ir e clique na seta à esquerda do nome do parâmetro.

> [!NOTE] 
> Os parâmetros não são substituídos pelos valores reais quando pré-visualização ou enviar um teste.

Para guardar as alterações ao modelo de correio eletrónico, clique em **guardar**, ou para cancelar a alterações, clique em **rejeitar**.
 

[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
