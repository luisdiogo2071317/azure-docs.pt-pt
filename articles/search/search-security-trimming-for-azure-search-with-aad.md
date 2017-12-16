---
title: "Filtros de segurança para corte utilizar identidades do Active Directory de resultados de pesquisa do Azure | Microsoft Docs"
description: "Controlo de acesso no conteúdo de Azure Search utilizando filtros de segurança e de identidades do Active Directory."
services: search
author: revitalbarletz
manager: jlembicz
ms.service: search
ms.topic: article
ms.date: 11/07/2017
ms.author: revitalb
ms.openlocfilehash: 2113b59d6fec15067acbef8b4d4c1fc34c141e62
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="security-filters-for-trimming-azure-search-results-using-active-directory-identities"></a>Filtros de segurança para corte resultados de pesquisa do Azure com identidades do Active Directory

Este artigo demonstra como utilizar as identidades de segurança do Azure Active Directory (AAD), juntamente com os filtros na Azure Search para limitar os resultados da pesquisa com base na associação de grupo do utilizador.

Este artigo abrange as seguintes tarefas:
> [!div class="checklist"]
- Criar utilizadores e grupos do AAD
- Associar o utilizador com o grupo que criou
- Colocar em cache novos grupos
- Índice de documentos com grupos associados
- Emitir um pedido de pesquisa com filtro de identificadores de grupo

>[!NOTE]
> Fragmentos de código de exemplo neste artigo são escritos em c#. Pode localizar o código de origem completo [no GitHub](http://aka.ms/search-dotnet-howto). 

## <a name="prerequisites"></a>Pré-requisitos

O índice da Azure Search tem de ter um [campo segurança](search-security-trimming-for-azure-search.md) para armazenar a lista de identidades de grupo ter acesso de leitura ao documento. Neste caso, utilize assume uma correspondência de e-mail de um para um entre um item com capacidade de segurança (por exemplo, a aplicação de ensino superior de um indivíduo) e um campo de segurança que especificar quem tem acesso a este item (técnico admissions).

Tem de ter permissões de administrador do AAD, necessárias esta explicação passo a passo para criar utilizadores, grupos e as associações no AAD.

A aplicação também tem de estar registada no AAD, conforme descrito no procedimento seguinte.

### <a name="register-your-application-with-aad"></a>Registar a sua aplicação no AAD

Este passo integra-se a aplicação AAD para fins de aceitar inícios de sessão do utilizador e contas de grupo. Se não for um administrador do AAD na sua organização, poderá ter de [criar um novo inquilino](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) para efetuar os seguintes passos.

