---
title: Compreender o acesso a aplicações de LUIS – Azure | Documentos da Microsoft
description: Saiba como aceder a criação de LUIS.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: f55574f7a9ffbcc2a1c8bd160bb66336b59c348c
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888606"
---
# <a name="authoring-and-endpoint-user-access"></a>Criação e o ponto final de acesso de utilizador
Acesso de criação está disponível para os proprietários e colaboradores. Para uma aplicação privada, o acesso de ponto final está disponível para os proprietários e colaboradores. Para uma aplicação pública, acesso de ponto final está disponível para todos os utilizadores que tem sua própria conta de LUIS tem o ID de. a aplicação pública 

## <a name="access-to-authoring"></a>Acesso a criação
Acesso para a aplicação a partir da [LUIS](luis-reference-regions.md#luis-website) Web site ou o [APIs de criação](https://aka.ms/luis-authoring-apis) é controlado pelo proprietário da aplicação. 

O proprietário e todos os funcionários têm acesso para criar a aplicação. 

|Inclui acesso de criação|Notas|
|--|--|
|Adicionar ou remover chaves de ponto final||
|Exportação de versão||
|Exportar registos de ponto final||
|Importação de versão||
|Tornar a aplicação pública|Quando uma aplicação é pública, qualquer pessoa com uma chave de criação ou ponto de extremidade pode consultar a aplicação.|
|Modificar modelo|
|Publicar|
|Reveja as expressões de ponto final para [aprendizagem ativa](label-suggested-utterances.md)|
|Preparar|

## <a name="access-to-endpoint"></a>Acesso ao ponto final
Acesso ao ponto final à consulta de LUIS é controlado pela **pública** definição da aplicação no **definições** página. Consulta de ponto final de uma aplicação privada está selecionada para uma chave autorizada com restantes resultados de quota. Consulta de ponto final de uma aplicação pública deve também fornecer uma chave de ponto de extremidade (a partir de quem está fazendo a consulta) que também é analisado relativamente à restante acertos de quota. 

O cabeçalho da mensagem de solicitação ou chave do ponto final é transmitido na cadeia de consulta do pedido GET.

![Aplicação de conjunto para o público](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Segurança de ponto final de aplicativos privados
Ponto final da aplicação privada só está disponível para o seguinte:

|Chave e o utilizador|Explicação|
|--|--|--|
|Chave de criação do proprietário| Até o ponto final de 1000 ocorrências|
|Chaves de criação dos colaboradores| Até o ponto final de 1000 ocorrências|
|Adicionado a partir de chaves de ponto final **[Publish](luis-how-to-publish-app.md)** página|Proprietário e os funcionários podem adicionar chaves de ponto final|

Outras chaves de criação ou o ponto final tem **nenhuma** acesso.

### <a name="public-app-endpoint-access"></a>Acesso de ponto final de aplicações públicas
Configurar a aplicação como **pública** sobre o **definições** página da aplicação. Depois de uma aplicação é configurada como pública, _qualquer_ LUIS válido, a criação de chave ou chave de ponto final do LUIS pode consultar a sua aplicação, desde que a chave não utilizado a quota de ponto final de todo.

Um utilizador que não é um proprietário ou funcionário, só pode aceder a uma aplicação pública se for indicado o ID da aplicação. LUIS não tem um público _mercado_ ou outra forma para procurar uma aplicação pública.  

## <a name="microsoft-user-accounts"></a>Contas de utilizador da Microsoft
Autores e colaboradores podem adicionar chaves para LUIS na página de publicação. A conta de utilizador da Microsoft que cria a chave de LUIS no portal do Azure tem do proprietário da aplicação ou um funcionário da aplicação. 

Ver [utilizador de inquilino do Azure Active Directory](luis-how-to-account-settings.md#azure-active-directory-tenant-user) para saber mais sobre contas de utilizador do Active Directory. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Proteger o ponto final 
Pode controlar quem pode ver a sua chave de ponto final do LUIS chamando-lo num ambiente de servidor a servidor. Se estiver a utilizar o LUIS de um bot, a ligação entre o bot e o LUIS já é segura. Se estiver a chamar diretamente o ponto de extremidade do LUIS, deve criar uma API do lado do servidor (como do Azure [função](https://azure.microsoft.com/services/functions/)) com o acesso controlado (tal como [AAD](https://azure.microsoft.com/services/active-directory/)). Quando é chamado de API do lado do servidor e autenticação e autorização são verificadas, passe a chamada para o LUIS. Embora essa estratégia não impede ataques man-in-the-middle, obfuscates o ponto final dos seus utilizadores, permite-lhe controlar o acesso e permite-lhe adicionar o registo de resposta do ponto final (tal como [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Conformidade de segurança
LUIS concluída com êxito o ISO 27001:2013 e auditoria de 27018:2014 ISO com ZERO não-conformities (descobertas) no relatório de auditoria. Além disso, o LUIS também obteve a certificação de CSA STAR com o mais alto do prémio de Gold possíveis para a avaliação de maturidade de capacidade. O Azure é o fornecedor de serviços de nuvem pública único grande ganhar essa certificação. Para obter mais detalhes, pode encontrar o LUIS incluído na declaração de escopo atualizado em principal do Azure [descrição geral de conformidade](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) documento que é referenciado no [Centro de fidedignidade](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) páginas ISO.  

## <a name="next-steps"></a>Passos Seguintes

Ver [melhores práticas](luis-concept-best-practices.md) para aprender a utilizar as intenções e entidades para as melhores previsões.