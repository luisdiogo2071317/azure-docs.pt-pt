---
title: Compreender o acesso a aplicações LUIS - Azure | Microsoft Docs
description: Saiba como aceder LUIS criação.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 44380e12e6d095e8d40675af0b6b2fddc5e4c4e9
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264272"
---
# <a name="authoring-and-endpoint-user-access"></a>Criação e o ponto final de acesso de utilizador
Acesso de criação está disponível para os proprietários e colaboradores. Para uma aplicação privada, o acesso de ponto final está disponível para os proprietários e colaboradores. Para uma aplicação pública, o acesso de ponto final está disponível para todos os utilizadores que tenha a sua própria conta LUIS e tem o ID. da aplicação público 

## <a name="access-to-authoring"></a>Acesso à criação
Acesso para a aplicação a partir de [LUIS] [ LUIS] Web site ou a [criação APIs](https://aka.ms/luis-authoring-apis) é controlado pelo proprietário da aplicação. 

O proprietário e todos os funcionários têm acesso para criar a aplicação. 

|Acesso de criação de conteúdos inclui|Notas|
|--|--|
|Adicionar ou remover as chaves de ponto final||
|Exportação de versão||
|Exportar registos de ponto final||
|Importação de versão||
|Tornar a aplicação pública|Quando uma aplicação é pública, qualquer pessoa com uma chave de criação ou o ponto final pode consultar a aplicação.|
|Modificar modelo|
|Publicar|
|Reveja utterances de ponto final para [learning Active Directory](label-suggested-utterances.md)|
|Formação|

## <a name="access-to-endpoint"></a>Acesso ao ponto final
O acesso ao ponto final de consulta LUIS é controlado pelo **pública** definição da aplicação no **definições** página. Consulta de ponto final de uma aplicação privada é verificada para uma chave autorizada com a restante pedidos de quota. Consulta de ponto final de uma aplicação público tem também de fornecer uma chave de ponto final (realizem quem está a consulta) que também é analisado relativamente à restante pedidos de quota. 

A chave de ponto final é transmitida na cadeia de consulta do pedido GET ou o cabeçalho da mensagem de pedido.

![Aplicação set público](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Segurança de ponto final da aplicação privada
Ponto final de uma aplicação privada só está disponível para o seguinte:

|Chave e do utilizador|Explicação|
|--|--|--|
|Chave de criação do proprietário| Até o ponto final de 1000 pedidos|
|Chaves de criação dos colaboradores| Até o ponto final de 1000 pedidos|
|Chaves de ponto final adicionadas a partir de **[publicar](publishapp.md)** página|Proprietário e colaboradores que podem adicionar as chaves de ponto final|

Outras chaves de criação ou o ponto final tem **não** acesso.

### <a name="public-app-endpoint-access"></a>Acesso de ponto final público da aplicação
Configurar a aplicação como **pública** no **definições** página da aplicação. Depois de uma aplicação está configurada como pública, _qualquer_ LUIS válido criação chave ou chave de ponto final de LUIS pode consultar a sua aplicação, desde que a chave não utilizou a quota de ponto final de todo.

Um utilizador que não é um proprietário ou colaborador, só podem aceder a uma aplicação pública se indicados o ID de aplicação. LUIS não tem um público _mercado_ ou outra forma para procurar uma aplicação pública.  

## <a name="microsoft-user-accounts"></a>Contas de utilizador da Microsoft
Autores e colaboradores podem adicionar chaves LUIS na página de publicar. A conta de utilizador da Microsoft que cria a chave de LUIS no portal do Azure tem do proprietário da aplicação ou um colaborador de aplicação. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Proteger o ponto final 
Pode controlar quem pode ver a sua chave de ponto final LUIS chamando-la num ambiente de servidor para servidor. Se estiver a utilizar LUIS de um bot, a ligação entre o bot e LUIS já é segura. Se estiver a chamar o ponto final LUIS diretamente, deve criar uma API do lado do servidor (tais como um Azure [função](https://azure.microsoft.com/services/functions/)) com o acesso controlado (tais como [AAD](https://azure.microsoft.com/services/active-directory/)). Quando chama a API do lado do servidor e autenticação e autorização são verificadas, passe a chamada para LUIS. Enquanto esta estratégia não impedir ataques man-in-the-middle, obfuscates o ponto final dos seus utilizadores, permite-lhe controlar o acesso e permite-lhe adicionar o registo de resposta do ponto final (tais como [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Conformidade de segurança
LUIS concluída com êxito o 27001: 2013 ISO e auditoria de 27018:2014 ISO com ZERO não-conformities (findings) no relatório de auditoria. Além disso, LUIS também obter a certificação de ESTRELA CSA com o premiado Gold possíveis mais elevado para a avaliação da capacidade de maturidade. Azure é o fornecedor de serviços de nuvem pública apenas principais mais significativos beneficiar deste certificação. Para obter mais detalhes, pode encontrar o LUIS incluído na instrução âmbito atualizado no principal do Azure [descrição geral da compatibilidade](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) documento que é referenciado no [Centro de confiança](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) páginas ISO.  

## <a name="next-steps"></a>Passos Seguintes

Consulte [melhores práticas](luis-concept-best-practices.md) para aprender a utilizar entidades e pendentes para as predições melhor.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website