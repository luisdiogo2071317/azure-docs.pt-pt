---
title: Registo convergido para SSPR do Azure AD e MFA (pré-visualização pública)
description: Registo (pré-visualização pública) de reposição de AD multi-factor Authentication do Azure e a palavra-passe self-service
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66d4dc7e6fc1545e1de1e3b50887585c4156b4c0
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208792"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication-public-preview"></a>Convergida registo para reposição de palavra-passe self-service e o Azure multi-factor Authentication (pré-visualização pública)

Até agora, os utilizadores tinham de se registrar métodos de autenticação para o Azure multi-factor Authentication (MFA) e a reposição de palavra-passe self-service (SSPR) em dois portais de diferentes. Número de utilizadores que estavam confusos que métodos semelhantes foram utilizados para o MFA do Azure e o SSPR e não se registraram em ambos os portais. Alguns utilizadores, em seguida, é possível utilizar o MFA do Azure ou o SSPR quando necessário, à esquerda para chamadas de suporte técnico. 

Agora, os utilizadores podem registar uma vez e obter os benefícios da MFA do Azure e SSPR. Os utilizadores não precisam de registar os respetivos métodos de autenticação para estas funcionalidades duas vezes.  

Antes de ativar esta nova experiência para a sua organização, recomendamos que reveja este artigo e o [documentação do utilizador](https://aka.ms/securityinfoguide) para compreender o impacto que a experiência terão os seus utilizadores. Pode utilizar a documentação do utilizador para treinar e preparar os seus utilizadores para a nova experiência e ajudar a garantir uma implementação com êxito.

|     |
| --- |
| Convergida registo para reposição de palavra-passe self-service e o Azure multi-factor Authentication é uma funcionalidade de pré-visualização pública do Azure Active Directory (Azure AD). Para obter mais informações sobre pré-visualizações, veja [Termos de Utilização Suplementares do Microsoft Azure para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Para permitir aos utilizadores para se registar métodos de autenticação Azure multi-factor Authentication e senhas de auto-atendimento repor numa experiência única, conclua os seguintes passos:

1. Inicie sessão no portal do Azure como um administrador global ou administrador de utilizadores.
2. Navegue até **do Azure Active Directory** > **definições de utilizador** > **gerir as definições para as funcionalidades de pré-visualização do painel de acesso**.
3. Sob **os utilizadores podem utilizar funcionalidades de pré-visualização para registar e gerir informações de segurança**, pode optar por ativar para um **Selected** grupo de utilizadores ou para **todos os** utilizadores.

Os utilizadores agora podem acessar [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) para registar os seus métodos de autenticação para o MFA e o SSPR. Para saber mais sobre o que os utilizadores irão ver com esta nova experiência, consulte a [documentação do utilizador](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Depois de ativar esta experiência, os utilizadores que registar ou confirmar o seu número de telefone ou a aplicação móvel através da nova experiência podem utilizá-los para a MFA e o SSPR, se esses métodos estão ativados nas políticas do MFA e o SSPR. Se, em seguida, desativa essa experiência, os utilizadores que aceda à página de registo SSPR anterior em https:/aka.ms/ssprsetup serão necessário para efetuar a autenticação multifator para que poderem aceder a página.  

## <a name="how-it-works"></a>Como funciona

Se um utilizador registou anteriormente métodos de autenticação através de experiências de registo MFA e o SSPR separados, não precisam de se registar novamente essas informações. Mas, se as definições de exigem que os utilizadores para se registar para MFA ou SSPR, poderão ver uma linha de comandos para rever as informações de segurança ao iniciar sessão.

Se um usuário tiver registrado um método que pode ser utilizado para a MFA, são-lhe pedidos para efetuar a autenticação multifator para que poderem aceder a nova experiência.

Se tiver aplicado registo para MFA ou SSPR e um utilizador ainda não se registou,-lhes pedido que se registem quando iniciam sessão.

Os utilizadores que solicitado que registe durante a assinatura Verão a experiência do seguinte:

![Registo convergido. Configure métodos como um novo utilizador.](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Esta experiência é apresentada apenas quando um utilizador deve registar ao iniciar sessão. Os utilizadores que vão diretamente para a experiência da https://aka.ms/setupsecurityinfo ver uma versão diferente da experiência, que está descrita neste artigo.

A alteração de métodos de autenticação apresentado com base nos métodos ativados nas políticas de MFA e o SSPR. É pedido ao utilizador para registar o número mínimo de métodos de autenticação necessário para estar em conformidade com a política da MFA, política SSPR ou ambos. Se houver flexibilidade nos quais métodos de autenticação do utilizador pode registar, pode selecionar **escolher as informações de segurança** escolher outros métodos de autenticação.  

> [!NOTE]
> Se ativar a utilização de notificação de aplicação móvel e o código de aplicação móvel, os utilizadores que registar a aplicação Microsoft Authenticator por meio de uma notificação podem utilizar para verificar a respetiva identidade de notificação e código.

Ao contrário da experiência de registo MFA anterior, não é pedido aos utilizadores para se registar uma palavra-passe de aplicação quando falar sobre a nova experiência de registo. Em vez disso, eles devem seguir os passos apresentados no tutorial de palavras-passe de aplicações para registar as palavras-passe de aplicação na nova experiência.  

Depois de um usuário concluir o registo, o método de MFA de padrão é automaticamente definido. Se o utilizador registou uma aplicação de autenticação, o método predefinido está definido para a aplicação. Se o utilizador não se uma aplicação de autenticação e registrados apenas o número de telefone, o método predefinido está definido para chamada telefónica. Os utilizadores podem alterar seu padrão ao aceder https://aka.ms/setupsecurityinfo e selecionando **alterar padrão**.  

Se o registo não é imposto, os utilizadores podem gerir o seus próprios métodos de autenticação https://aka.ms/setupsecurityinfo. Se um utilizador registou anteriormente um método que pode ser utilizado para a MFA, é-lhes pedido para efetuar a autenticação multifator para que poderem aceder a página.  

![Registo convergido. Edite métodos como usuário registrado.](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

Nesta página, os utilizadores veem métodos de autenticação registrado anteriormente e métodos de autenticação registrados para os mesmos, como o telefone do escritório. Os utilizadores também podem adicionar, editar ou eliminar os respetivos métodos de autenticação (excluindo o telefone do escritório).  

Registos de auditoria para esta nova experiência existem na categoria de métodos de autenticação de registo de auditoria.  

## <a name="known-issues"></a>Problemas conhecidos

**Método MFA de predefinição está definido para chamada telefónica quando um usuário registra um telefone usando a mensagem de texto**

   * Alguns usuários podem observar que o seu método MFA de predefinição está definido como chamada telefónica, depois de registem o número de telefone com a mensagem de texto. Os utilizadores podem resolver este problema ao alterar o método predefinido ao seguir as instruções no artigo [gerir as suas informações de segurança (pré-visualização)](../user-help/security-info-manage-settings.md#change-your-info).

**Utilizador não é possível aceder a nova experiência de registo após um administrador desative o seu método padrão**

   * Alguns utilizadores poderão não conseguir aceder a nova experiência de registo, se o seu administrador desativou o seu método MFA de um padrão registrado anteriormente. Eis um cenário de exemplo:
      1. Anteriormente, o utilizador registado o respetivo número de telefone e definir o método padrão para chamada telefónica.
      2. Administrador desative chamada telefónica como um método de MFA para o inquilino.
      3. Utilizador deve registar durante o início de sessão por terem de se registar um método adicional para cumprir o política da SSPR do inquilino.
      4. Utilizador tenta registar, mas não é possível aceder à página e fica preso num loop porque não está definido um método padrão.

## <a name="next-steps"></a>Passos Seguintes

[Saiba como implementar o Azure de reposição de palavras-passe self-service de AD](howto-sspr-deployment.md)

[Saiba como exigir autenticação multifator para iniciar sessão](howto-mfa-getstarted.md)

[Saiba como configurar métodos de autenticação de utilizador](https://aka.ms/securityinfoguide)
