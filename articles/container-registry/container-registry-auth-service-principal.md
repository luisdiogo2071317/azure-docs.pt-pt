---
title: "Autenticação de registo de contentor do Azure com os principais de serviço"
description: "Saiba como fornecer acesso a imagens no seu registo de contentor privada através da utilização de um principal de serviço do Azure Active Directory."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 97036ecabceb12b87b76c6ecb7e521157cbef827
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Autenticação de registo de contentor do Azure com os principais de serviço

Pode utilizar um principal de serviço do Azure Active Directory (Azure AD) para fornecer a imagem de contentor `docker push` e `pull` acesso ao seu registo de contentor. Ao utilizar um principal de serviço, pode fornecer acesso a aplicações e serviços "sem interface".

## <a name="what-is-a-service-principal"></a>O que é um principal de serviço?

Azure AD *principais de serviço* fornecer acesso aos recursos do Azure na sua subscrição. Pode considerar um serviço principal como uma identidade de utilizador para um serviço, onde "serviço" é qualquer aplicação, serviço ou plataforma que precisa de aceder aos recursos. Pode configurar um principal de serviço com direitos de acesso no âmbito apenas a esses recursos que especificar. Em seguida, pode configurar a aplicação ou serviço para utilizar as credenciais o principal de serviço para aceder a esses recursos.

No contexto de registo de contentor do Azure, pode criar um Azure AD principal de serviço com permissões de solicitação, push e pull ou proprietário para o registo de Docker privado no Azure.

## <a name="why-use-a-service-principal"></a>Porquê utilizar um principal de serviço?

Utilizando um principal de serviço do Azure AD, pode fornecer o âmbito de acesso ao seu registo de contentor privada. Pode criar principais de serviço diferentes para cada uma das suas aplicações ou serviços, cada um com direitos de acesso personalizáveis ao seu registo. Além disso, porque pode evitar credenciais entre aplicações e serviços de partilha, pode rodar credenciais ou revogar o acesso para apenas o principal de serviço (e, consequentemente, a aplicação) que escolher.

Por exemplo, a sua aplicação web pode utilizar um principal de serviço que fornece-lhe imagem `pull` aceder apenas, enquanto o sistema de compilação, pode utilizar um principal de serviço que fornece-lhe ambos `push` e `pull` acesso. Se o desenvolvimento da sua aplicação alterações às mãos, pode rodar as credenciais do princípio de serviço sem afetar o sistema de compilação.

## <a name="when-to-use-a-service-principal"></a>Quando utilizar um principal de serviço

Deve utilizar um principal de serviço para fornecer acesso de registo em **cenários sem interface**. Ou seja, qualquer aplicação, serviço ou script que tem de push ou pull contentor imagens de forma automática ou de outra forma automática.

Para acesso individual para um registo, por exemplo, quando lhe manualmente solicitar uma imagem de contentor para a estação de trabalho de desenvolvimento, deve em vez disso, utilizar a sua própria [identidade do Azure AD](container-registry-authentication.md#individual-login-with-azure-ad) para acesso de registo (por exemplo, com [az acr início de sessão][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="next-steps"></a>Passos Seguintes

Depois de ter um principal de serviço, que concedeu acesso ao seu registo de contentor, pode utilizar as credenciais nas suas aplicações e serviços para interação de registo.

Enquanto a configuração de aplicações individuais para utilizar credenciais de principal de serviço está fora do âmbito deste artigo, pode encontrar instruções para alguns serviços específicos e plataformas aqui:

* [Autenticar com o registo de contentor do Azure do serviço de contentor do Azure (AKS)](container-registry-auth-aks.md)
* [Autenticar com o registo de contentor do Azure de instâncias de contentor do Azure (ACI)](container-registry-auth-aci.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az_acr_login