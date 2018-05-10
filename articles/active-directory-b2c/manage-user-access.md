---
title: Gerir o acesso de utilizador no Azure AD B2C | Microsoft Docs
description: Saiba como identificar lesar, recolher data de nascimento e país dados e obter a aceitação dos termos de utilização na sua aplicação com o Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 05/04/2018
ms.author: davidmu
ms.openlocfilehash: c44135a3069966b14d8760e4daa009ab8d39ccca
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="manage-user-access-in-azure-ad-b2c"></a>Gerir o acesso de utilizador no Azure AD B2C

Este artigo fornece informações sobre como pode gerir o acesso de utilizador às suas aplicações com o Azure Active Directory (AD) B2C. Gestão de acesso na sua aplicação inclui:

- Identificar lesar e controlar o acesso ao utilizar a aplicação
- Exigir a autorização parental consentimento para que os menores utilizem as suas aplicações
- Recolha de dados de data de nascimento e país do utilizador
- Captura termos de utilização contrato e o controlo de acesso

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

>[!Note] 
>Este artigo fornece informações que podem ser utilizadas para suportar as obrigações sob o GDPR. Se estiver à procura de informações gerais sobre GDPR, consulte o [secção GDPR do portal do serviço de confiança](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="control-minor-access"></a>Controlo de acesso secundário

Aplicações e as organizações podem optar por bloquear lesar da utilização de aplicações e serviços que não são direcionados para este público-alvo. Em alternativa, aplicações e as organizações podem decidir aceitar lesar, subsequentemente, gerir o consentimento autorização parental e fornecer experiências permitidas para que os menores, conforme determinado pelas regras de negócio e permitido pelo Regulamento. 

Se um utilizador for identificado como uma secundária, o fluxo de utilizador no Azure AD B2C pode ser definido como uma das três opções:

- **Enviar uma id_token JWT assinado novamente à aplicação** - o utilizador está registado no diretório e é devolvido um token para a aplicação. A aplicação, em seguida, prossegue com as regras de negócio. Por exemplo, pode continuar a aplicação com um processo de autorização parental consentimento. Para tal, optar por receber o **ageGroup** e **consentProvidedForMinor** afirmações da aplicação.
- **Enviar um token JSON não assinado da aplicação** -Azure AD B2C notifica a aplicação que o utilizador é um menor e fornece o estado de consentimento do utilizador de autorização parental. A aplicação, em seguida, prossegue com as regras de negócio. Um token JSON não for concluída uma autenticação com êxito com a aplicação. A aplicação tem de processar o utilizador não autorizado, de acordo com as afirmações incluídas no token JSON, que pode incluir **nome**, **e-mail**, **ageGroup**e **consentProvidedForMinor**.
- **Bloquear o utilizador** - se o utilizador é um menor e autorização parental consentimento não foi fornecido.  O Azure AD B2C, em seguida, pode apresentar um ecrã para o utilizador que informa de que está a ser bloqueado.  Nenhum token emitido, o acesso é bloqueado e a conta de utilizador não é criada durante um journey de registo. Para implementar este, forneça uma página de conteúdo HTML/CSS adequada para informar o utilizador e as opções apropriadas presentes. É necessária nenhuma ação adicional pela aplicação para novos registos.

## <a name="get-parental-consent"></a>Obter consentimento autorização parental

Consoante regulamento de aplicação, consentimento autorização parental poderá ser necessário para ser concedida por um utilizador validado como um adulto.  O Azure AD B2C não fornece uma experiência para verificar se a duração de um indivíduo e permitir que um adulto verificado conceder a autorização parental consentimento para uma secundária.  Esta experiência tem de ser fornecida pela aplicação ou de outro fornecedor de serviços.

Segue-se um exemplo de um fluxo de utilizador para a recolha de autorização parental consentimento:

1. Um [Active Directory Graph API do Azure](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog) operação identifica o utilizador como um secundária e devolve os dados de utilizador para a aplicação sob a forma de um token JSON não assinado.
2. A aplicação processa o token JSON e mostra um ecrã para o menor notificar que consentimento autorização parental é necessário e pede consentimento de um elemento principal online. 
3. O Azure AD B2C mostra um início de sessão journey na qual o utilizador tem permissão para iniciar sessão normalmente e emite um token para a aplicação que está definida para incluir **legalAgeGroupClassification = "minorWithParentalConsent"** a aplicação recolhe o endereço de e-mail do principal e verifica que principal é um adulto através de uma origem fidedigna, como um national ID office, verificação de licença ou cartão de crédito prova. Se tiver êxito, a aplicação pede-lhe o menor para iniciar sessão com o fluxo de utilizador do Azure AD B2C. Se a autorização foi negada (por exemplo, **legalAgeGroupClassification = "minorWithoutParentalConsent"**, Azure AD B2C devolve um token JSON (não um início de sessão) para a processo de consentimento de reinício da aplicação. É possível que, opcionalmente personalizar o fluxo de utilizador na qual um menor ou um adulto pode recuperar o acesso à conta de uma secundária ao enviar um código de registo para o endereço de correio eletrónico a secundária ou endereço de correio eletrónico a adulto no registo.
4. A aplicação oferece uma opção para o menor para revogar a autorização.
5. Quando o menor ou adulto revoga consentimento, o AD Graph API do Azure pode ser utilizado para alterar **consetProvidedForMinor** para **negado**. Em alternativa, a aplicação pode optar por eliminar um menor cujo consentimento foi revogado. É possível que, opcionalmente personalizar o fluxo de utilizador na qual o menor autenticado (ou principal utilizando a conta do menor) pode revogar a autorização. Registos do Azure AD B2C **consentProvidedForMinor** como **negado**.

Para obter mais informações sobre o **legalAgeGroupClassification**, **consentProvidedForMinor**, e **ageGroup**, consulte [o tipo de recurso de utilizador](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/user). Para obter mais informações sobre os atributos personalizados, consulte [utilizar atributos personalizados para recolher informações sobre os consumidores](active-directory-b2c-reference-custom-attr.md). Quando o endereçamento atributos expandidos utilizando AD Graph API do Azure, a versão do atributo comprimento tem de ser utilizada como "extension_18b70cf9bb834edd8f38521c2583cd86_dateOfBirth": "2011-01-01T00:00:00Z"

## <a name="gather-date-of-birth-and-country-data"></a>Recolher a data de nascimento e país dados

As aplicações possam depender do Azure AD B2C para recolher a data de nascimento (DOB) e o país de todos os utilizadores durante o registo. Se DOB ou informações de país ainda não existir, pode ser-lhe pedido do utilizador durante o próximo journey de autenticação (início de sessão). Os utilizadores não conseguirão continuar sem fornecer DOB e informações de país. Com base no país e DOB fornecido, o Azure AD B2C determina se o indivíduo é considerado uma secundária, de acordo com as normas de regulamentação desse país. 

Um fluxo de utilizador personalizadas pode recolher DOB e informações de país e a utilização do Azure AD B2C afirmações de transformação para determinar o **ageGroup** e manter o resultado (ou manter o país e informações de DOB diretamente) no diretório.

Os passos seguintes mostram a lógica que é utilizada para calcular **ageGroup** da data de nascimento:

1. Tente localizar o país/região, o código de país na lista. Se não for encontrado o país/região, reverter para **predefinido**.
2. Se o **MinorConsent** nós se encontra presente no elemento de país:  <br>a. Calcule a data mínima que teria-se que o utilizador ter sido born para ser considerado um adulto. Exemplo: data de nascimento é 3/14/2015 e **MinorConsent** é 18, data de nascimento mínimo seria 3/14/2000.
    <br>b. Compare a data de nascimento mínima com a data de nascimento real. Se a data de nascimento mínimo é antes da data de nascimento do utilizador, o cálculo devolve **secundárias** como o cálculo de antiguidade.
3. Se o **MinorNoConsentRequired** nós se encontra presente no elemento de país, repita os passos 2a e 2b utilizar o valor do **MinorNoConsentRequired**. Devolve o resultado do 2b **MinorNoConsentRequired** se a data de nascimento mínimo é antes da data de nascimento do utilizador. 
4. Se nenhum dos cálculos devolveu true, o cálculo devolve **para adultos**.

Se tiver uma aplicação DOB fiável recolhido ou dados de país por outros métodos, a aplicação pode utilizar a GRAPH API para atualizar o registo de utilizador com estas informações. Por exemplo:

- Se um utilizador é conhecido como sendo um adulto, atualize o atributo de diretório **ageGroup** com um valor de **para adultos**.
- Se um utilizador é conhecido como sendo uma secundária, atualize o atributo de diretório **ageGroup** com um valor de **secundárias** e defina **consentProvidedForMinor** conforme adequado.

Para obter mais informações sobre a recolha de dados de DOB, consulte [utilizando a idade controlo no Azure AD B2C](basic-age-gating.md).

## <a name="capture-terms-of-use-agreement"></a>Capturar termos do contrato de utilização

Quando desenvolver a sua aplicação, normalmente, capturar a aceitação do utilizador dos termos de utilização nas respetivas aplicações, sem qualquer, ou apenas secundária participação do diretório do utilizador.  É possível, no entanto, para utilizar do Azure AD B2C utilizador fluir para recolher a aceitação dos termos de utilização, restringir o acesso, a menos que é concedida a aceitação e para impor a aceitação das alterações futuras aos termos de utilização com base na data da última aceitação e a data da versio mais recente n dos termos de utilização.

**Termos de utilização** também podem incluir "Consentimento para partilhar dados com terceiros".  A aceitação positiva estas condições de um utilizador pode ser tecnicamente reunida como combinado ou o utilizador pode ser capaz de aceitar uma e não outros, consoante a legislação local e as regras de negócio.

Os passos seguintes descrevem as capacidades para gerir os termos de utilização:

1. Registo aceitação dos termos de utilização e a data de aceitação de utilizar a Graph API e atributos expandidos. Isto pode ser feito através de ambos os fluxos de utilizador incorporadas e personalizadas. É recomendado que criar e utilizar o **extension_termsOfUseConsentDateTime** e **extension_termsOfUseConsentVersion** atributos.
2. Criar uma caixa de verificação necessária intitulada "Aceitar os termos de utilização" e registe o resultado durante a inscrição. Isto pode ser feito através de ambos os fluxos de utilizador incorporadas e personalizadas.
3. O Azure AD B2C armazena os termos do contrato de utilização e consentimento. A Graph API pode ser utilizada para consultar o estado de qualquer utilizador ao ler o atributo de extensão utilizado para registar a resposta, por exemplo ler **termsOfUseTestUpdateDateTime**. Isto pode ser feito através de ambos os fluxos de utilizador incorporadas e personalizadas.
4. Exigir a aceitação dos termos atualizados de utilização comparando a data de aceitação para a data da versão mais recente dos termos de utilização. Isto só pode ser feito através de um fluxo de utilizador personalizadas. Utilize o atributo expandido **extension_termsOfUseConsentDateTime** e comparar o valor para a afirmação de **termsOfUseTextUpdateDateTime**, se aceitação é antiga, em seguida, forçar uma novo aceitação caso contrário, personalizada permitido ecrã, bloquear o acesso com a lógica da política.
5. Exigir a aceitação dos termos atualizados de utilização ao comparar o número de versão de aceitação para o último número de versão foi aceite. Isto só pode ser feito através de um fluxo de utilizador personalizadas. Utilize o atributo expandido **extension_termsOfUseConsentDateTime** e comparar o valor para a afirmação de **extension_termsOfUseConsentVersion**, se aceitação é antiga, em seguida, forçar uma novo aceitação caso contrário, personalizada permitido ecrã, bloquear o acesso com a lógica da política.

Capturar termos de utilização consentimento pode ser apresentada ao utilizador com os seguintes cenários:

- Um novo utilizador é inscrever-se. Os termos de utilização são apresentados o resultado de autorização é armazenado.
- Um utilizador iniciar sessão e anteriormente já foi aceite consentimento a mais recente ou Active Directory termos do contrato. Não são apresentados os termos de utilização.
- Um utilizador iniciar sessão e não aceitou já consentimento a mais recente ou Active Directory termos de utilização. Os termos de utilização são apresentados o resultado de autorização é armazenado.
- Um utilizador iniciar sessão e já foi aceite o consentimento para um anterior termos de utilização, que agora é atualizada para uma versão posterior. Os termos de utilização são apresentados o resultado de autorização é armazenado.

A imagem seguinte mostra o fluxo de utilizador recomendada:

![fluxo de utilizador de aceitação](./media/manage-user-access/user-flow.png) 

Segue-se um exemplo de DateTime com base em termos de consentimento de utilizar uma afirmação de:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentDateTime" TransformationMethod="GetCurrentDateTime">
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="currentDateTime" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequired" TransformationMethod="IsTermsOfUseConsentRequired">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="termsOfUseConsentDateTime" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="termsOfUseTextUpdateDateTime" DataType="dateTime" Value="2098-01-30T23:03:45" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="result" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Segue-se um exemplo de termos de consentimento de utilizar uma afirmação de versão com base em:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetEmptyTermsOfUseConsentVersionForNewUser" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value=""/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentVersion" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value="V1"/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="compareTo" DataType="string" Value="V1" />
      <InputParameter Id="operator" DataType="string" Value="not equal" />
      <InputParameter Id="ignoreCase" DataType="string" Value="true" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations> 
```

## <a name="next-steps"></a>Passos Seguintes

- Saiba como eliminar e exportar dados de utilizador no [gerir dados do utilizador](manage-user-data.md)
