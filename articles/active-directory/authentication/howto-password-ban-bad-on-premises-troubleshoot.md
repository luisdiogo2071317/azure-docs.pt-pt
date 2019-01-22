---
title: Resolução de problemas e de registo na pré-visualização de proteção de palavra-passe do Azure AD
description: Compreender a proteção de palavra-passe do Azure AD pré-visualizar o registo e resolução de problemas comuns
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 008fba2fd6052ac9043de6ec217cb71d17f3ecce
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54427105"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Pré-visualização: O Azure AD monitorização de proteção de palavra-passe, relatórios e resolução de problemas

|     |
| --- |
| Proteção de palavra-passe do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Após a implementação de proteção de palavra-passe do Azure AD, monitorização e relatórios são tarefas essenciais. Este artigo apresenta detalhes para ajudar a que compreender onde cada serviço regista informações e como um relatório sobre a utilização de proteção de palavra-passe do Azure AD.

## <a name="on-premises-logs-and-events"></a>Os registos e eventos no local

### <a name="dc-agent-admin-log"></a>Registo de administração do agente DC

Em cada controlador de domínio, o software de serviço do agente DC grava os resultados da sua validações de palavra-passe (e outro Estado) para um registo de eventos local:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

Os eventos são registados pelos vários componentes do agente DC usando os seguintes intervalos:

|Componente |Intervalo de ID de evento|
| --- | --- |
|Dll de filtro de palavras-passe do agente de DC| 10000-19999|
|Processo de hospedagem do serviço de agente do controlador de domínio| 20000-29999|
|Lógica de validação de política de serviço de agente de DC| 30000-39999|

Para uma operação de validação da palavra-passe com êxito, geralmente não há um evento registado a partir da dll de filtro de palavra-passe do controlador de domínio agente. Para uma ativação operação de validação da palavra-passe, há geralmente dois eventos registados, um do serviço de agente do controlador de domínio e um a partir da dll de filtro de palavra-passe do agente de controlador de domínio.

Os eventos discretos para capturar essas situações são registados, com base em torno dos seguintes fatores:

* Se uma determinada palavra-passe está a ser definido ou foi alterada.
* Se a validação de uma determinada palavra-passe foi aprovada ou reprovada.
* Se a validação falhou devido aos vs de política global da Microsoft a política organizacional.
* Se modo só de auditoria está atualmente ativada ou desativada para a política de palavra-passe atual.

Os principais eventos relacionados à validação de palavra-passe são os seguintes:

|   |Alteração da palavra-passe |Conjunto de palavra-passe|
| --- | :---: | :---: |
|Passagem |10014 |10015|
|Falha (não passou cliente política de palavra-passe)| 10016, 30002| 10017, 30003|
|Falha (não passou na política de palavra-passe da Microsoft)| 10016, 30004| 10017, 30005|
|Só de auditoria Pass (seria ter falhado política de palavra-passe de cliente)| 10024, 30008| 10025, 30007|
|Só de auditoria Pass (seria ter falhado política de palavra-passe da Microsoft)| 10024, 30010| 10025, 30009|

> [!TIP]
> Palavras-passe de entrada são validadas em relação a lista de palavra-passe global da Microsoft pela primeira vez; Se isso falhar, sem processamento adicional é executada. Este é o mesmo comportamento, como, executadas em alterações de palavra-passe no Azure.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Mensagem de registo de eventos de exemplo para o ID de evento 10014 (conjunto de palavra-passe com êxito)

```
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Mensagem de registo de eventos de exemplo para o ID de evento 10017 e 30003 (conjunto de tentativas de senha)

10017:

```
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

#### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Mensagem de registo de eventos de exemplo para o ID de evento 30001 (palavra-passe aceite devido a nenhuma política disponível)

```
The password for the specified user was accepted because an Azure password policy is not available yet

UserName: SomeUser
FullName: Some User

This condition may be caused by one or more of the following reasons:%n

1. The forest has not yet been registered with Azure.

   Resolution steps: an administrator must register the forest using the Register-AzureADPasswordProtectionForest cmdlet.

2. An Azure AD password protection Proxy is not yet available on at least one machine in the current forest.

   Resolution steps: an administrator must install and register a proxy using the Register-AzureADPasswordProtectionProxy cmdlet.

3. This DC does not have network connectivity to any Azure AD password protection Proxy instances.

   Resolution steps: ensure network connectivity exists to at least one Azure AD password protection Proxy instance.

4. This DC does not have connectivity to other domain controllers in the domain.

   Resolution steps: ensure network connectivity exists to the domain.
```

#### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Mensagem de registo de eventos de exemplo para o ID de evento 30006 (nova política a ser imposta)

