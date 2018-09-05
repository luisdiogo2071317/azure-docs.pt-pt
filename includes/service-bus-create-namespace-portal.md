---
title: incluir ficheiro
description: incluir ficheiro
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 07/03/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 377ea6629223c365ef3d00c756929bf23ddffde5
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702646"
---
Para começar a utilizar as entidades de mensagens do Service Bus no Azure, tem de, primeiro, criar um espaço de nomes que seja exclusivo em todo o Azure. Um espaço de nomes fornece um contentor de âmbito para abordar os recursos do Service Bus na sua aplicação.

Para criar um espaço de nomes:

1. Inicie sessão no [Portal do Azure][Azure portal].
2. No painel de navegação à esquerda do portal, clique em **+ Criar um recurso** e, em seguida, clique em **Integração** e em **Service Bus**.
3. Na caixa de diálogo **Criar espaço de nomes**, introduza um nome de espaço de nomes. O sistema verifica imediatamente a disponibilidade do nome.
4. Após se certificar de que o espaço de nomes está disponível, selecione o escalão de preço (Básico, Standard ou Premium). Se quiser utilizar os [tópicos e subscrições](../articles/service-bus-messaging/service-bus-queues-topics-subscriptions.md#topics-and-subscriptions), certifique-se de que escolhe Standard ou Premium. Os tópicos/subscrições não são suportadas no escalão de preço Básico.
5. No campo **Subscrição**, selecione a subscrição do Azure em que pretende criar o espaço de nomes.
6. No campo **Grupo de recursos**, selecione um grupo de recursos existente em que o espaço de nomes será colocado ou crie um novo.      
7. Em **Localização**, selecione o país ou a região em que o espaço de nomes deverá ser alojado.
   
    ![Create namespace][create-namespace]
8. Clique em **Criar**. O sistema cria o espaço de nomes e ativa-o. Poderá ter de aguardar alguns minutos enquanto o sistema aprovisiona recursos para a sua conta.

### <a name="obtain-the-management-credentials"></a>Obter as credenciais de gestão

A criação de um espaço de nomes gera automaticamente uma regra inicial de Assinatura de Acesso Partilhado (SAS) com um par de chaves primárias e secundárias associado que cada uma concede controlo total sobre todos os aspetos do espaço de nomes. Veja [Service Bus authentication and authorization](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) (Autenticação e autorização do Service Bus) para obter informações sobre como criar mais regras com direitos mais limitados para remetentes e recetores regulares. Para copiar a regra inicial, siga estes passos: 

1. Clique em **Todos os recursos** e clique no nome do espaço de nomes criado recentemente.
2. Na janela de espaço de nomes, clique em **Políticas de acesso partilhado**.
3. No ecrã **Políticas de acesso partilhado**, clique em **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. Na janela **Política: RootManageSharedAccessKey**, clique no botão de copiar junto a **Cadeia de Ligação Primária** para copiar a cadeia de ligação para a sua área de transferência para utilizar mais tarde. Cole este valor no Bloco de Notas ou noutra localização temporária.
   
    ![connection-string][connection-string]

5. Repita o passo anterior, copie e cole o valor da **Chave primária** para uma localização temporária para utilizar mais tarde.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
