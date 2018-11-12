---
title: Compreender o controlo de acesso baseado em funções de duplos Digital do Azure | Documentos da Microsoft
description: Aprenda a autenticação no Digital duplos com controlo de acesso baseado em funções.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: b9ccdb9030a24520be8f24f757c279241f3a07e1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014793"
---
# <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

Duplos Digital do Azure permite que o controlo de acesso exatos para as ações no seu gráfico espacial, recursos e dados específicos. Ele faz isso por meio da função granular e gestão de permissões chamado controlo de acesso baseado em funções (RBAC). Consiste em RBAC _funções_ e _atribuições de funções_. Funções de identificam o nível de permissões. Atribuições de funções de associar uma função de um utilizador ou dispositivo.

Utilizando o RBAC, pode ser concedida permissão para:

- Um utilizador.
- Um dispositivo.
- Um principal de serviço.
- Uma função definida pelo utilizador. 
- Todos os utilizadores que pertencem a um domínio. 
- Um inquilino.
 
O grau de acesso também pode ser otimizado.

RBAC é o único que permissões são herdadas para baixo do gráfico espacial.

## <a name="what-can-i-do-with-rbac"></a>O que posso fazer com o RBAC?

Um desenvolvedor poderá utilizar o RBAC para:

* Conceda a capacidade para gerir os dispositivos para uma compilação inteira ou apenas para uma sala específica ou o piso de um utilizador.
* Concessão de um administrador global acesso a todos os nós de gráfico geográficos para um gráfico inteiro ou apenas para uma secção do gráfico.
* Conceda um acesso de leitura de especialista em suporte ao gráfico, exceto para as chaves de acesso.
* Conceda a todos os membros de um acesso de leitura de domínio para todos os objetos de gráfico.

## <a name="rbac-best-practices"></a>Melhores práticas RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Funções

### <a name="role-definitions"></a>Definições de funções

Uma definição de função é um conjunto de permissões e, às vezes, é chamada de uma função. As listas de definição de função as operações permitidas, que incluem criar, read, update e delete. Também especifica quais tipos de objeto aplicam estas permissões.

As seguintes funções estão disponíveis no duplos Digital do Azure:

* **Administrador de espaço**: criar, ler, atualizar e eliminar a permissão para o espaço especificado e todos os nós por baixo. Permissão global.
* **Utilizador administrador**: criar, ler, atualizar e eliminar a permissão para os utilizadores e os objetos relacionadas ao usuário. Permissão de leitura para os espaços.
* **Administrador do dispositivo**: criar, ler, atualizar e eliminar a permissão para dispositivos e os objetos relacionados com o dispositivo. Permissão de leitura para os espaços.
* **Chave de administrador**: criar, ler, atualizar e eliminar a permissão para chaves de acesso. Permissão de leitura para os espaços.
* **Token de administrador**: permissão de leitura e de atualização para as chaves de acesso. Permissão de leitura para os espaços.
* **Utilizador**: permissão de leitura para usuários, sensores e espaços, que inclui suas correspondentes objetos relacionados com.
* **Especialista de suporte**: permissão de leitura para tudo, exceto as chaves de acesso.
* **Instalador de dispositivo**: objetos relacionados com permissão de leitura e atualização para os dispositivos e sensores, que inclui suas correspondente. Permissão de leitura para os espaços.
* **Dispositivo de gateway**: criar a permissão para sensores. Permissão de leitura para os dispositivos e sensores, que inclui suas correspondentes relacionados a objetos.

>[!NOTE]
> Para obter as definições de completas para as funções anteriores, consulte as API de sistema/funções.

### <a name="object-types"></a>Tipos de objeto

O `ObjectIdType` refere-se para o tipo de identidade que tenha dado uma função. Apart a partir da `DeviceId` e `UserDefinedFunctionId` tipos, os tipos correspondem a uma propriedade de um objeto do Azure Active Directory (Azure AD):
  
* O `UserId` tipo atribui uma função a um utilizador.
* O `DeviceId` tipo atribui uma função a um dispositivo.
* O `DomainName` tipo atribui uma função a um nome de domínio. Cada utilizador com o nome de domínio tem os direitos de acesso de função correspondente.
* O `TenantId` tipo atribui uma função a um inquilino. Cada utilizador que pertence ao especificado ID de inquilino do Azure AD tem os direitos de acesso de função correspondente.
* O `ServicePrincipalId` tipo atribui uma função a um ID de objeto do principal de serviço.
* O `UserDefinedFunctionId` tipo atribui uma função a uma função definida pelo utilizador (UDF).

> [!div class="nextstepaction"]
> [Consulta ou o ID de objeto para um utilizador](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Obter o ID de objeto para um principal de serviço](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Obter o ID de objeto para um inquilino do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Atribuições de funções

Para conceder permissões a um destinatário, crie uma atribuição de função. Para revogar as permissões, remova a atribuição de função. Uma atribuição de função duplos Digital do Azure associa um objeto, como um utilizador ou um inquilino do Azure AD, uma função e um espaço. Permissões são concedidas a todos os objetos que pertencem a esse espaço. O espaço inclui todo o gráfico geográfico abaixo dele.

Por exemplo, um utilizador recebe uma atribuição de função com a função `DeviceInstaller` para o nó de raiz de um gráfico espacial, que representa um edifício. O utilizador pode, em seguida, ler e atualizar os dispositivos para esse nó e todos os outros espaços de subordinados no prédio.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre segurança de duplos Digital do Azure, leia [criar e gerir atribuições de funções](./security-create-manage-role-assignments.md).
