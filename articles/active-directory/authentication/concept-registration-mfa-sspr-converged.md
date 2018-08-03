---
title: Registo convergido para SSPR do Azure AD e MFA (pré-visualização pública)
description: Authenticaiton de multi-factor do Azure AD e a palavra-passe self-service de reposição do registo (pré-visualização pública)
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: a0fec9743cd213d0d41d9143a2b1e9e1c3db9967
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447966"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication-public-preview"></a>Convergida registo para reposição de palavra-passe self-service e o Azure multi-factor Authentication (pré-visualização pública)

Até agora, os utilizadores tinham de se registrar métodos de autenticação para o Azure multi-factor Authentication (MFA) e a reposição de palavra-passe self-service (SSPR) em dois portais de diferentes. Número de utilizadores que foram confusa pelo fato de que métodos semelhantes foram utilizados para o MFA do Azure e o SSPR e não se registraram em ambos os portais. Este disparidade levou a alguns utilizadores a ser não é possível utilizar o MFA do Azure ou o SSPR quando necessário, que leva a uma chamada de suporte técnico e, potencialmente, um utilizador aborrecido. Agora, os utilizadores podem registar uma vez e obter os benefícios da MFA do Azure e SSPR, eliminando a necessidade de registrar seus métodos de autenticação para estas funcionalidades duas vezes.  

Antes de ativar esta nova experiência para a sua organização, recomendamos que reveja este artigo, bem como nossos [documentação do utilizador final](https://aka.ms/securityinfoguide) para compreender o impacto que a experiência terão os seus utilizadores. Pode utilizar o [documentação do utilizador final](https://aka.ms/securityinfoguide) treinar e preparar os seus utilizadores para a nova experiência e certifique-se de uma implementação com êxito.

|     |
| --- |
| Convergida registo para reposição de palavra-passe self-service e o Azure multi-factor Authentication é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Para permitir aos utilizadores para se registar métodos de autenticação Azure multi-factor Authentication e senhas de auto-atendimento repor numa experiência única, conclua os seguintes passos:

1. Inicie sessão no portal do Azure como um administrador global ou administrador de utilizadores.
2. Navegue até **do Azure Active Directory**, **definições do utilizador**, **gerir as definições para as funcionalidades de pré-visualização do painel de acesso**.
3. Sob **os utilizadores podem utilizar funcionalidades de pré-visualização para registar e gerir informações de segurança**, pode optar por ativar para um **Selected** grupo de utilizadores ou para **todos os** utilizadores.

Os utilizadores agora podem acessar [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) para registar os seus métodos de autenticação para o MFA e o SSPR. Para saber mais sobre o que os utilizadores irão ver com esta nova experiência, consulte nosso [documentação do utilizador final](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Depois de ativar esta experiência, os utilizadores que registar ou confirmar o número de telefone ou aplicação móvel através da nova experiência terão a capacidade para utilizá-los para a MFA e o SSPR, se esses métodos estão ativados nas políticas do MFA e o SSPR. Se, em seguida, desativa essa experiência, os utilizadores que navegue para a página de registo SSPR anterior em aka.ms/ssprsetup terão de executar a MFA antes de poder aceder a página.  

## <a name="how-it-works"></a>Como funciona

Se um utilizador registou anteriormente métodos de autenticação através de experiências de registo MFA e o SSPR separados, não é necessário registrar novamente essas informações. No entanto, se as definições de exigem que os utilizadores para se registar para MFA ou SSPR, poderá ver uma linha de comandos para rever as informações de segurança ao iniciar sessão.

Se um usuário tiver registrado um método que pode ser utilizado para a MFA, serão solicitados para executar a MFA antes de poder aceder a nova experiência.

Se tiver aplicado registo para MFA ou SSPR e um utilizador ainda não se registou, ser-lhe pedido para se registar no início de sessão.

Os utilizadores que recebem um pedido para registar-se ao iniciar sessão irão ver a experiência do seguinte:

![Registo convergido. Configurar métodos como um novo utilizador](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Esta experiência só será apresentada quando um utilizador deve registar ao iniciar sessão. Os utilizadores que vão diretamente para a nova experiência em aka.ms/setupsecurityinfo irão ver uma versão diferente da experiência, que está descrita neste artigo.

Os métodos de autenticação mostrados mudar com base nos métodos ativados nas políticas de MFA ou SSPR. O usuário será solicitado para registar o número mínimo de métodos de autenticação necessário para estar em conformidade com a política da MFA, política SSPR ou ambos. Se houver flexibilidade nos quais métodos de autenticação do utilizador pode registar, pode selecionar **escolher as informações de segurança** escolher outros métodos de autenticação.  

Ao contrário da experiência de registo anterior do MFA, os utilizadores não serão solicitados para registar uma palavra-passe de aplicação quando falar sobre a nova experiência de registo. Em vez disso, eles devem seguir os passos apresentados no nosso tutorial de palavras-passe de aplicações para registar as palavras-passe de aplicação na nova experiência.  

Assim que um utilizador concluir o registo, o método de MFA de predefinição irá definir automaticamente. Se o utilizador registou uma aplicação de autenticação, o método predefinido será definido para a aplicação. Se o utilizador não se uma aplicação de autenticação e registrados apenas o número de telefone, o método predefinido será definido para chamada telefónica. Os utilizadores podem alterar seu padrão ao aceder [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) e selecionar **alterar padrão**.  

Se o registo não é imposto, os utilizadores podem gerir o seus próprios métodos de autenticação [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo). Se um utilizador registou anteriormente um método que pode ser utilizado para executar a MFA, serão solicitados para executar a MFA antes de poder aceder a página.  

![Registo convergido. Editar métodos como usuário registrado](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

Nesta página, os utilizadores verão métodos de autenticação registrado anteriormente e métodos de autenticação registrados para os mesmos, como o telefone do escritório. Os utilizadores também podem adicionar, editar ou eliminar os respetivos métodos de autenticação (excluindo o telefone do escritório).  

Registos de auditoria para esta nova experiência existem na categoria de métodos de autenticação de registo de auditoria.  

## <a name="known-issues"></a>Problemas conhecidos

**Método MFA de predefinição está definido para chamada telefónica quando o utilizador se regista telefone usando a mensagem de texto**
   * Alguns usuários poderão reparar que o seu método MFA de predefinição está definido para chamada telefónica, depois de registar o respetivo número de telefone com a mensagem de texto. Os utilizadores podem resolver este problema ao alterar o método predefinido ao seguir estas instruções. 

**Não é possível aceder a nova experiência de registo depois do administrador desative o método predefinido do utilizador**
   * Alguns usuários podem não conseguir aceder a nova experiência de registo, se o seu padrão registrado anteriormente método MFA foi desabilitado pelo seu administrador. Eis um cenário de exemplo: 
      1. Anteriormente, o utilizador registado o respetivo número de telefone e definir o método padrão para chamada telefónica.
      2. Administrador desative chamada telefónica como um método de MFA para o inquilino.
      3. Utilizador deve registar durante o início de sessão por terem de se registar um método adicional para cumprir o política da SSPR do inquilino.
      4. Utilizador tenta registar, mas não é possível aceder à página devido a não ter um método padrão definido e fica preso num loop.

## <a name="next-steps"></a>Passos Seguintes

[Saiba como implementar o Azure de reposição de palavras-passe self-service de AD](howto-sspr-deployment.md)

[Saiba como exigir a autenticação multifator ao iniciar sessão](howto-mfa-getstarted.md)

[Documentação de configuração de método de autenticação de utilizador final](https://aka.ms/securityinfoguide)