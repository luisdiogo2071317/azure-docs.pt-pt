---
title: Utilizar a personalização de idioma - Azure AD B2C | Microsoft Docs
description: Saiba mais sobre como personalizar a experiência de idioma.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/26/2018
ms.author: davidmu
ms.openlocfilehash: 7c72c1d43d9a5fa541c72a8ba7a5ccedeafdaaff
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>O Azure Active Directory B2C: Personalização de idioma a utilizar

>[!NOTE]
>Esta funcionalidade está em pré-visualização pública.
>

Personalização de idioma permite que a política acomodar idiomas diferentes, de acordo com as suas necessidades de cliente.  A Microsoft fornece traduções para 36 idiomas (consulte [informações adicionais](#additional-information)), mas também pode fornecer as suas próprias traduções para nenhum idioma.  Mesmo se a sua experiência só é fornecida para um único idioma, pode personalizar qualquer texto nas páginas.  

## <a name="how-does-language-customization-work"></a>Como funciona a personalização de idioma?
Personalização de idioma permite-lhe selecionar os idiomas da sua viagem de utilizador está disponível.  Depois da funcionalidade está ativada, pode fornecer o parâmetro de cadeia de consulta, ui_locales, da sua aplicação.  Quando chamar para o Azure AD B2C, iremos traduzir a página para a região que indicou.  Este tipo de configuração dá-lhe controlo total sobre os idiomas da sua viagem de utilizador e ignora as definições de idioma do browser do cliente. Em alternativa, pode não necessitar desse nível de controlo sobre os idiomas, consulte do seu cliente.  Se não fornecer um parâmetro de ui_locales, experiência do cliente é ditada pelo definições do seu browser.  Pode continuar a controlar os idiomas da sua viagem de utilizador é convertida em adicionando-o como um idioma suportado.  Se o browser de um cliente está definida para mostrar um idioma que não pretende suportar, o idioma selecionado por predefinição no culturas suportadas é apresentado em vez disso.

1. **IU regiões especificado idioma** -depois de ativar a personalização de idioma, da sua viagem de utilizador é convertida para o idioma especificado aqui
2. **Browser pedida idioma** -não se foi especificado nenhum regiões IU, traduz-se o browser pedida idioma, **se foi incluído nos idiomas suportados**
3. **Linguagem predefinida de política** -se o browser não especifica um idioma ou especifica um que não é suportada, mesmo traduz-se o idioma predefinido de política

>[!NOTE]
>Se estiver a utilizar os atributos de utilizador personalizado, terá de fornecer as suas próprias traduções. Consulte '[personalizar as cadeias](#customize-your-strings)' para obter mais detalhes.
>

## <a name="support-uilocales-requested-languages"></a>Pedido de suporte ui_locales idiomas 
As políticas que foram criadas antes do lançamento de disponibilidade geral de personalização de idioma serão necessário ativar esta funcionalidade pela primeira vez.  As políticas criadas após terão de personalização de idioma ativada por predefinição.  Ao ativar 'Personalização de idioma' numa política, agora pode controlar o idioma de journey o utilizador adicionando o parâmetro ui_locales.
1. [Siga estes passos para navegar para a página de funcionalidades do B2C no portal do Azure.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Navegue para uma política que pretende ativar para traduções.
3. Clique em **personalização de idioma**.  
4. Clique em **ativar a personalização de idioma** na parte superior.
5. Leia a caixa de diálogo e clique em 'Sim'.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Selecione os idiomas da sua viagem de utilizador estão ativados 
Ative um conjunto de idiomas da sua viagem de utilizador ser convertido no quando o parâmetro ui_locales não foi fornecido.
1. Certifique-se a política de 'Personalização de idioma' ativada a partir de instruções anteriores.
2. Do seu **Editar política** página, selecione **personalização de idioma**.
3. Selecione um idioma que pretende suportar.
4. No painel de propriedades, alternar **ativado** como Sim.  
5. Clique em **guardar** no topo do painel de propriedades.

>[!NOTE]
>Se não for fornecido um parâmetro de ui_locales, em seguida, a página é convertida em idioma do browser do cliente apenas se estiver ativada
>

## <a name="customize-your-strings"></a>Personalizar a sua cadeias
'Personalização de idioma' permite-lhe personalizar a qualquer cadeia da sua viagem de utilizador.
1. Certifique-se a política de 'Personalização de idioma' ativada a partir das instruções anteriores.
2. Do seu **Editar política** página, selecione **personalização de idioma**.
3. Selecione o idioma que pretende personalizar.
4. Selecione a página que pretende editar.
5. Clique em **transferir predefinições** (ou **transferir substituições** se anteriormente tiver editado neste idioma). 

Estes passos dão-lhe um ficheiro JSON que pode utilizar para iniciar as cadeias de edição.

### <a name="changing-any-string-on-the-page"></a>Alterar qualquer cadeia na página
1. Abra o ficheiro JSON que transferiu do instruções anteriores num editor de JSON.
2. Encontrar o elemento que pretende alterar.  Pode encontrar o `StringId` da cadeia de procura ou procure o `Value` que pretende alterar.
3. Atualização do `Value` atributo com o que pretende apresentar.
4. Para cada cadeia que pretende alterar, lembre-se de alternar `Override` para **verdadeiro**.
5. Guarde o ficheiro e carregar as suas alterações (que pode encontrar o controlo de carregamento no mesmo local como onde transferiu o ficheiro JSON). 

>[!IMPORTANT]
>Se precisar de uma cadeia de substituição, certifique-se definir o `Override` valor `true`.  Se o valor não é alterado, a entrada é ignorada. 
>

### <a name="changing-extension-attributes"></a>Atributos de extensão de alteração
Se pretender alterar a cadeia para um atributo de utilizador personalizada ou pretender adicionar um para o JSON, é o seguinte formato:
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

### <a name="using-localizedcollections"></a>Utilizar LocalizedCollections
Se pretender fornecer uma lista de conjunto de valores de respostas, terá de criar um `LocalizedCollections`.  A `LocalizedCollections` é uma matriz de `Name` e `Value` pares.  O `Name` é o que é apresentado e o `Value` é o que é devolvido na afirmação.  Para adicionar um `LocalizedCollections`, tem o seguinte formato:

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

* `ElementId` é o utilizador de atributos que este `LocalizedCollections` for uma resposta a
* `Name` é apresentado o valor para o utilizador
* `Value` é o que é devolvido na afirmação quando esta opção está selecionada

### <a name="upload-your-changes"></a>Carregar as suas alterações
1. Depois de concluir as alterações para o ficheiro JSON, navegue para o inquilino do B2C.
2. Do seu **Editar política** página, selecione **personalização de idioma**.
3. Selecione o idioma em que pretenda fornecer traduções.
4. Selecione a página que pretenda fornecer traduções.
5. Clique no ícone de pastas e selecione o ficheiro JSON para carregar.
6. Este alterado é guardado na sua política automaticamente.

## <a name="using-page-ui-customization-with-language-customization"></a>Utilizando a página IU personalização com a personalização de idioma

Existem duas formas de localizar o conteúdo HTML.  Ao ativar ['Personalização de idioma'](active-directory-b2c-reference-language-customization.md).  Ativar esta funcionalidade permite que o Azure AD B2C reencaminhar o parâmetro abrir ID Connect `ui-locales`, para o ponto final.  O servidor de conteúdo pode utilizar este parâmetro para fornecer páginas HTML personalizadas que são específicas do idioma.

Em alternativa, iremos pode extrair conteúdo de diferentes locais com base no idioma que está a ser utilizado.  O ponto final de CORS ativada, pode configurar uma estrutura de pasta para alojar conteúdo em idiomas específicos e irá chamamos o um direito se colocar o valor de caráter universal, `{Culture:RFC5646}`.  Por exemplo, se tiver tal como o meu personalizadas de URI de página:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Pode carregar a minha página no `fr` e quando a extração de conteúdo html e css, irá solicitar de:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="custom-locales"></a>Regiões personalizados

Também pode adicionar idiomas atualmente a Microsoft não cede traduções para.  Terá de fornecer as traduções para todas as cadeias na política.

1. Do seu **Editar política** página, selecione **personalização de idioma**.
2. Selecione **Adicionar idioma personalizado** da parte superior da página.
3. No painel de contexto que se abre, identifique o idioma que está a fornecer traduções para introduzindo um código de região válido.
4. Para cada página pode transferir um conjunto de substituições para inglês e as traduções de trabalho.
5. Quando tiver terminado com os ficheiros JSON, pode carregá-los para cada página.
6. Selecione **ativar** e a política agora pode mostrar neste idioma para os seus utilizadores.
7. Lembre-se guardar uma linguagem assim que o tiver ativado.

## <a name="additional-information"></a>Informações adicionais

### <a name="page-ui-customization-labels-are-persisted-as-your-first-set-of-overrides-once-language-customization-is-enabled"></a>Etiquetas de personalização de página IU são mantidas, como o primeiro conjunto de substituições, depois de 'Personalização de idioma' está ativada
Quando ativar a personalização do idioma, as edições que efectuou anterior para as etiquetas a utilizar a personalização de página IU são mantidas num ficheiro JSON para inglês (en).  Pode continuar a alterar as etiquetas e noutras cadeias através do carregamento de recursos do idioma 'Personalização de idioma'.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>A Microsoft está empenhada para fornecer as traduções mais atualizadas à sua para utilização
Iremos continuamente melhorar traduções e mantê-las em conformidade para si.  Iremos identificar erros e de alterações na terminologia global e disponibilizar as atualizações de que irão funcionar de forma totalmente integrada da sua viagem de utilizador.
### <a name="support-for-right-to-left-languages"></a>Suporte para idiomas de direita para a esquerda
Estamos atualmente não estão fornecer suporte para idiomas de direita para a esquerda, se necessitar desta funcionalidade, votar para esta funcionalidade no [Azure comentários](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Redes sociais traduções de fornecedor de identidade
Podemos fornecer o parâmetro OIDC ui_locales para inícios de sessão de redes sociais, mas não são cumpridas por alguns fornecedores de identidade de redes sociais, incluindo Facebook e Google. 
### <a name="browser-behavior"></a>Comportamento de browser
Chrome Firefox ambos de pedidos de e para o idioma do conjunto e se se tratar de um idioma suportado, é apresentada antes da predefinição.  Atualmente, o limite não pedir um idioma e vai diretamente para o idioma predefinido.

### <a name="what-languages-are-supported"></a>Que idiomas são suportados?

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
| Hindi                 | hi            |
| Croata              | hr            |
| Húngaro             | hu            |
| Italiano               | it            |
| Japonês              | ja            |
| Canarim               | kn            |
| Coreano                | ko            |
| Malayalam             | ml            |
| Marati               | MR            |
| Malaio                 | ms            |
| Norueguês Bokmal      | nb            |
| Neerlandês                 | nl            |
| Punjabi               | pa            |
| Polaco                | pl            |
| Português - Brasil   | pt-br         |
| Português - Portugal | pt-pt         |
| Romeno              | ro            |
| Russo               | ru            |
| Eslovaco                | SK            |
| Sueco               | sv            |
| Tamil                 | dados            |
| Télego                | te            |
| Tailandês                  | ésimo            |
| Turco               | TR            |
| Chinês - simplificada  | zh-hans       |
| Chinês - tradicional | zh-hant       |
