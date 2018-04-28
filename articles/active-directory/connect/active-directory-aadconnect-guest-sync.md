---
title: Sincronizar contas de utilizador de utilizadores convidados que utilizam o e-mail para início de sessão no Azure | Microsoft Docs
description: Este artigo explica como sincronizar as contas de utilizador de convidados que utilizam um ID alternativo para início de sessão para aplicações.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2018
ms.author: billmath
ms.openlocfilehash: d21f124858a7f98227eb301a97b9837e3adbba68
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="synchronizing-guest-user-accounts-that-use-email-for-sign-in-preview"></a>Sincronizar as contas de utilizador de convidados que utilizam o e-mail para início de sessão (pré-visualização)

>[!NOTE]
> Esta funcionalidade está atualmente em pré-visualização pública.

O cenário a seguir a situação em que podem ter utilizadores externos no seu local de endereços ambiente do AD, como parceiros, quem utilizar um método de início de sessão alternativo.

No exemplo anterior, Nina Morin funciona da Fabrikam e tem o seguinte endereço de e-mail: nmorin@fabrikam.com. Nina é um parceiro com Contoso e ela precisa de aceder a determinadas aplicações de que a Contoso tem. Contoso tiver criado uma conta para Nina e tem direcionado Nina para utilizar o respetivo endereço de correio eletrónico para início de sessão para aplicações.

Para as aplicações no local, este cenário tem trabalho great. Mas, agora, a Contoso está a mover estas aplicações para a nuvem e pretende ter a mesma experiência para os respetivos parceiros.

Este cenário endereços nesta situação.


## <a name="pre-requisites-and-assumptions"></a>Pré-requisitos e pressupostos
Esta secção contém uma lista de pré-requisitos e pressupostos que tem de ter em consideração antes de tentar configurar este cenário.

### <a name="pre-requisites"></a>Pré-requisitos
- Credenciais de administrador global para configurar o Azure AD Connect, certifique-se de domínios e configurar as definições de Federação do domínio
- O Azure AD Connect versão 1.1.524.0 ou superior
- Domínio verificado ao definir o UPN dos utilizadores externos de nuvem (exemplo: bmcontoso.com).
- Serviço de Federação para autenticar os utilizadores externos. Se utilizar o AD FS, tem de ser 2012 R2 ou superior
- MSOL PowerShell v-1.1 está instalado num computador para verificar as definições de Federação. Para obter mais informações, consulte [Azure ActiveDirectory (MSOnline)](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).


### <a name="assumptions"></a>Pressupostos 
- O Azure AD Connect tem já foi configurada e foi instalado com êxito. Para obter informações sobre como instalar o Azure AD Connect, consulte [do Azure AD Connect e a Federação](active-directory-aadconnectfed-whatis.md).
Este documento faz com que os seguintes pressupostos:
- Se tiver um serviço de Federação, configurar e que está com êxito a autenticar os utilizadores.
- os utilizadores externos podem autenticar utilizando o respetivo endereço de e-mail externos.
- - Utilizar um ID alternativo para início de sessão foi configurado e configurado. Os utilizadores podem autenticar utilizando o respetivo ID alternativo Para obter informações adicionais sobre como configurar um ID alternativo com o AD FS, consulte [configurar ID de início de sessão alternativo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id).

## <a name="task-1--prepare-the-environment"></a>Tarefa 1: Preparar o ambiente
A seguinte tarefa é mais de um informativa, de modo a que está pronto para começar a sincronizar as contas de externas para que estes podem iniciar sessão utilizando alternativa i, tais como o atributo de correio.

Defina os itens na tabela abaixo antes de passar para a segunda tarefa.

![Arquitetura](media/active-directory-aadconnect-guest-sync/guest2.png)

