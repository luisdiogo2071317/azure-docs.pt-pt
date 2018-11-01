---
title: Operações do pré-visualização de proteção de palavra-passe do Azure AD e relatórios
description: Operações do Azure AD palavra-passe proteção pré-visualização pós-implementação e relatórios
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 6a61fdeaf1a751ab4001257335abdcbd6fac9cbf
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739469"
---
# <a name="preview-azure-ad-password-protection-operational-procedures"></a>Pré-visualização: Procedimentos de proteção de palavra-passe do Azure AD operacionais

|     |
| --- |
| Proteção de palavra-passe do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Depois de concluir o [instalação de proteção de palavra-passe do Azure AD](howto-password-ban-bad-on-premises-deploy.md) no local, existem alguns itens que devem ser configurados no portal do Azure.

## <a name="configure-the-custom-banned-password-list"></a>Configurar a lista de palavras-passe banidas personalizado

Siga as instruções no artigo [configurar a lista de palavras-passe banidas personalizado](howto-password-ban-bad-configure.md) para obter passos para personalizar a lista de palavra-passe banidas para a sua organização.

## <a name="enable-password-protection"></a>Ativar a proteção de palavra-passe

1. Entrar para o [portal do Azure](https://portal.azure.com) e procure **Azure Active Directory**, **métodos de autenticação**, em seguida, **proteção de palavra-passe (pré-visualização)**.
1. Definir **ativar a proteção de palavra-passe no Windows Server Active Directory** para **Sim**
1. Conforme mencionado na [guia de implementação](howto-password-ban-bad-on-premises-deploy.md#deployment-strategy), é recomendado para definir inicialmente o **modo** para **auditoria**
   * Quando estiver familiarizado com a funcionalidade, pode mudar a **modo** para **imposto**
1. Clicar em **Guardar**

![Ativar a componentes de proteção de palavra-passe do Azure AD no portal do Azure](./media/howto-password-ban-bad-on-premises-operations/authentication-methods-password-protection-on-prem.png)

## <a name="audit-mode"></a>Modo de Auditoria

Modo de auditoria destina-se como uma forma de executar o software num modo "what if". Cada serviço do agente DC avalia uma senha de entrada, de acordo com a política atualmente ativa. Se a política atual estiver configurada para estar no modo de auditoria, as palavras-passe "bad" resultam em mensagens de registo de eventos, mas são aceites. Esta é a única diferença entre o modo de auditoria e impor; todas as outras operações, execute o mesmo.

> [!NOTE]
> A Microsoft recomenda que a implantação inicial e teste sempre começam no modo de auditoria. Eventos no registo de eventos, em seguida, devem ser monitorizados para tentar prever se todos os processos operacionais existentes iriam ser seja afetados quando o modo de imposição está ativado.

## <a name="enforce-mode"></a>Modo de imposição

Impor o modo destina-se como configuração final. Como no modo de auditoria acima, cada serviço do agente DC avalia entradas palavras-passe, de acordo com a política atualmente ativa. Se o modo de imposição está ativado no entanto, uma palavra-passe que é considerada não segura, de acordo com a política será rejeitada.

Quando uma palavra-passe é rejeitada no modo imposição, a proteção de palavra-passe do Azure AD agente DC, o impacto visível visto por um usuário final é idêntico ao que seria vêem se a palavra-passe foi rejeitada pela imposição de complexidade de palavra-passe tradicionais no local. Por exemplo, um utilizador poderá ver a seguinte mensagem de erro tradicional no ecrã de palavra-passe do Windows logon\change:

`Unable to update the password. The value provided for the new password does not meet the length, complexity, or history requirements of the domain.`

Esta mensagem é apenas um exemplo de vários resultados possíveis. A mensagem de erro específicas pode variar consoante o cenário que está a tentar definir uma palavra-passe não segura ou software real.

Os utilizadores finais afetados poderá ter de trabalhar com sua equipe de TI para compreender os requisitos novos e mais pode escolher as palavras-passe seguras.

## <a name="enable-mode"></a>Ativar o modo

Esta definição normalmente deve ser deixada no estado predefinido ativado (Sim). Configurar esta definição para desativado (não) fará com que todas implementada do Azure AD palavra-passe agentes de proteção DC entrar num modo inativo em que todas as senhas são aceites como-é, e não existem atividades de validação será executada nenhuma forma (por exemplo, nem mesmo os eventos de auditoria será emitido).

## <a name="usage-reporting"></a>Relatórios de utilização

O `Get-AzureADPasswordProtectionSummaryReport` cmdlet pode ser utilizado para produzir uma vista de resumo da atividade. Um exemplo de saída deste cmdlet é o seguinte:

```
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

O âmbito do relatório do cmdlet pode ser influenciado através de um dos parâmetros de – – DomainController, - domínio ou floresta. Não especificar um parâmetro implica – floresta.

> [!NOTE]
> Este cmdlet funciona ao abrir uma sessão do Powershell para cada controlador de domínio. Por ordem para o sucesso, o suporte de sessão remota do Powershell tem de estar ativado em cada controlador de domínio e o cliente tem de ter privilégios suficientes. Para obter mais informações sobre requisitos de sessão remota do Powershell, execute "Get-Help about_Remote_Troubleshooting" numa janela do Powershell.

> [!NOTE]
> Este cmdlet funciona ao consultar o registo de eventos do administrador do serviço de agente cada controlador de domínio remotamente. Se os registos de eventos contêm um grande número de eventos, o cmdlet poderá demorar muito tempo a concluir. Além disso, consultas de rede em massa de grandes conjuntos de dados podem afetar o desempenho do controlador de domínio. Por conseguinte, este cmdlet deve ser usado com cuidado em ambientes de produção.

## <a name="next-steps"></a>Passos Seguintes

[Resolução de problemas e monitoramento de proteção de palavra-passe do Azure AD](howto-password-ban-bad-on-premises-troubleshoot.md)