1. Vá para o [ **Portal de registo de aplicação**](https://apps.dev.microsoft.com) >  **aplicação Convergida** > **adicionar uma aplicação**.
2. Introduza um nome para a sua aplicação, em seguida, clique em **criar**. 
3. Selecione a aplicação recentemente registada na página de aplicações My.
4. Na página de registo de aplicações do > **plataformas** > **adicionar plataforma**, escolha **Web API**.
5. Ainda na página de registo de aplicação, aceda ao > **Microsoft Graph permissões** > **adicionar**.
6. Em Selecionar permissões, adicione as seguintes permissões delegadas e, em seguida, clique em **OK**:

   + **Directory.ReadWrite.All**
   + **Group.ReadWrite.All**
   + **User.ReadWrite.All**

Microsoft Graph fornece uma API que permite o acesso programático ao AAD através de uma API REST. O exemplo de código para esta instrução utiliza as permissões para chamar a Graph API do Microsoft para criar grupos, os utilizadores e as associações. As APIs também são utilizadas para os identificadores de grupos de cache para filtrar mais rapidamente.

## <a name="create-users-and-groups"></a>Criar utilizadores e grupos

Se estiver a adicionar pesquisa a uma aplicação estabelecida, poderá ter utilizador existente e os identificadores de grupos no AAD. Neste caso, pode ignorar os três passos. 

No entanto, se não tiver utilizadores existentes, pode utilizar o Microsoft Graph APIs para criar a segurança principais. Os fragmentos de código seguintes demonstram como gerar identificadores, que tornam-se os valores de dados para o campo de segurança no seu índice da Azure Search. Na nossa aplicação admissions hipotético de ensino superior, isto seria os identificadores de segurança para os funcionários admissions.

Utilizador e a associação de grupo podem ser muito fluid, especialmente em grandes organizações. Código que cria as identidades de utilizador e grupo deve ser executada com frequência suficiente para recolher as alterações na associação da organização. Da mesma forma, o índice da Azure Search necessita de uma agenda de atualização semelhantes para refletir o estado atual de utilizadores permitidos e recursos.

### <a name="step-1-create-aad-grouphttpsdevelopermicrosoftcomgraphdocsapi-referencev10apigrouppostgroups"></a>Passo 1: Criar [grupo AAD](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/group_post_groups) 
```csharp
// Instantiate graph client 
GraphServiceClient graph = new GraphServiceClient(new DelegateAuthenticationProvider(...));
Group group = new Group()
{
    DisplayName = "My First Prog Group",
    SecurityEnabled = true,
    MailEnabled = false,
    MailNickname = "group1"
}; 
Group newGroup = await graph.Groups.Request().AddAsync(group);
```
   
### <a name="step-2-create-aad-userhttpsdevelopermicrosoftcomgraphdocsapi-referencev10apiuserpostusers"></a>Passo 2: Criar [utilizador AAD](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_post_users) 
```csharp
User user = new User()
{
    GivenName = "First User",
    Surname = "User1",
    MailNickname = "User1",
    DisplayName = "First User",
    UserPrincipalName = "User1@FirstUser.com",
    PasswordProfile = new PasswordProfile() { Password = "********" },
    AccountEnabled = true
};
User newUser = await graph.Users.Request().AddAsync(user);
```

### <a name="step-3-associate-user-and-group"></a>Passo 3: Associar utilizadores e grupos
```csharp
await graph.Groups[newGroup.Id].Members.References.Request().AddAsync(newUser);
```

### <a name="step-4-cache-the-groups-identifiers"></a>Passo 4: Colocar em Cache os identificadores de grupos
Opcionalmente, para reduzir a latência de rede, pode colocar em cache as associações de grupo de utilizadores para que quando um pedido de pesquisa é emitido, grupos são devolvidos da cache, guardar uma ida e volta para o AAD. Pode utilizar (AAD API do Batch) [https://developer.microsoft.com/graph/docs/concepts/json_batching] para enviar um pedido de Http único com vários utilizadores e criar a cache.

Microsoft Graph foi concebido para processar um grande volume de pedidos. Se ocorrer um número muito confuso de pedidos, o Microsoft Graph falhar o pedido com o código de estado HTTP 429. Para obter mais informações, consulte [Microsoft Graph limitação](https://developer.microsoft.com/graph/docs/concepts/throttling).

## <a name="index-document-with-their-permitted-groups"></a>Documento de índice com os respetivos grupos permitidos

Operações de consulta na Azure Search são executadas ao longo de um índice da Azure Search. Neste passo, uma operação de indexação importa dados pesquisáveis para um índice, incluindo os identificadores utilizados como filtros de segurança. 

A pesquisa do Azure não autenticar identidades de utilizador, ou forneça lógica para estabelecer o conteúdo um utilizador tem permissão para ver. O caso de utilização para corte de segurança parte do princípio de que fornece a associação entre um documento confidencial e o identificador do grupo ter acesso a um documento, importado intact para um índice de pesquisa. 

No exemplo hipotético, o corpo do pedido PUT sobre um índice de pesquisa do Azure inclui essay de ensino superior a um candidato ou transcript juntamente com o identificador de grupo com permissão para ver esse conteúdo. 

No exemplo genérico utilizado no código de exemplo para esta explicação passo a passo, a ação de índice poderá ter o seguinte aspeto:

```csharp
var actions = new IndexAction<SecuredFiles>[]
              {
                  IndexAction.Upload(
                  new SecuredFiles()
                  {
                      FileId = "1",
                      Name = "secured_file_a",
                      GroupIds = new[] { groups[0] }
                  }),
              ...
             };

var batch = IndexBatch.New(actions);

_indexClient.Documents.Index(batch);  
```

## <a name="issue-a-search-request"></a>Emitir um pedido de pesquisa

Para efeitos de corte de segurança, os valores do campo de segurança no índice são estáticos valores utilizados para incluindo ou excluindo documentos nos resultados da pesquisa. Por exemplo, se o identificador de grupo para Admissions "A11B22C33D44-E55F66G77-H88I99JKK", os documentos num índice da Azure Search presença esse identificador no segurança registada são incluídos (ou excluídos) nos resultados da pesquisa enviados para o requerente.

Para filtrar documentos devolvidos nos resultados de pesquisa com base nos grupos do utilizador emitir o pedido, reveja os seguintes passos.

### <a name="step-1-retrieve-users-group-identifiers"></a>Passo 1: Obter os identificadores de grupos do utilizador

Se a grupos do utilizador não foram já colocados em cache ou, a cache tiver expirado, execute o [grupos](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/directoryobject_getmembergroups) pedido
```csharp
private static void RefreshCacheIfRequired(string user)
{
    if (!_groupsCache.ContainsKey(user))
    {
        var groups = GetGroupIdsForUser(user).Result;
        _groupsCache[user] = groups;
    }
}

private static async Task<List<string>> GetGroupIdsForUser(string userPrincipalName)
{
    List<string> groups = new List<string>();
    var allUserGroupsRequest = graph.Users[userPrincipalName].GetMemberGroups(true).Request();

    while (allUserGroupsRequest != null) 
    {
        IDirectoryObjectGetMemberGroupsRequestBuilder allUserGroups = await allUserGroupsRequest.PostAsync();
        groups = allUserGroups.ToList();
        allUserGroupsRequest = allUserGroups.NextPageRequest;
    }
    return groups;
}
``` 

### <a name="step-2-compose-the-search-request"></a>Passo 2: Compor o pedido de pesquisa

Partindo do princípio de que tem a associação a grupos do utilizador, pode emitir o pedido de pesquisa com os valores de filtro adequado.

```csharp
string filter = String.Format("groupIds/any(p:search.in(p, '{0}'))", string.Join(",", groups.Select(g => g.ToString())));
SearchParameters parameters = new SearchParameters()
             {
                 Filter = filter,
                 Select = new[] { "application essays" }
             };

DocumentSearchResult<SecuredFiles> results = _indexClient.Documents.Search<SecuredFiles>("*", parameters);
```
### <a name="step-3-handle-the-results"></a>Passo 3: Processar os resultados

A resposta inclui uma lista de documentos, constituída por aqueles que o utilizador tem permissão para visualizar filtrada. Dependendo de como construir a página de resultados de pesquisa, pode querer incluir ajudas visuais para refletir o conjunto de resultados filtrados.

## <a name="conclusion"></a>Conclusão

Nestas instruções, aprendeu técnicas para filtrar documentos nos resultados de pesquisa do Azure, utilizando o AAD inícios de sessão trimming os resultados de documentos que não correspondem do filtro fornecido no pedido.

## <a name="see-also"></a>Consultar também

+ [Controlo de acesso baseado na identidade utilizando filtros de pesquisa do Azure](search-security-trimming-for-azure-search.md)
+ [Filtros na pesquisa do Azure](search-filters.md)
+ [Controlo de acesso e segurança de dados em operações de pesquisa do Azure](search-security-overview.md)
