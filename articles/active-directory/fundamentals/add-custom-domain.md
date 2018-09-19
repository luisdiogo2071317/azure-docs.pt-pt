---
title: Como adicionar o seu domínio personalizado ao Azure Active Directory | Documentos da Microsoft
description: Saiba como adicionar um domínio personalizado com o portal do Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: lizross
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: e49e362528f5fcd00a13a9fc1b233e62a569fe5a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46297951"
---
# <a name="how-to-add-your-custom-domain-name-using-the-azure-active-directory-portal"></a>Como: adicionar o seu nome de domínio personalizado com o portal do Azure Active Directory
Cada novo Azure inquilino AD vem com um nome de domínio inicial *domainname*. onmicrosoft.com. Não é possível alterar ou eliminar o nome de domínio inicial, mas pode adicionar nomes da sua organização para a lista. Adicionar nomes de domínio personalizados ajuda-o a criar nomes de utilizador que estão familiarizados para seus usuários, como _alain@contoso.com_.

>[!Note]
>Tem de repetir todo este processo, do início ao fim, para cada um dos seus nomes de domínio personalizado.

## <a name="add-a-custom-domain-name"></a>Adicionar um nome de domínio personalizado
Em primeiro lugar, tem de adicionar seu nome de domínio personalizado para o inquilino do Azure AD.

### <a name="to-add-a-custom-domain-name"></a>Para adicionar um nome de domínio personalizado
1. Inicie sessão para o [portal do Azure AD](https://portal.azure.com/) com uma conta de Administrador Global do diretório.

<<<<<<< HEAD
> [!TIP]
> Se pretende federar o seu Windows Server AD no local com o Azure AD, tem de selecionar a caixa de configuração **Pretendo configurar este domínio para o início de sessão único com o meu Active Directory local** ao executar a ferramenta Azure AD Connect para sincronizar os diretórios. Também tem de registar o mesmo nome de domínio que seleciona para a federação com o seu diretório no local no passo **Domínio do Azure AD** no assistente. Pode ver como funciona esse passo no assistente [nestas instruções](../hybrid/how-to-connect-install-custom.md#verify-the-azure-ad-domain-selected-for-federation). Se não tiver a ferramenta Azure AD Connect, pode [transferi-lo aqui](http://go.microsoft.com/fwlink/?LinkId=615771).
=======
2. Selecione **do Azure Active Directory**, selecione **nomes de domínio personalizado**e, em seguida, selecione **Adicionar domínio personalizado**.
>>>>>>> 73f447b1499ba1f189efb6832ad759f1230e55a2

    ![Fabrikam - Custom domain names blade, with Add custom domain option highlighted](media/add-custom-domain/add-custom-domain.png)

3. Escreva o seu nome de domínio empresarial novo para o **nome de domínio personalizado** caixa (por exemplo, _contoso.com_) e, em seguida, selecione **Adicionar domínio**.

    >[!Important]
    >Tem de incluir. com, .net ou qualquer outra extensão de nível superior para que isso funcione corretamente.

    ![Fabrikam – painel de nomes de domínio personalizado, com o botão de domínio de adicionar realçado](media/add-custom-domain/add-custom-domain-blade.png)

4. Copie as informações de entrada DNS do **Contoso** painel.

    ![Contoso painel com informações de entrada DNS](media/add-custom-domain/contoso-blade-with-dns-info.png)

## <a name="add-your-domain-name-with-a-domain-name-registrar"></a>Adicionar o seu nome de domínio com uma entidade de registo de nome de domínio
Em seguida, tem de atualizar o ficheiro de zona DNS para o seu domínio personalizado novo. Pode usar [zonas DNS](https://docs.microsoft.com/azure/dns/dns-getstarted-portal) para o Azure, Office 365, ou registos DNS externos ou pode adicionar a nova entrada DNS usando uma entidade de registo DNS diferente (por exemplo, [InterNIC](https://go.microsoft.com/fwlink/p/?LinkId=402770)).

### <a name="to-add-your-domain-name"></a>Para adicionar o seu nome de domínio 
1. Inicie sessão para a entidade de registo de nome de domínio para o seu domínio personalizado. Se não tiver as permissões corretas para atualizar a sua participação, terá de contactar a alguém com essas permissões.

2. Depois da entrada DNS é atualizada com a entidade de registo, tem de atualizar o ficheiro de zona DNS com as informações fornecidas pelo Azure AD.

    >[!Note]
    >A entrada DNS não altera a como funciona o roteamento de emails ou hospedagem na web.

## <a name="verify-your-custom-domain-name"></a>Verificar o seu nome de domínio personalizado
Depois de registar o seu nome de domínio personalizado, pode demorar alguns segundos para algumas horas antes das informações de DNS é propagada para onde o Azure AD pode vê-lo como válido.

### <a name="to-verify-your-custom-domain-name"></a>Para verificar o seu nome de domínio personalizado
1. Inicie sessão para o [portal do Azure AD](https://portal.azure.com/) com uma conta de Administrador Global do diretório.

2. Selecione **do Azure Active Directory**e, em seguida, selecione **nomes de domínio personalizado**.

3. Sobre o **Fabrikam - nomes de domínio personalizado** painel, selecione o nome de domínio personalizado **Contoso**.

    ![Fabrikam – painel de nomes de domínio personalizado, com a contoso realçado](media/add-custom-domain/custom-blade-with-contoso-highlighted.png)

4. Sobre o **Contoso** painel, selecione **verificar** para se certificar-se de que o domínio personalizado está corretamente registado e é válido para o Azure AD.

    ![Contoso painel com informações de entrada DNS e no botão verificar](media/add-custom-domain/contoso-blade-with-dns-info-verify.png)

### <a name="common-verification-issues"></a>Problemas comuns de verificação
Se o Azure AD não é possível verificar um nome de domínio personalizado, experimente as sugestões seguintes:
- **Aguarde, pelo menos, uma hora e tente novamente**. Registos DNS devem propagar antes do Azure AD pode verificar que o domínio e este processo podem demorar uma hora ou mais.

- **Certifique-se de que o registo DNS está correto.** Volte para o site de entidade de registo de nome de domínio e certifique-se a entrada existe e que corresponde às informações de entrada DSN fornecidas pelo Azure AD.

    Se não é possível atualizar o registo no site da entidade de registo, tem de partilhar a entrada com alguém que tenha as permissões corretas para adicionar a entrada e certifique-se de que ela está correta.

- **Certifique-se de que o nome de domínio não estiver noutro diretório.** Um nome de domínio só pode ser verificado num diretório, o que significa que se o seu nome de domínio estiver atualmente verificado noutro diretório, ele não pode também ser verificado no novo diretório. Para corrigir esse problema de duplicação, tem de eliminar o nome de domínio do diretório antigo. Para obter mais informações sobre como eliminar nomes de domínio, consulte [gerir nomes de domínio personalizado](../users-groups-roles/domains-manage.md).    

## <a name="next-steps"></a>Passos Seguintes
- Adicionar utilizadores ao seu domínio, consulte [gerir nomes de domínio personalizado](../users-groups-roles/domains-manage.md).

- Se tiver versões no local do Windows Server que pretende utilizar em conjunto com o Azure Active Directory, consulte [integrar seus diretórios no local com o Azure Active Directory](../connect/active-directory-aadconnect.md).