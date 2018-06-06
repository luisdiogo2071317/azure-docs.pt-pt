---
title: Vários domínios do Azure AD Connect
description: Este documento descreve como configurar e a configuração de vários domínios de nível superior com o Office 365 e o Azure AD.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2f596f64041b3d429b99db482cd635ab441bf468
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801512"
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Suporte para Vários Domínios para Federação com o Azure AD
A seguinte documentação fornece orientações sobre a utilização de vários domínios de nível superior e subdomínios utilizados quando a Federação com o Office 365 ou em domínios do Azure AD.

## <a name="multiple-top-level-domain-support"></a>Suporte para vários domínios de nível superior
Federação de vários, domínios de nível superior com o Azure AD requer configuração adicional que não é necessária quando a federar com um domínio de nível superior.

Quando um domínio está Federado com o Azure AD, várias propriedades estão definidas no cabeçalho do domínio no Azure.  Um importante é IssuerUri.  Esta propriedade é um URI que é utilizado pelo Azure AD para identificar o domínio que o token está associado.  O URI não tem de resolver para qualquer caráter, mas tem de ser um URI válido.  Por predefinição, do Azure AD define o URI para o valor do identificador do serviço de Federação no seu no local do AD FS configuration.

> [!NOTE]
> O identificador do serviço de Federação é um URI que identifica exclusivamente um serviço de Federação.  O serviço de Federação é uma instância do AD FS que funções como o serviço de token de segurança.
>
>

Pode ver o IssuerUri utilizando o comando do PowerShell `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Um problema acontecer ao adicionar mais de um domínio de nível superior.  Por exemplo, vamos supor que tiver configurado a Federação entre o Azure AD e o seu ambiente no local.  Para este documento, o domínio, bmcontoso.com está a ser utilizado.  Agora um nível superior, segundo domínio, bmfabrikam.com foi adicionado.

![Domínios](./media/active-directory-multiple-domains/domains.png)

Quando tenta converter o domínio bmfabrikam.com ser federado, ocorre um erro.  O motivo é, o Azure AD dispõe de uma restrição que não permite a propriedade IssuerUri ter o mesmo valor para mais de um domínio.  

![Erro de Federação](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Parâmetro SupportMultipleDomain
Para contornar esta restrição, terá de adicionar um IssuerUri diferentes, o que pode ser efetuado utilizando o `-SupportMultipleDomain` parâmetro.  Este parâmetro é utilizado com os seguintes cmdlets:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Este parâmetro permite do Azure AD para configurar o IssuerUri para que o se baseia no nome do domínio.  O IssuerUri irá ser exclusivo em diretórios no Azure AD.  Utilizando o parâmetro permite que o comando do PowerShell concluir com êxito.

![Erro de Federação](./media/active-directory-multiple-domains/convert.png)

Analisar as definições para o domínio bmfabrikam.com pode ver o seguinte:

![Erro de Federação](./media/active-directory-multiple-domains/settings.png)

`-SupportMultipleDomain` Não altere os outros pontos finais, que ainda estão configurados para apontar para o serviço de Federação no adfs.bmcontoso.com.

Outra coisa que `-SupportMultipleDomain` does é que garante que o sistema do AD FS inclui o valor de emissor correto no tokens emitidos para o Azure AD. Este valor é definido por colocar a parte do domínio dos utilizadores UPN e defini-la como o domínio no IssuerUri, ou seja, https://{upn sufixo} / adfs/services/confiança.

Deste modo, durante a autenticação para o Azure AD ou o Office 365, o elemento de IssuerUri o token do utilizador é utilizado para localizar o domínio no Azure AD.  Se, não é possível encontrar uma correspondência, a autenticação irá falhar.

Por exemplo, se o UPN de um utilizador é bsimon@bmcontoso.com, o elemento de IssuerUri no token, problemas do AD FS, será definido para http://bmcontoso.com/adfs/services/trust. Este elemento corresponderá à configuração do Azure AD e autenticação terá êxito.

Segue-se a regra de afirmação personalizadas que implementa esta lógica:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> Para utilizar o parâmetro - SupportMultipleDomain durante a tentativa de adicionar novas ou converter domínios já existentes, a fidedignidade federada tem de já ter sido configurado para os suportar.
>
>

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Como atualizar a confiança entre o AD FS e o Azure AD
Se não foi possível configurar a confiança federada entre o AD FS e a instância do Azure AD, poderá ter de voltar a criar esta confiança.  O motivo é, quando originalmente é configurada sem o `-SupportMultipleDomain` parâmetro, a IssuerUri está definido com o valor predefinido.  Na captura de ecrã abaixo, pode ver o IssuerUri está definido como https://adfs.bmcontoso.com/adfs/services/trust.

Se tiver adicionado com êxito um novo domínio no portal do Azure AD e, em seguida, tentar convertê-lo utilizando `Convert-MsolDomaintoFederated -DomainName <your domain>`, obterá o erro seguinte.

![Erro de Federação](./media/active-directory-multiple-domains/trust1.png)

Se tentar adicionar a `-SupportMultipleDomain` mudar, receberá o seguinte erro:

![Erro de Federação](./media/active-directory-multiple-domains/trust2.png)

Simplesmente, tentar executar `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` no domínio original também resultará num erro.

![Erro de Federação](./media/active-directory-multiple-domains/trust3.png)

Utilize os passos abaixo para adicionar um domínio de nível superior adicional.  Se já adicionou um domínio e não utilizou o `-SupportMultipleDomain` parâmetro, começar com os passos para remover e atualizar o seu domínio original.  Se ainda não adicionou um domínio de nível superior, pode iniciar com os passos para adicionar um domínio através do PowerShell do Azure AD Connect.

Utilize os seguintes passos para remover a confiança do Microsoft Online e atualizar o seu domínio original.

1. No seu servidor de Federação do AD FS, abra **gestão do AD FS.**
2. À esquerda, expanda **relações de confiança** e **confianças da entidade Confiadora**
3. À direita, elimine o **plataforma de identidade do Microsoft Office 365** entrada.
   ![Remover o Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
4. Num computador que tenha [módulo Azure Active Directory para Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) instalado no mesmo executar o seguinte: `$cred=Get-Credential`.  
5. Introduza o nome de utilizador e palavra-passe de um administrador global para o domínio do Azure AD que são federar com.
6. No PowerShell, introduza: `Connect-MsolService -Credential $cred`
7. No PowerShell, introduza `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Esta atualização é para o domínio original.  Ao utilizar os domínios acima, seria:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