```
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

#### <a name="dc-agent-operational-log"></a>Registo operacional de agente do controlador de domínio

O serviço de agente do controlador de domínio também irá registar eventos operacionais relacionados com o registo seguinte:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

#### <a name="dc-agent-trace-log"></a>Registo de rastreio de agente do controlador de domínio

O serviço de agente do controlador de domínio também pode registar eventos de rastreio verboso do nível de depuração no registo seguinte:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

Registo de rastreio está desativado por predefinição.

> [!WARNING]
>  Quando ativada, o registo de rastreio recebe um grande volume de eventos e pode afetar o desempenho do controlador de domínio. Por conseguinte, este registo melhorado só deve ser ativado quando um problema requer uma investigação mais profunda e, em seguida, apenas para uma quantidade mínima de tempo.

#### <a name="dc-agent-text-logging"></a>Registo de texto do agente de DC

O serviço de agente do controlador de domínio pode ser configurado para escrever para um registo de texto ao definir o valor de registo seguinte:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters!EnableTextLogging = 1 (REG_DWORD value)

O registo de texto é desativado por predefinição. Reiniciar o serviço de agente do controlador de domínio é necessário para que as alterações a este valor para entrar em vigor. Quando o controlador de domínio de ativado o serviço de agente irá escrever num arquivo de log, localizado em:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> O registo de texto recebe as mesmo entradas de nível de depuração que podem ser registadas no registo de rastreio, mas geralmente é um formato mais fácil para rever e analisar.

> [!WARNING]
> Quando ativada, este registo recebe um grande volume de eventos e pode afetar o desempenho do controlador de domínio. Por conseguinte, este registo melhorado só deve ser ativado quando um problema requer uma investigação mais profunda e, em seguida, apenas para uma quantidade mínima de tempo.

### <a name="azure-ad-password-protection-proxy-service"></a>Serviço do proxy de proteção de palavra-passe do Azure AD

#### <a name="proxy-service-event-logs"></a>Registos de eventos do serviço de proxy

O serviço de Proxy emite um conjunto mínimo de eventos para os seguintes registos de eventos:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

O serviço de Proxy também pode registar eventos de rastreio verboso do nível de depuração no registo seguinte:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

Registo de rastreio está desativado por predefinição.

> [!WARNING]
> Quando ativada, o registo de rastreio recebe um grande volume de eventos e isto pode afetar o desempenho do anfitrião proxy. Por conseguinte, este registo deve ser ativado apenas quando um problema requer mais investigação e, em seguida, apenas para uma quantidade mínima de tempo.

#### <a name="proxy-service-text-logging"></a>Registo de texto do serviço de proxy

O serviço de Proxy pode ser configurado para escrever para um registo de texto ao definir o valor de registo seguinte:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters!EnableTextLogging = 1 (REG_DWORD value)

O registo de texto é desativado por predefinição. Reiniciar o serviço de Proxy é necessário para que as alterações a este valor para entrar em vigor. Quando ativar o Proxy de serviço irá escrever num arquivo de log, localizado em:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> O registo de texto recebe as mesmo entradas de nível de depuração que podem ser registadas no registo de rastreio, mas geralmente é um formato mais fácil para rever e analisar.

> [!WARNING]
> Quando ativada, este registo recebe um grande volume de eventos e pode afetar o desempenho do controlador de domínio. Por conseguinte, este registo melhorado só deve ser ativado quando um problema requer uma investigação mais profunda e, em seguida, apenas para uma quantidade mínima de tempo.

#### <a name="powershell-cmdlet-logging"></a>Registo de cmdlet do PowerShell

A maioria da proteção de palavra-passe do Azure AD Powershell cmdlets escreverá um registo de texto localizado em:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Se ocorrer um erro de cmdlet e a solução de e/ou direitos de causa não é imediatamente aparente, estes registos de texto podem também ser consultada com a maior.

### <a name="emergency-remediation"></a>Remediação de emergência

Caso de uma situação em que o serviço de agente do controlador de domínio está a causar problemas, o serviço de agente do controlador de domínio pode ser imediatamente desligado. A dll de filtro de palavras-passe de agente do DC ainda tenta chamar o serviço de não execução e registrará em log os eventos de aviso (10012, 10013), mas todas as senhas de entrada são aceites durante esse período. O serviço do agente DC, em seguida, também pode ser configurado através do Gestor de controlo de serviço de Windows com um tipo de arranque de "Disabled" conforme necessário.

### <a name="performance-monitoring"></a>Monitorização de desempenho

O software de serviço do agente DC instala um objeto de contador de desempenho chamado **proteção de palavra-passe do Azure AD**. Os seguintes contadores de desempenho estão atualmente disponíveis:

|Nome do contador de desempenho | Descrição|
| --- | --- |
|Palavras-passe processadas |Este contador mostra o número total de palavras-passe processados (aceites ou rejeitados) desde o último reinício.|
|Palavras-passe aceites |Este contador mostra o número total de palavras-passe que foram aceites desde o último reinício.|
|Palavras-passe rejeitadas |Este contador mostra o número total de palavras-passe que foram rejeitados desde o último reinício.|
|Pedidos de filtro de palavra-passe em curso |Este contador mostra o número de pedidos de filtro de palavras-passe atualmente em curso.|
|Pedidos de filtro de palavra-passe de pico |Este contador mostra o número máximo de pedidos de filtro de palavras-passe em simultâneo, desde o último reinício.|
|Erros de pedido de filtro de palavras-passe |Este contador mostra o número total de pedidos de filtro de palavra-passe falhados devido a um erro, desde o último reinício. Podem ocorrer erros quando o serviço de agente de proteção DC de palavra-passe do Azure AD não está em execução.|
|Filtro de palavras-passe pedidos/seg |Este contador mostra a taxa a que as palavras-passe estão a ser processadas.|
|Tempo de processamento de solicitação de filtro de palavras-passe |Este contador mostra o tempo médio necessário para processar um pedido de filtro de palavras-passe.|
|Tempo de processamento de solicitação de filtro de palavras-passe de pico |Este contador mostra o pedido de filtro de palavra-passe de pico de processamento tempo desde o último reinício.|
|As palavras-passe aceitaram devido ao modo de auditoria |Este contador mostra o número total de palavras-passe que faria normalmente foram rejeitadas, mas foram aceites uma vez que a política de palavra-passe tiver sido configurada para estar no modo de auditoria (desde o último reinício).|

## <a name="directory-services-repair-mode"></a>Modo de reparação de serviços de diretório

Se o controlador de domínio é iniciado no modo de reparação dos serviços de diretório, o serviço do agente DC Deteta esta fazendo com que todas as atividades de imposição para ser desativado, independentemente da configuração de diretiva atualmente ativo ou validação da palavra-passe.

## <a name="domain-controller-demotion"></a>Despromoção do controlador de domínio

É suportada para despromover um controlador de domínio que ainda está a executar o software do agente DC. Os administradores devem estar cientes entretanto que o software do agente DC mantém em execução e continua a impor a política de palavra-passe atual durante o procedimento de despromoção. A nova senha da conta administrador local (especificada como parte da operação de despromoção) é validada como qualquer outra palavra-passe. A Microsoft recomenda que as palavras-passe seguras ser escolhido para as contas de administrador locais como parte de um procedimento de despromoção do controlador de domínio; No entanto, a validação da nova senha de conta de administrador local, o software do agente DC pode ser perturbador para pré-existente procedimentos operacionais de despromoção.

Depois da despromoção foi concluída com êxito, e o controlador de domínio foi reinicializado e é executado novamente como um servidor membro normal, o software do agente DC é revertido para em execução num modo passivo. Em seguida, pode ser desinstalado em qualquer altura.

## <a name="removal"></a>Remoção

Se é decidido para desinstalar o software de pré-visualização pública e a limpeza de estado de todas as respetivas dos domínios e floresta, esta tarefa pode ser feita usando os seguintes passos:

> [!IMPORTANT]
> É importante executar estes passos por ordem. Se qualquer instância do serviço de Proxy é deixada em execução periodicamente novamente criará seu objeto de serviceConnectionPoint. Se qualquer instância do serviço de agente do controlador de domínio for deixada em execução periodicamente novamente criará seu objeto de serviceConnectionPoint e o estado de sysvol.

1. Desinstale a software de Proxy de proteção de palavra-passe de todas as máquinas. Este passo faz **não** requerem um reinício.
2. Desinstale o software do agente de controlador de domínio de todos os controladores de domínio. Este passo **requer** um reinício.
3. Remova manualmente todos os pontos de ligação de serviço de Proxy em cada contexto de nomenclatura de domínio. A localização desses objetos pode ser detetada com o seguinte comando do Powershell do Active Directory:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{ebefb703-6113-413d-9167-9f8dd4d24468}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   Não é omitir o asterisco ("*") no final o valor da variável $keywords.

   O objeto resultante (s) encontrada por meio da `Get-ADObject` comando, em seguida, pode ser enviado por pipe para `Remove-ADObject`, ou eliminada manualmente. 

4. Remova manualmente todos os pontos de ligação do agente de controlador de domínio em cada contexto de nomenclatura de domínio. Pode haver um desses objetos por controlador de domínio na floresta, dependendo de quão amplamente foi implementado o software de pré-visualização pública. A localização desse objeto pode ser detetada com o seguinte comando do Powershell do Active Directory:

   ```Powershell
   $scp = "serviceConnectionPoint"
   $keywords = "{2bac71e6-a293-4d5b-ba3b-50b995237946}*"
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -like $keywords }
   ```

   O objeto resultante (s) encontrada por meio da `Get-ADObject` comando, em seguida, pode ser enviado por pipe para `Remove-ADObject`, ou eliminada manualmente.

5. Remova manualmente o estado de configuração ao nível da floresta. O estado de configuração de floresta é mantido num contentor no contexto de nomenclatura de configuração do Active Directory. Pode ser detetado e eliminado da seguinte forma:

   ```Powershell
   $passwordProtectonConfigContainer = "CN=Azure AD Password Protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject -Recursive $passwordProtectonConfigContainer
   ```

6. Remova todos os sysvol relacionadas com o estado da manualmente eliminar manualmente a pasta seguinte e todos os respetivos conteúdos:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Se necessário, este caminho pode também ser acedido localmente num controlador de domínio de determinado; a localização predefinida deve ser algo como o seguinte caminho:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Este caminho é diferente se tiver configurada a partilha sysvol numa localização não padrão.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as listas globais e personalizadas banidas palavra-passe, consulte o artigo [banir palavras-passe incorretas](concept-password-ban-bad.md)
