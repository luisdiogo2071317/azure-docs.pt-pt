---
title: Termos de utilização do Azure Active Directory| Microsoft Docs
description: Os Termos de utilização do Azure AD oferecem, tanto ao Utilizador, como à empresa do mesmo, a capacidade de fornecer Termos de utilização aos utilizadores dos serviços do Azure AD.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 11/02/2018
ms.author: rolyon
ms.openlocfilehash: 8fddcdbb8aa523cf3a98a8f2b203440ceedbdf06
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51015216"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Funcionalidade Termos de utilização do Azure Active Directory
Os Termos de utilização do Azure AD fornecem um método simples que as organizações podem utilizar para apresentar informações aos utilizadores finais. Tal disponibilização garante a visualização das exclusões de responsabilidade relevantes no que se refere a requisitos legais ou de conformidade por parte dos utilizadores. Este artigo descreve como começar com os termos de utilização.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Vídeos de descrição geral

O vídeo seguinte apresenta uma rápida visão geral dos termos de utilização.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Para ver vídeos adicionais, consulte:
- [Como implementar os termos de utilização no Azure Active Directory](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Como implementar termos de utilização no Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>O que posso fazer com os Termos de utilização?
Os Termos de utilização do Azure AD permitem-lhe fazer o seguinte:
- Pedir aos seus colaboradores ou convidados que concordem com os seus Termos de utilização antes de obterem acesso.
- Apresentar Termos de utilização gerais para todos os utilizadores da sua organização.
- Apresentar Termos de utilização específicos baseados nos atributos de um utilizador (por exemplo, médicos por oposição a enfermeiros ou colaboradores nacionais por oposição a colaboradores internacionais, uma distinção feita através da utilização de [grupos dinâmicos](../users-groups-roles/groups-dynamic-membership.md)).
- Apresentar Termos de utilização específicos ao aceder a aplicações de elevado impacto empresarial, como o Salesforce.
- Apresentar Termos de utilização em idiomas diferentes.
- Ajudar a cumprir os regulamentos de privacidade.
- Listar os utilizadores que aceitaram ou não os seus Termos de utilização.
- Exiba um log de termos de utilização atividade de conformidade e auditoria.
- Criar e gerir os termos de utilização com [Microsoft Graph APIs](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) (atualmente em pré-visualização).

## <a name="prerequisites"></a>Pré-requisitos
Para utilizar e configurar os Termos de utilização Azure AD, tem de ter:

- Uma subscrição Premium P1, Premium P2, EMS E3 ou EMS E5 do Azure AD.
    - Se não tiver uma destas subscrições, pode [obter o Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) ou [ativar a versão de avaliação do Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).
- Uma das seguintes contas de administrador para o diretório que pretende configurar:
    - Administrador global
    - Administrador de segurança
    - Administrador de acesso condicional

## <a name="terms-of-use-document"></a>Documento Termos de utilização

Os Termos de utilização do Azure AD utilizam o formato PDF para apresentar conteúdo. O ficheiro PDF pode ter qualquer conteúdo, como documentos contratuais existentes, permitindo-lhe recolher contratos durante o utilizador iniciar sessão de utilizador final. O tamanho do tipo de letra recomendado no PDF é 24.

## <a name="add-terms-of-use"></a>Adicionar os Termos de utilização
Depois de finalizar o documento Termos de utilização, utilize o procedimento que se segue para o adicionar.

1. Inicie sessão no Azure como um administrador Global, administrador de Segurança ou administrador de Acesso condicional.

1. Navegue para **Termos de utilização** em [https://aka.ms/catou](https://aka.ms/catou).

    ![Painel Termos de utilização](./media/active-directory-tou/tou-blade.png)

1. Clique em **Novos termos**.

    ![Adicionar Termos de Utilização](./media/active-directory-tou/new-tou.png)

1. Introduza o **Nome** para os Termos de utilização

2. Introduza o **Nome a apresentar**.  Este é o cabeçalho que os utilizadores veem quando iniciam sessão.

3. **Navegue** para o PDF finalizado dos Termos de utilização e selecione-o.

4. **Selecione** um idioma para os Termos de utilização.  A opção de idioma permite-lhe carregar vários Termos de utilização, cada um com um idioma diferente.  A versão dos Termos de utilização que um utilizador final verá será baseada nas respetivas preferências do browser.

5. Para **Exigir que os utilizadores expandam os Termos de utilização**, selecione ativado ou desativado.  Se esta definição estiver Ativada, será pedido aos utilizadores finais que vejam os Termos de utilização antes de os aceitarem.

6. Em **Acesso Condicional** pode **Impor** os Termos de utilização carregados ao selecionar um modelo na lista pendente ou uma política de acesso condicional personalizada.  As políticas de acesso condicional personalizadas permitem especificar Termos de utilização detalhados, ao ponto de individualizar uma aplicação da cloud ou um grupo de utilizadores em particular.  Para obter mais informações, veja [configurar políticas de acesso condicional](../../cognitive-services/qnamaker/concepts/best-practices.md).

    >[!IMPORTANT]
    >Os controlos de política de acesso condicional (incluindo os Termos de utilização) não suportam a imposição em contas de serviço.  Recomendamos excluir todas as contas de serviço da política de acesso condicional.

7. Clique em **Criar**.

8. No caso de ter selecionado um modelo de acesso condicional personalizado, será apresentado um novo ecrã que lhe permite personalizar a política de acesso condicional.

    Posto isto, deverá agora conseguir ver os novos Termos de utilização.

    ![Adicionar Termos de Utilização](./media/active-directory-tou/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Ver relatório de quem tem aceitaram e recusaram
O painel Termos de utilização mostra uma contagem dos utilizadores que aceitaram e recusaram. Estas contagens e os utilizadores que aceitaram/recusaram são armazenados durante a vigência dos Termos de utilização.

1. Inicie sessão no Azure e navegue para **Termos de utilização** em [https://aka.ms/catou](https://aka.ms/catou).

    ![Evento de Auditoria](./media/active-directory-tou/view-tou.png)

1. Clique nos números em **Aceitou** ou **Recusou** para ver o estado atual dos utilizadores.

    ![Evento de Auditoria](./media/active-directory-tou/accepted-tou.png)

## <a name="view-azure-ad-audit-logs"></a>Registos de auditoria de vista do Azure AD
Se pretender ver atividade adicional, os Termos de utilização do Azure AD incluem registos de auditoria. Cada consentimento do usuário dispara um evento nos registos de auditoria que é armazenado durante 30 dias. Pode ver estes registos no portal ou transferi-los como um ficheiro. csv.

Para começar a utilizar com o Azure AD registos de auditoria, utilize o seguinte procedimento:

1. Inicie sessão no Azure e navegue para **Termos de utilização** em [https://aka.ms/catou](https://aka.ms/catou).

1. Clique em **Ver registos de auditoria**.

    ![Evento de Auditoria](./media/active-directory-tou/audit-tou.png)

1. No ecrã de registos de auditoria do Azure AD, pode filtrar as informações com a ajuda das listas pendentes fornecidas para se focar em informações específicas do registo de auditoria.

    ![Evento de Auditoria](./media/active-directory-tou/audit-logs-tou.png)

1. Também pode clicar em **Transferir** para transferir as informações para um ficheiro. csv, para utilização local.

## <a name="what-terms-of-use-looks-like-for-users"></a>Qual o aspeto dos Termos de utilização para os utilizadores
Depois de termos de utilização é criado e imposta, os utilizadores que estão no âmbito, verá o ecrã seguinte durante o início de sessão.

![Evento de Auditoria](./media/active-directory-tou/user-tou.png)

O ecrã seguinte mostra o aspeto dos Termos de utilização em dispositivos móveis.

![Evento de Auditoria](./media/active-directory-tou/mobile-tou.png)

Os utilizadores só têm de aceitar os termos de utilização de uma vez e não irão ver os termos de utilização novamente no subsequentes inícios de sessão.

### <a name="how-users-can-review-their-terms-of-use"></a>Como os utilizadores podem rever os Termos de utilização
Os utilizadores podem rever e ver os termos de utilização que aceitaram, utilizando o procedimento seguinte.

1. Inicie sessão em [https://myapps.microsoft.com](https://myapps.microsoft.com).

1. No canto superior direito, clique no seu nome e selecione **Perfil** no menu pendente.

    ![Perfil](./media/active-directory-tou/tou14.png)

1. Na página Perfil, clique em **Rever os termos de utilização**.

    ![Evento de Auditoria](./media/active-directory-tou/tou13a.png)

1. A partir daí, pode rever os Termos de utilização que aceitou. 

## <a name="edit-terms-of-use-details"></a>Editar detalhes de utilização dos termos de
É possível editar alguns detalhes dos termos de utilização, mas não é possível modificar um documento existente. O procedimento seguinte descreve como editar os detalhes.

1. Inicie sessão no Azure e navegue para **Termos de utilização** em [https://aka.ms/catou](https://aka.ms/catou).

1. Selecione os termos de utilização que pretende editar.

1. Clique em **editar termos**.

1. Em termos de edição do painel de utilização, altere o nome, nome a apresentar ou exigir que os utilizadores expandam os valores.

    ![Adicionar Termos de Utilização](./media/active-directory-tou/edit-tou.png)

1. Clique em **guardar** para guardar as alterações.

    Depois de guardar as alterações, os utilizadores terão voltem os novos termos.

## <a name="add-a-terms-of-use-language"></a>Adicionar termos de linguagem de utilização
O procedimento seguinte descreve como adicionar termos de linguagem de utilização.

1. Inicie sessão no Azure e navegue para **Termos de utilização** em [https://aka.ms/catou](https://aka.ms/catou).

1. Selecione os termos de utilização que pretende editar.

1. No painel de detalhes, clique nas **idiomas** separador.

    ![Adicionar Termos de Utilização](./media/active-directory-tou/languages-tou.png)

1. Clique em **Adicionar idioma**.

1. Em termos de adicionar do painel de linguagem de uso, carregue o localizados PDF e selecione o idioma.

    ![Adicionar Termos de Utilização](./media/active-directory-tou/language-add-tou.png)

1. Clique em **adicionar** para adicionar o idioma.

## <a name="delete-terms-of-use"></a>Eliminar Termos de utilização
Pode eliminar Termos de utilização antigos com o procedimento seguinte.

1. Inicie sessão no Azure e navegue para **Termos de utilização** em [https://aka.ms/catou](https://aka.ms/catou).

1. Selecione os Termos de utilização que quer remover.

1. Clique em **Eliminar termos**.

1. Na mensagem que é apresentada a perguntar se pretende continuar, clique em **Sim**.

    ![Adicionar Termos de Utilização](./media/active-directory-tou/delete-tou.png)

    Posto isto, deverá deixar de ver os Termos de utilização.

## <a name="deleted-users-and-active-terms-of-use"></a>Utilizadores eliminados e Termos de utilização ativos
Por predefinição, um utilizador eliminado fica num estado eliminado no Azure AD durante um período de 30 dias, durante o qual pode ser restaurado por um administrador, se necessário.  Após 30 dias, esse utilizador é eliminado permanentemente.  Além disso, através do portal do Azure Active Directory, um administrador Global pode explicitamente [eliminar permanentemente um utilizador recentemente eliminado](../fundamentals/active-directory-users-restore.md) antes desse período de tempo ser atingido.  Depois de um utilizador ser eliminado de maneira permanente, os dados subsequentes sobre esse utilizador serão removidos dos Termos de utilização ativos.  As informações de auditoria sobre os utilizadores eliminados permanecem no registo de auditoria.

## <a name="policy-changes"></a>Alterações de política
As políticas de acesso condicional entram imediatamente em vigor. Quando isto acontecer, o administrador começa a "nuvens sad" ou "Problemas de token do Azure AD". O administrador tem de terminar sessão e inicie sessão novamente para atender a nova política.

>[!IMPORTANT]
> Os utilizadores abrangidos pelo âmbito definido terão de terminar e iniciar sessão de modo a satisfazerem uma nova política:
> - uma política de acesso condicional é ativada num documento Termos de utilização
> - ou é criado um segundo documento Termos de utilização

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

**P: Como posso ver se e quando um utilizador aceitou os Termos de utilização?**</br>
R: em termos do painel de utilização, clique no número em **aceite**. Também pode ver ou procurar a atividade de aceitar no Azure AD registos de auditoria. Para obter mais informações, consulte [Ver relatório de quem tem aceitaram e recusaram](#view-who-has-accepted-and-declined) e [registos de auditoria de vista do Azure AD](#view-azure-ad-audit-logs).

**P: Por quanto tempo as informações são armazenadas?**</br>
R: o utilizador conta em termos de relatório de utilização e que aceite/recusado é armazenado durante o ciclo de vida dos termos de utilização. A auditoria do Azure AD, os registos são armazenados durante 30 dias.

**P: por que vejo um número diferente de consentimentos em termos de relatório de utilização vs. o Azure AD, registos de auditoria?**</br>
R: os termos de relatório de utilização são armazenados durante a vida útil do que termos de utilização, ao mesmo tempo a auditoria do Azure AD, os registos são armazenados durante 30 dias. Além disso, os termos de relatório de utilização só apresenta o estado de consentimento atual de utilizadores. Por exemplo, se um utilizador recusa e, em seguida, aceita, os termos de utilização relatório mostrará apenas esse utilizador aceite. Se precisar de ver o histórico, pode utilizar o Azure AD registos de auditoria.

**P: se posso editar os detalhes para os termos de utilização, é necessário que os utilizadores aceitar novamente?**</br>
R: Sim, se um administrador editar os detalhes para os termos de utilização, ele requer que os utilizadores voltem os novos termos.

**P: posso atualizar uma existente termos de utilização documento?**</br>
R: atualmente, não é possível atualizar uma existente termos de documento de utilização. Para alterar os termos de utilização de documento, terá de criar novos termos de instância de utilização.

**P: se hiperlinks são em termos de utilizar o documentos PDF, os utilizadores finais será capazes de clicar nas mesmas?**</br>
R: o PDF é processado por predefinição como um JPEG, pelo que não são clicáveis hiperlinks. Os utilizadores têm a opção de selecionar **com problemas em visualizar? Clique aqui**, que renderiza o PDF nativamente onde os hiperlinks são suportados.

**P: Os Termos de utilização podem suportar vários idiomas?**</br>
R: Sim. Atualmente, existem 108 idiomas diferentes, um administrador pode configurar para um único termos de utilização.

**P: Quando é que os Termos de utilização são acionados?**</br>
R: Os Termos de utilização são acionados durante a experiência de início de sessão.

**P: Para que aplicações posso orientar os Termos de utilização?**</br>
R: Pode criar uma política de acesso condicional nas aplicações empresariais que utilizam autenticação moderna.  Para obter mais informações, consulte [aplicações empresariais](./../manage-apps/view-applications-portal.md).

**P: Posso adicionar vários Termos de utilização para um determinado utilizador ou aplicação?**</br>
R: Sim, através da criação de várias políticas de acesso condicional orientadas para esses grupos ou aplicações. Se um utilizador for abrangido pelo âmbito de vários Termos de utilização, concorda com um dos Termos de utilização de cada vez.

**P: O que acontece se um utilizador recusar os Termos de utilização?**</br>
R: O acesso do utilizador à aplicação é bloqueado. O utilizador terá de iniciar sessão novamente e aceitar os termos para conseguir obter acesso.

**P: é possível unaccept termos de utilização que foram anteriormente aceites?**</br>
R: pode [revisão ainda aceitado os termos de utilização](#how-users-can-review-their-terms-of-use), mas atualmente não há uma forma de unaccept.

**P: o que acontece se também estou a utilizar o Intune termos e condições?**</br>
R: Se tiver configurado a ambos os termos de AD do Azure de utilização e [Intune termos e condições](/intune/terms-and-conditions-create), irá ser pedido ao utilizador para aceitar ambos. Para obter mais informações, consulte a [escolher o direito de termos de solução para a sua postagem do blog de organização](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

## <a name="next-steps"></a>Passos Seguintes

- [Melhores práticas para acesso condicional no Azure Active Directory](../../active-directory/conditional-access/best-practices.md)