Utilize os seguintes passos para adicionar o novo domínio de nível superior com o PowerShell

1. Num computador que tenha [módulo Azure Active Directory para Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) instalado no mesmo executar o seguinte: `$cred=Get-Credential`.  
2. Introduza o nome de utilizador e palavra-passe de um administrador global para o domínio do Azure AD que são a Federação com o
3. No PowerShell, introduza: `Connect-MsolService -Credential $cred`
4. No PowerShell, introduza: `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Utilize os seguintes passos para adicionar o novo domínio de nível superior com o Azure AD Connect.

1. Inicie o Azure AD Connect no ambiente de trabalho ou iniciar de menu
2. Escolha "Adicionar um domínio do Azure adicionais do AD" ![adicionar outro domínio do Azure AD](./media/active-directory-multiple-domains/add1.png)
3. Introduza o seu Azure AD e credenciais do Active Directory
4. Selecione o domínio de segundo que pretende configurar para Federação.
   ![Adicionar outro domínio do Azure AD](./media/active-directory-multiple-domains/add2.png)
5. Clique em Instalar

### <a name="verify-the-new-top-level-domain"></a>Verifique se o novo domínio de nível superior
Utilizando o comando do PowerShell `Get-MsolDomainFederationSettings -DomainName <your domain>`pode ver o IssuerUri atualizado.  Captura de ecrã abaixo mostra a Federação definições foram atualizadas no domínio original http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

E IssuerUri no novo domínio foi definido como https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-subdomains"></a>Suporte para os subdomínios
Quando adiciona um subdomínio, devido a domínios processado de forma para o Azure AD, que irá herdar as definições do principal.  Por isso, IssuerUri, tem de corresponder de principais.

Permite Sim supor que, por exemplo, que tem bmcontoso.com e, em seguida, adicionar corp.bmcontoso.com.  IssuerUri para um utilizador de corp.bmcontoso.com terá de ser  **http://bmcontoso.com/adfs/services/trust.**  No entanto, a regra padrão implementada acima para o Azure AD, irá gerar um token com um emissor como  **http://corp.bmcontoso.com/adfs/services/trust.** que não irá corresponder do domínio necessária valor e a autenticação falhará.

### <a name="how-to-enable-support-for-subdomains"></a>Como ativar o suporte para os subdomínios
Para contornar este comportamento, AD FS para o Microsoft Online confiança de entidade confiadora tem de ser atualizado.  Para tal, tem de configurar uma regra de afirmação personalizada para que o separa desativar quaisquer subdomínios de sufixo UPN do utilizador ao construir o valor de emissor personalizado.

A afirmação seguinte irá efetuar este procedimento:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
O último número no conjunto de expressão regular é são quantos domínios de principal não existe no seu domínio de raiz. Aqui bmcontoso.com é utilizada, pelo que os dois domínios principal são necessários. Se os três principais domínios foram manter (ou seja,: corp.bmcontoso.com), em seguida, o número seria ter sido três. Eventualmente, um intervalo podem ser indicados, será sempre efetuada a correspondência para fazer corresponder o número máximo de domínios. "{2,3}" corresponderá a duas ou três domínios (ou seja,: bmfabrikam.com e corp.bmcontoso.com).

Utilize os seguintes passos para adicionar uma afirmação personalizada para suportar os subdomínios.

1. Gestão Open AD FS
2. Clique com o botão direito a confiança RP Online da Microsoft e escolha as regras de afirmação editar
3. Selecione a regra de afirmação terceira e substitua ![afirmação de edição](./media/active-directory-multiple-domains/sub1.png)
4. Substitua a afirmação atual:

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));

       with

        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Substituir afirmações](./media/active-directory-multiple-domains/sub2.png)

5. Clique em Ok.  Clique em aplicar.  Clique em Ok.  Feche a Gestão do AD FS.

## <a name="next-steps"></a>Passos Seguintes
Agora que já tem o Azure AD Connect instalado, pode [verificar a instalação e atribuir licenças](active-directory-aadconnect-whats-next.md).

Saiba mais acerca destas funcionalidades que foram ativadas com a instalação: [Atualização automática](active-directory-aadconnect-feature-automatic-upgrade.md), [Impedir eliminações acidentais](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) e [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Saiba mais acerca destes tópicos comuns: [agendador e como acionar a sincronização](active-directory-aadconnectsync-feature-scheduler.md).

Saiba mais sobre como [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md).