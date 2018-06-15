---
title: Aquisições de administrador de um diretório não gerido ou o inquilino de sombra de volumes no Azure Active Directory | Microsoft Docs
description: Como assumir um nome de domínio DNS num diretório não gerido (inquilino de sombra de volumes) no Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 04/06/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: fa299603a14ef94ab4cf3eac2b1123def1a0b6de
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34594888"
---
# <a name="take-over-an-unmanaged-directory-as-administrator-in-azure-active-directory"></a>Assuma um diretório não gerido como administrador no Azure Active Directory
Este artigo descreve duas formas de assumir um nome de domínio DNS num diretório no Azure Active Directory (Azure AD) não gerido. Quando um utilizador autónomo se inscreve num serviço cloud que utiliza o Azure AD, é adicionado a um diretório do Azure AD não gerido, com base no domínio do respetivo e-mail Para mais informações sobre o Self-Service ou inscrever "viral" para um serviço, consulte [que é a inscrição self-service do Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-self-service-signup)

## <a name="decide-how-you-want-to-take-over-an-unmanaged-directory"></a>Decida como pretende assumir um diretório de não gerido
Durante o processo de obtenção do controlo administrativo, pode provar a propriedade, conforme descrito em [Add a custom domain name to Azure AD](add-custom-domain.md) (Adicionar um nome de domínio personalizado ao Azure AD). As secções seguintes explicam a experiência de administração mais detalhadamente, mas pode ver este resumo:

* Quando faz uma [obtenção do controlo administrativo “interna”](#internal-admin-takeover) de um diretório do Azure não gerido, é adicionado como o administrador global desse diretório. Nenhum utilizador, domínio ou plano de serviço é migrado para outro diretório que seja administrado por si.

* Quando faz uma [obtenção do controlo administrativo “externo”](#external-admin-takeover) de um diretório do Azure não gerido, adiciona o nome de domínio DNS desse diretório ao seu diretório do Azure gerido. Quando adiciona o nome de domínio, é criado um mapeamento dos utilizadores para os recursos no diretório do Azure gerido, para que os utilizadores possam continuar a aceder a serviços sem interrupções. 

## <a name="internal-admin-takeover"></a>Aquisições admin interno

Alguns produtos que incluem o SharePoint e o OneDrive, como o Office 365, não suportam aquisições externa. Se for esse seu cenário, ou se for administrador e pretende assumir um não geridos ou inquilino "sombra de volumes" Criar por utilizadores que utilizou a inscrição Self-Service, pode fazê-lo com uma administrador interno aquisições.

1. Crie um contexto de utilizador no inquilino não gerido através de inscrever com, tais como o Power BI. Para sua comodidade do exemplo, estes passos partem do princípio de que o caminho.

2. Abra o [site Power BI](https://powerbi.com) e selecione **iniciar livre**. Introduza uma conta de utilizador que utiliza o nome de domínio para a organização; Por exemplo, `admin@fourthcoffee.xyz`. Depois de introduzir o código de verificação, verifique o código de confirmação de correio eletrónico.

3. No e-mail de confirmação do Power BI, selecionar **Sim, que é-me**.

4. Iniciar sessão para o [Centro de administração do Office 365](https://portal.office.com/adminportal/Home) com a conta de utilizador do Power BI. Receberá uma mensagem que indica a **tornar-se o administrador** do nome de domínio que já foi verificado o inquilino não gerido. Selecione **Sim, pretendo que seja administrador**.
  
  ![captura de ecrã primeiro para Become administrador](./media/domains-admin-takeover/become-admin-first.png)
  
5. Adicionar o registo TXT para provar que é seu o nome de domínio **fourthcoffee.xyz** na sua entidade de registo de nome de domínio. Neste exemplo, é GoDaddy.com.
  
  ![Adicione um registo txt para o nome de domínio](./media/domains-admin-takeover/become-admin-txt-record.png)

Quando os registos DNS TXT são verificados na sua entidade de registo de nome de domínio, pode gerir o inquilino do Azure AD.

Quando concluir os passos anteriores, está agora o administrador global do inquilino café quarta no Office 365. Para integrar o nome de domínio com outros serviços do Azure, pode removê-lo a partir do Office 365 e adicioná-lo para um inquilino gerido diferente no Azure.

### <a name="adding-the-domain-name-to-a-managed-tenant-in-azure-ad"></a>Adicionar o nome de domínio para um inquilino gerido no Azure AD 

1. Abra o [Centro de administração do Office 365](https://portal.office.com/adminportal/Home).
2. Selecione **utilizadores** separador e criar uma nova conta de utilizador com um nome como *user@fourthcoffeexyz.onmicrosoft.com* que não utiliza o nome de domínio personalizado. 
3. Certifique-se de que a nova conta de utilizador tem privilégios de administrador global para o inquilino do Azure AD.
4. Abra **domínios** separador no Centro de administração do Office 365, selecione o nome de domínio e selecione **remover**. 
  
  ![Remova o nome de domínio do Office 365](./media/domains-admin-takeover/remove-domain-from-o365.png)
  
5. Se tiver quaisquer utilizadores ou grupos do Office 365 que referenciam o nome de domínio removidos, tem de ser mudados para o. c o m domínio. Se forçar a eliminar o nome de domínio, todos os utilizadores são automaticamente mudar o nome, neste exemplo *user@fourthcoffeexyz.onmicrosoft.com*.
  
6. Iniciar sessão para o [Centro de administração do Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) com uma conta que seja administrador global do inquilino do Azure AD.
  
7. Selecione **nomes de domínio personalizado**, em seguida, adicione o nome de domínio. Terá de introduzir os registos TXT de DNS para verificar a propriedade do nome de domínio. 
  
  ![domínio adicionado ao Azure AD](./media/domains-admin-takeover/add-domain-to-azure-ad.png)
  
> [!NOTE]
> Os utilizadores do Power BI ou do Azure Rights Management service que tem licenças atribuídas no inquilino do Office 365 tem de guardar os seus dashboards se o nome de domínio for removido. Devem iniciar sessão com um nome de utilizador como *user@fourthcoffeexyz.onmicrosoft.com* vez *user@fourthcoffee.xyz*.

## <a name="external-admin-takeover"></a>Aquisições admin externo

Se já gere um inquilino com serviços do Azure ou do Office 365, não é possível adicionar um nome de domínio personalizado se já está a ser verificado no outro inquilino do Azure AD. No entanto, do seu inquilino gerido no Azure AD, pode demorar mais um inquilino não gerido como uma admin externo aquisições. O procedimento geral segue o artigo [adicionar um domínio personalizado ao Azure AD](add-custom-domain.md).

Quando verificar a propriedade do nome de domínio, o Azure AD remove o nome de domínio do inquilino não gerido e move-o para o inquilino existente. Admin externo aquisições de um diretório não gerido requer o mesmo processo de validação de TXT de DNS como administrador interno aquisições. A diferença é que os seguintes são também movidos através de com o nome de domínio:

- Utilizadores
- Subscrições
- Atribuições de licenças

### <a name="support-for-external-admin-takeover"></a>Suporte para aquisições admin externo
Aquisições admin externo é suportada pelos seguintes serviços online:

- Power BI
- Azure Rights Management
- Exchange Online

Os planos de serviço suportados incluem:

- Power BI livre
- Power BI Pro
- PowerApps livre
- PowerFlow livre
- RMS para indivíduos
- Microsoft Stream
- Versão de avaliação gratuita do Dynamics 365

Aquisições admin externo não é suportada relativamente a qualquer serviço que tenha os planos de serviço que incluem o SharePoint, o OneDrive ou o Skype para empresas; Por exemplo, através de uma subscrição gratuita do Office ou o SKU básico do Office. Opcionalmente, pode utilizar o [ **ForceTakeover** opção](#azure-ad-powershell-cmdlets-for-the-forcetakeover-option) para remover o nome de domínio do inquilino não gerido e verificá-lo no inquilino pretendido. Esta opção ForceTakeover não irá mover através de utilizadores ou manter o acesso à subscrição. Em vez disso, esta opção apenas se move o nome de domínio. 

#### <a name="more-information-about-rms-for-individuals"></a>Obter mais informações sobre o RMS para indivíduos

Para [RMS para indivíduos](/information-protection/understand-explore/rms-for-individuals), quando o inquilino não gerido na mesma região que o inquilino que lhe pertence, criados automaticamente [chave de inquilino do Azure Information Protection](/information-protection/plan-design/plan-implement-tenant-key) e [predefinido modelos de proteção](/information-protection/deploy-use/configure-usage-rights#rights-included-in-the-default-templates) adicionalmente são movidos através de com o nome de domínio. 

A chave e os modelos não são movidos através de quando o inquilino não gerido está numa região diferente. Por exemplo, o inquilino não gerido está a ser o inquilino que possui está a ser americano norte e Europa. 

Embora o RMS para indivíduos é concebido para suportar a autenticação do Azure AD para abrir conteúdos protegidos, esta não impedir que os utilizadores também proteger conteúdo. Se os utilizadores proteger conteúdo com a subscrição do RMS para indivíduos e a chave e os modelos não foram movidos através de, esse conteúdo não estarão acessível após a aquisições de domínio.    

### <a name="azure-ad-powershell-cmdlets-for-the-forcetakeover-option"></a>Cmdlets do Azure AD PowerShell para a opção de ForceTakeover
Pode ver estes cmdlets utilizados no [exemplo do PowerShell](#powershell-example).


Cmdlet | Utilização 
------- | -------
`connect-msolservice` | Quando lhe for solicitado, inicie sessão no seu inquilino gerido.
`get-msoldomain` | Mostra os nomes de domínio associados ao inquilino atual.
`new-msoldomain –name <domainname>` | Adiciona o nome de domínio ao inquilino como Unverified (nenhuma verificação DNS ter sido efetuada ainda).
`get-msoldomain` | O nome de domínio está agora incluído na lista de nomes de domínio associado ao seu inquilino gerido, mas está listado como **Unverified**.
`get-msoldomainverificationdns –Domainname <domainname> –Mode DnsTxtRecord` | Fornece as informações para colocar em novo registo TXT de DNS para o domínio (MS = xxxxx). Verificação poderá não acontecer imediatamente porque demora algum tempo para o registo TXT se propague, por isso, aguarde alguns minutos antes de considerar a **- ForceTakeover** opção. 
`confirm-msoldomain –Domainname <domainname> –ForceTakeover Force` | <li>Se o seu nome de domínio ainda não está a ser verificado, pode avançar com a **- ForceTakeover** opção. Verifica se o registo TXT foi criado e arranca, o processo de alimentar.<li>O **- ForceTakeover** opção deve ser adicionada ao cmdlet, apenas quando forçar uma aquisições admin externo, como quando o inquilino não gerido tem serviços do Office 365 bloquear a aquisições.
`get-msoldomain` | A lista de domínio mostra agora o nome de domínio como **Verified**.

### <a name="powershell-example"></a>Exemplo do PowerShell

1. Ligar ao Azure AD com as credenciais que foram utilizadas para responder a oferta de self-service:
  ````
    import-module MSOnline
    $msolcred = get-credential
    
    connect-msolservice -credential $msolcred
  ````
2. Obter uma lista de domínios:
  
  ````
    Get-MsolDomain
  ````
3. Execute o cmdlet Get-MsolDomainVerificationDns para criar um desafio de:
  ````
    Get-MsolDomainVerificationDns –DomainName *your_domain_name* –Mode DnsTxtRecord
  
    For example:
  
    Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord
  ````

4. Copie o valor (o desafio) que é devolvido a partir deste comando. Por exemplo:
  ````
    MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9
  ````
5. No seu espaço de nomes DNS público, crie um registo txt DNS que contém o valor que copiou no passo anterior. O nome para este registo é o nome do domínio principal, por isso, se criar este registo de recursos utilizando a função DNS do Windows Server, deixe colar registo nome em branco e apenas o valor na caixa de texto.
6. Execute o cmdlet MsolDomain confirmar para verificar o desafio:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName *your_domain_name*
  ````
  
  Por exemplo:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName contoso.com
  ````

Um desafio bem-sucedida devolve o pedido sem um erro.

## <a name="next-steps"></a>Passos Seguintes
* [Adicionar um nome de domínio personalizado ao Azure AD](add-custom-domain.md)
* [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Referência de Cmdlet do Azure](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
