---
title: Não é possível aceder este erro de aplicação empresarial quando utilizar uma aplicação de Proxy de aplicações | Microsoft Docs
description: Como resolver problemas de acesso comuns com aplicações de Proxy de aplicações do Azure AD.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 1e13fe6a9a2e5251d167bb56a05721e613300e83
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36334835"
---
# <a name="cant-access-this-corporate-application-error-when-using-an-application-proxy-application"></a>Erro "Não é possível aceder a esta aplicação empresarial" quando utilizar uma aplicação de Proxy de aplicações

Este artigo ajuda-o a resolver problemas comuns para o erro "não é possível aceder esta aplicação empresarial" uma aplicação de Proxy de aplicações do Azure AD.

## <a name="overview"></a>Descrição geral
Quando vir este erro, localize o código de estado na página de erro. Esse código é, provavelmente, uma dos seguintes códigos de estado:

-   **Tempo limite do gateway**: não é possível alcançar o conector do serviço do Proxy de aplicações. Este erro normalmente indica um problema com a atribuição de conector, o conector propriamente, ou o funcionamento em rede regras em torno do conector.

-   **Gateway incorreto**: O conector não é possível alcançar a aplicação de back-end. Este erro pode indicar uma configuração incorreta da aplicação.

-   **Proibido**: O utilizador não está autorizado a aceder à aplicação. Este erro pode acontecer quando o utilizador não está atribuído à aplicação no Azure Active Directory, ou se no back-end, o utilizador não tem permissão para aceder à aplicação.

Para localizar o código, observe o texto à parte inferior esquerda da mensagem de erro para o campo de "Código de estado". Também procure qualquer sugestões adicionais na parte inferior da página.

   ![Erro de tempo limite do gateway](./media/application-proxy/connection-problem.png)

Para obter detalhes sobre como resolver a causa raiz destes erros e obter mais detalhes sobre correções sugeridos, consulte a secção correspondente abaixo.

## <a name="gateway-timeout-errors"></a>Erros de tempo limite do gateway

Um tempo limite do gateway ocorre quando o serviço tenta contactar o conector e não é possível para o período de tempo limite. Este erro é habitualmente causado por uma aplicação atribuída a um grupo de conector com conectores não funcional ou algumas portas de que o conector não estão abertas.


## <a name="bad-gateway-errors"></a>Erros de Gateway incorretos

Um erro de gateway incorreto indica que o conector não é possível alcançar a aplicação de back-end. Certifique-se de que tiver publicado a aplicação correta. Prende comuns que causar este erro é:

-   Um erro de digitação ou erro no URL interno

-   Não publicar a raiz da aplicação. Por exemplo, publicar <http://expenses/reimbursement> mas tentar acesso <http://expenses>

-   Problemas com a configuração de delegação restrita de Kerberos (KCD)

-   Problemas com a aplicação de back-end

## <a name="forbidden-errors"></a>Erros proibidos

Se vir um erro proibido, não foi atribuída ao utilizador para a aplicação. Este erro dever-se no Azure Active Directory ou a aplicação de back-end.

