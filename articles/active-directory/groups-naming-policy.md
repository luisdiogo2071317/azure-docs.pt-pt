---
title: "Grupo de definições de política de nome de grupos do Office 365 no Azure Active Directory (pré-visualização) | Microsoft Docs"
description: "Como configurar a expiração de grupos do Office 365 no Azure Active Directory (pré-visualização)"
services: active-directory
documentationcenter: 
author: curtand
manager: michael.tillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 02/28/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: cc3ea7f81a924f3f4baa6fd2866c4e552b7c160e
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="enforce-a-naming-policy-for-office-365-groups-in-azure-active-directory-preview"></a>Impor uma política de atribuição de nomes de grupos do Office 365 no Azure Active Directory (pré-visualização)

Para impor as convenções de nomenclatura consistentes de grupos do Office 365 criados ou editados pelos seus utilizadores, configure um grupo de política de atribuição de nomes para os inquilinos no Azure Active Directory (Azure AD). Por exemplo, pode utilizar a política de nomenclatura para comunicar a função de um grupo, a associação, a região geográfica ou quem criou o grupo. Também pode utilizar a política de nomenclatura para o ajudar a categorizar os grupos no livro de endereços. Pode utilizar a política para bloquear palavras específicas de que está a ser utilizado em nomes de grupo e aliases.

> [!IMPORTANT]
> Utilizar a pré-visualização de política de atribuição de nomes do Office 365 grupos requer licenças do Azure Active Directory Premium P1 para cada utilizador exclusivo que é um membro de um ou mais grupos do Office 365.

É aplicada a política de nomenclatura para criação ou edição de grupos criados através de cargas de trabalho (por exemplo, Outlook, Teams da Microsoft, SharePoint, Exchange ou Planner). É aplicada para o nome de grupo e o alias de grupo. Se configurou a política de atribuição de nomes no Azure AD e tiver um grupo de Exchange existente nomenclatura política, o Azure AD nomenclatura política é aplicado.

## <a name="naming-policy-features"></a>Funcionalidades de política de atribuição de nomes
Pode impor a política de atribuição de nomes de grupos do Office 365 de duas formas diferentes:

-   **Política de nomenclatura de sufixo de prefixo** pode definir os prefixos ou sufixos, em seguida, são automaticamente adicionados à impor uma convenção de nomenclatura nos seus grupos (por exemplo, no nome do grupo "GRP\_Japão\_meu grupo\_ Equipa técnica", GRP\_Japão\_ é o prefixo e \_engenharia é o sufixo). 

-   **Personalizada bloqueado palavras** pode carregar um conjunto de bloqueados palavras específicas para a sua organização a ser bloqueadas no grupos criados por utilizadores (por exemplo, "CEO, pagamentos, RH").

### <a name="prefix-suffix-naming-policy"></a>Política de nomenclatura de sufixo de prefixo

A estrutura geral de convenção de nomenclatura é 'Sufixo de prefixo [GroupName]'. Enquanto pode definir vários prefixos e sufixos, só pode ter uma instância do [GroupName] na definição. Os prefixos ou sufixos podem ser cadeias fixas ou os atributos de utilizador, tais como \[departamento\] que são substituídas com base no utilizador que está a criar o grupo. O número total de carateres para as cadeias de prefixo e o sufixo combinadas permitido é de 53 carateres. 

Os prefixos e sufixos podem conter carateres especiais que são suportados no nome do grupo e alias de grupo. Os caracteres no prefixo ou sufixo que não são suportados no alias de grupo ainda são aplicados no nome do grupo, mas removidos o alias de grupo. Devido a esta restrição, os prefixos e sufixos aplicados para o nome do grupo podem ser diferentes das aplicadas ao alias de grupo. 

#### <a name="fixed-strings"></a>Cadeias de fixas

Pode utilizar cadeias para facilitar a análise e a distinguir grupos na lista de endereços global e nas ligações de navegação esquerdo de cargas de trabalho de grupo. Alguns dos prefixos comuns são palavras-chave, como ' Grp\_Name', '\#Name ','\_Name'

#### <a name="user-attributes"></a>Atributos do utilizador

Pode utilizar os atributos que podem ajudá-lo e os seus utilizadores identificam quais departamento, o office ou a região geográfica para o qual o grupo foi criado. Por exemplo, se definir a política de atribuição de nomes como `PrefixSuffixNamingRequirement = “GRP [GroupName] [Department]”`, e `User’s department = Engineering`, em seguida, um nome de grupo aplicadas poderá ser o "Grupo My GRP engenharia". Suportada do Azure AD atributos são \[departamento\], \[empresa\], \[Office\], \[StateOrProvince\], \[CountryOrRegion \], \[Título\]. Os atributos de utilizador não suportados são tratados como cadeias fixas; Por exemplo, "\[postalCode\]". Atributos de extensões e atributos personalizados não são suportados.

