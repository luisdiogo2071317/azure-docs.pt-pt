---
title: O Azure Active Directory Identity Protection | Documentos da Microsoft
description: Saiba como o Azure AD Identity Protection permite-lhe limitar a capacidade de um atacante explore uma identidade comprometida ou dispositivo e para proteger uma identidade ou um dispositivo que foi anteriormente suspeita ou conhecido por ser comprometido.
services: active-directory
keywords: proteção de identidade do Azure Active Directory, descoberta de aplicações na cloud, gestão de aplicações, a segurança, a risco, a nível de risco, a vulnerabilidade, a política de segurança
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 06e3a596b60bf96319071fff68b0bf1655869559
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003802"
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

O Azure Active Directory Identity Protection é uma funcionalidade da edição Azure AD Premium P2 que lhe permite:

- Detetar potenciais vulnerabilidades que afetam as identidades da sua organização

- Configurar respostas automáticas para as ações suspeitas detetadas relacionadas com identidades da sua organização  

- Investigar incidentes suspeitos e tomar as medidas adequadas para resolvê-los   


## <a name="getting-started"></a>Introdução

Microsoft tem protegido identidades baseadas na cloud há mais de uma década. Com o Azure Active Directory Identity Protection, no seu ambiente, pode utilizar os mesmos sistemas de proteção, a Microsoft utiliza para proteger as identidades.

A grande maioria das falhas de segurança ocorrem quando os atacantes obtêm acesso a um ambiente, por roubo de identidade de um utilizador. Ao longo dos anos, os atacantes têm se tornado cada vez mais eficazes em tirar partido das violações de terceiros e utilize ataques de phishing sofisticado. Assim que um atacante obtiver acesso a contas de utilizador com privilégios baixos, é relativamente fácil para obter acesso aos recursos de importantes da empresa através de movimento lateral.

Como conseqüência disso, terá de:

- Proteger as identidades de todos, independentemente de seu nível de privilégio

- Proativamente impedir que as identidades comprometidas a ser utilizada

Detetar identidades comprometidas não é tarefa fácil. O Azure Active Directory utiliza algoritmos de aprendizagem automática adaptáveis e heurística para detetar anomalias e incidentes suspeitos que indiquem potencialmente comprometidos identidades. Utilizando estes dados, o Identity Protection gera relatórios e alertas que lhe permite avaliar os problemas detetados e tomar apropriado de atenuação ou ações de remediação.

O Azure Active Directory Identity Protection é mais do que uma monitorização e a ferramenta de relatório. Para proteger as identidades da sua organização, pode configurar as políticas baseadas em risco, que respondam automaticamente aos problemas detetados quando for atingido um nível de risco especificado. Automaticamente estas políticas, além de outros Controles de acesso condicional fornecidas pelo Azure Active Directory e o EMS, podem bloquear ou iniciar ações de remediação adaptável redefinições de senha incluindo e imposição de multi-factor authentication.


#### <a name="identity-protection-capabilities"></a>Capacidades de proteção de identidade

**Detetar vulnerabilidades e contas de risco:**  

* Fornecendo recomendações personalizadas para melhorar a postura de segurança geral, realce de vulnerabilidades
* Calcular os níveis de risco de início de sessão
* Calcular os níveis de risco de utilizador


**Investigar eventos de risco:**

* Enviar notificações para eventos de risco
* Investigar eventos de risco com informações relevantes e contextuais
* Fornecimento de fluxos de trabalho básicos para controlar as investigações
* Proporciona acesso fácil para ações de remediação, tais como a reposição de palavra-passe

**Políticas de acesso condicional baseado no risco:**

* Política para mitigar o risco de inícios de sessão ao inícios de sessão de bloquear ou exigir que os desafios de autenticação multifator
* Política para bloquear ou contas de utilizador de risco de seguro
* Política para exigir que os utilizadores se registarem na autenticação multifator



## <a name="identity-protection-roles"></a>Funções de proteção de identidade

O balanceamento de carga as atividades de gestão em torno de sua implementação do Identity Protection, pode atribuir várias funções. O Azure AD Identity Protection suporta 3 funções de diretório:

| Função                         | Pode fazer                          | Não é possível fazer
| :--                          | ---                                |  ---   |
| Administrador global         | Acesso total ao Identity Protection, carregar Identity Protection| |
| Administrador de segurança       | Acesso total ao Identity Protection | Carregar Identity Protection, repor palavras-passe para um utilizador |
| Leitor de segurança              | Acesso só de leitura ao Identity Protection | Carregar Identity Protection, os utilizadores de remidiate, configurar políticas, repor palavras-passe |