Para saber como atribuir utilizadores para a aplicação no Azure, consulte o [documentação configuração](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal#add-a-test-user).

Se a confirmar que o utilizador tem atribuída para a aplicação no Azure, verifique a configuração de utilizador da aplicação de back-end. Se estiver a utilizar autenticação do Kerberos restrita delegação/integrada do Windows, consulte a página de resolução de problemas de KCD diretrizes.

## <a name="check-the-applications-internal-url"></a>Verifique o URL interno da aplicação

Como primeiro passo rápido, faça duplo verificação e corrigir o URL interno ao abrir a aplicação através de **aplicações empresariais**, em seguida, selecionar o **Proxy de aplicações** menu. Certifique-se de que o URL interno é utilizada a partir da sua rede no local para aceder à aplicação.

## <a name="check-the-application-is-assigned-to-a-working-connector-group"></a>Certifique-se que a aplicação é atribuída a um conector do grupo de trabalho

Para verificar a aplicação é atribuído a um conector do grupo de trabalho:

1.  Abra a aplicação no portal, acedendo a **do Azure Active Directory**, clicando no **aplicações empresariais**, em seguida, **todas as aplicações.** Abra a aplicação, em seguida, selecione **Proxy de aplicações** no menu esquerdo.

2.  Observe o campo de grupo de conector. Se não houver nenhum conectores Active Directory no grupo, verá um aviso. Se não vir quaisquer avisos, mova em "verificar todas as portas necessárias estão na lista de permissões".

3.  Se o problema está visível conector grupo, utilize o na lista pendente para selecionar o grupo correto e confirmar que deixarem de ver quaisquer avisos. Se o grupo de conector pretendido está a ser mostrada, clicar na mensagem de aviso para abrir a página com a gestão do conector.

4.  Aqui, existem algumas formas de obter informações completas:

  * Mova um conector do Active Directory para o grupo: Se tiver um conector do Active Directory deve pertencer a este grupo e com a linha de visão para a aplicação de back-end de destino, pode mover o conector para o grupo atribuído. Para tal, clique o conector. No campo "Grupo de conector", utilize a lista pendente para selecionar o grupo correto e clique em Guardar.

  * Transfira um novo conector para esse grupo: nesta página, pode obter a ligação para [transfira um novo conector](https://download.msappproxy.net/Subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/Connector/Download). Instale o conector num computador com a linha direta de visão, para a aplicação de back-end. Typicall, o conector está instalado no mesmo servidor que a aplicação. Utilize a ligação de conector de transferência para transferir um conector para o computador de destino. Em seguida, clique o conector e utilize o menu pendente "conector do grupo" para se certificar de que pertença ao grupo à direita.

  * Investigar um conector inativo: se um conector mostra como inativo, é possível alcançar o serviço. Este erro normalmente é devido a algumas as portas necessárias a ser bloqueadas. Para resolver este problema, mover em "verificar todas as portas necessárias estão na lista de permissões".

Depois de utilizar estes passos para garantir que a aplicação é atribuída a um grupo com trabalhar conectores, teste novamente a aplicação. Se esta ainda não está a funcionar, continue para a secção seguinte.

## <a name="check-all-required-ports-are-whitelisted"></a>Verifique todas as portas necessárias estão na lista de permissões

Para verificar se todas as necessárias portas são abertas, consulte a documentação sobre a abertura de portas. Se as portas necessárias estão abertas, mova para a secção seguinte.

## <a name="check-for-other-connector-errors"></a>Verifique a existência de outros erros de conector

Se nenhuma das respostas anteriores resolver o problema, o passo seguinte é procurar problemas ou de erros com o conector propriamente dito. Pode ver alguns erros comuns no [documento de resolução de problemas](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#connector-errors). 

Pode também ver diretamente os registos de conector para identificar eventuais erros. Muitas das mensagens de erro partilham recomendações específicas para correções. Para ver os registos, consulte o [documentação de conectores](manage-apps/application-proxy-connectors.md#under-the-hood).

## <a name="additional-resolutions"></a>Resoluções adicionais

Se o procedimento acima não corrigir o problema, existem alguns diferentes as causas possíveis. Para identificar o problema:

Se a aplicação está configurada para utilizar a autenticação integrada de Windows (IWA), teste a aplicação sem o início de sessão único. Caso contrário, para avançar para o parágrafo seguinte. Para verificar a aplicação sem o início de sessão único, abra a aplicação através de **aplicações da empresa,** e vá para o **Single Sign-On** menu. Altere o na lista pendente de "Autenticação integrada do Windows" para "Do Azure AD single sign-on desativada". 

Agora, abra um browser e tente aceder novamente a aplicação. Deve ser pedida a autenticação e aceder à aplicação. Se é possível efetuar a autenticação, é o problema com a configuração de delegação restrita de Kerberos (KCD) que permite o início de sessão. Para obter mais informações, consulte a página KCD resolver problemas.

Se continuar a ver o erro, avance para a máquina onde o conector é instalado, abra um browser e tentam aceder o URL interno utilizado para a aplicação. O conector funciona como outro cliente do mesmo computador. Se não conseguirem contactar a aplicação, investigue o motivo pelo qual que a máquina não é possível alcançar a aplicação, ou utilizar um conector num servidor que é capaz de aceder à aplicação.

Se pode aceder a aplicação do que a máquina, para procurar problemas ou de erros com o conector propriamente dito. Pode ver alguns erros comuns no [documento de resolução de problemas](manage-apps/application-proxy-troubleshoot.md#connector-errors). Pode também ver diretamente os registos de conector para identificar eventuais erros. Muitos dos nossas mensagens de erro poderá partilhar recomendações mais específicas para correções. Para saber como ver os registos, consulte [nossa documentação de conectores](manage-apps/application-proxy-connectors.md#under-the-hood).

## <a name="next-steps"></a>Passos Seguintes
[Compreender os conectores de Proxy de aplicações do Azure AD](manage-apps/application-proxy-connectors.md)
