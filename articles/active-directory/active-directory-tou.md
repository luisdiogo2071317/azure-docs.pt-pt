---
title: Termos de Utilização do Azure Active Directory| Microsoft Docs
description: Os Termos de Utilização do Azure AD oferecem, tanto ao Utilizador, como à empresa do mesmo, a capacidade de fornecer termos de utilização aos utilizadores dos serviços do Azure AD.
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/29/2018
ms.author: billmath
ms.openlocfilehash: 208a65c09b13acad62c9b6d8e55b6050041c9f5d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Funcionalidade Termos de Utilização do Azure Active Directory
Os Termos de Utilização do Azure AD fornecem um método simples que as organizações podem utilizar para apresentar informações aos utilizadores finais.  Tal disponibilização garante a visualização das exclusões de responsabilidade relevantes no que se refere a requisitos legais ou de conformidade por parte dos utilizadores.

Os Termos de Utilização do Azure AD utilizam o formato pdf para apresentar conteúdo.   O pdf pode ter qualquer tipo de conteúdo, como documentos contratuais existentes, permitindo-lhe recolher contratos de utilizador final durante o início de sessão do utilizador.  Pode utilizar os termos de utilização para aplicações, grupos de utilizadores ou caso tenha vários termos de utilização para diferentes fins.

A parte restante deste documento descreve como lidar com os Termos de Utilização do Azure AD.  

## <a name="why-use-azure-ad-terms-of-use"></a>Razões para utilizar os Termos de Utilização do Azure AD
Está com dificuldades em conseguir que os seus colaboradores ou convidados concordem com os seus termos de utilização antes de obterem acesso? Precisa de ajuda para determinar quem aceitou ou não aceitou os termos de utilização da sua empresa?  Os Termos de Utilização do Azure AD fornecem um método simples que as organizações podem utilizar para apresentar informações aos utilizadores finais.  Tal disponibilização garante a visualização das exclusões de responsabilidade relevantes no que se refere a requisitos legais ou de conformidade por parte dos utilizadores.

Os Termos de Utilização do Azure AD podem ser utilizados nos seguintes cenários:
-   Termos de utilização gerais para todos os utilizadores da sua organização.
-   Termos de utilização específicos baseados nos atributos de um utilizador (por exemplo, médicos por oposição a enfermeiros ou colaboradores nacionais por oposição a colaboradores internacionais, uma distinção feita através da utilização de [grupos dinâmicos](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups)).
-   Termos de utilização específicos centrados no acesso a aplicações de elevado impacto empresarial, como o Salesforce.


## <a name="prerequisites"></a>Pré-requisitos
Utilize os seguintes passos para configurar os Termos de Utilização do Azure AD:

1. Inicie sessão no Azure AD com um perfil de administrador global, administrador de segurança ou administrador com acesso condicional para o diretório em que pretende configurar os Termos de Utilização do Azure AD.
2. Certifique-se de que o diretório tem uma subscrição Premium P1, Premium P2, EMS E3 ou EMS E5 do Azure AD.  Caso isto não se verifique, [obtenha o Azure AD Premium](active-directory-get-started-premium.md) ou [inicie uma avaliação](https://azure.microsoft.com/trial/get-started-active-directory/).
3. Veja o Dashboard dos Termos de Utilização do Azure AD em [https://aka.ms/catou](https://aka.ms/catou).

>[!IMPORTANT]
>Os controlos de política de acesso condicional (incluindo os termos de utilização) não suportam a imposição em contas de serviço.  Recomendamos excluir todas as contas de serviço da política de acesso condicional.

## <a name="add-company-terms-of-use"></a>Adicionar Termos de Utilização da Empresa
Depois de finalizar os seus Termos de Utilização, utilize o procedimento que se segue para os adicionar.

### <a name="to-add-terms-of-use"></a>Para adicionar os Termos de Utilização
1. Navegue de volta para o dashboard em [https://aka.ms/catou](https://aka.ms/catou)
2. Clique em Adicionar.</br>
![Adicionar Termos de Utilização](media/active-directory-tou/tou12.png)
3. Introduza o **Nome** a utilizar para os Termos de Utilização
4. Introduza o **Nome a Apresentar**.  O cabeçalho é o que os utilizadores veem quando iniciam sessão.
5. **Navegue** para o pdf finalizado dos termos de utilização e selecione-o.  O tamanho do tipo de letra recomendado é 24.
6. **Selecione** um idioma para os termos de utilização.  A opção de idioma permite-lhe carregar vários termos de utilização, cada um com um idioma diferente.  A versão dos termos de utilização que um utilizador final verá serão baseados nas respetivas preferências do browser.
7. Selecione ativado ou desativado para **Exigir que os utilizadores expandam os termos de utilização**.  Se estiver ativado, será pedido aos utilizadores finais que vejam os termos de utilização antes de os aceitarem.
8. Em **Acesso Condicional** pode **Impor** os termos de utilização carregados ao selecionar um modelo no menu pendente ou uma política de acesso condicional personalizada.  As políticas de acesso condicional personalizadas permitem especificar termos de utilização detalhados, ao ponto de individualizar uma aplicação da cloud ou um grupo de utilizadores em particular.  Para obter mais informações, consulte [configurar políticas de acesso condicional](active-directory-conditional-access-best-practices.md)
9. Clique em **Criar**.
10. No caso de ter selecionado um modelo de acesso condicional personalizado, será apresentado um novo ecrã que lhe permite personalizar a política de AC.
11. Posto isto, deverá conseguir ver os novos Termos de Utilização.</br>

![Adicionar Termos de Utilização](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Eliminar Termos de Utilização
Pode remover ou eliminar termos de utilização antigos com o seguinte procedimento:

### <a name="to-delete-terms-of-use"></a>Para eliminar Termos de Utilização
1. Navegue de volta para o dashboard em [https://aka.ms/catou](https://aka.ms/catou)
2. Selecione os termos de utilização que quer remover.
3. Clique em **Eliminar**.
4. Posto isto, deverá deixar de ver os novos termos de utilização.


## <a name="viewing-current-user-status"></a>Ver o estado do utilizador atual
Reparará que os seus termos de utilização mostram uma contagem dos utilizadores que aceitaram e recusaram.

![Evento de Auditoria](media/active-directory-tou/tou15.png)

Pode clicar nos números em **aceites** ou **recusados** para ver o estado atual dos utilizadores.

![Evento de Auditoria](media/active-directory-tou/tou16.png)

## <a name="audit-terms-of-use"></a>Auditar Termos de Utilização
Se quiser ver as aceitações e recusas históricas e não apenas o estado atual, os Termos de Utilização do Azure AD proporcionam uma auditoria fácil de utilizar.  Esta auditoria permite-lhe ver quem aceitou os termos de utilização e quando.  

Pode utilizar a auditoria de duas formas, consoante o que queira fazer neste momento.  


Para dar início a uma auditoria, utilize o seguinte procedimento:

### <a name="to-audit-terms-of-use"></a>Para auditar Termos de Utilização
1. Navegue de volta para o dashboard em [https://aka.ms/catou](https://aka.ms/catou)
2. Clique em “Ver registos de auditoria”.</br>
![Evento de Auditoria](media/active-directory-tou/tou8.png)
3.  No ecrã de registos de auditoria do Azure AD, pode filtrar as informações com a ajuda das listas pendentes fornecidas para se focar em informações específicas do registo de auditoria.
![Evento de Auditoria](media/active-directory-tou/tou9.png)
4.  Também pode transferir as informações para um ficheiro. csv para utilização local.

## 

## <a name="what-users-see"></a>Conteúdo apresentado aos utilizadores
Aos utilizadores abrangidos pelo âmbito definido será apresentado o seguinte após a criação e imposição dos termos de utilização.  Estes ecrãs ser-lhes-ão apresentados durante o início de sessão.
-   A melhor prática recomendada é utilizar o tipo de letra com o tamanho 24 no PDF.
![Evento de Auditoria](media/active-directory-tou/tou10.png)
-   Este é o formato do ecrã apresentado nos telemóveis</br></br>
![Evento de Auditoria](media/active-directory-tou/tou11.png)

### <a name="review-terms-of-use"></a>Rever termos de utilização
Os utilizadores podem rever e ver os termos de utilização aceitaram.  Para rever os termos de utilização, utilize o seguinte procedimento:

1. Navegue e inicie sessão em [https://myapps.microsoft.com](https://myapps.microsoft.com).
2. No canto superior direito, clique no seu nome e selecione **Perfil** no menu pendente.
![Perfil](media/active-directory-tou/tou14.png)

3. No seu Perfil, clique em **Rever os termos de utilização**.
![Evento de Auditoria](media/active-directory-tou/tou13a.png)

4.  A partir daí, pode rever os termos de utilização que aceitou. 


## <a name="additional-information"></a>Informações adicionais
As informações que se seguem são dados a ter em consideração e podem ajudar na utilização dos termos de utilização.

>[!IMPORTANT]
> Os utilizadores abrangidos pelo âmbito definido terão de terminar e iniciar sessão de modo a satisfazerem uma nova política:
> - se for ativada uma política de acesso condicional num documento de termos de utilização
> - ou se for criado um segundo documento de termos de utilização
>
>As políticas de acesso condicional entram imediatamente em vigor. Quando isto acontece, o administrador começa a receber indicações de "clouds com problemas" ou "problemas com o token do Azure AD". O administrador tem de terminar e iniciar sessão novamente a fim de satisfazer a nova política.





## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

**P: Como posso ver se e quando um utilizador aceitou os termos de utilização?**</br>
R: Pode simplesmente clicar no número em “Aceites”, junto aos termos de utilização.  Para obter mais informações, veja [Viewing current user status](#viewing-current-user-status) (Ver o estado do utilizador atual).  Além disso, quando um utilizador aceita os termos de utilização, essa informação é guardada no registo de auditoria. Pode pesquisar o registo de auditoria do Azure AD para ver os resultados.  

**P: Se alterar os termos de utilização, isso significa que os utilizadores têm de os aceitar novamente?**</br>
R: Sim, os administradores podem alterar os termos de utilização, o que significa que os termos novos têm de voltar a ser aceites.

**P: Os termos de utilização podem suportar vários idiomas?**</br>
R: Sim.  Atualmente, existem 18 idiomas diferentes que um administrador pode configurar para um único termo de utilização. 

**P: Quando é que os termos de utilização são acionados?**</br>
R: Os termos de utilização são acionados durante a experiência de início de sessão.

**P: Para que aplicações posso orientar os termos de utilização?**</br>
R: Pode criar uma política de acesso condicional nas aplicações empresariais que utilizam autenticação moderna.  Para obter mais informações, consulte [aplicações empresariais](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal).

**P: Posso adicionar vários termos de utilização para um determinado utilizador ou aplicação?**</br>
R: Sim, através da criação de várias políticas de acesso condicional orientadas para esses grupos ou aplicações. Se um utilizador for abrangido pelo âmbito de vários termos de utilização, concorda com um documento de termos de utilização de cada vez.
 
**P: O que acontece se um utilizador recusar os termos de utilização?**</br>
R: O acesso do utilizador à aplicação é bloqueado. O utilizador teria de reiniciar sessão e concordar com os termos para conseguir obter acesso.