Recomendamos que utilize os atributos que têm valores preenchidos para todos os utilizadores na sua organização e não utilizem os atributos que têm valores de comprimento.

### <a name="custom-blocked-words"></a>Palavras bloqueadas personalizadas

Uma lista de bloqueadas word é uma lista separada por vírgulas das expressões até ser bloqueado nos nomes de grupo e aliases. Nenhum pesquisas subcadeia são executadas. É necessária uma correspondência exata entre o nome do grupo e um ou mais das palavras bloqueadas personalizadas para acionar uma falha. Não é efetuada a pesquisa de subcadeia para que os utilizadores podem utilizar palavras comuns, como 'Class', mesmo se 'lass' é uma palavra bloqueada.

Regras de lista de bloqueadas palavra:
- Palavras bloqueadas não são sensíveis a maiúsculas e minúsculas.
- Quando um utilizador introduzir uma palavra bloqueada como parte de um nome de grupo, verá uma mensagem de erro com a palavra bloqueada.
- Não existem sem restrições de caráter no palavras bloqueadas.
- Não há um limite superior de 5000 expressões que podem ser configuradas na lista de bloqueadas palavras. 

### <a name="administrator-override"></a>Substituição de administrador

Administradores selecionados podem ser excluídos estas políticas em todas as cargas de trabalho de grupo e os pontos finais, para que podem criar grupos com palavras bloqueadas e com as seus próprios convenções de nomenclatura. Seguem-se a lista de funções de administrador isentas do grupo de política de atribuição de nomes.

- Administrador global
- Suporte de camada 1 de parceiro
- Suporte de camada 2 parceiro
- Administrador de conta de utilizador
- Escritores de diretórios

## <a name="install-powershell-cmdlets-to-configure-a-naming-policy"></a>Instalar os cmdlets do PowerShell para configurar uma política de atribuição de nomes

Lembre-se de que desinstale qualquer versão anterior do Azure Active Directory PowerShell para o módulo de gráfico para o Windows PowerShell e instalar [Azure Active Directory PowerShell para gráfico: versão de pré-visualização pública 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) antes de executar o Comandos do PowerShell. 

1. Abra a aplicação do Windows PowerShell como administrador.
2. Desinstale qualquer versão anterior do AzureADPreview.
  
  ````
  Uninstall-Module AzureADPreview
  ````
3. Instale a versão mais recente do AzureADPreview.
  
  ````
  Install-Module AzureADPreview
  ````
Se lhe for pedido sobre aceder a um repositório não fidedigno, digite **Y**. Poderá demorar alguns minutos para que o módulo de novo instalar.

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-ad-powershell"></a>Configurar o grupo de política de nomenclatura para um inquilino com o Azure AD PowerShell

1. Abra uma janela do Windows PowerShell no seu computador. Pode abri-lo sem privilégios elevados.

2. Execute os comandos seguintes para se preparar para executar os cmdlets.
  
  ````
  Import-Module AzureADPreview
  Connect-AzureAD
  ````
  No **iniciar sessão na sua conta** ecrã que se abre, introduza a conta de administrador e a palavra-passe para estabelecer a ligação ao seu serviço e selecione **sessão**.

3. Siga os passos no [cmdlets do Azure Active Directory para configurar definições de grupo](active-directory-accessmanagement-groups-settings-cmdlets.md) para criar definições de grupo para este inquilino.

### <a name="view-the-current-settings"></a>Ver as definições atuais

1. Obter a política de nomenclatura atual para ver as definições atuais.
  
  ````
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ````
  
2. Apresenta as definições do grupo atual.
  
  ````
  $Setting.Values
  ````
  
### <a name="set-the-naming-policy-and-custom-blocked-words"></a>Definir a política de atribuição de nomes e palavras bloqueadas personalizadas

1. Defina os prefixos de nome de grupo e sufixos no Azure AD PowerShell.
  
  ````
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ````
  
2. Defina as palavras bloqueadas personalizadas que pretende restringir. O exemplo seguinte ilustra como adicionar as suas próprias palavras personalizadas.
  
  ````
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ````
  
3. Guarde as definições para a nova política ser eficaz, tal como no exemplo seguinte.
  
  ````
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ````
  
E já está. Tiver definido a política de atribuição de nomes e adicionado as palavras bloqueadas.

## <a name="export-or-import-the-list-of-custom-blocked-words"></a>Exportar ou importar a lista de palavras bloqueadas personalizadas

Para obter mais informações, consulte o artigo [cmdlets do Azure Active Directory para configurar definições de grupo](active-directory-accessmanagement-groups-settings-cmdlets.md).

Eis um exemplo de um script do PowerShell para exportar vários palavras bloqueadas:

````
$Words = (Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value CustomBlockedWordsList -EQ 
Add-Content "c:\work\currentblockedwordslist.txt" -Value $words.value.Split(",").Replace("`"","")  
````

Eis um exemplo de script do PowerShell para importar vários palavras bloqueadas:

````
$BadWords = Get-Content "C:\work\currentblockedwordslist.txt"
$BadWords = [string]::join(",", $BadWords)
$Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}
if ($Settings.Count -eq 0)
    {$Template = Get-AzureADDirectorySettingTemplate | Where-Object {$_.DisplayName -eq "Group.Unified"}
    $Settings = $Template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $Settings
    $Settings = Get-AzureADDirectorySetting | Where-Object {$_.DisplayName -eq "Group.Unified"}}
$Settings["CustomBlockedWordsList"] = $BadWords
$Settings["EnableMSStandardBlockedWords"] = $True
Set-AzureADDirectorySetting -Id $Settings.Id -DirectorySetting $Settings 
````

## <a name="naming-policy-experiences-across-office-365-apps"></a>Nomenclatura experiências de política em todas as aplicações do Office 365

Depois de definir uma política de atribuição de nomes de grupo no Azure AD, quando um utilizador cria um grupo numa aplicação do Office 365, consulte: 

* A pré-visualização do nome de acordo com a política de atribuição de nomes (com prefixos e sufixos) logo que os tipos de utilizador no nome do grupo
* Se o utilizador introduz bloqueadas palavras, irá veem uma mensagem de erro, de modo que podem remover as palavras bloqueadas.

Carga de trabalho | Conformidade
----------- | -------------------------------
Portais de Active Directory do Azure | Portal do Azure AD e portal do painel de acesso mostram o nome da política imposta nomenclatura quando o utilizador escrever um nome de grupo quando criar ou editar um grupo. Quando um utilizador introduz uma palavra bloqueada personalizada, é apresentada uma mensagem de erro com a palavra bloqueada para que o utilizador pode removê-la.
Outlook Web Access (OWA) | Outlook Web Access mostra a política de nomenclatura imposta nome quando o utilizador escrever um nome de grupo ou alias de grupo. Quando um utilizador introduz uma palavra bloqueada personalizada, uma mensagem de erro é apresentada na IU do juntamente com a palavra bloqueada para que o utilizador pode removê-la.
Ambiente de trabalho do Outlook | Grupos criados no ambiente de trabalho do Outlook são compatíveis com as definições de política de atribuição de nomes. Aplicação de ambiente de trabalho do Outlook ainda não mostra a pré-visualização do nome de grupo aplicada e não devolve os erros personalizados word bloqueado quando o utilizador introduz o nome do grupo. No entanto, a política de atribuição de nomes é aplicada automaticamente quando criar ou editar um grupo e os utilizadores veem mensagens de erro, se existirem personalizadas palavras bloqueadas no alias ou nome do grupo.
Microsoft Teams | Microsoft Teams mostra o grupo de nomenclatura do nome da política imposta quando o utilizador introduz um nome de agrupamento. Quando um utilizador introduz uma palavra bloqueada personalizada, uma mensagem de erro é apresentada juntamente com a palavra bloqueada para que o utilizador pode removê-la.
SharePoint  |  SharePoint mostra o nome da política imposta nomenclatura quando os tipos de utilizador um site de nome ou endereço de correio eletrónico de grupo. Quando um utilizador introduz uma palavra bloqueada personalizada, uma mensagem de erro é apresentada, juntamente com a palavra bloqueada para que o utilizador pode removê-la.
Microsoft Stream | Microsoft Stream mostra o grupo de nomenclatura do nome da política imposta quando o utilizador escrever um nome de grupo ou alias de correio eletrónico de grupo. Quando um utilizador introduz uma palavra bloqueada personalizada, uma mensagem de erro é apresentada com a palavra bloqueada para que o utilizador pode removê-la.
Outlook iOS e a aplicação Android | Grupos criados no Outlook aplicações são compatíveis com a política de atribuição de nomes configurada. Aplicação móvel do Outlook ainda não mostra a pré-visualização do nome da política imposta nomenclatura e não devolve erros personalizados word bloqueado quando o utilizador introduz o nome do grupo. No entanto, a política de atribuição de nomes é aplicada automaticamente ao clicar em criar/editar e os utilizadores veem mensagens de erro, se existirem personalizadas palavras bloqueadas no alias ou nome do grupo.
Grupos de aplicações móveis | Grupos criados na aplicação móvel grupos são compatíveis com a política de atribuição de nomes. Grupos de aplicações móveis não mostram a pré-visualização da política de atribuição de nomes e não devolvem erros personalizados word bloqueado quando o utilizador introduz o nome do grupo. Mas a política de atribuição de nomes é aplicada automaticamente quando criar ou editar um grupo e os utilizadores não é apresentada com erros adequados se existirem personalizadas palavras bloqueadas no alias ou nome do grupo.
Planner | Planner está em conformidade com a política de atribuição de nomes. Planner mostra a pré-visualização de política de atribuição de nomes ao introduzir o nome do plano. Quando um utilizador introduz uma palavra bloqueada personalizada, uma mensagem de erro é apresentada ao criar o plano.
Dynamics 365 para o Engagement de cliente | Dynamics 365 para o Engagement de cliente é compatível com a política de atribuição de nomes. Dynamics 365 mostra o nome da política imposta nomenclatura quando o utilizador escrever um nome de grupo ou alias de correio eletrónico de grupo. Quando o utilizador introduzir uma palavra bloqueada personalizada, uma mensagem de erro é apresentada com a palavra bloqueada para que o utilizador pode removê-la.
Sincronização de dados de profissional (SDS) | Grupos criados através de SDS cumprem a política de atribuição de nomes, mas a política de atribuição de nomes não é aplicada automaticamente. Os administradores de SDS tem de anexar os prefixos e sufixos a nomes de classe para o qual grupos têm de ser criada e, em seguida, carregada SDS. Criar grupo ou editar iria falhar caso contrário.
Gestor de cliente do Outlook (OCM) | Gestor de cliente do Outlook está em conformidade com a política de atribuição de nomes, que é aplicada automaticamente para o grupo criado no Gestor de cliente do Outlook. Se for detetada uma palavra bloqueada personalizada, criação de grupo no OCM está bloqueada e o utilizador está bloqueado de utilizar a aplicação OCM.
Sala de aulas aplicação | Grupos criados na sala de aulas aplicação está em conformidade com a política de atribuição de nomes, mas não é aplicada automaticamente a política de atribuição de nomes e a pré-visualização de política de atribuição de nomes não é apresentada aos utilizadores ao introduzir um nome de grupo sala de aula. Os utilizadores tem de introduzir o nome do grupo de sala de aulas imposta com prefixos e sufixos. Caso contrário, o grupo de sala de aulas criar ou editar a operação falhar com erros.
Power BI | Áreas de trabalho do Power BI são compatíveis com a política de atribuição de nomes.    
Yammer | Yammer grupos ligados não impõem a política de atribuição de nomes configurada. Para organizações com atribuição de nome de política ativada, o Yammer cria grupos de Yammer legados que não estão ligados ao Office 365 para grupos que não está em conformidade com a política de atribuição de nomes.
StaffHub  | As equipas de StaffHub não siga a política de atribuição de nomes, mas não de grupo do Office 365 subjacente. Nome do agrupamento StaffHub não se aplica a prefixos e sufixos e procurar palavras bloqueadas personalizadas. Mas StaffHub aplicam-se os prefixos e sufixos e remove palavras bloqueadas do grupo do Office 365 subjacente.
PowerShell do Exchange | Cmdlets do PowerShell do Exchange são compatíveis com a política de atribuição de nomes. Os utilizadores recebem mensagens de erro apropriada com prefixos sugeridos e sufixos de e para personalizado palavras bloqueadas se estes não siga a política de atribuição de nomes no nome do grupo e alias de grupo (mailNickname).
Cmdlets do PowerShell do Active Directory do Azure | Cmdlets do PowerShell do Active Directory do Azure estão em conformidade com a política de atribuição de nomes. Os utilizadores recebem mensagens de erro apropriada com prefixos sugeridos e sufixos de e para personalizado palavras bloqueadas se estes não siga a Convenção de nomenclatura nos nomes de grupo e alias de grupo.
Centro de administração do Exchange | Centro de administração do Exchange está em conformidade com a política de atribuição de nomes. Os utilizadores recebem mensagens de erro apropriada com prefixos sugeridos e sufixos de e para personalizado palavras bloqueadas se estes não siga a Convenção de nomenclatura no nome do grupo e alias de grupo.
Centro de administração do Office 365 | Centro de administração do Office 365 é compatível com a política de atribuição de nomes. Quando um utilizador cria ou edições de nomes de grupo, a política de atribuição de nomes é aplicada automaticamente e aos utilizadores recebem erros de adequadas quando introduzidos palavras bloqueadas personalizadas. O Centro de administração do Office 365 ainda não mostra uma versão de pré-visualização da política de atribuição de nomes e não devolve erros personalizados word bloqueado quando o utilizador introduz o nome do grupo.

## <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais sobre os grupos do Azure AD.

* [Consulte os grupos existentes](active-directory-groups-view-azure-portal.md)
* [Política de expiração para grupos do Office 365](active-directory-groups-lifecycle-azure-portal.md)
* [Gerir as definições de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerir os membros de um grupo](active-directory-groups-members-azure-portal.md)
* [Gerir membros de um grupo](active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas para os utilizadores de um grupo](active-directory-groups-dynamic-membership-azure-portal.md)
