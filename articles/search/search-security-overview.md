---
title: "Proteger dados e as operações na Azure Search | Microsoft Docs"
description: "Segurança de pesquisa do Azure baseia-se em certificados SOC 2 conformidade, a encriptação, autenticação e acesso de identidade através de utilizador e identificadores de segurança de grupo em filtros de pesquisa do Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: cgronlun
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/19/2018
ms.author: heidist
ms.openlocfilehash: c3aa4883e33b1f3494f8502fe7f8b12f7d64a72f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="security-and-controlled-access-in-azure-search"></a>Segurança e o acesso controlado na Azure Search

A pesquisa do Azure é [SOC 2 compatíveis](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports), com uma segurança de físico spanning com arquitetura de segurança completa, transmissões encriptadas, armazenamento encriptado e as proteções de toda a plataforma de software. Operacionalmente, a Azure Search só aceita pedidos autenticados. Opcionalmente, pode adicionar controlos de acesso por utilizador no conteúdo. Este artigo tocar em segurança em cada camada, mas principalmente é concentra-se em como os dados e as operações são protegidas na Azure Search.

![Diagrama de blocos de camadas de segurança](media/search-security-overview/azsearch-security-diagram.png)

## <a name="physical-security"></a>segurança física

Centros de dados da Microsoft fornecem segurança física líder da indústria e são compatíveis com um portefólio um vasto conjunto de normas e regulamentos. Para obter mais informações, visite o [centros de dados globais](https://www.microsoft.com/cloud-platform/global-datacenters) página ou ver um breve vídeo em dados do Centro de segurança.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]

## <a name="encrypted-transmission-and-storage"></a>Transmissão encriptado e armazenamento

Expande a encriptação em todo o pipeline de indexação todo: de ligações, através da transmissão e para baixo para indexada dados armazenados na Azure Search.

| Camada de segurança | Descrição |
|----------------|-------------|
| Encriptação em trânsito | A pesquisa do Azure escuta na porta HTTPS 443. Através da plataforma, são encriptadas ligações aos serviços do Azure. |
| Encriptação inativa | Encriptação é totalmente internalized no processo de indexação, sem afetar mensuráveis indexação tempo para conclusão ou tamanho do índice. -Ocorre automaticamente em todos os indexação, incluindo no atualizações incrementais para um índice que não são totalmente encriptados (criado antes de Janeiro de 2018).<br><br>Internamente, encriptação baseia-se no [encriptação do serviço de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), através de 256 bits [encriptação AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|
| [Conformidade de certificados SOC 2](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) | Todos os serviços de pesquisa são totalmente AICPA SOC 2 em conformidade, todos os centros de dados, fornecendo da Azure Search. Para rever o relatório completo, visite [Azure - e Azure Government SOC 2 tipo II relatório](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). |

A encriptação é interna para a Azure Search, com certificados e chaves de encriptação internamente gerida pela Microsoft e aplicadas universalmente. Não é possível ativar ou desativar a encriptação, gerir ou substituir as suas próprias chaves ou ver as definições de encriptação no portal ou através de programação. 

Encriptação de Inativos foi anunciou a 24 de Janeiro de 2018 e aplica-se a todos os escalões de serviço, incluindo os serviços partilhados (gratuitos), em todas as regiões. Para a encriptação completa, índices criados antes dessa data tem de ser removidos e reconstruídos por ordem para a encriptação ocorrer. Caso contrário, adicionados após 24 de Janeiro de apenas novos dados são encriptados.

## <a name="azure-wide-logical-security"></a>Segurança lógicos em todo o Azure

Vários mecanismos de segurança estão disponíveis em toda a pilha do Azure e assim automaticamente disponíveis para os recursos de pesquisa do Azure que cria.

+ [Bloqueios da subscrição ou ao nível de recursos para evitar a eliminação](../azure-resource-manager/resource-group-lock-resources.md)
+ [Baseada em funções controlo de acesso (RBAC) para controlar o acesso a informações e operações administrativas](../active-directory/role-based-access-control-what-is.md)

Todos os serviços do Azure suportam controlos de acesso baseado em funções (RBAC) para definir níveis de acesso de forma consistente em todos os serviços. Por exemplo, visualização de dados confidenciais, tais como a chave de administrador, está restrito às funções de proprietário e contribuinte, enquanto que a visualizar o estado do serviço está disponível para os membros de qualquer função. RBAC fornece funções de proprietário, leitor e contribuinte. Por predefinição, todos os administradores de serviço são membros da função de proprietário.

## <a name="service-access-and-authentication"></a>Acesso de serviço e autenticação

Enquanto a Azure Search herda as proteções de segurança de plataforma do Azure, também fornece a suas próprias autenticação baseada em chave. O tipo de chave (admin ou consulta) determina o nível de acesso. Submissão de uma chave válida é considerado uma prova do pedido tem origem uma entidade fidedigna. 

É necessária a autenticação em cada pedido, em que cada pedido é composto por uma chave obrigatória, uma operação e um objeto. Quando encadeados em conjunto, os dois níveis de permissão (completos ou só de leitura) e o contexto é suficiente para fornecer segurança completo espetro nas operações de serviço. 

|Chave|Descrição|Limites|  
|---------|-----------------|------------|  
|Administração|Atribui direitos totais para todas as operações, incluindo a capacidade de gerir o serviço, criar e eliminar índices, indexadores e origens de dados.<br /><br /> Dois admin **chaves de api**, referidos como *primário* e *secundário* chaves no portal, são gerados quando o serviço é criado e pode ser regenerado individualmente a pedido . Ter duas chaves permite-lhe implementar através de uma chave ao utilizar a segunda chave de acesso contínuo ao serviço.<br /><br /> Apenas são especificadas chaves de administração nos cabeçalhos de pedido HTTP. Não é possível colocar uma chave de api de administração num URL.|Máximo de 2 por serviço|  
|Consulta|Concede acesso só de leitura aos índices e documentos e, normalmente, são distribuídas por aplicações cliente que emitem pedidos de pesquisa.<br /><br /> Chaves de consulta são criadas a pedido. Pode criá-los manualmente no portal ou de forma programática através de [API de REST de gestão](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Chaves de consulta podem ser especificadas no cabeçalho de pedido HTTP para pesquisa, sugestão ou operação de pesquisa. Em alternativa, pode transmitir uma chave de consulta como um parâmetro num URL. Dependendo da forma como a aplicação cliente formulates o pedido, poderá ser mais fácil passar a chave como um parâmetro de consulta:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01&api-key=A8DA81E03F809FE166ADDB183E9ED84D`|50 por serviço|  

 Não há visualmente, sem distinção entre uma chave de administrador ou a chave de consulta. Ambas as chaves são cadeias compostas 32 aleatoriamente gerado carateres alfanuméricos. Se perder a controlar de que tipo de chave for especificado na sua aplicação, pode [Verifique os valores de chave no portal do](https://portal.azure.com) ou utilize o [REST API](https://docs.microsoft.com/rest/api/searchmanagement/) para devolver o valor e o tipo de chave.  

> [!NOTE]  
>  É considerada uma prática de segurança fraca para transmitir dados confidenciais, tais como um `api-key` no URI do pedido. Por este motivo, pesquisa do Azure só aceita uma chave de consulta como uma `api-key` na consulta cadeia e deve evitar se o fizer, a menos que o conteúdo do seu índice deve ser publicamente disponível. Regra geral, recomendamos que passou o `api-key` como um cabeçalho de pedido.  

### <a name="how-to-find-the-access-keys-for-your-service"></a>Como localizar as chaves de acesso para o seu serviço

Pode obter chaves de acesso no portal ou através de [API de REST de gestão](https://docs.microsoft.com/rest/api/searchmanagement/). Para obter mais informações, consulte [gerir chaves](search-manage.md#manage-api-keys).

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Lista o [procurar serviços](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) para a sua subscrição.
3. Selecione o serviço e na página de serviço, localizar **definições** >**chaves** para ver as chaves de administração e a consulta.

![Secção de chaves de página do portal, as definições,](media/search-security-overview/settings-keys.png)

## <a name="index-access"></a>Acesso de índice

Na Azure Search, um índice individuais não é um objeto com capacidade de segurança. Em vez disso, o acesso a um índice é determinado na camada de serviço (leitura ou escrita acesso), juntamente com o contexto de uma operação.

No caso de acesso de utilizador final, pode estrutura os pedidos de consulta na sua aplicação para estabelecer ligação utilizando uma chave de consulta, o que faz com que qualquer pedido de só de leitura e incluir o índice específico utilizado pela sua aplicação. Um pedido de consulta, não há nenhum conceito de associar índices ou aceder em simultâneo vários índices para um único índice de destino a todos os pedidos por definição. Como tal, a estrutura do pedido de consulta próprio (uma chave de mais de um índice de destino única) define o limite de segurança.

Acesso de administrador e programador para índices é undifferentiated: ambos precisam de acesso de escrita para criar, eliminar e atualizar objetos geridos pelo serviço. Qualquer pessoa com uma chave de administrador para o serviço pode ler, modificar ou eliminar um índice no mesmo serviço. Para proteção contra eliminação acidental ou maliciosa de índices, o controlo de origem interna para elementos de código é o remedy para inverter uma eliminação indesejável índice ou a modificação. A pesquisa do Azure tem de ativação pós-falha do cluster para garantir a disponibilidade, mas não armazena nem executar código proprietário utilizado para criar ou carregar índices.

Soluções de arquitetura "multitenancy" necessidade de limites de segurança ao nível do índice, essas soluções incluem, geralmente, uma camada média, os clientes utilizam para processar o isolamento de índice. Para mais informações sobre o caso de utilização do multi-inquilino, consulte [padrões para aplicações de SaaS multi-inquilino e da Azure Search de conceção](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>Acesso de administrador a partir de aplicações de cliente

A API de REST de gestão do Azure Search é uma extensão do Gestor de recursos do Azure e partilhe as respetivas dependências. Como tal, o Active Directory é um pré-requisito para a administração do serviço de pesquisa do Azure. Todos os pedidos administrativos a partir do código de cliente tem de ser autenticados utilizando o Azure Active Directory antes do pedido atinge o Gestor de recursos.

Pedidos de dados contra da Azure Search ponto final do serviço, tais como criar índice (API do REST de serviço de pesquisa do Azure) ou documentos sobre pesquisa (serviço de API REST Azure Search), utilizam uma chave de api no cabeçalho do pedido.

Se o código da aplicação processa as operações de administração do serviço, bem como as operações de dados em índices de pesquisa ou documentos, implementar duas abordagens de autenticação no seu código: a chave de acesso nativa da Azure Search e a autenticação do Active Directory metodologia necessária pelo Gestor de recursos. 

Para obter informações sobre structuring um pedido na pesquisa do Azure, consulte [REST de serviço de pesquisa do Azure](https://docs.microsoft.com/rest/api/searchservice/). Para obter mais informações sobre os requisitos de autenticação para o Resource Manager, consulte [utilize o Gestor de recursos autenticação API para subscrições de acesso](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Acesso de utilizador a conteúdo de índice

Acesso por utilizador para o conteúdo de um índice é implementado através de filtros de segurança em consultas, devolvendo documentos associados com uma identidade de segurança especificado. Em vez de funções predefinidas e atribuições de funções, controlo de acesso baseado na identidade é implementado como um filtro que torta resultados de documentos e conteúdo com base em identidades da pesquisa. A tabela seguinte descreve as duas abordagens para os resultados da pesquisa de corte de conteúdo não autorizado.

| Abordagem | Descrição |
|----------|-------------|
|[Corte de segurança com base em filtros de identidade](search-security-trimming-for-azure-search.md)  | Documentos o fluxo de trabalho básico para implementar o controlo de acesso de identidade do utilizador. Abrange adicionar identificadores de segurança para um índice e, em seguida, explica filtragem esse campo para limitar os resultados de conteúdo proibido. |
|[Corte de segurança com base em identidades do Azure Active Directory](search-security-trimming-for-azure-search-with-aad.md)  | Este artigo expande no artigo anterior, fornecendo passos para obter as identidades do Azure Active Directory (AAD), um do [os serviços gratuitos](https://azure.microsoft.com/free/) na plataforma em nuvem do Azure. |

## <a name="table-permissioned-operations"></a>Tabela: Operações de Permissioned

A tabela seguinte resume as operações permitidas na Azure Search e qual a chave desbloqueia acesso uma operação específica.

| Operação | Permissões |
|-----------|-------------------------|
| Criar um serviço | Marcador de posição de subscrição do Azure|
| Dimensionar um serviço | Chave de administrador, RBAC proprietário ou contribuinte no recurso  |
| Eliminar um serviço | Chave de administrador, RBAC proprietário ou contribuinte no recurso |
| Criar, modificar, eliminar objetos do serviço: <br>Os índices e partes de componente (incluindo definições de analisador, perfis de classificação, opções de CORS), indexadores, origens de dados, sinónimos, dos sugestores. | Chave de administrador, RBAC proprietário ou contribuinte no recurso  |
| Consultar um índice | Chave de administrador ou de consulta (RBAC não aplicável) |
| Consultar as informações do sistema, tais como devolver estatísticas, contagens e apresenta uma lista de objetos. | Chave de administrador, RBAC do recurso (proprietário, Contribuidor, leitor) |
| Gerir chaves de administração | Chave de administrador, RBAC proprietário ou contribuinte no recurso. |
| Gerir chaves de consulta |  Chave de administrador, RBAC proprietário ou contribuinte no recurso. Leitor do RBAC pode ver as chaves de consulta. |


## <a name="see-also"></a>Consulte também

+ [Introdução ao .NET (demonstra a utilização de uma chave de administrador para criar um índice)](search-create-index-dotnet.md)
+ [Obter iniciado REST (demonstra a utilização de uma chave de administrador para criar um índice)](search-create-index-rest-api.md)
+ [Controlo de acesso baseado na identidade utilizando filtros de pesquisa do Azure](search-security-trimming-for-azure-search.md)
+ [Controlo de acesso baseado na identidade diretório ativo utilizando filtros de pesquisa do Azure](search-security-trimming-for-azure-search-with-aad.md)
+ [Filtros na pesquisa do Azure](search-filters.md)