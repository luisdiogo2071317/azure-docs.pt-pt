---
title: Regras de associação de grupo dinâmico com base em atributos no Azure Active Directory | Documentos da Microsoft
description: Como criar regras avançadas para associação ao grupo dinâmico, incluindo suporte operadores de regra de expressão e parâmetros.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 07/05/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a48dcff6eedc2aa6e8bb6cd5b0668af72259493b
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869096"
---
# <a name="create-attribute-based-rules-for-dynamic-group-membership-in-azure-active-directory"></a>Criar regras baseadas em atributos para associação a grupos dinâmicos no Azure Active Directory
No Azure Active Directory (Azure AD), pode criar regras personalizadas para ativar complexas baseadas em atributos filiação dinâmica para grupos. Este artigo detalha os atributos e a sintaxe para criar regras de associação dinâmica para utilizadores ou dispositivos. Pode configurar uma regra de filiação dinâmica em grupos de segurança ou grupos do Office 365.

Quando alterar a todos os atributos de um utilizador ou dispositivo, o sistema avalia todas as regras de grupos dinâmicos num diretório para ver se a alteração dispararia qualquer grupo adiciona ou remove. Se um utilizador ou dispositivo cumprir uma regra num grupo, eles são adicionados como um membro desse grupo. Se deixar de cumprir a regra, serão removidos.

> [!NOTE]
> Esta funcionalidade necessita de uma licença do Azure AD Premium P1 para cada utilizador exclusivo que é um membro de um ou mais grupos dinâmicos. Não é necessário atribuir licenças aos utilizadores para os mesmos para serem membros de grupos dinâmicos, mas tem de ter o número mínimo de licenças no inquilino para cobrir todos esses usuários. Por exemplo, se tivesse um total de 1000 utilizadores exclusivos em todos os grupos dinâmicos no seu inquilino, seria necessário pelo menos de 1000 licenças para o Azure AD Premium P1 cumprir o requisito de licença.
>
> Pode criar um grupo dinâmico para os dispositivos ou para os utilizadores, mas não é possível criar uma regra que contém utilizadores e dispositivos.
> 
> No momento, não é possível criar um grupo de dispositivos com base em atributos do utilizador proprietário. Regras de associação de dispositivo só podem referenciar imediatos atributos de objetos de dispositivo no diretório.