Para obter mais detalhes, consulte [atribuir funções de administrador no Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)



## <a name="detection"></a>Deteção

### <a name="vulnerabilities"></a>Vulnerabilidades

O Azure Active Directory Identity Protection analisa a configuração e detecta vulnerabilidades que podem ter um impacto sobre as identidades dos utilizadores. Para obter mais detalhes, consulte [vulnerabilidades detetadas pelo Azure Active Directory Identity Protection](vulnerabilities.md).

### <a name="risk-events"></a>Eventos de risco

O Azure Active Directory utiliza algoritmos de aprendizagem automática adaptáveis e heurística para detetar as ações suspeitas relacionadas com as identidades dos utilizadores. O sistema cria um registo para cada ação suspeita detetada. Estes registos também são conhecidos como eventos de risco.  
Para obter mais detalhes, veja [Eventos de risco do Azure Active Directory](../active-directory-identity-protection-risk-events.md).


## <a name="investigation"></a>Investigação
Sua viagem por meio de proteção de identidade normalmente começa com o dashboard do Identity Protection.

![Remediação](./media/overview/1000.png "remediação")

O dashboard dá-lhe o acesso a:

* Por exemplo, os relatórios **utilizadores sinalizados para risco**, **eventos de risco** e **vulnerabilidades**
* Definições, tais como a configuração da sua **políticas de segurança**, **notificações** e **registo de autenticação multifator**

Normalmente é seu ponto de partida para a investigação, o que é o processo de revisão das atividades, registos e outras informações relevantes relacionados com um evento de risco para decidir se os passos de remediação ou atenuação são necessários, e como foi a identidade comprometido e compreender como a identidade comprometida foi utilizada.

Pode vincular suas atividades de investigação para o [notificações](notifications.md) Azure Active Directory Protection envia por e-mail.

As secções seguintes fornecem mais detalhes e os passos que estão relacionadas com uma investigação.  


## <a name="risky-sign-ins"></a>Inícios de sessão de risco

