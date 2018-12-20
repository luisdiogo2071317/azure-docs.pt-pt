---
title: Rotação de chaves de acesso para o serviço Azure SignalR
description: Uma visão geral sobre por que o cliente precisar regularmente rodar as chaves de acesso e de como fazê-lo com o portal GUI e a CLI.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 2c0f60b0ef3a90372fc4a095c830f39bc148f354
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636161"
---
# <a name="access-key-rotation-for-azure-signalr-service"></a>Rotação de chaves de acesso para o serviço Azure SignalR

Cada instância do serviço Azure SignalR tem um par de chaves de acesso: Primário e secundárias chaves. São utilizados para autenticar clientes de SignalR quando são efetuados pedidos ao serviço. As chaves estão associadas com o url de ponto final de instância. Mantenha as suas chaves seguras e girá-las regularmente. São fornecidas duas chaves de acesso, para que possa manter ligações utilizando uma chave enquanto Regenera a outra.

## <a name="why-rotate-access-keys"></a>Por que motivo Rodar chaves de acesso?

Para o requisito de razão e a conformidade de segurança, os desenvolvedores são recomendados para girar regularmente as chaves de acesso.

## <a name="how-to-regenerate-access-keys"></a>Como voltar a gerar chaves de acesso?

1. Aceda ao [portal do Azure](https://portal.azure.com/) e inicie sessão com as suas credenciais.

1. Encontrar o **chaves** secção da instância do serviço Azure SignalR que pretende regenerar as chaves.

1. Clique em **chaves** no menu de navegação.

1. Clique em **regenerar a chave primária** ou **voltar a gerar chave secundária**.

Uma nova cadeia de ligação de chave e correspondente será criada e apresentada.

 ![Regenerar Chaves](media/signalr-key-rotation/regenerate-keys.png)

Também pode voltar a gerar chaves utilizando [CLI do Azure](/cli/azure/ext/signalr/signalr/key?view=azure-cli-latest#ext-signalr-az-signalr-key-renew).

## <a name="update-configurations-with-new-connection-strings"></a>Configurações de atualização com novas cadeias de ligação

1. Copie a cadeia de ligação gerados recentemente.

1. Atualize todas as configurações para utilizar a nova cadeia de ligação.

1. Reinicie a aplicação conforme necessário.

## <a name="forced-access-key-regeneration"></a>Regeneração da chave de acesso forçado

Serviço Azure SignalR podem impor uma regeneração da chave de acesso obrigatórios em determinada situação. O serviço notificará os clientes por e-mail e notificação do portal. Se receber esta comunicação ou se ocorrer falha de serviço devido a chave de acesso, roda as chaves ao seguir este guia.

## <a name="next-steps"></a>Passos Seguintes

Recomendamos a rotação das chaves de acesso regularmente como uma boa prática de segurança.

Neste guia, aprendeu sobre como voltar a gerar chaves de acesso. Avance para os tutoriais seguintes sobre a autenticação OAuth ou com as funções do Azure.

> [!div class="nextstepaction"]
> [Integrar com a identidade do ASP.NET Core](./signalr-authenticate-oauth.md)

> [!div class="nextstepaction"]
> [Criar uma Aplicação sem Servidor em Tempo Real com a Autenticação](./signalr-authenticate-azure-functions.md)