## <a name="to-create-an-advanced-rule"></a>Para criar uma regra avançada
1. Inicie sessão para o [Centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que seja um administrador global ou um administrador de conta de utilizador.
2. Selecione **utilizadores e grupos**.
3. Selecione **todos os grupos**e selecione **novo grupo**.

   ![Adicionar novo grupo](./media/groups-dynamic-membership/new-group-creation.png)

4. Sobre o **grupo** painel, introduza um nome e descrição para o novo grupo. Selecione um **tipo de associação** deles **utilizador dinâmico** ou **dispositivos dinâmicos**, dependendo se pretende criar uma regra para utilizadores ou dispositivos e, em seguida, selecione **Adicionar consulta dinâmica**. Pode usar o construtor de regra para criar uma regra simples ou escrever uma regra avançada por conta própria. Este artigo contém mais informações sobre os atributos de utilizador e dispositivo disponíveis, bem como exemplos de regras avançadas.

   ![Adicionar regra de associação dinâmica](./media/groups-dynamic-membership/add-dynamic-group-rule.png)

5. Depois de criar a regra, selecione **adicionar consulta** na parte inferior do painel.
6. Selecione **Create** sobre o **grupo** painel para criar o grupo.

> [!TIP]
> Falha ao criar o grupo se a regra que introduziu foi formado incorretamente ou não é válido. É apresentada uma notificação no canto superior direito do portal, que contém uma explicação de por que não foi possível processar a regra. Lê-lo cuidadosamente para compreender como precisa de ajustar a regra para que seja válido.

## <a name="status-of-the-dynamic-rule"></a>Estado da regra dinâmica

Pode ver a associação ao processamento de estado e a data da última atualização na página de descrição geral do seu grupo dinâmico.
  
  ![visor de estado de grupo dinâmico](./media/groups-dynamic-membership/group-status.png)


As seguintes mensagens de estado podem ser exibidas para **processamento de associação** Estado:
* **Avaliar**: foi recebida a alteração do grupo e as atualizações estão a ser avaliadas.
* **Processamento**: atualizações estão a ser processadas.
* **Atualização concluída**: processamento foi concluído e todas as atualizações aplicáveis foram feitas.
* **Erro de processamento**: foi encontrado um erro ao avaliar a regra de associação e não foi possível concluir o processamento.
* **Atualizações em pausa**: regra de associação dinâmica atualizações foram interrompidas pelo administrador. MembershipRuleProcessingState está definido como "Em pausa".

As seguintes mensagens de estado podem ser exibidas para **associação atualizado pela última vez** Estado:
* &lt;**Data e hora**&gt;: A última vez que a associação foi atualizada.
* **Em curso**: atualizações estão atualmente em curso.
* **Desconhecido**: não é possível obter a hora da última atualização. Ele pode dever-se o grupo que está a ser criado de novo.

Se ocorrer um erro ao processar a regra de associação para um grupo específico, um alerta é mostrado na parte superior dos **página de descrição geral** para o grupo. Se não pendente de associação de grupo dinâmica atualizações podem ser processadas para todos os grupos no inquilino para obter mais, em seguida, 24 horas, um alerta é mostrado na parte superior do **todos os grupos**.

![mensagem de erro de processamento](./media/groups-dynamic-membership/processing-error.png)

## <a name="constructing-the-body-of-an-advanced-rule"></a>Construir o corpo de uma regra avançada
A regra avançada que pode criar para a filiação dinâmica para grupos é, essencialmente, uma expressão binária que consiste em três partes e resulta num resultado true ou false. As três partes são:

* Parâmetro à esquerda
* Operador binário
* Constante certo

Uma regra avançada completa é semelhante a este: (leftParameter binaryOperator "RightConstant"), em que a abertura e o parêntesis de fecho são opcionais para toda a expressão binária, aspas duplas são opcionais, apenas o necessário para a direita constante quando ele é a cadeia de caracteres e a sintaxe para o parâmetro à esquerda é user.property. Uma regra avançada pode consistir em mais de um binárias expressões separadas por- e, - ou e - operadores lógicos não.

Seguem-se exemplos de uma regra avançada construído corretamente:
```
(user.department -eq "Sales") -or (user.department -eq "Marketing")
(user.department -eq "Sales") -and -not (user.jobTitle -contains "SDE")
```
Para obter a lista completa de parâmetros suportados e operadores de regra de expressão, consulte as secções abaixo. Para os atributos usados para regras de dispositivo, consulte [utilizar atributos para criar regras para objetos de dispositivo](#using-attributes-to-create-rules-for-device-objects).

O comprimento total do corpo da sua regra avançada não pode exceder 2048 carateres.

> [!NOTE]
> As operações de cadeia de caracteres e regex não estão maiúsculas de minúsculas. Também pode executar verificações de nulo, usando *nulo* como uma constante, por exemplo, user.department - eq *nulo*.
> Cadeias de caracteres que contêm aspas "deve ter escapado utilizando" caráter, por exemplo, user.department - eq \`"Vendas".

## <a name="supported-expression-rule-operators"></a>Operadores de regra de expressão suportados
A tabela seguinte apresenta uma lista de todos os operadores de regra de expressão suportados e sua sintaxe a ser utilizado no corpo da regra avançada:

| Operador | Sintaxe |
| --- | --- |
| Não é igual a |-ne |
| Igual a |-eq |
| Não começa com |-notStartsWith |
| Começa com |-startsWith |
| Não contém |-notContains |
| Contains |-contém |
| Não corresponde |-notMatch |
| Correspondência |-corresponder |
| Em | -no |
| Não em | -notIn |

## <a name="operator-precedence"></a>Precedência de operador

Todos os operadores estão listados abaixo por precedência de inferior para superior. Operadores na mesma linha são em igual prioridade:
````
-any -all
-or
-and
-not
-eq -ne -startsWith -notStartsWith -contains -notContains -match –notMatch -in -notIn
````
Todos os operadores podem ser utilizados com ou sem o prefixo de hífen. Parênteses são necessárias apenas quando a precedência não cumpre os requisitos.
Por exemplo:
```
   user.department –eq "Marketing" –and user.country –eq "US"
```
é equivalente a:
```
   (user.department –eq "Marketing") –and (user.country –eq "US")
```
## <a name="using-the--in-and--notin-operators"></a>Usando a opção - no e - notIn operadores

Se deseja comparar o valor de um atributo de utilizador em relação a um número de valores diferentes pode utilizar a opção - na ou - notIn operadores. Eis um exemplo usando a opção - no operador:
```
   user.department -In ["50001","50002","50003",“50005”,“50006”,“50007”,“50008”,“50016”,“50020”,“50024”,“50038”,“50039”,“51100”]
```
Observe o uso do "[" e "]" no início e no final da lista de valores. Esta condição for avaliada como True do valor do user.department igual a um dos valores na lista.


## <a name="query-error-remediation"></a>Correção de erro de consulta
A tabela seguinte lista erros comuns e como corrigi-las

| Erro de análise de consulta | Utilização de erro | Utilização corrigida |
| --- | --- | --- |
| Erro: O atributo não suportado. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/><br/>Certifique-se de que o atributo for a [suportada a lista de propriedades](#supported-properties). |
| Erro: O operador não é suportado no atributo. |(user.accountEnabled-contém o verdadeiro) |(user.accountEnabled - eq verdadeiro)<br/><br/>O operador usado não é suportado para o tipo de propriedade (neste exemplo,-contém não pode ser utilizado no tipo Booleano). Utilize os operadores corretos para o tipo de propriedade. |
| Erro: Erro de compilação de consulta. |1. (user.department -eq "Sales") (user.department -eq "Marketing")<br/><br/>2. (user.userPrincipalName-corresponder a "*@domain.ext") |1. Operador em falta. Utilize o - e ou - ou dois associar predicados<br/><br/>(user.department -eq "Sales") -or (user.department -eq "Marketing")<br/><br/>2. Erro de corresponder a expressão regular utilizada com -<br/><br/>(user.userPrincipalName-corresponde ao ". *@domain.ext"), em alternativa: (user.userPrincipalName-correspondência "\@domain.ext$")|

## <a name="supported-properties"></a>Propriedades suportadas
Seguem-se todas as propriedades de utilizador que pode utilizar na sua regra avançada:

### <a name="properties-of-type-boolean"></a>Propriedades de tipo Booleano
Operadores permitidos

* -eq
* -ne

| Propriedades | Valores permitidos | Utilização |
| --- | --- | --- |
| accountEnabled |Verdadeiro Falso |user.accountEnabled - eq true |
| dirSyncEnabled |Verdadeiro Falso |user.dirSyncEnabled - eq true |

### <a name="properties-of-type-string"></a>Propriedades do tipo string
Operadores permitidos

* -eq
* -ne
* -notStartsWith
* -StartsWith
* -contém
* -notContains
* -corresponder
* -notMatch
* -no
* -notIn

| Propriedades | Valores permitidos | Utilização |
| --- | --- | --- |
| city |Qualquer valor de cadeia ou *nulo* |(user.city - eq "value") |
| país |Qualquer valor de cadeia ou *nulo* |(User. Country - eq "value") |
| companyName | Qualquer valor de cadeia ou *nulo* | (user.companyName -eq "value") |
| departamento |Qualquer valor de cadeia ou *nulo* |(user.department - eq "value") |
| displayName |Qualquer valor de cadeia |(user.displayName - eq "value") |
| employeeId |Qualquer valor de cadeia |(user.employeeId - eq "value")<br>(user.employeeId - ne *nulo*) |
| facsimileTelephoneNumber |Qualquer valor de cadeia ou *nulo* |(user.facsimileTelephoneNumber - eq "value") |
| givenName |Qualquer valor de cadeia ou *nulo* |(user.givenName - eq "value") |
| jobTitle |Qualquer valor de cadeia ou *nulo* |(user.jobTitle - eq "value") |
| mail |Qualquer valor de cadeia ou *nulo* (endereço SMTP do utilizador) |(user.mail - eq "value") |
| mailNickName |Qualquer valor de cadeia de caracteres (alias de correio do utilizador) |(user.mailNickName -eq "value") |
| móvel |Qualquer valor de cadeia ou *nulo* |(user.mobile - eq "value") |
| objectId |GUID do objeto de utilizador |(user.objectId - eq "11111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | No local o identificador de segurança (SID) para os utilizadores que foram sincronizados no local para a cloud. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
| passwordPolicies |Nenhum DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies -eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Qualquer valor de cadeia ou *nulo* |(user.physicalDeliveryOfficeName -eq "value") |
| postalCode |Qualquer valor de cadeia ou *nulo* |(user.postalCode -eq "value") |
| preferredLanguage |Código do ISO 639-1 |(user.preferredLanguage - eq "en-US") |
| sipProxyAddress |Qualquer valor de cadeia ou *nulo* |(user.sipProxyAddress -eq "value") |
| state |Qualquer valor de cadeia ou *nulo* |(user.state - eq "value") |
| streetAddress |Qualquer valor de cadeia ou *nulo* |(user.streetAddress - eq "value") |
| Apelido |Qualquer valor de cadeia ou *nulo* |(user.surname - eq "value") |
| telephoneNumber |Qualquer valor de cadeia ou *nulo* |(user.telephoneNumber - eq "value") |
| usageLocation |Indicativo de país de letras dois |(user.usageLocation -eq "US") |
| userPrincipalName |Qualquer valor de cadeia |(user.userPrincipalName -eq "alias@domain") |
| userType |o convidado de membro *nulo* |(user.userType - eq "Membro") |

### <a name="properties-of-type-string-collection"></a>Propriedades de coleção de cadeia de caracteres de tipo
Operadores permitidos

* -contém
* -notContains

| Propriedades | Valores permitidos | Utilização |
| --- | --- | --- |
| otherMails |Qualquer valor de cadeia |(user.otherMails-contém "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp: alias@domain |(user.proxyAddresses-contém "SMTP: alias@domain") |

## <a name="multi-value-properties"></a>Propriedades de valores múltiplos
Operadores permitidos

* -qualquer (satisfeitos quando, pelo menos, um item na coleção corresponde a condição)
* -todos (satisfeitos quando a condição correspondem a todos os itens na coleção)

| Propriedades | Valores | Utilização |
| --- | --- | --- |
| assignedPlans |Cada objeto da coleção expõe as seguintes propriedades de cadeia de caracteres: capabilityStatus, serviço, servicePlanId |user.assignedPlans-qualquer (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- e assignedPlan.capabilityStatus - eq "Ativada") |

Com múltiplos valores propriedades são coleções de objetos do mesmo tipo. Pode utilizar - qualquer - todos os operadores e para aplicar uma condição a um ou todos os itens da coleção, respectivamente. Por exemplo:

o assignedPlans é uma propriedade de valor múltiplos que apresenta uma lista de todos os planos de serviço atribuídos ao utilizador. A abaixo da expressão irá selecionar os utilizadores que têm o plano do serviço Exchange Online (plano 2), que também está no Estado ativado:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(O identificador de Guid identifica o plano do serviço Exchange Online (plano 2).)

> [!NOTE]
> Isto é útil se quiser identificar todos os utilizadores aos quais um Office 365 (ou outro serviço Online da Microsoft) capacidade tiver sido ativada, por exemplo, para direcioná-las com um determinado conjunto de políticas.

A seguinte expressão irá selecionar todos os utilizadores com qualquer plano de serviço que está associado com o serviço do Intune (identificado pelo nome de serviço "SCO"):
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Utilização de valores nulos

Para especificar um valor nulo numa regra, pode utilizar o *nulo* valor. Tenha cuidado para não use aspas ao redor do mundo *nulo* -se o fizer, será interpretado como um valor de cadeia literal. -Não operador não pode ser utilizado como um operador comparativa para nulo. Se usá-la, obterá um erro se usar nulo ou $null. Em alternativa, utilize - eq ou - ne. A maneira correta para o valor nulo de referência é o seguinte:
```
   user.mail –ne $null
```

## <a name="extension-attributes-and-custom-attributes"></a>Atributos de extensão e os atributos personalizados
Atributos de extensão e os atributos personalizados são suportados nas regras de associação de grupo dinâmica.

Atributos de extensão são sincronizados a partir de locais de Windows Server AD e tirar o formato de "ExtensionAttributeX", onde X é igual a 1 a 15.
Seria um exemplo de uma regra que utiliza um atributo de extensão
```
(user.extensionAttribute15 -eq "Marketing")
```
Atributos personalizados são sincronizados a partir do Windows Server do AD no local ou a partir de um aplicativo SaaS conectado e o formato de "user.extension_[GUID]\__ [atributo]", em que [GUID] é o identificador exclusivo no AAD para a aplicação que criou o atributo no AAD e [atributo] é o nome do atributo à medida que ele foi criado.
É um exemplo de uma regra que utiliza um atributo personalizado
```
user.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  
```
O nome de atributo personalizada pode ser encontrado no diretório ao consultar um utilizador do atributo com o Explorador do gráfico e procurar o nome de atributo.

## <a name="direct-reports-rule"></a>Regra de "Direct Reports"
Pode criar um grupo que contém todos os relatórios diretos de um gerente. Quando direto os o Gestor relatórios são alterados no futuro, associação a grupo será ajustada automaticamente.

> [!NOTE]
> 1. Para a regra funcionar, certifique-se de que o **ID de gestor** propriedade está definida corretamente, os utilizadores no seu inquilino. Pode verificar o valor atual de um utilizador no respetivo **guia perfil**.
> 2. Esta regra só suporta **direto** relatórios. Atualmente não é possível criar um grupo para uma hierarquia aninhada; Por exemplo, um grupo que inclui relatórios diretos e os relatórios.
> 3. Esta regra não pode ser combinada com quaisquer outras regras avançadas.

**Para configurar o grupo**

1. Siga os passos 1 a 5 da secção [para criar a regra avançada](#to-create-the-advanced-rule)e selecione um **tipo de associação** de **utilizador dinâmico**.
2. Sobre o **regras de associação dinâmica** painel, introduza a regra com a seguinte sintaxe:

    *Relatórios diretos para "{objectID_of_manager}"*

    Um exemplo de uma regra de válida:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. Depois de guardar a regra, todos os utilizadores com o valor de ID de gestor especificado serão adicionados ao grupo.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Utilizar atributos para criar regras para objetos de dispositivo
Também pode criar uma regra de que seleciona objetos de dispositivo para a associação num grupo. Os seguintes atributos do dispositivo podem ser utilizados.

 Atributo do dispositivo  | Valores | Exemplo
 ----- | ----- | ----------------
 accountEnabled | Verdadeiro Falso | (device.accountEnabled - eq verdadeiro)
 displayName | qualquer valor de cadeia |(device.displayName -eq "Rob Iphone”)
 deviceOSType | qualquer valor de cadeia | (device.deviceOSType -eq "iPad") -or (device.deviceOSType -eq "iPhone")
 deviceOSVersion | qualquer valor de cadeia | (device.OSVersion -eq "9.1")
 deviceCategory | um nome de categoria de dispositivo válido | (device.deviceCategory -eq "BYOD")
 deviceManufacturer | qualquer valor de cadeia | (device.deviceManufacturer - eq "Samsung")
 deviceModel | qualquer valor de cadeia | (device.deviceModel -eq "iPad Air")
 deviceOwnership | Pessoal, empresa, desconhecido | (device.deviceOwnership - eq "Empresa")
 domainName | qualquer valor de cadeia | (device.domainName -eq "contoso.com")
 enrollmentProfileName | Nome do perfil de perfil de inscrição de dispositivos da Apple ou do Windows Autopilot | (device.enrollmentProfileName -eq "DEP iPhones")
 isRooted | Verdadeiro Falso | (device.isRooted - eq verdadeiro)
 managementType | MDM (para dispositivos móveis)<br>PC (para computadores geridos pelo agente de PC do Intune) | (device.managementType -eq "MDM")
 organizationalUnit | qualquer valor de cadeia que corresponda ao nome da unidade organizacional definido por um Active Directory no local | (device.organizationalUnit -eq "US PCs")
 deviceId | um ID de dispositivo do Azure AD | (device.deviceId -eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d")
 objectId | ID de objeto de um válido do Azure AD |  (device.objectId -eq 76ad43c9-32c5-45e8-a272-7b58b58f596d")



## <a name="changing-dynamic-membership-to-static-and-vice-versa"></a>A alteração de associação dinâmica para estático e vice-versa
É possível alterar a forma como a associação é gerida num grupo. Isto é útil quando pretender manter o mesmo nome do grupo e o ID no sistema, para que todas as referências existentes para o grupo ainda são válidas; criar um novo grupo exigiria a atualizar essas referências.

Atualizámos o Centro de administração do Azure AD para adicionar suporte a essa funcionalidade. Agora, os clientes podem converter grupos existentes de associação de grupo dinâmica atribuída de associação e vice-versa por meio do Centro de administração do Azure AD ou cmdlets do PowerShell, conforme mostrado abaixo.

> [!WARNING]
> Quando a alteração um grupo estático existente para um grupo dinâmico, todos os membros existentes serão removidos do grupo e, em seguida, será processada a regra de associação para adicionar novos membros. Se o grupo é utilizado para controlar o acesso a aplicações ou recursos, os membros originais podem perder o acesso até que a regra de associação é totalmente processada.
>
> Recomendamos que teste a nova regra de associação com antecedência para se certificar de que a nova associação no grupo é conforme esperado.

### <a name="using-azure-ad-admin-center-to-change-membership-management-on-a-group"></a>Usando o Centro de administração do Azure AD para o gerenciamento de associação num grupo de alterações 

1. Inicie sessão para o [Centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que seja um administrador global ou um administrador de conta de utilizador no seu inquilino.
2. Selecione **grupos**.
3. Partir do **todos os grupos** lista, abra o grupo que pretende alterar.
4. Selecione **propriedades**.
5. Sobre o **propriedades** página para o grupo, selecione um **tipo de associação** de atribuído (estático), dinâmica de utilizador ou dispositivo dinâmico, consoante o tipo de associação pretendido. Para a associação dinâmica, pode usar o construtor de regra para selecionar as opções para uma regra simples ou escreve uma regra avançada. 

Os seguintes passos são um exemplo da alteração de um grupo de estático para a filiação dinâmica para um grupo de utilizadores. 

1. No **propriedades** página para o seu grupo selecionado, selecione um **tipo de associação** de **utilizador dinâmico**, em seguida, selecione Sim na caixa de diálogo que explica as alterações ao grupo associação para continuar. 
  
   ![Selecione o tipo de associação do utilizador dinâmico](./media/groups-dynamic-membership/select-group-to-convert.png)
  
2. Selecione **adicionar consulta dinâmica**e, em seguida, forneça a regra.
  
   ![Introduza a regra](./media/groups-dynamic-membership/enter-rule.png)
  
3. Depois de criar a regra, selecione **adicionar consulta** na parte inferior da página.
4. Selecione **salvar** sobre o **propriedades** página para o grupo guardar as alterações. O **tipo de associação** do grupo é atualizada imediatamente na lista do grupo.

> [!TIP]
> Grupo conversão poderá falhar se a regra avançada que introduziu estava incorreta. É apresentada uma notificação no canto superior direito do portal que contém uma explicação de por que a regra não pode ser aceitos pelo sistema. Lê-lo cuidadosamente para compreender a forma como pode ajustar a regra para que seja válido.

### <a name="using-powershell-to-change-membership-management-on-a-group"></a>Utilizar o PowerShell para o gerenciamento de associação num grupo de alterações

> [!NOTE]
> Para alterar as propriedades de grupo dinâmico, terá de utilizar os cmdlets do **a versão de pré-visualização** [Azure AD PowerShell versão 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0). Pode instalar a pré-visualização dos [galeria do PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).

Eis um exemplo das funções que mudar o gerenciamento da associação de um grupo existente. Neste exemplo, é tomar cuidado para manipular a propriedade GroupTypes e preservar os valores que não estão relacionadas com associação de grupo dinâmica corretamente.

```
#The moniker for dynamic groups as used in the GroupTypes property of a group object
$dynamicGroupTypeString = "DynamicMembership"

function ConvertDynamicGroupToStatic
{
    Param([string]$groupId)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -eq $null -or !$groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a static group. Aborting conversion.";
    }


    #remove the type for dynamic groups, but keep the other type values
    $groupTypes.Remove($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to remove the dynamic type, ii) pause execution of the current rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "Paused"
}

function ConvertStaticGroupToDynamic
{
    Param([string]$groupId, [string]$dynamicMembershipRule)

    #existing group types
    [System.Collections.ArrayList]$groupTypes = (Get-AzureAdMsGroup -Id $groupId).GroupTypes

    if($groupTypes -ne $null -and $groupTypes.Contains($dynamicGroupTypeString))
    {
        throw "This group is already a dynamic group. Aborting conversion.";
    }
    #add the dynamic group type to existing types
    $groupTypes.Add($dynamicGroupTypeString)

    #modify the group properties to make it a static group: i) change GroupTypes to add the dynamic type, ii) start execution of the rule, iii) set the rule
    Set-AzureAdMsGroup -Id $groupId -GroupTypes $groupTypes.ToArray() -MembershipRuleProcessingState "On" -MembershipRule $dynamicMembershipRule
}
```
Para tornar um grupo estático:
```
ConvertDynamicGroupToStatic "a58913b2-eee4-44f9-beb2-e381c375058f"
```
Para tornar um grupo dinâmico:
```
ConvertStaticGroupToDynamic "a58913b2-eee4-44f9-beb2-e381c375058f" "user.displayName -startsWith ""Peter"""
```
## <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais sobre os grupos no Azure Active Directory.

* [Ver grupos existentes](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Criar um novo grupo e adicionar membros](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Gerir definições de um grupo](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Gerir membros de um grupo](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas para os utilizadores num grupo](groups-dynamic-membership.md)