O Azure Active Directory Deteta [tipos de eventos de risco](../reports-monitoring/concept-risk-events.md#risk-event-types) no offline e em tempo real. Cada evento de risco que foi detetado um início de sessão de um utilizador contribui para um conceito lógico chamado o início de sessão arriscado. Um risco início de sessão é um indicador de uma tentativa de início de sessão não poderão ter sido executada pelo proprietário legítimo de uma conta de utilizador.


### <a name="sign-in-risk-level"></a>Nível de risco do início de sessão

Um nível de risco de início de sessão é uma indicação (elevada, média ou baixa) da probabilidade de que uma tentativa de início de sessão não foi efetuada pelo proprietário legítimo de uma conta de utilizador.

### <a name="mitigating-sign-in-risk-events"></a>Mitigar eventos de risco de início de sessão

Uma atenuação é uma ação para limitar a capacidade de um atacante explore uma identidade comprometida ou dispositivo sem restaurar o dispositivo ou a identidade para um estado seguro. Uma atenuação não resolve eventos de risco de início de sessão anteriores associados com a identidade ou dispositivo.

Para mitigar o risco de inícios de sessão automaticamente, pode configurar políticas de segurança de risco de início de sessão. Utilizar estas políticas, considere o nível de risco do utilizador ou o início de sessão para inícios de sessão de risco de bloquear ou exigir que o utilizador para efetuar a autenticação multifator. Estas ações podem impedir que um invasor explorando uma identidade roubada provocar danos e podem lhe oferecer algum tempo para proteger a identidade.

### <a name="sign-in-risk-security-policy"></a>Política de segurança de risco de início de sessão
Uma política de risco de início de sessão é uma política de acesso condicional que avalia o risco para um início de sessão-in específico e aplica-se mitigações com base em regras e condições predefinidas.

![Política de início de sessão de risco](./media/overview/1014.png "política de risco de início de sessão")

O Azure AD Identity Protection ajuda a gerir a redução de inícios de sessão de risco ao permitir-lhe:

* Defina os utilizadores e grupos que a política aplica-se a:

    ![Política de início de sessão de risco](./media/overview/1015.png "política de risco de início de sessão")
* Defina o risco de início de sessão ao nível limiar (baixa, média ou alta) que aciona a política:

    ![Política de início de sessão de risco](./media/overview/1016.png "política de risco de início de sessão")
* Defina os controlos a serem impostas quando a política aciona:  

    ![Política de início de sessão de risco](./media/overview/1017.png "política de risco de início de sessão")
* Alterne o estado da política:

    ![Registo na MFA](./media/overview/403.png "registo na MFA")
* Rever e avaliar o impacto de uma alteração antes de ativar ele:

    ![Política de início de sessão de risco](./media/overview/1018.png "política de risco de início de sessão")

#### <a name="what-you-need-to-know"></a>O que precisa saber
Pode configurar uma política de segurança de risco de início de sessão para exigir autenticação multifator:

![Política de início de sessão de risco](./media/overview/1017.png "política de risco de início de sessão")

No entanto, por motivos de segurança, esta definição só funciona para os utilizadores que já tenham sido registados para a autenticação multifator. Se a condição para exigir autenticação multifator for satisfeita para um utilizador que ainda não está registado na autenticação multifator, o utilizador está bloqueado.

Como melhor prática, se quiser exigir autenticação multifator para risco inícios de sessão, deve:

1. Ativar a [política de registo de autenticação multifator](#multi-factor-authentication-registration-policy) para os utilizadores afetados.
2. Requer que os utilizadores afetados para início de sessão numa sessão não risco para efetuar um registo na MFA

Concluir estes passos garante que a autenticação multifator é necessária um risco para início de sessão.

#### <a name="best-practices"></a>Melhores práticas
Escolher uma **elevada** limiar reduz o número de vezes que uma política é acionada e minimiza o impacto para os utilizadores.  

No entanto, exclui **baixa** e **médio** inícios de sessão sinalizados para risco da política, que não pode bloquear um invasor a exploração de uma identidade comprometida.

Ao definir a política

* Excluir os usuários que não o fizer / não é possível ter a autenticação multifator
* Excluir os usuários em localidades onde o ativar a política não é prático (por exemplo, sem acesso ao suporte técnico)
* Excluir utilizadores que têm propensão para gerar muitos falsos positivos (desenvolvedores, analistas de segurança)
* Utilize um **elevada** limiar durante o política inicial com, ou se deve minimizar desafios vistos pelos utilizadores finais.
* Utilize um **baixa** limiar se a sua organização precisar de maior segurança. Selecionar uma **baixa** limiar introduz utilizador adicional início de sessão desafios, mas uma maior segurança.

A predefinição recomendada na maioria das organizações é configurar uma regra para um **médio** limiar para encontrar o equilíbrio entre segurança e usabilidade.

A política de risco de início de sessão é:

* Aplicadas a todo o tráfego de navegador e inícios de sessão que utilizam autenticação moderna.
* Não é aplicada a aplicações através de protocolos de segurança mais antigos, desativando o ponto de extremidade do WS-Trust no IDP federado, por exemplo, o AD FS.

O **eventos de risco** página na consola do Identity Protection apresenta uma lista de todos os eventos:

* Esta política foi aplicada a
* Pode rever a atividade e determinar se a ação foi apropriada ou não

Para uma descrição geral da experiência do usuário relacionadas, consulte:

* [Recuperação de início de sessão arriscada](flows.md#risky-sign-in-recovery)
* [Risco início de sessão bloqueado](flows.md#risky-sign-in-blocked)  
* [Experiências de início de sessão com o Azure AD Identity Protection](flows.md)  

**Para abrir a caixa de diálogo de configuração relacionados**:

- Sobre o **do Azure AD Identity Protection** painel, na **configurar** secção, clique em **política de risco de início de sessão**.

    ![Política de risco do utilizador](./media/overview/1014.png "política de risco do utilizador")



## <a name="users-flagged-for-risk"></a>Utilizadores marcados em risco

Todas as ativas [eventos de risco](../reports-monitoring/concept-risk-events.md) que foram detetadas pelo Azure Active Directory para um utilizador contribuir para um conceito lógico chamado risco de utilizador. Um utilizador sinalizado para risco é um indicador de uma conta de utilizador que pode ter sido comprometida.

![Utilizadores marcados em risco](./media/overview/1200.png)


### <a name="user-risk-level"></a>O utilizador tem um nível de risco elevado

Um nível de risco do utilizador é uma indicação (elevada, média ou baixa) da probabilidade de que a identidade do utilizador foi comprometida. Esta é calculada com base sobre os eventos de risco de utilizador que estão associados a identidade do utilizador.

O estado de um evento de risco está **Active Directory** ou **Closed**. Apenas os eventos que são de risco **Active Directory** contribuir para o cálculo de nível de risco de utilizador.

O nível de risco do utilizador é calculado usando as seguintes entradas:

* Eventos de risco de Active Directory que afetam o utilizador
* Nível de risco desses eventos
* Se as ações de remediação foram executadas

![Riscos de usuário](./media/overview/1031.png "riscos de utilizador")

Pode utilizar os níveis de risco do utilizador para criar políticas de acesso condicional a bloquear utilizadores de risco de iniciar sessão ou forçá-los em segurança alterar a palavra-passe.

### <a name="closing-risk-events-manually"></a>Fechar eventos de risco manualmente

Na maioria dos casos, realizará ações de remediação, como uma palavra-passe segura repor fechar automaticamente os eventos de risco. No entanto, isso pode não sempre ser possível.  
Isto é, por exemplo, o caso, quando:

* Um utilizador com eventos de risco de Active Directory foi eliminado
* Uma investigação revela que tem sido realizar um evento de risco comunicados pelo usuário legítimo

Como os eventos de risco que são **Active Directory** contribuir para o cálculo de risco do utilizador, poderá ter de reduza manualmente um nível de risco fechando os eventos de risco manualmente.  
Durante a investigação, pode optar por colocar uma destas ações para alterar o estado de um evento de risco:

![Ações](./media/overview/34.png "ações")

* **Resolver** - se depois de investigar um evento de risco, que realizou uma ação de remediação adequadas fora de proteção de identidade e considera que deve ser considerado o evento de risco fechado, marque o evento como resolvido. Resolver eventos irão definir o estado do evento de risco para fechado e o evento de risco já não irá contribuir para o risco de utilizador.
* **Marcar como falso-positivos** -em alguns casos, pode investigar um evento de risco e detetar que incorretamente foi sinalizado como um risco. Pode ajudar a reduzir o número de tais ocorrências, marcando o evento de risco como falso positivo. Isso ajudará os algoritmos de machine learning para melhorar a classificação de eventos semelhantes no futuro. É o estado de eventos de Falso-positivos **fechado** e já não irão contribuir para o risco de utilizador.
* **Ignorar** – se não tomar qualquer ação de remediação, mas o evento de risco a serem removidos da lista Active Directory, pode marcar um evento de risco ignorar e o estado do evento será fechado. Eventos ignorados não contribuem para o risco de utilizador. Esta opção deve ser usada apenas em circunstâncias invulgares.
* **Reativar** -eventos de risco que foram fechados manualmente (escolhendo **resolver**, **falso positivo**, ou **ignorar**) pode ser reativado, definindo o evento novamente o estado **Active Directory**. Eventos de risco reativado contribuem para o cálculo de nível de risco de utilizador. Eventos de risco fechados através de remediação (como repor uma palavra-passe segura) não podem ser reativados.

**Para abrir a caixa de diálogo de configuração relacionados**:

1. Sobre o **do Azure AD Identity Protection** painel, em **investigar**, clique em **eventos de risco**.

    ![Reposição de palavra-passe manual](./media/overview/1002.png "de reposição de palavra-passe Manual")
2. Na **eventos de risco** , clique num risco.

    ![Reposição de palavra-passe manual](./media/overview/1003.png "de reposição de palavra-passe Manual")
3. No painel de risco, com o botão direito um utilizador.

    ![Reposição de palavra-passe manual](./media/overview/1004.png "de reposição de palavra-passe Manual")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Fechar todos os eventos de risco de um utilizador manualmente
Em vez de fechar manualmente individualmente eventos de risco de um utilizador de segurança, do Azure Active Directory Identity Protection também fornece um método para fechar todos os eventos para um utilizador com um só clique.

![Ações](./media/overview/2222.png "ações")

Quando clica em **dispensar todos os eventos**, todos os eventos são fechados e o utilizador afectado já não está em risco.

### <a name="remediating-user-risk-events"></a>Resolver eventos de risco do utilizador

Uma atualização é uma ação para proteger uma identidade ou um dispositivo que foi anteriormente suspeita ou conhecido por ser comprometido. Uma ação de remediação restaura a identidade ou um dispositivo para um estado seguro e resolve anterior eventos de risco associados com a identidade ou dispositivo.

Para remediar eventos de risco do utilizador, pode:

* Executar uma palavra-passe segura repor manualmente a remediar eventos de risco do utilizador
* Configurar uma política de segurança de risco do utilizador para mitigar ou remediar automaticamente eventos de risco do utilizador
* Recriar a imagem do dispositivo infetado  

#### <a name="manual-secure-password-reset"></a>Reposição de palavra-passe segura manual
Uma reposição de palavra-passe segura é uma remediação em vigor a partir de muitos eventos de risco e quando executada, automaticamente fecha estes eventos de risco e recalcula o nível de risco do utilizador. Pode utilizar o dashboard do Identity Protection para iniciar uma palavra-passe reposta para um utilizador de risco.

A caixa de diálogo relacionada fornece dois métodos diferentes para repor uma palavra-passe:

**Repor palavra-passe** - selecione **exigir que o utilizador para repor a palavra-passe** para permitir ao utilizador para a Self-recuperação se o usuário tiver registrado para multi-factor authentication. Durante próximo início de sessão o usuário, o utilizador será necessária para resolver um desafio de autenticação multifator com êxito e, em seguida, forçado a alterar a palavra-passe. Esta opção não está disponível se a conta de utilizador não estiver registado multi-factor authentication.

**Palavra-passe temporária** - selecione **gerar uma palavra-passe temporária** imediatamente invalidar a palavra-passe existente e criar uma nova palavra-passe temporária ao utilizador. Envie a nova palavra-passe temporária para um endereço de correio eletrónico alternativo para o utilizador ou para o Gestor do utilizador. Uma vez que a palavra-passe é temporária, será pedido ao utilizador para alterar a palavra-passe após o início de sessão.

![Política](./media/overview/1005.png "política")

**Para abrir a caixa de diálogo de configuração relacionados**:

1. Sobre o **do Azure AD Identity Protection** painel, clique em **utilizadores sinalizados para risco**.

    ![Reposição de palavra-passe manual](./media/overview/1006.png "de reposição de palavra-passe Manual")
2. Na lista de utilizadores, selecione um utilizador com eventos de risco de pelo menos um.

    ![Reposição de palavra-passe manual](./media/overview/1007.png "de reposição de palavra-passe Manual")
3. No painel do utilizador, clique em **Repor palavra-passe**.

    ![Reposição de palavra-passe manual](./media/overview/1008.png "de reposição de palavra-passe Manual")

### <a name="user-risk-security-policy"></a>Política de segurança de risco do utilizador
Uma política de segurança de risco do utilizador é uma política de acesso condicional que avalia o nível de risco para um utilizador específico e aplica-se ações de remediação e atenuação com base em regras e condições predefinidas.

![Política de risco do utilizador](./media/overview/1009.png "política de risco do utilizador")

O Azure AD Identity Protection ajuda a gerir a mitigação e remediação de utilizadores sinalizados para risco ao permitir-lhe:

* Defina os utilizadores e grupos que a política aplica-se a:

    ![Política de risco do utilizador](./media/overview/1010.png "política de risco do utilizador")
* Defina o utilizador risco nível limiar (baixa, média ou alta) que aciona a política:

    ![Política de risco do utilizador](./media/overview/1011.png "política de risco do utilizador")
* Defina os controlos a serem impostas quando a política aciona:

    ![Política de risco do utilizador](./media/overview/1012.png "política de risco do utilizador")
* Alterne o estado da política:

    ![Política de risco do utilizador](./media/overview/403.png "registo na MFA")
* Rever e avaliar o impacto de uma alteração antes de ativar ele:

    ![Política de risco do utilizador](./media/overview/1013.png "política de risco do utilizador")

Escolher uma **elevada** limiar reduz o número de vezes que uma política é acionada e minimiza o impacto para os utilizadores.
No entanto, exclui **baixa** e **médio** utilizadores sinalizados para risco da política, que pode não proteger identidades ou dispositivos que foram anteriormente suspeita ou conhecidos por ser comprometido.

Ao definir a política

* Excluir utilizadores que têm propensão para gerar muitos falsos positivos (desenvolvedores, analistas de segurança)
* Excluir os usuários em localidades onde o ativar a política não é prático (por exemplo, sem acesso ao suporte técnico)
* Utilize um **elevada** limiar durante o política inicial com, ou se deve minimizar desafios vistos pelos utilizadores finais.
* Utilize um **baixa** limiar se a sua organização precisar de maior segurança. Selecionar uma **baixa** limiar introduz utilizador adicional início de sessão desafios, mas uma maior segurança.

A predefinição recomendada na maioria das organizações é configurar uma regra para um **médio** limiar para encontrar o equilíbrio entre segurança e usabilidade.

Para uma descrição geral da experiência do usuário relacionadas, consulte:

* [Comprometido de fluxo de recuperação de conta](flows.md#compromised-account-recovery).  
* [Comprometido fluxo conta bloqueada](flows.md#compromised-account-blocked).  

**Para abrir a caixa de diálogo de configuração relacionados**:

- Sobre o **Azure AD Identity Protection** painel, na **configurar** secção, clique em **política de risco do utilizador**.

    ![Política de risco do utilizador](./media/overview/1009.png "política de risco do utilizador")

### <a name="mitigating-user-risk-events"></a>Mitigar eventos de risco do utilizador
Os administradores podem definir uma política de segurança de risco do utilizador para impedir que os utilizadores após o início de sessão dependendo do nível de risco.

Bloquear um início de sessão:

* Impede que a geração de novos eventos de risco de utilizador para o utilizador afectado
* Permite aos administradores manualmente remediar os eventos de risco que afetam a identidade do utilizador e restaurá-lo para um estado seguro



## <a name="multi-factor-authentication-registration-policy"></a>Política de registo de autenticação multifator
Autenticação multifator do Azure é um método de verificação quem é o que requer a utilização de mais do que apenas um nome de utilizador e palavra-passe. Ele fornece uma segunda camada de segurança para inícios de sessão de utilizador e transações.  
Recomendamos que exigem multi-factor authentication para inícios de sessão de utilizador, porque ele:

* Proporciona uma autenticação segura com uma variedade de opções de verificação simples
* Desempenha um papel fundamental na preparação de sua organização para proteger e recuperar a partir de comprometimentos de conta

![Política de risco do utilizador](./media/overview/1019.png "política de risco do utilizador")

Para obter mais detalhes, consulte [o que é o Azure multi-factor Authentication?](../authentication/multi-factor-authentication.md)

O Azure AD Identity Protection ajuda a gerir o roll-cancelar o registro de autenticação multifator ao configurar uma política que permite-lhe:

* Defina os utilizadores e grupos que a política aplica-se a:

    ![Política da MFA](./media/overview/1020.png "política da MFA")
* Defina os controlos a serem impostas quando a política aciona::  

    ![Política da MFA](./media/overview/1021.png "política da MFA")
* Alterne o estado da política:

    ![Política da MFA](./media/overview/403.png "política da MFA")
* Ver o estado atual do registo:

    ![Política da MFA](./media/overview/1022.png "política da MFA")

Para uma descrição geral da experiência do usuário relacionadas, consulte:

* [Fluxo de registo de autenticação multifator](flows.md#multi-factor-authentication-registration).  
* [Experiências de início de sessão com o Azure AD Identity Protection](flows.md).  

**Para abrir a caixa de diálogo de configuração relacionados**:

- Sobre o **do Azure AD Identity Protection** painel, na **configurar** secção, clique em **registo de multi-factor authentication**.

    ![Política da MFA](./media/overview/1019.png "política da MFA")

## <a name="next-steps"></a>Passos Seguintes
* [O Channel 9: Azure AD e mostrar de identidade: pré-visualização de proteção de identidade](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Ativar o Azure Active Directory Identity Protection](enable.md)

* [Vulnerabilidades detetadas pelo Azure Active Directory Identity Protection](vulnerabilities.md)

* [Eventos de risco do Azure Active Directory](../active-directory-identity-protection-risk-events.md)

* [Notificações do Azure Active Directory Identity Protection](notifications.md)

* [Playbook do Azure Active Directory Identity Protection](playbook.md)

* [Glossário do Azure Active Directory Identity Protection](glossary.md)

* [Experiências de início de sessão com o Azure AD Identity Protection](flows.md)

* [O Azure Active Directory Identity Protection - como desbloquear utilizadores](howto-unblock-user.md)

* [Introdução ao Azure Active Directory Identity Protection e Microsoft Graph](graph-get-started.md)