|Aspeto de ambiente|O que é utilizado para?|Implementação no seu ambiente|
|-----|-----|-----|
|Atributo UPN de nuvem|O atributo que preenche o UPN dos objetos de utilizador externo na nuvem. O sufixo UPN para as contas externos tem de ser um definido na pré-requisitos. Este é o domínio verificado.|* Exemplo: UserPrincipalName (nmorin@bmcontoso.com)|
|Endereço de início de sessão|O atributo que os utilizadores externos escrever no quando iniciar sessão. Este atributo tem de ter um formato de endereço de correio eletrónico e, na maioria dos casos coincide com o endereço de correio eletrónico real do utilizador externo.|* Exemplo: de correio (nmorin@fabrikam.com)|
|Filtro de âmbito do Azure AD Connect|O filtro que permite a filtragem as identidades externas ao âmbito as regras de sincronização definido mais tarde neste guia. Formas comuns de âmbito incluem: uma UO previamente definida na organização, uma convenção de nomenclatura determinada, um domínio específico, etc.|* Exemplo: UO contém Externals|
|Inquilino do Azure AD|O nome do inquilino do Azure AD, conforme é apresentado ao Azure AD Connect.|Exemplo: contoso.onmicrosoft.com|

A seguinte captura de ecrã tem três caixas descritas.
- O **Externals** UO, que é utilizado no Azure AD Connect no âmbito filtro e é a localização dos utilizadores externos.
- O **correio** atributo, o que é utilizado pelos utilizadores externos para início de sessão.
- O **userPrincipalName** atributo, o que é o domínio verificado que o ambiente no local está Federado com.

![Utilizador](media/active-directory-aadconnect-guest-sync/guest1.png)

