---
title: Personalização de idioma no Azure Active Directory B2C | Microsoft Docs
description: Saiba mais sobre como personalizar a experiência de idioma.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 72a2bd20d08cd12cc1965bd06090d2cd705fc111
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711941"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Personalização de idioma no Azure Active Directory B2C

Personalização de idioma no Azure Active Directory B2C (Azure AD B2C) permite a política acomodar idiomas diferentes, de acordo com as suas necessidades de cliente.  A Microsoft fornece traduções para [36 idiomas](#supported-languages), mas também pode fornecer as suas próprias traduções para nenhum idioma. Mesmo se a sua experiência é fornecida para apenas um único idioma, pode personalizar qualquer texto nas páginas.  

## <a name="how-language-customization-works"></a>Como funciona a personalização de idioma
Utilizar personalização de idioma para selecionar os idiomas da sua viagem de utilizador está disponível. Depois da funcionalidade está ativada, pode fornecer o parâmetro de cadeia de consulta, `ui_locales`, da sua aplicação. Quando chamar para o Azure AD B2C, a página é convertida na região que indicou. Este tipo de configuração dá-lhe controlo total sobre os idiomas da sua viagem de utilizador e ignora as definições de idioma do browser do cliente. 

Não poderá ser necessário desse nível de controlo sobre os idiomas vê do seu cliente. Se não fornecer um `ui_locales` parâmetro, experiência do cliente é ditada pelo definições do seu browser.  Pode continuar a controlar os idiomas da sua viagem de utilizador é convertida em adicionando-o como um idioma suportado. Se o browser de um cliente está definida para mostrar um idioma que não pretende suportar, o idioma que selecionou como uma predefinição no culturas suportadas é apresentado em vez disso.

- **IU regiões especificado idioma**: depois de ativar a personalização de idioma, da sua viagem de utilizador é convertida para o idioma que é especificado aqui.
- **Idioma de pedido de browser**: Se não `ui_locales` foi especificado um parâmetro, da sua viagem de utilizador é convertida para o idioma pedido de browser, *se o idioma é suportado*.
- **Linguagem predefinida de política**: se o browser não especifica um idioma ou especifica um que não é suportada, journey o utilizador é convertido para o idioma predefinido de política.

>[!NOTE]
>Se estiver a utilizar os atributos de utilizador personalizado, terá de fornecer as suas próprias traduções. Para obter mais informações, consulte [personalizar as cadeias](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Suporte de idiomas pedidos para ui_locales 
Políticas que foram criadas antes da disponibilidade geral da personalização de idioma necessário ativar esta funcionalidade pela primeira vez. As políticas que foram criadas depois de tem ativada por predefinição de personalização de idioma. 

Quando ativar a personalização de idioma numa política, pode controlar o idioma de journey o utilizador ao adicionar o `ui_locales` parâmetro.
1. [Aceda à página de funcionalidades do B2C no portal do Azure](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings).
2. Navegue para uma política que pretende ativar para traduções.
3. Selecione **personalização de idioma**.  
4. Selecione **ativar a personalização de idioma**.
5. Ler as informações na caixa de diálogo e selecionar **Sim**.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Selecione os idiomas da sua viagem de utilizador estão ativados 
Ativar um conjunto de idiomas da sua viagem de utilizador ser convertido em quando solicitado pelo browser sem o `ui_locales` parâmetro.
1. Certifique-se de que a política tem ativada a partir de instruções anteriores de personalização de idioma.
2. Do **Editar política** página, selecione **personalização de idioma**.
3. Selecione um idioma que pretende suportar.
4. No painel de propriedades, altere **ativado** para **Sim**.  
5. Selecione **guardar** no topo do painel de propriedades.

>[!NOTE]
>Se um `ui_locales` parâmetro não for fornecido, a página é convertida em idioma do browser do cliente apenas se estiver ativada.
>

## <a name="customize-your-strings"></a>Personalizar a sua cadeias
Personalização de idioma permite-lhe personalizar a qualquer cadeia da sua viagem de utilizador.
1. Certifique-se de que a política tem ativada a partir das instruções anteriores de personalização de idioma.
2. Do **Editar política** página, selecione **personalização de idioma**.
3. Selecione o idioma que pretende personalizar.
4. Selecione a página que pretende editar.
5. Selecione **transferir predefinições** (ou **transferir substituições** se anteriormente tiver editado neste idioma). 

Estes passos dão-lhe um ficheiro JSON que pode utilizar para iniciar as cadeias de edição.

### <a name="change-any-string-on-the-page"></a>Altere qualquer cadeia na página
1. Abra o ficheiro JSON que transferiu do instruções anteriores num editor de JSON.
2. Encontrar o elemento que pretende alterar.  Pode encontrar `StringId` para a cadeia de procura ou procure o `Value` atributo que pretende alterar.
3. Atualização do `Value` atributo com o que pretende apresentar.
4. Para cada cadeia que pretende alterar, alterar `Override` para `true`.
5. Guarde o ficheiro e carregar as suas alterações. (Pode encontrar o controlo de carregamento no mesmo local como onde transferiu o ficheiro JSON.) 

>[!IMPORTANT]
>Se precisar de uma cadeia de substituição, certifique-se definir o `Override` valor `true`.  Se o valor não é alterado, a entrada é ignorada. 
>

### <a name="change-extension-attributes"></a>Alterar os atributos de extensão
Se pretender alterar a cadeia para um atributo de utilizador personalizada ou que pretende adicionar um para o JSON, está no seguinte formato:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Substitua `<ExtensionAttribute>` com o nome do seu atributo de utilizador personalizadas.  

Substitua `<ExtensionAttributeValue>` com a nova cadeia a apresentar.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Forneça uma lista de valores utilizando LocalizedCollections
Se pretender fornecer uma lista de conjunto de valores de respostas, terá de criar um `LocalizedCollections` atributo.  `LocalizedCollections` é uma matriz de `Name` e `Value` pares. A ordem dos itens será ordem que são apresentadas.  Para adicionar `LocalizedCollections`, utilize o seguinte formato:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` é o utilizador de atributos que este `LocalizedCollections` atributo for uma resposta a.
* `Name` é o valor que é apresentado ao utilizador.
* `Value` é o que é devolvido na afirmação quando esta opção está selecionada.

### <a name="upload-your-changes"></a>Carregar as suas alterações
1. Depois de concluir as alterações para o ficheiro JSON, volte ao seu inquilino do B2C.
2. Do **Editar política** página, selecione **personalização de idioma**.
3. Selecione o idioma que pretende converter para.
4. Selecione a página onde pretende fornecer traduções.
5. Selecione o ícone de pastas e selecione o ficheiro JSON para carregar.
 
As alterações são guardadas na sua política automaticamente.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Personalizar a página IU através da utilização de personalização de idioma

Existem duas formas de localizar o conteúdo HTML. É uma forma para ativar o [personalização de idioma](active-directory-b2c-reference-language-customization.md). Ativar esta funcionalidade permite que o Azure AD B2C reencaminhar o parâmetro abrir ID Connect `ui-locales`, para o ponto final.  O servidor de conteúdo pode utilizar este parâmetro para fornecer páginas HTML personalizadas que são específicas do idioma.

Em alternativa, pode solicitar conteúdo a partir de diferentes locais com base na região que é utilizado. O ponto final ativado para CORS, pode configurar uma estrutura de pasta para alojar conteúdo em idiomas específicos. Irá chamar o um direito se utilizar o valor de caráter universal `{Culture:RFC5646}`.  Por exemplo, suponha que este é a URI de página personalizada:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Pode carregar a página no `fr`. Quando a página solicita conteúdo HTML e CSS, é extrair de:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Adicionar idiomas personalizados

Também pode adicionar idiomas atualmente a Microsoft não cede traduções para. Terá de fornecer as traduções para todas as cadeias na política.  Códigos de idioma e região estão limitados no padrão de 639 1 ISO. 

1. Do **Editar política** página, selecione **personalização de idioma**.
2. Selecione **Adicionar idioma personalizado** da parte superior da página.
3. No painel de contexto que se abre, identifique o idioma que está a fornecer traduções para introduzindo um código de região válido.
4. Para cada página, pode transferir um conjunto de substituições para inglês e as traduções de trabalho.
5. Depois de terminado com os ficheiros JSON, pode carregá-los para cada página.
6. Selecione **ativar**, e a política agora pode mostrar neste idioma para os seus utilizadores.
7. Guarde o idioma.

>[!IMPORTANT]
>Terá de ativar os idiomas personalizados ou carregar substituições para o mesmo para poder guardar.
>

## <a name="additional-information"></a>Informações adicionais

### <a name="page-ui-customization-labels-as-overrides"></a>Etiquetas de personalização de página IU como substituições
Quando ativar a personalização de idioma, as edições que efectuou anterior para as etiquetas a utilizar a personalização da página IU são mantidas num ficheiro JSON para inglês (en). Pode continuar a alterar as etiquetas e noutras cadeias através do carregamento de recursos do idioma na personalização de idioma.
### <a name="up-to-date-translations"></a>Traduções atualizadas
A Microsoft está empenhada em fornecer as traduções mais atualizadas à sua para utilização. A Microsoft continuamente melhora traduções e mantém-nos em conformidade para si. Microsoft irá identificar erros e de alterações na terminologia global e disponibilizar atualizações de irão funcionar de forma totalmente integrada da sua viagem de utilizador.
### <a name="support-for-right-to-left-languages"></a>Suporte para idiomas de direita para a esquerda
Microsoft atualmente não fornece suporte para idiomas de direita para a esquerda. Isto é possível utilizando regiões personalizados e utilizar CSS para alterar a forma como as cadeias são apresentadas.  Se precisar desta funcionalidade,. votá-lo em [Azure comentários](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Traduções de fornecedor de identidade de redes sociais
A Microsoft oferece o `ui_locales` parâmetro OIDC para inícios de sessão de redes sociais. Mas alguns fornecedores de identidade de redes sociais, incluindo o Facebook e Google, não respeite-los. 
### <a name="browser-behavior"></a>Comportamento de browser
Chrome e Firefox que ambos pedem para o idioma de conjunto. Se se tratar de um idioma suportado, é apresentada antes da predefinição. Atualmente, o limite não pedir um idioma e vai diretamente para o idioma predefinido.

### <a name="supported-languages"></a>Linguagens suportadas

| Idioma              | Código de idioma |
|-----------------------|---------------|
| Bangla                | Bn            |
| Checo                 | cs            |
| Dinamarquês                | da            |
| Alemão                | de            |
| Grego                 | el            |
| Português               | en            |
| Espanhol               | es            |
| Finlandês               | fi            |
| Francês                | fr            |
| Guzarate              | gu            |
| Hindi                 | Olá            |
| Croata              | hr            |
| Húngaro             | hu            |
| Italiano               | it            |
| Japonês              | ja            |
| Canarim               | kN            |
| Coreano                | ko            |
| Malayalam             | ml            |
| Marati               | MR            |
| Malaio                 | ms            |
| Norueguês Bokmal      | nb            |
| Neerlandês                 | nl            |
| Punjabi               | Pa            |
| Polaco                | pl            |
| Português - Brasil   | pt-br         |
| Português - Portugal | pt-pt         |
| Romeno              | ro            |
| Russo               | ru            |
| Eslovaco                | SK            |
| Sueco               | sv            |
| Tamil                 | dados            |
| Télego                | te            |
| Tailandês                  | º            |
| Turco               | TR            |
| Chinês - simplificada  | zh-hans       |
| Chinês - tradicional | zh-hant       |
