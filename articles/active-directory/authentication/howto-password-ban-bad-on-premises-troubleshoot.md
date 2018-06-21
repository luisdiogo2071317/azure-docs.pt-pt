---
title: Resolução de problemas e de registo na pré-visualização de proteção de palavra-passe do Azure AD
description: Compreender a proteção de palavra-passe do Azure AD registo e a resolução de problemas comuns de pré-visualização
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: e5b3dc1bfa7c7890be83529e863907ec056f188f
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295660"
---
# <a name="preview-azure-ad-password-protection-monitoring-reporting-and-troubleshooting"></a>Pré-visualização: Do Azure AD monitorização de proteção de palavra-passe, relatórios e resolução de problemas

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banned personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para mais informações sobre pré-visualizações, consulte [suplementares os termos de utilização para a pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Após a implementação da proteção de palavra-passe do Azure AD monitorização e relatórios são tarefas essenciais. Este artigo entra em detalhe para ajudar a que compreender onde cada serviço regista as informações e como um relatório sobre a utilização da proteção de palavra-passe do Azure AD.

## <a name="on-premises-logs-and-events"></a>No local e os registos de eventos

### <a name="dc-agent-service"></a>Serviço de agente de DC

Em cada controlador de domínio, o software de serviço do agente DC escreve os resultados da respetivas validações de palavra-passe (e outros Estados) para um registo de eventos local: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin

Os eventos são registados pelos vários componentes do agente DC utilizando os seguintes intervalos de:

|Componente |Intervalo de ID de evento|
| --- | --- |
|Dll de filtro de palavras-passe do agente DC| 10000 19999|
|Processo de alojamento do serviço de agente do DC| 20000 29999|
|Lógica de validação de política de serviço de agente de DC| 30000 39999|

Para uma operação de validação da palavra-passe bem sucedida, geralmente, há um evento registado a partir da dll de filtro de palavra-passe de agente de DC. Para uma falha operação de validação da palavra-passe, normalmente, existem dois eventos registados, uma do serviço de agente do DC e uma partir da dll de filtro de palavra-passe de agente de DC.

Os eventos discretos para capturar estas situações são registados, com base em torno dos seguintes fatores:

* Indica se uma palavra-passe fornecida está a ser definido ou foi alterado.
* Indica se a validação de uma palavra-passe fornecida transmitido ou falhou.
* Indica se validação falhou devido aos vs de política global da Microsoft a política organizacional.
* Indica se modo só de auditoria está atualmente ou desativar para a política de palavra-passe atual.

Os eventos relacionados com validação de palavra-passe de chaves são os seguintes:

|   |Alteração da palavra-passe |Conjunto de palavra-passe|
| --- | :---: | :---: |
|Passagem |10014 |10015|
|Falhar (não passou cliente política de palavra-passe)| 10016, 30002| 10017, 30003|
|Falhar (não passou na política de palavras-passe da Microsoft)| 10016, 30004| 10017, 30005|
|Passagem só de auditoria (seria falharam política de palavras-passe do cliente)| 10024, 30008| 10025, 30007|
|Passagem só de auditoria (seria falharam política de palavras-passe da Microsoft)| 10024, 30010| 10025, 30009|

> [!TIP]
> Palavras-passe de entrada são validadas face à lista de palavra-passe globais do Microsoft primeiro; Se não conseguir, sem processamento adicional é executado. Este é o mesmo comportamento como efetuada em alterações de palavra-passe no Azure.

#### <a name="sample-event-log-message-for-event-id-10014-successful-password-set"></a>Mensagens de registo de eventos de exemplo para conjunto de palavra-passe de êxito 10014 do ID de evento

A palavra-passe foi alterada para o utilizador especificado foi validada como compatível com a política de palavra-passe do Azure atual.

 Nome de utilizador: BPL_02885102771 FullName:

#### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Mensagens de registo de eventos de exemplo para o conjunto de palavra-passe do ID de evento 10017 e 30003 falhou

10017:

A repor palavra-passe para o utilizador especificado foi rejeitado porque não foi possível cumprir a política de palavra-passe do Azure atual. Consulte a mensagem do registo de eventos correlacionados para obter mais detalhes.

 Nome de utilizador: BPL_03283841185 FullName:

30003:

A repor palavra-passe para o utilizador especificado foi rejeitado porque corresponde à, pelo menos, um dos tokens presentes na lista de palavra-passe por inquilino banned da política de palavra-passe do Azure atual.

 Nome de utilizador: BPL_03283841185 FullName:

Algumas outras mensagens de chave de registo de eventos a ter em consideração são:

#### <a name="sample-event-log-message-for-event-id-30001"></a>Mensagens de registo de eventos de exemplo para o ID de evento 30001

A palavra-passe para o utilizador especificado foi aceite porque uma política de palavra-passe do Azure ainda não está disponível

Nome de utilizador: <user> FullName: <user>

Esta condição pode ser causada por um ou mais dos seguintes razões: % n

1. A floresta ainda não foi registada com o Azure.

   Passos de resolução: um administrador tem de registar a floresta utilizando o cmdlet Register-AzureADPasswordProtectionForest.

2. Uma proteção de palavra-passe do Azure AD Proxy ainda não está disponível no computador, pelo menos, um na floresta atual.

   Passos de resolução: um administrador tem de instalar e registar um proxy utilizando o cmdlet Register-AzureADPasswordProtectionProxy.

3. Este DC não tem conectividade de rede para quaisquer instâncias de Proxy de proteção de palavra-passe do Azure AD.

   Passos de resolução: Certifique-se de que existe conectividade de rede para, pelo menos, uma instância de Proxy do proteção palavra-passe do Azure AD.

4. Este DC não tem conectividade com outros controladores de domínio no domínio.

   Passos de resolução: Certifique-se de que existe conectividade de rede ao domínio.

#### <a name="sample-event-log-message-for-event-id-30006"></a>Mensagens de registo de eventos de exemplo para o ID de evento 30006

O serviço está agora impor a seguinte política de palavra-passe do Azure.

 AuditOnly: 1

 Data de política global: 2018-05-15T00:00:00.000000000Z

 Data de política de inquilino: 2018-06-10T20:15:24.432457600Z

 Impor a política de inquilino: 1

#### <a name="dc-agent-log-locations"></a>Localizações de registo do agente DC

O serviço do agente DC também registará o operacional eventos relacionados com o registo do seguinte: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational

O serviço do agente DC também pode registar eventos de rastreio verboso do nível de depuração no registo seguinte: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace

> [!WARNING]
> O registo de rastreio está desativado por predefinição. Quando ativado, este registo recebe um grande volume de eventos e pode afetar o desempenho do controlador de domínio. Por conseguinte, este registo melhorado deve ser ativado apenas quando um problema requerem a investigação mais aprofundada e, em seguida, apenas para uma quantidade mínima de tempo.

### <a name="azure-ad-password-protection-proxy-service"></a>Serviço do proxy de proteção de palavra-passe do Azure AD

A serviço de Proxy de proteção de palavra-passe emite um conjunto mínimo de eventos no registo de eventos seguintes: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational

A serviço de Proxy de proteção de palavra-passe também pode registar eventos de rastreio verboso do nível de depuração no registo seguinte: \Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace

> [!WARNING]
> O registo de rastreio está desativado por predefinição. Quando ativado, este registo recebe um grande volume de eventos e isto pode afetar o desempenho do anfitrião proxy. Por conseguinte, este registo deve ser ativado apenas quando um problema requerem a investigação mais aprofundada e, em seguida, apenas para uma quantidade mínima de tempo.

### <a name="dc-agent-discovery"></a>Deteção de agente de DC

O `Get-AzureADPasswordProtectionDCAgent` cmdlet pode ser utilizado para apresentar informações básicas sobre os vários agentes de DC em execução num domínio ou floresta. Estas informações são obtidas dos objetos de serviceConnectionPoint registados pelos serviços de agente de DC em execução. Segue-se um exemplo de resultado deste cmdlet:

```
PS C:\> Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
Heartbeat             : 2/16/2018 8:35:01 AM
```

Várias propriedades são atualizadas por cada serviço do agente DC numa base por hora aproximada. Os dados são ainda sujeitos a latência de replicação do Active Directory.

Âmbito da consulta o cmdlet poderá deve ser influenciado utilizando – domínio ou floresta – parâmetros.

### <a name="emergency-remediation"></a>Remediação de emergência

Caso uma acontecimento infeliz situação em que o serviço do agente DC está a causar problemas, o serviço do agente DC poderão ser imediatamente encerrado. A dll de filtro de palavra-passe do DC agente tenta chamar o serviço não em execução e irá registar eventos de aviso (10012, 10013), mas todas as palavras-passe de entrada são aceites durante esse período. O serviço do agente DC pode, em seguida, também ser configurado através do Gestor de controlo de serviço de Windows com um tipo de arranque de "Desativado" conforme necessário.

### <a name="performance-monitoring"></a>Monitorização de desempenho

O software de serviço do agente DC instala um objecto de contador de desempenho com o nome **proteção de palavra-passe do Azure AD**. Os seguintes contadores de desempenho estão atualmente disponíveis:

|Nome do contador de desempenho | Descrição|
| --- | --- |
|Palavras-passe processadas |Este contador apresenta o número total de palavras-passe processado (aceite ou rejeitado) desde o último reinício.|
|Palavras-passe aceite |Este contador apresenta o número total de palavras-passe que foram aceites desde o último reinício.|
|Palavras-passe rejeitadas |Este contador apresenta o número total de palavras-passe que foram rejeitados desde o último reinício.|
|Pedidos de filtro de palavras-passe em curso |Este contador apresenta o número de pedidos de filtro de palavras-passe atualmente em curso.|
|Filtrar pedidos de palavra-passe das horas de ponta |Este contador apresenta o número máximo de pedidos de filtro de palavras-passe simultâneas desde o último reinício.|
|Erros de pedido de filtro de palavras-passe |Este contador apresenta o número total de pedidos de filtro de palavras-passe que falhou devido a um erro desde o último reinício. Erros podem ocorrer quando o serviço de agente de proteção DC de palavra-passe do Azure AD não está em execução.|
|Filtro de palavras-passe pedidos/seg |Este contador apresenta a taxa a que as palavras-passe estão a ser processadas.|
|Tempo de processamento de pedidos de filtro de palavras-passe |Este contador apresenta o tempo médio necessário para processar um pedido de filtro de palavras-passe.|
|Tempo de processamento de pedido de filtro de palavras-passe de pico |Este contador apresenta o pedido de filtro de palavras-passe das horas de ponta processamento tempo desde o último reinício.|
|As palavras-passe aceitaram devido ao modo de auditoria |Este contador apresenta o número total de palavras-passe que seria normalmente foram rejeitados, mas foram aceites porque a política de palavra-passe foi configurada para estar no modo de auditoria (desde o último reinício).|

## <a name="directory-services-repair-mode"></a>Modo de reparação de serviços de diretório

Se o controlador de domínio é iniciado no modo de reparação dos serviços de diretório, o serviço do agente DC Deteta esta fazendo com que todos os validação da palavra-passe ou as atividades de imposição para ser desativado, independentemente da configuração da política atualmente ativos.

## <a name="domain-controller-demotion"></a>Despromoção do controlador de domínio

É suportada para despromover um controlador de domínio que ainda está a executar o software do agente DC. Os administradores devem ter conhecimento contudo que o software do agente DC mantém em execução e continua a impor a política de palavra-passe atual durante o procedimento de despromoção. O novo administrador conta palavra-passe local (especificado como parte da operação de despromoção) é validado como qualquer outra palavra-passe. A Microsoft recomenda que as palavras-passe seguras ser escolhidos para as contas de administrador locais como parte de um procedimento de despromoção do DC; No entanto, a validação do novo administrador conta palavra-passe local, o software do agente DC pode ser incómoda para pré-existente procedimentos operacionais de despromoção.
Depois de é concluída com êxito a despromoção e o controlador de domínio tiver sido reiniciado e a ser executados novamente como um servidor membro normal, o software do agente DC reverte para em execução num modo passivo. Em seguida, pode ser desinstalado em qualquer altura.

## <a name="removal"></a>Remoção

Se é decidido para desinstalar o software de pré-visualização pública e limpeza do Estado de todos os relacionados os domínios e florestas, esta tarefa pode ser conseguida através dos seguintes passos:

> [!IMPORTANT]
> É importante efetuar estes passos por ordem. Se for deixada qualquer instância da proteção de palavra-passe serviço Proxy executá-lo irá voltar a criar o objeto de serviceConnectionPoint periodicamente, bem como periodicamente voltar a criar o estado de sysvol.

1. Desinstale a software de Proxy de proteção de palavra-passe de todas as máquinas. Este passo **não** exigir um reinício.
2. Desinstale o software do agente DC todos os controladores de domínio. Este passo **requer** um reinício.
3. Remova manualmente todos os pontos de ligação de serviço de proxy em cada contexto de nomenclatura de domínio. A localização de um destes objetos pode ser detetada com o seguinte comando do Powershell do Active Directory:
   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{EBEFB703-6113-413D-9167-9F8DD4D24468}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -eq $keywords }
   ```

   Não omitir o asterisco ("*") no fim do valor da variável $keywords.

   O objeto resultante encontrado através de `Get-ADObject` comando, em seguida, pode ser direcionado para `Remove-ADObject`, ou eliminada manualmente. 

4. Remova manualmente todos os pontos de ligação do agente DC cada contexto de nomenclatura de domínio. Pode haver um destes objetos por controlador de domínio na floresta, dependendo de como amplamente o software de pré-visualização pública foi implementado. A localização desse objeto pode ser detetada com o seguinte comando do Powershell do Active Directory:

   ```
   $scp = “serviceConnectionPoint”
   $keywords = “{B11BB10A-3E7D-4D37-A4C3-51DE9D0F77C9}*”
   Get-ADObject -SearchScope Subtree -Filter { objectClass -eq $scp -and keywords -eq $keywords }
   ```

   O objeto resultante encontrado através de `Get-ADObject` comando, em seguida, pode ser direcionado para `Remove-ADObject`, ou eliminada manualmente.

5. Remova manualmente o estado de configuração ao nível da floresta. O estado de configuração de floresta é mantido num contentor no contexto de nomenclatura de configuração do Active Directory. Pode ser detetado e eliminado da seguinte forma:

   ```
   $passwordProtectonConfigContainer = "CN=Azure AD password protection,CN=Services," + (Get-ADRootDSE).configurationNamingContext
   Remove-ADObject $passwordProtectonConfigContainer
   ```

6. Remova todos os sysvol relacionadas com o estado da manualmente como eliminar manualmente a pasta seguinte e todo o conteúdo:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Se necessário, este caminho pode também ser acedido localmente num controlador de domínio indicado; a localização predefinida será semelhante ao seguinte caminho seguinte:

   `%windir%\sysvol\domain\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Este caminho é diferente se a partilha sysvol tiver sido configurada numa localização não predefinidas.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as listas de global e personalizadas banned palavra-passe, consulte o artigo [banir palavras-passe incorretas](concept-password-ban-bad.md)
