---
title: Resolução de problemas na pré-visualização de proteção de palavra-passe do Azure AD
description: Compreender o Azure AD palavra-passe proteção pré-visualização comum de resolução de problemas
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 7474027368949d5ad2202881ac68096fac2b8bd2
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55693909"
---
# <a name="preview-azure-ad-password-protection-troubleshooting"></a>Pré-visualização: Resolução de problemas de proteção de palavra-passe do AD do Azure

|     |
| --- |
| Proteção de palavra-passe do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Após a implementação de proteção de palavra-passe do Azure AD, a resolução de problemas pode ser necessária. Este artigo apresenta detalhes para ajudar a compreender alguns passos de resolução de problemas comuns.

## <a name="weak-passwords-are-not-getting-rejected-as-expected"></a>Não são introdução rejeitadas senhas fracas, conforme o esperado

Isto pode ter várias causas possíveis:

1. Os agentes do DC não tem transferido ainda uma política. O sintoma é 30001 eventos no registo de eventos de administração do agente de controlador de domínio.

    As causas possíveis para este problema incluem:

    1. Floresta ainda não está registada
    2. Proxy ainda não está registado
    3. Problemas de conectividade de rede estão a impedir o serviço de Proxy de comunicação com o Azure (requisitos de Proxy HTTP de verificação)

2. O modo de imposição de política de palavra-passe ainda está definido para auditoria. Se for este o caso, reconfigurá-la para impor com o portal de proteção de palavra-passe do Azure AD. Veja [proteção por senha ativar](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

3. A política de palavra-passe foi desativada. Se for este o caso, reconfigurá-la ativada com o portal de proteção de palavra-passe do Azure AD. Veja [proteção por senha ativar](howto-password-ban-bad-on-premises-operations.md#enable-password-protection).

4. O algoritmo de validação da palavra-passe pode funcionar como esperado. Veja [como a palavras-passe são avaliadas](concept-password-ban-bad.md#how-are-passwords-evaluated).

## <a name="directory-services-repair-mode"></a>Modo de reparação de serviços de diretório

Se o controlador de domínio é iniciado no modo de reparação dos serviços de diretório, o serviço do agente DC detecta isso e fará com que todas as atividades de imposição para ser desativado, independentemente da configuração de diretiva atualmente ativo ou validação da palavra-passe.

## <a name="emergency-remediation"></a>Remediação de emergência

Caso de uma situação em que o serviço de agente do controlador de domínio está a causar problemas, o serviço de agente do controlador de domínio pode ser imediatamente desligado. A dll de filtro de palavras-passe de agente do DC ainda tenta chamar o serviço de não execução e registrará em log os eventos de aviso (10012, 10013), mas todas as senhas de entrada são aceites durante esse período. O serviço do agente DC, em seguida, também pode ser configurado através do Gestor de controlo de serviço de Windows com um tipo de arranque de "Disabled" conforme necessário.

Outra medida de remediação seria definir o modo de ativação para não no portal de proteção de palavra-passe do Azure AD. Assim que tiver sido baixada a política atualizada, cada serviço de agente do controlador de domínio irão entrar num modo inativo em que todas as senhas são aceites como-é. Para obter mais informações, consulte [modo imposição](howto-password-ban-bad-on-premises-operations.md#enforce-mode).

## <a name="domain-controller-demotion"></a>Despromoção do controlador de domínio

É suportada para despromover um controlador de domínio que ainda está a executar o software do agente DC. Os administradores devem estar cientes entretanto que o software do agente DC continua a impor a política de palavra-passe atual durante o procedimento de despromoção. A nova senha da conta administrador local (especificada como parte da operação de despromoção) é validada como qualquer outra palavra-passe. A Microsoft recomenda que as palavras-passe seguras ser escolhido para as contas de administrador locais como parte de um procedimento de despromoção do controlador de domínio; No entanto, a validação da nova senha de conta de administrador local, o software do agente DC pode ser perturbador para pré-existente procedimentos operacionais de despromoção.

Depois da despromoção foi concluída com êxito, e o controlador de domínio foi reinicializado e é executado novamente como um servidor membro normal, o software do agente DC é revertido para em execução num modo passivo. Em seguida, pode ser desinstalado em qualquer altura.

## <a name="removal"></a>Remoção

Se é decidido para desinstalar o software de pré-visualização pública e a limpeza de estado de todas as respetivas dos domínios e floresta, esta tarefa pode ser feita usando os seguintes passos:

> [!IMPORTANT]
> É importante executar estes passos por ordem. Se qualquer instância do serviço de Proxy é deixada em execução periodicamente novamente criará seu objeto de serviceConnectionPoint. Se qualquer instância do serviço de agente do controlador de domínio for deixada em execução periodicamente novamente criará seu objeto de serviceConnectionPoint e o estado de sysvol.

1. Desinstale o software de Proxy de todas as máquinas. Este passo faz **não** requerem um reinício.
2. Desinstale o software do agente de controlador de domínio de todos os controladores de domínio. Este passo **requer** um reinício.
3. Remova manualmente todos os pontos de ligação de serviço de Proxy em cada contexto de nomenclatura de domínio. A localização desses objetos pode ser detetada com o seguinte comando do PowerShell do Active Directory:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Não é omitir o asterisco ("*") no final o valor da variável $keywords.

   O objeto resultante (s) encontrada por meio da `Get-ADObject` comando, em seguida, pode ser enviado por pipe para `Remove-ADObject`, ou eliminada manualmente. 

4. Remova manualmente todos os pontos de ligação do agente de controlador de domínio em cada contexto de nomenclatura de domínio. Pode haver um desses objetos por controlador de domínio na floresta, dependendo de quão amplamente foi implementado o software de pré-visualização pública. A localização desse objeto pode ser detetada com o seguinte comando do PowerShell do Active Directory:

   ```PowerShell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   O objeto resultante (s) encontrada por meio da `Get-ADObject` comando, em seguida, pode ser enviado por pipe para `Remove-ADObject`, ou eliminada manualmente.

   Não é omitir o asterisco ("*") no final o valor da variável $keywords.

5. Remova manualmente o estado de configuração ao nível da floresta. O estado de configuração de floresta é mantido num contentor no contexto de nomenclatura de configuração do Active Directory. Pode ser detetado e eliminado da seguinte forma:

   ```PowerShell
   $passwordProtectionConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectionConfigContainer
   ```

6. Remova todos os sysvol relacionadas com o estado da manualmente eliminar manualmente a pasta seguinte e todos os respetivos conteúdos:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   Se necessário, este caminho pode também ser acedido localmente num controlador de domínio de determinado; a localização predefinida deve ser algo como o seguinte caminho:

   `%windir%\sysvol\domain\Policies\AzureADPasswordProtection`

   Este caminho é diferente se tiver configurada a partilha sysvol numa localização não padrão.

## <a name="next-steps"></a>Passos Seguintes

[Perguntas mais frequentes sobre proteção de palavra-passe do Azure AD](howto-password-ban-bad-on-premises-faq.md)

Para obter mais informações sobre as listas globais e personalizadas banidas palavra-passe, consulte o artigo [banir palavras-passe incorretas](concept-password-ban-bad.md)
