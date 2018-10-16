---
title: Noções básicas sobre Azure duplos Digital baseada em funções de controlo de acesso | Documentos da Microsoft
description: Aprenda a autenticação no Digital duplos com controlo de acesso baseado em funções.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: 7a6d8565a0f85b4cb81d9f5f23b04fe6b2edc53e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324217"
---
# <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

Duplos Digital do Azure permite que o controlo de acesso exatos para as ações no seu gráfico espacial, recursos e dados específicos. Isso é feito por meio da função granular e gestão de permissões chamado _controlo de acesso baseado em funções_. Controlo de acesso baseado em funções é composta por _funções_, ou o nível de permissões, e _atribuições de funções_, ou a associação de uma função para um utilizador ou dispositivo.

Utilizar o controlo de acesso baseado em funções, permissão pode ser concedida a um utilizador, um dispositivo, um principal de serviço, uma função definida pelo utilizador, todos os utilizadores que pertencem a um domínio ou um inquilino. Além disso, também pode ser otimizado o grau de acesso.

Controlo de acesso baseado em funções é o único que permissões são herdadas para baixo do gráfico espacial.

## <a name="what-can-i-do-with-role-based-access-control"></a>O que posso fazer com o controlo de acesso baseado em funções?

Um desenvolvedor poderá usar o controle de acesso baseado em funções para:

* Conceda a capacidade para gerir os dispositivos para uma compilação inteira ou apenas para uma sala específica ou o piso de um utilizador.
* Concessão de um administrador global acesso a todos os nós de gráfico geográficos para um gráfico inteiro ou apenas para uma secção do gráfico.
* Conceda um acesso de leitura de especialista em suporte ao gráfico, exceto para as chaves de acesso.
* Conceda a todos os membros de um acesso de leitura de domínio para todos os objetos de gráfico.

## <a name="role-based-access-control-best-practices"></a>Práticas recomendadas de controlo de acesso baseado em funções

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Funções

### <a name="role-definitions"></a>Definições de funções

R **definição de função** é um conjunto de permissões e é por vezes denominado um **função**. A definição de função apresenta uma lista as operações permitidas, incluindo *crie*, *ler*, *atualizar*, e *eliminar*. Também especifica quais tipos de objeto aplicam estas permissões.

As seguintes funções estão disponíveis no duplos Digital do Azure:

* **Administrador de espaço**: criar, ler, atualizar e eliminar a permissão para o espaço especificado e todos os nós por baixo. Permissão global.
* **Utilizador administrador**: criar, ler, atualizar e eliminar a permissão para os utilizadores e os objetos relacionadas ao usuário. Permissão de leitura para os espaços.
* **Administrador do dispositivo**: criar, ler, atualizar e eliminar a permissão para dispositivos e os objetos relacionados com o dispositivo. Permissão de leitura para os espaços.
* **Chave de administrador**: criar, ler, atualizar e eliminar a permissão para chaves de acesso. Permissão de leitura para os espaços.
* **Token de administrador**: permissão de leitura e de atualização para as chaves de acesso. Permissão de leitura para os espaços.
* **Utilizador**: permissão de leitura para usuários, sensores e espaços, incluindo seus correspondentes objetos relacionados com.
* **Especialista de suporte**: permissão de leitura para tudo, exceto as chaves de acesso.
* **Instalador de dispositivo**: objetos relacionados com permissão de leitura e de atualização para os dispositivos e sensores, incluindo seus correspondentes. Permissão de leitura para os espaços.
* **Dispositivo de gateway**: criar a permissão para sensores. Permissão de leitura para os dispositivos e sensores, incluindo seus correspondentes relacionados a objetos.

>[!NOTE]
> *As definições de completas para o procedimento acima podem ser obtidas consultando as API de sistema/funções.*

### <a name="object-types"></a>Tipos de objeto

O `ObjectIdType` refere-se para o tipo de identidade a ser dada uma função. Apart a partir da `DeviceId` e `UserDefinedFunctionId` tipos, os tipos correspondem a uma propriedade de um objeto do Azure Active Directory (Azure AD):
  
* O `UserId` tipo atribui uma função a um utilizador.
* O `DeviceId` tipo atribui uma função a um dispositivo.
* O `DomainName` tipo atribui uma função a um nome de domínio. Cada utilizador com o nome de domínio especificado terá os direitos de acesso de função correspondente.
* O `TenantId` tipo atribui uma função a um inquilino. Cada utilizador que pertençam a especificado ID de inquilino do Azure AD terão os direitos de acesso de função correspondente.
* O `ServicePrincipalId` tipo atribui uma função a um ID de objeto do principal de serviço.
* O `UserDefinedFunctionId` tipo atribui uma função para uma função de definidas pelo utilizador (UDF).

> [!div class="nextstepaction"]
> [Consulta ou o ID de objeto para um utilizador](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Obter o ID de objeto para um principal de serviço](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Obter o ID de objeto para um inquilino do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Atribuições de funções

As permissões são concedidas a um destinatário ao criar uma atribuição de função e revogadas ao remover uma atribuição de função. Uma atribuição de função duplos Digital do Azure associa um objeto (usuário, inquilino do Azure AD, etc.) e um espaço de função. Em seguida, são concedidas permissões a todos os objetos que pertencem a esse espaço, incluindo todo o gráfico geográfico abaixo dele.

Por exemplo, um utilizador recebe uma atribuição de função com a função `DeviceInstaller` para o nó de raiz de um gráfico espacial, que representa um edifício. O usuário, em seguida, é capaz de ler e atualizar os dispositivos não apenas para esse nó, mas todos os outros espaços de subordinados no prédio.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre segurança de duplos Digital do Azure, leia [criar e gerir atribuições de funções](./security-create-manage-role-assignments.md).
