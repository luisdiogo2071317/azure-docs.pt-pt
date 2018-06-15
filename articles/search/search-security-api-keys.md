---
title: Criar, gerir e proteger chaves de api admin e a consulta para o Azure Search | Microsoft Docs
description: chaves de API controlam o acesso ao ponto final do serviço. Chaves de administração de conceder acesso de escrita. Chaves de consulta podem ser criadas para acesso só de leitura.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: heidist
ms.openlocfilehash: 4215795b7cd2a25427a3ce9b3cde16bfc69cb009
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32185948"
---
# <a name="create-and-manage-api-keys-for-an-azure-search-service"></a>Criar e gerir chaves de api para um serviço da Azure Search

Todos os pedidos para um serviço de pesquisa tem uma chave de api que foi gerada especificamente para o seu serviço. Esta chave de api é o mecanismo único para a autenticação de acesso para o ponto final de serviço de pesquisa. 

Uma chave de api é uma cadeia composta por letras e números gerados aleatoriamente. Através de [permissões baseadas em função](search-security-rbac.md), pode eliminar ou ler as chaves, mas não é possível substituir uma chave com uma palavra-passe definida pelo utilizador ou utilizar o Active Directory como a metodologia de autenticação primária para aceder às operações de pesquisa. 

Dois tipos de chaves são utilizados para aceder ao seu serviço de pesquisa: admin (leitura / escrita) e a consulta (só de leitura).

|Chave|Descrição|Limites|  
|---------|-----------------|------------|  
|Administração|Atribui direitos totais para todas as operações, incluindo a capacidade de gerir o serviço, criar e eliminar índices, indexadores e origens de dados.<br /><br /> Duas chaves de administração, referida como *primário* e *secundário* chaves no portal, são gerados quando o serviço é criado e pode ser regenerado individualmente a pedido. Ter duas chaves permite-lhe implementar através de uma chave ao utilizar a segunda chave de acesso contínuo ao serviço.<br /><br /> Apenas são especificadas chaves de administração nos cabeçalhos de pedido HTTP. Não é possível colocar uma chave de api de administração num URL.|Máximo de 2 por serviço|  
|Consulta|Concede acesso só de leitura aos índices e documentos e, normalmente, são distribuídas por aplicações cliente que emitem pedidos de pesquisa.<br /><br /> Chaves de consulta são criadas a pedido. Pode criá-los manualmente no portal ou de forma programática através de [API de REST de gestão](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Chaves de consulta podem ser especificadas no cabeçalho de pedido HTTP para pesquisa, sugestão ou operação de pesquisa. Em alternativa, pode transmitir uma chave de consulta como um parâmetro num URL. Dependendo da forma como a aplicação cliente formulates o pedido, poderá ser mais fácil passar a chave como um parâmetro de consulta:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11&api-key=[query key]`|50 por serviço|  

 Não há visualmente, sem distinção entre uma chave de administrador ou a chave de consulta. Ambas as chaves são cadeias compostas 32 aleatoriamente gerado carateres alfanuméricos. Se perder a controlar de que tipo de chave for especificado na sua aplicação, pode [Verifique os valores de chave no portal do](https://portal.azure.com) ou utilize o [REST API](https://docs.microsoft.com/rest/api/searchmanagement/) para devolver o valor e o tipo de chave.  

> [!NOTE]  
>  É considerada uma prática de segurança fraca para transmitir dados confidenciais, tais como um `api-key` no URI do pedido. Por este motivo, pesquisa do Azure só aceita uma chave de consulta como uma `api-key` na consulta cadeia e deve evitar se o fizer, a menos que o conteúdo do seu índice deve ser publicamente disponível. Regra geral, recomendamos que passou o `api-key` como um cabeçalho de pedido.  

## <a name="find-api-keys-for-your-service"></a>Encontrar chaves de api para o seu serviço

Pode obter chaves de acesso no portal ou através de [API de REST de gestão](https://docs.microsoft.com/rest/api/searchmanagement/). Para obter mais informações, consulte [gerir chaves de api admin e a consulta](search-security-api-keys.md).

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Lista o [procurar serviços](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) para a sua subscrição.
3. Selecione o serviço e na página de serviço, localizar **definições** >**chaves** para ver as chaves de administração e a consulta.

![Secção de chaves de página do portal, as definições,](media/search-security-overview/settings-keys.png)

## <a name="regenerate-admin-keys"></a>Regenerar chaves de administração

Duas chaves de administração são criadas para cada serviço, de modo a que possa rollover de chave primária, utilizando a chave secundária para acesso contínuo.

Se voltar a gerar chaves primárias e secundárias ao mesmo tempo, todas as aplicações utilizando a chave para aceder às operações de serviço deixará de ter acesso ao serviço.

1. No **definições** >**chaves** página, copie a chave secundária.
2. Para todas as aplicações, Atualize as definições de chave de api a utilizar a chave secundária.
3. Regenere a chave primária.
4. Atualize todas as aplicações a utilizar a nova chave primária.

## <a name="secure-api-keys"></a>Proteger chaves de api
Segurança de chave é certificar-se ao restringir o acesso através do portal ou interfaces de Gestor de recursos (PowerShell ou interface de linha de comandos). Conforme indicado, os administradores da subscrição podem ver e voltar a gerar chaves de api todos os. Como precaução, reveja as atribuições de funções para compreender quem tem acesso às chaves de administração.

+ No dashboard de serviço, clique em **(IAM) do controlo de acesso** para ver as atribuições de funções para o seu serviço.

Os membros das seguintes funções podem ver e voltar a gerar chaves: proprietário, contribuinte, [os contribuintes do serviço de pesquisa](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Para acesso com base em identidades através de resultados da pesquisa, pode criar filtros de segurança para limitar os resultados pela identidade, remover documentos para o qual o requerente não deve ter acesso. Para obter mais informações, consulte [filtros de segurança](search-security-trimming-for-azure-search.md) e [seguro com o Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="see-also"></a>Consulte também

+ [Controlo de acesso baseado em funções na Azure Search](search-security-rbac.md)
+ [Gerir com o PowerShell](search-manage-powershell.md) 
+ [Desempenho e a otimização de artigo](search-performance-optimization.md)