## <a name="task-2--configure-azure-ad-connect"></a>Tarefa 2: Configurar o Azure AD Connect
Depois de ter informações de acima definidas, podemos avançar para configurar as regras de sincronização do Azure AD Connect. Para configurar as regras, utilize o editor de regras de sincronização do Azure AD Connect. Para obter mais informações sobre o editor, consulte [aprovisionamento declarativa](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

### <a name="how-to-configure-the-synchronization-rule"></a>Como configurar a regra de sincronização
Utilize o procedimento seguinte para configurar o Azure AD Connect.

1. Abra o editor de regras de sincronização do Azure AD Connect, acedendo a **editor de regras de sincronização de Start - Azure AD Connect -**.
2. No **Editor de regras de sincronização** ecrã, certifique-se de que é a direção **entrada** e à direita, clique em **Adicionar nova regra**.
3. No **Descrição** página, configure o seguinte e clique em **seguinte**.
    - **Nome** -introduza um nome para a regra 
    - **Sistema ligado:** -no local ambiente do AD
    - **Tipo de objeto sistema ligado:** -utilizador
    - **Tipo de objeto de Metaverso:** -pessoa
    - **Tipo de ligação:** -aderir
    - **Precedência:** - 90
    - 
![](media/active-directory-aadconnect-guest-sync/guest3.png)

4. No **filtro de âmbito** ecrã, clique em **adicionar grupo**.
5. Utilize as listas pendentes para configurar o filtro. Introduza o seguinte e clique em **seguinte**. Esta ação cria um filtro que só se aplica a objetos localizados na UO externo.
    - **Atributo** -dn
    - **Operador** -contém
    - **Valor** -Externals
 
 ![Filtro](media/active-directory-aadconnect-guest-sync/guest4.png)

6. No **regras de associação** ecrã, clique em **seguinte**.
7. No **transformações** ecrã, clique em **adicionar transformação**. Introduza as seguintes informações:
    - **FlowType** - constante
    - **Atributo de destino** -userType
    - **Origem** - convidados
8. No **transformações** ecrã, clique em **adicionar transformação**. Introduza as seguintes informações:
    - **FlowType** - direto
    - **Atributo de destino** -onPremisesUserPrincipalName
    - **Origem** -correio
9. No **transformações** ecrã, clique em **adicionar transformação**. Introduza as seguintes informações:
    - **FlowType** - direto
    - **Atributo de destino** -userPrincipalName
    - **Origem** -userPrincipalName ![transformação](media/active-directory-aadconnect-guest-sync/guest5.png)
10. Clique em **Adicionar**. 
11. No **Editor de regras de sincronização** ecrã, certifique-se de que é a direção **saída** e à direita, clique em **Adicionar nova regra**.
12. No **Descrição** página configurar o seguinte e clique em **seguinte**.
    - **Nome** -introduza um nome para a regra 
    - **Sistema ligado:** -inquilino do Azure AD
    - **Tipo de objeto sistema ligado:** -utilizador
    - **Tipo de objeto de Metaverso:** -pessoa
    - **Tipo de ligação** -aderir
    - **Precedência:** - 90
    - 
![Regra](media/active-directory-aadconnect-guest-sync/guest6.png)

13. No **Scoping filtro** ecrã, clique em **seguinte**.
14. No **regras de associação** ecrã, clique em **seguinte**.
15. No **transformações** ecrã, clique em **adicionar transformação**.  Introduza as seguintes informações:
    - **FlowType** - direto
    - **Atributo de destino** -userType
    - **Origem** -userType
9. No **transformações** ecrã, clique em **adicionar transformação**. Introduza as seguintes informações:
    - **FlowType** - direto
    - **Atributo de destino** -onPremisesUserPrincipalName
    - **Origem** -onPremisesUserPrincipalName ![transformação](media/active-directory-aadconnect-guest-sync/guest7.png)
10. Clique em **Adicionar**. 
11. Assim que tiver configurado estas regras terá de executar uma sincronização completa. Utilize o PowerShell para iniciar uma sincronização completa. Depois de concluída a sincronização pode avançar para o passo seguinte.

``` powershell
    Start-ADSyncSyncCycle -PolicyType Initial
```

## <a name="task-3--federation"></a>Tarefa 3: Federação
A seguinte tarefa é um informativa em algumas coisas que precisa de ter no local para que o cenário de trabalho.

Pode verificar as definições de federação com o Azure utilizando o Azure AD PowerShell. Este documento utiliza o v 1.1 do MSOL PowerShell. Pode instalar esta versão [aqui](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0).

### <a name="verify-the-federation-settings"></a>Verifique as definições de Federação
Utilize o procedimento seguinte para verificar as definições de Federação.
1. Abra o Windows PowerShell e ligar ao Azure AD utilizando o seguinte comando:
``` powershell
      Connect-MSOLservice
```
2.  Introduza as credenciais de administrador global
3.  Agora, introduza o seguinte comando:
  ``` powershell
      Get-MSOLDomainFederationSettings
  ```
4.   Tenha em atenção que as informações de Federação devem ser devolvidas. Tenha em atenção o **ActiveLogonUri** é o URL do servidor de Federação.

  ![de Federação](media/active-directory-aadconnect-guest-sync/guest8.png)

### <a name="verify-alternate-login-id"></a>Verificar o ID de início de sessão alternativo
Este documento utiliza do AD FS, como o fornecedor de identidade (Idp). Se estiver a utilizar um Idp diferentes, poderão muito estes passos.

1. Abra o Windows PowerShell e introduza o seguinte comando:
   ```powershell
    Get-ADFSClaimsProviderTrust
   ```
2. Deverá ver as informações do AD FS.  Tenha em atenção o **AlternateLoginID** e **LookupForests**.

![ADFS](media/active-directory-aadconnect-guest-sync/guest9.png)

## <a name="task-4--testing"></a>Tarefa 4: testar
Para verificar se este está a funcionar corretamente, terá de início de sessão para um ponto final que foi configurado para autenticar com Idp. Para testar isto, iremos implementado um Web site no Azure e estiver a utilizar o seguinte url: contososampapp.azurewebsites.net

### <a name="verify-that-you-can-sign-in-with-the-alternate-id"></a>Certifique-se de que que pode iniciar sessão com o ID alternativo
1. Início de sessão para o ponto final.</br>
![Endpoint](media/active-directory-aadconnect-guest-sync/guest10.png)
1. Introduza o seu nome de utilizador e será redirecionada para a página de início de sessão Federação.
![Federação](media/active-directory-aadconnect-guest-sync/guest11.png)
1. Introduza as suas credenciais.
2. Que deverá agora ser com êxito iniciou sessão.
![Êxito](media/active-directory-aadconnect-guest-sync/guest12.png)

## <a name="next-steps"></a>Passos Seguintes
- [Propriedades de um utilizador de colaboração do Azure Active Directory B2B](../../active-directory/active-directory-b2b-user-properties.md#key-properties-of-the-azure-ad-b2b-collaboration-user)
- [Configurar o ID de início de sessão alternativo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)
- [Azure AD Connect: histórico de versões](active-directory-aadconnect-version-history.md)
