---
title: Escrever expressões para mapeamentos de atributos no Azure Active Directory | Documentos da Microsoft
description: Saiba como utilizar os mapeamentos de expressão para transformar valores de atributo num formato aceitável durante o aprovisionamento automatizado de objetos de aplicação SaaS no Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: b13c51cd-1bea-4e5e-9791-5d951a518943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.openlocfilehash: c0c3e6fab27ff16f0cc75fde3587d280278be882
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215293"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Escrever expressões para mapeamentos de atributos no Azure Active Directory
Quando configurar o aprovisionamento a uma aplicação SaaS, um dos tipos de mapeamentos de atributos que pode especificar é um mapeamento de expressão. Para eles, deve escrever uma expressão de tipo de script que permite transformar os dados dos seus utilizadores em formatos que são mais aceitáveis para a aplicação SaaS.

## <a name="syntax-overview"></a>Descrição geral da sintaxe
A sintaxe para expressões para mapeamentos de atributos é que sobrou do Visual Basic para funções de Applications (VBA).

* Toda a expressão tem de ser definida em termos de funções, que são compostas por um nome, seguido de argumentos parênteses: <br>
  *FunctionName (<< argumento 1 >>, <<argument N>>)*
* Pode aninhar funções dentro de uns dos outros. Por exemplo: <br> *FunctionOne(FunctionTwo(<<argument1>>))*
* Pode passar três tipos de argumentos diferentes em funções:
  
  1. Atributos, que devem estar entre parênteses Retos. Por exemplo: [attributeName]
  2. Constantes de cadeia de caracteres, têm de estar entre aspas duplas. Por exemplo: "United States"
  3. Outras funções. Por exemplo: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* Para constantes de cadeia de caracteres, se precisar de uma barra invertida (\) ou aspas (") na cadeia de caracteres, ele deve ser escrito com o símbolo de barra invertida (\). Por exemplo: "nome da empresa: \"Contoso\""

## <a name="list-of-functions"></a>Lista de funções
[Acrescentar](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [associar](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [NormalizeDiacritics](#normalizediacritics) [não](#not) &nbsp; &nbsp; &nbsp; &nbsp; [substituir](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [SingleAppRoleAssignment](#singleapproleassignment) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [Comutador](#switch)

- - -
### <a name="append"></a>Acrescentar
**Função:**<br> Append(Source, suffix)

**Descrição:**<br> Assume um valor de cadeia de caracteres de origem e acrescenta o sufixo ao final do mesmo.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Cadeia |Normalmente, o nome do atributo de objeto de origem |
| **suffix** |Necessário |Cadeia |A cadeia a acrescentar ao final do valor de origem. |

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Função:**<br> FormatDateTime (origem, inputFormat, outputFormat)

**Descrição:**<br> Precisa de uma cadeia de data de um formato e os converte num formato diferente.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Cadeia |Normalmente, o nome do atributo de objeto de origem. |
| **inputFormat** |Necessário |Cadeia |Formato esperado do valor de origem. Para formatos suportados, consulte [ http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx ](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Necessário |Cadeia |Formato da data de saída. |

- - -
### <a name="join"></a>Associar
**Função:**<br> Junte-se (separador, source1 source2,...)

**Descrição:**<br> JOIN() é semelhante ao Append(), exceto pelo fato de que possa combinar várias **origem** valores de cadeia de caracteres numa única cadeia de caracteres, e cada valor ficarão separado por um **separador** cadeia de caracteres.

Se um dos valores de origem é um atributo com múltiplos valor, em seguida, cada valor nesse atributo irá ser unidas, separados o valor de separador.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **separator** |Necessário |Cadeia |Cadeia utilizada para separar os valores de origem quando eles são concatenados numa cadeia de caracteres. Pode ser "" se nenhum separador é necessário. |
| * * source1... sourceN * * |Variável-número de vezes, necessário |Cadeia |Cadeia de valores a ser unidas. |

- - -
### <a name="mid"></a>Mid
**Função:**<br> Mid (origem, início, comprimento)

**Descrição:**<br> Devolve uma subcadeia do valor de origem. Uma subcadeia é uma cadeia que contém apenas alguns dos caracteres da cadeia de origem.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Cadeia |Normalmente, o nome do atributo. |
| **start** |Necessário |inteiro |Índice no **origem** cadeia de caracteres onde a subcadeia deve começar. Primeiro caractere na cadeia de caracteres terão o índice de 1, o segundo caráter terão índice 2 e assim por diante. |
| **Comprimento** |Necessário |inteiro |Comprimento da subcadeia. Se o comprimento termina fora o **origem** cadeia de caracteres, a função devolve a subcadeia do **iniciar** índice até o final da **origem** cadeia de caracteres. |

- - -
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Função:**<br> NormalizeDiacritics(source)

**Descrição:**<br> Requer um argumento de cadeia de caracteres. Devolve a cadeia de caracteres, mas com nenhum dos caracteres diacritical substituídos por carateres não diacritical equivalentes. Normalmente, utilizada para converter os nomes de primeiro e último carateres diacritical (marcas de acento sem) em valores legais que podem ser utilizadas em vários identificadores de utilizador, tais como nomes de principal de utilizador, nomes de conta SAM e endereços de e-mail.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Cadeia | Normalmente, um nome ou o último atributo de nome |

- - -
### <a name="not"></a>não
**Função:**<br> Not(Source)

**Descrição:**<br> Muda o valor booleano do **origem**. Se **origem** valor é "*True*", devolve "*False*". Caso contrário, devolve "*True*".

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Booleano da cadeia |Era esperado **origem** valores são "True" ou "False".... |

- - -
### <a name="replace"></a>Substituir
**Função:**<br> Substituir (origem, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, modelo)

**Descrição:**<br>
Substitui os valores dentro de uma cadeia de caracteres. Ele funciona de forma diferente consoante os parâmetros fornecidos:

* Quando **oldValue** e **replacementValue** são fornecidos:
  
  * Substitui todas as ocorrências de oldValue na origem com replacementValue
* Quando **oldValue** e **modelo** são fornecidos:
  
  * Substitui todas as ocorrências do **oldValue** no **modelo** com o **origem** valor
* Quando **regexPattern**, **regexGroupName**, **replacementValue** são fornecidos:
  
  * Substitui todos os valores correspondentes oldValueRegexPattern na cadeia de origem com replacementValue
* Quando **regexPattern**, **regexGroupName**, **replacementPropertyName** são fornecidos:
  
  * Se **origem** não tem nenhum valor **origem** é devolvido
  * Se **origem** tem um valor, utiliza **regexPattern** e **regexGroupName** para extrair o valor de substituição da propriedade com o **replacementPropertyName** . Valor de substituição é retornado como o resultado

**Parâmetros:**<br> 
| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Cadeia |Normalmente, o nome do atributo de objeto de origem. |
| **oldValue** |Opcional |Cadeia |Valor a ser substituído na **origem** ou **modelo**. |
| **regexPattern** |Opcional |Cadeia |Padrão RegEx para o valor a ser substituído na **origem**. Ou, quando é utilizado o replacementPropertyName, padrão para extrair o valor de propriedade de substituição. |
| **regexGroupName** |Opcional |Cadeia |Nome do grupo de dentro **regexPattern**. Apenas quando é utilizado o replacementPropertyName, extrairemos valor deste grupo como replacementValue da propriedade de substituição. |
| **replacementValue** |Opcional |Cadeia |Novo valor para substituir um antigo com. |
| **replacementAttributeName** |Opcional |Cadeia |Nome do atributo a ser utilizado para o valor de substituição, quando a origem não tem nenhum valor. |
| **Modelo** |Opcional |Cadeia |Quando **modelo** valor é fornecido, procurará **oldValue** dentro do modelo e substituí-lo com o valor de origem. |

- - -
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Função:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Descrição:**<br> Requer um argumento de cadeia de caracteres. Devolve a cadeia de caracteres, mas com qualquer repalced diacritical caracteres com caracteres não diacritical equivalentes.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Necessário |Cadeia |**[appRoleAssignments]**  objeto. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Função:**<br> StripSpaces(source)

**Descrição:**<br> Remove todo o espaço ("") carateres da cadeia de caracteres de origem.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Cadeia |**origem** valor para atualizar. |

- - -
### <a name="switch"></a>Comutador
**Função:**<br> Comutador (origem, defaultValue, chave1, value1, chave2, value2...)

**Descrição:**<br> Quando **origem** correspondências de valor um **chave**, devolve **valor** para que **chave**. Se **origem** valor não corresponde a quaisquer chaves, devolve **defaultValue**.  **Chave** e **valor** parâmetros tem sempre chegam em pares. A função espera sempre um número par de parâmetros.

**Parâmetros:**<br> 

| Nome | Obrigatório / repetidos | Tipo | Notas |
| --- | --- | --- | --- |
| **Origem** |Necessário |Cadeia |**Origem** valor para atualizar. |
| **defaultValue** |Opcional |Cadeia |Valor predefinido a utilizar quando a origem não corresponde a quaisquer chaves. Pode ser uma cadeia de caracteres vazia (""). |
| **chave** |Necessário |Cadeia |**Chave** comparar **origem** com de valor. |
| **valor** |Necessário |Cadeia |Valor de substituição para o **origem** correspondentes a chave. |

## <a name="examples"></a>Exemplos
### <a name="strip-known-domain-name"></a>Nome de domínio conhecidos de faixa
É necessário um nome de domínio conhecidos do e-mail de um utilizador para obter um nome de utilizador de faixa. <br>
Por exemplo, se o domínio "contoso.com", em seguida, poderia usar a seguinte expressão:

**Expressão:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Exemplo de entrada / saída:** <br>

* **ENTRADA** (correio): "john.doe@contoso.com"
* **SAÍDA**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Acrescentar constante sufixo ao nome de utilizador
Se estiver a utilizar uma área de segurança do Salesforce, precisará de acrescentar um sufixo adicional a todos os seus nomes de utilizador antes de sincronizá-los.

**Expressão:** <br>
`Append([userPrincipalName], ".test"))`

**Exemplo de entrada/saída:** <br>

* **ENTRADA**: (userPrincipalName): "John.Doe@contoso.com"
* **SAÍDA**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Gerar o alias do utilizador, concatenando partes do nome próprio e apelido
Terá de gerar um utilizador alias ao colocar os primeiros 3 letras do nome próprio do utilizador e os primeiros 5 letras do apelido do utilizador.

**Expressão:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Exemplo de entrada/saída:** <br>

* **ENTRADA** (givenName): "João"
* **ENTRADA** (sobrenome): "Doe"
* **SAÍDA**: "JohDoe"

### <a name="remove-diacritics-from-a-string"></a>Remover Diacríticos de uma cadeia de caracteres
É necessário substituir caracteres que contém marcas de acento sem com caracteres equivalentes que não contenham marcas de acento sem.

**Expressão:** <br>
NormalizeDiacritics([givenName])

**Exemplo de entrada/saída:** <br>

* **ENTRADA** (givenName): "Zoë"
* **SAÍDA**: "Zoe"

### <a name="output-date-as-a-string-in-a-certain-format"></a>Data de saída como uma cadeia de caracteres num determinado formato
Pretende enviar as datas para uma aplicação SaaS num determinado formato. <br>
Por exemplo, que pretende formatar datas do ServiceNow.

**Expressão:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Exemplo de entrada/saída:**

* **ENTRADA** (extensionAttribute1): "20150123105347.1Z"
* **SAÍDA**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Substituir um valor com base num conjunto predefinido de opções
Tem de definir o fuso horário do utilizador com base no código de estado armazenado no Azure AD. <br>
Se o código de estado não corresponder a qualquer uma das opções predefinidas, utilize o valor predefinido de "Austrália/Sydney".

**Expressão:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Exemplo de entrada/saída:**

* **ENTRADA** (estado): "QLD"
* **SAÍDA**: "Austrália/Brisbane"

## <a name="related-articles"></a>Artigos relacionados
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](active-directory-apps-index.md)
* [Automatizar o utilizador aprovisionamento/desaprovisionamento às aplicações SaaS](active-directory-saas-app-provisioning.md)
* [Personalizar mapeamentos de atributos para o aprovisionamento do utilizador](active-directory-saas-customizing-attribute-mappings.md)
* [Filtros de âmbito para o aprovisionamento do utilizador](active-directory-saas-scoping-filters.md)
* [Utilizar o SCIM para ativar o aprovisionamento automático de utilizadores e grupos do Azure Active Directory a aplicações](manage-apps/use-scim-to-provision-users-and-groups.md)
* [Notificações do aprovisionamento de contas](active-directory-saas-account-provisioning-notifications.md)
* [Lista de tutoriais sobre como integrar aplicações SaaS](saas-apps/tutorial-list.md)

