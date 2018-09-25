---
title: Azure códigos de erro autenticação e autorização do Active Directory | Documentos da Microsoft
description: Saiba mais sobre os códigos de erro AADSTS retornados do serviço de token de segurança do Azure AD (STS).
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin, justhu
ms.custom: aaddev
ms.openlocfilehash: 0643ae39d24f8ef0c1c1c18dcf73cf840c95e277
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956338"
---
# <a name="authentication-and-authorization-error-codes"></a>Códigos de erro de autenticação e autorização

À procura de informações sobre os códigos de erro AADSTS retornados do serviço de token de segurança do Azure Active Directory (Azure AD) (STS)? Ler este documento para localizar AADSTS descrições de erro, correções e algumas sugerem de soluções alternativas.

> [!NOTE]
> Estas informações são preliminares e estão sujeitas a alterações. Tem uma pergunta ou não é possível localizar o que está procurando? Criar um problema do GitHub ou veja [opções de ajuda e suporte para os desenvolvedores](active-directory-develop-help-support.md) para saber mais sobre outras formas pode obter ajuda e suporte.

## <a name="aadsts-error-codes"></a>Códigos de erro AADSTS

| Erro | Descrição |
|---|---|
| AADSTS16000 | SelectUserAccount - trata-se de uma interrupção emitida pelo Azure AD, o que resulta na interface do Usuário que permita ao usuário selecionar dentre várias sessões SSO válidos. Este erro é bastante comum e podem ser devolvido para o aplicativo se `prompt=none` está especificado. |
| AADSTS16001 | UserAccountSelectionInvalid - verá este erro se o usuário clica num mosaico que rejeitou a lógica de selecione de sessão. Quando acionado, este erro permite ao utilizador recuperar por escolha de uma lista atualizada de mosaicos/sessões ou ao selecionar outra conta. Este erro pode ocorrer devido a uma condição de defeitos ou corrida de código. |
| AADSTS16002 | Não foi cumprido AppSessionSelectionInvalid - o requisito de SID especificado de aplicação.  |
| AADSTS16003 | SsoUserAccountNotFoundInResourceTenant |
| AADSTS17003 | CredentialKeyProvisioningFailed |
| AADSTS20001 | WsFedSignInResponseError - existe um problema com o seu fornecedor de identidade federada. Contacte o seu IDP para resolver este problema. |
| AADSTS20012 | WsFedMessageInvalid - existe um problema com o seu fornecedor de identidade federada. Contacte o seu IDP para resolver este problema. |
| AADSTS20033 | FedMetadataInvalidTenantName - existe um problema com o seu fornecedor de identidade federada. Contacte o seu IDP para resolver este problema. |
| AADSTS40008 | OAuth2IdPUnretryableServerError - existe um problema com o seu fornecedor de identidade federada. Contacte o seu IDP para resolver este problema. |
| AADSTS40009 | OAuth2IdPRefreshTokenRedemptionUserError - existe um problema com o seu fornecedor de identidade federada. Contacte o seu IDP para resolver este problema. |
| AADSTS40010 | OAuth2IdPRetryableServerError - existe um problema com o seu fornecedor de identidade federada. Contacte o seu IDP para resolver este problema. |
| AADSTS40015 | OAuth2IdPAuthCodeRedemptionUserError - existe um problema com o seu fornecedor de identidade federada. Contacte o seu IDP para resolver este problema. |
| AADSTS50000 | TokenIssuanceError - existe um problema com o serviço de início de sessão. [Crie um pedido de suporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para resolver este problema. |
| AADSTS50001 | InvalidResource - o recurso está desabilitado ou não existe. Verifique o código da sua aplicação para se certificar de que especificou o URL de recurso exatamente para o recurso que está a tentar aceder.  |
| AADSTS50002 | NotAllowedTenant - início de sessão falhou devido a acesso restrito de proxy no inquilino. Se estiver em sua própria política de inquilino, pode alterar as definições de inquilino restrito para corrigir este problema. |
| AADSTS50003 | MissingSigningKey - início de sessão falhou devido à falta de certificado ou chave de assinatura. Isso poderá ser porque não havia nenhuma chave de assinatura configurado na aplicação. Veja as resoluções descritas em [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured). Se continuar a ver problemas, contacte o proprietário da aplicação ou um administrador de aplicações. |
| AADSTS50005 | DevicePolicyError - o utilizador tentou iniciar sessão dispositivo a partir de uma plataforma que não é atualmente suportada através da política de acesso condicional. |
| AADSTS50006 | InvalidSignature - verificação da assinatura falhou devido a uma assinatura inválida. |
| AADSTS50007 | PartnerEncryptionCertificateMissing - não foi encontrado o certificado de encriptação do parceiro para esta aplicação. [Abra um pedido de suporte](../fundamentals/active-directory-troubleshooting-support-howto.md) com a Microsoft para obter isso. |
| AADSTS50008 | InvalidSamlToken - asserção de SAML está em falta ou incorrectamente configurado no token. Contacte o seu fornecedor de federação. |
| AADSTS50010 | AudienceUriValidationFailed - público-alvo URI falha na validação da aplicação, uma vez que foram configuradas sem audiências de token. |
| AADSTS50011 | InvalidReplyTo - o endereço de resposta está em faltando, configurada incorretamente, ou não corresponde ao endereço de resposta configurado para a aplicação. Experimente a resolução indicada em [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Se continuar a ver problemas, contacte o proprietário da aplicação ou o administrador de aplicações. |
| AADSTS50012 | Falha na autenticação de AuthenticationFailed - para um dos seguintes motivos:<ul><li>O nome do requerente do certificado de assinatura não está autorizado</li><li>Uma política de autoridade fidedigna correspondente não foi encontrada para o nome do requerente autorizados</li><li>A cadeia de certificados não é válida</li><li>O certificado de assinatura não é válido</li><li>Política não está configurada no inquilino</li><li>Thumbprint do certificado de assinatura não está autorizado</li><li>Asserção de cliente contém uma assinatura inválida</li></ul> |
| AADSTS50013 | InvalidAssertion - asserção é inválido devido a vários motivos: O emissor de tokens não corresponde à api versão dentro de seu tempo válido no intervalo - expirado token de atualização - com formato incorreto - na asserção não é um token de atualização principal. |
| AADSTS50014 | GuestUserInPendingState |
| AADSTS50015 | ViralUserLegalAgeConsentRequiredState |
| AADSTS50017 | CertificateValidationFailed - certificação Falha ao validar razões pelos seguintes motivos:<ul><li>O certificado da emissora não foi encontrado na lista de certificados fidedignos</li><li>Não foi possível encontrar o CrlSegment esperado</li><li>O certificado da emissora não foi encontrado na lista de certificados fidedignos</li><li>O ponto de distribuição de Delta CRL está configurado sem um ponto de distribuição de CRL correspondente</li><li>Não é possível obter segmentos de CRL válidos devido a um problema de tempo de ligação excedido</li><li>Não é possível transferir a CRL</li></ul>Contacte o administrador do inquilino. |
| AADSTS50020 | UserUnauthorized - os utilizadores não estão autorizados a chamar este ponto final. |
| AADSTS50027 | InvalidJwtToken - token JWT inválido devido ao seguinte:<ul><li>Não contém a afirmação nonce, subafirmação</li><li>erro de correspondência do identificador de requerente</li><li>afirmação duplicada nas afirmações idToken</li><li>emissor inesperado</li><li>audiência inesperada</li><li>não está dentro do intervalo de tempo válido </li><li>o formato do token não é adequado</li><li>a verificação de assinatura falhou no token de ID externo do emissor</li></ul> |
| AADSTS50029 | URI inválido: o nome de domínio contém carateres inválidos. Contacte o administrador do inquilino. |
| AADSTS50032 | WeakRsaKey - indica a tentativa de utilizador incorretas que tenham para utilizar uma chave RSA fraca. |
| AADSTS50033 | RetryableError - indica um erro transitório não relacionadas com as operações de base de dados. |
| AADSTS50034 | UserAccountNotFound - para iniciar sessão nesta aplicação, a conta tem de ser adicionada ao diretório. |
| AADSTS50042 | UnableToGeneratePairwiseIdentifierWithMissingSalt - salt necessário para gerar um identificador pairwise está em falta no principal. Contacte o administrador do inquilino. |
| AADSTS50043 | UnableToGeneratePairwiseIdentifierWithMultipleSalts |
| AADSTS50048 | SubjectMismatchesIssuer - incompatibilidades de assunto emissor de afirmações na asserção de cliente. Contacte o administrador do inquilino. |
| AADSTS50049 | NoSuchInstanceForDiscovery - desconhecido ou inválido instância. |
| AADSTS50050 | MalformedDiscoveryRequest - o pedido tem um formato incorreto. |
| AADSTS50053 | IdsLocked - a conta está bloqueada porque o utilizador tentou iniciar sessão demasiadas vezes com um ID de utilizador incorretas ou a palavra-passe. |
| AADSTS50055 | InvalidPasswordExpiredPassword - a palavra-passe expirou. |
| AADSTS50056 | Palavra-passe inválido ou nulo-palavra-passe não existe no arquivo para este utilizador. |
| AADSTS50057 | UserDisabled - a conta de utilizador está desativada. A conta foi desativada por um administrador. |
| AADSTS50058 | UserInformationNotProvided - isso significa que um utilizador não estiver conectado. Este é um erro muito comum que é esperado quando um usuário não está autenticado e ainda não tem sessão iniciada no.</br>Se este erro é encorajado num contexto SSO em que o usuário apenas iniciou sessão anteriormente no, isso significa que a sessão SSO foi ou não foi encontrado ou é inválido.</br>Este erro pode ser devolvido ao aplicativo, se a linha de comandos = não for especificado nenhum. |
| AADSTS50059 | MissingTenantRealmAndNoUserInformationProvided - informações de identificação de inquilino não foi encontrada uma solicitação explícita ou implícita por quaisquer credenciais fornecidas. O utilizador pode contactar o administrador de inquilino para o ajudar a resolver o problema. |
| AADSTS50061 | SignoutInvalidRequest - pedido de fim de sessão é inválido. |
| AADSTS50064 | CredentialAuthenticationError |
| AADSTS50068 | SignoutInitiatorNotParticipant |
| AADSTS50070 | SignoutUnknownSessionIdentifier |
| AADSTS50071 | SignoutMessageExpired |
| AADSTS50072 | UserStrongAuthEnrollmentRequiredInterrupt - utilizador tem de se inscrever para autenticação de segundo fator (interativa). |
| AADSTS50074 | UserStrongAuthClientAuthNRequiredInterrupt - é necessária a autenticação forte e o utilizador não passou na submissão da MFA. |
| AADSTS50076 | UserStrongAuthClientAuthNRequired - o utilizador tem de utilizar a autenticação multifator para aceder a este recurso. Tente novamente com um novo pedido de autorizar para o recurso. |
| AADSTS50079 | UserStrongAuthEnrollmentRequired - devido a uma alteração de configuração efetuadas pelo administrador, ou porque o utilizador é movido para uma nova localização, é pedido ao utilizador para utilizar a autenticação multifator. |
| AADSTS50085 | O token de atualização precisa de início de sessão de IDP social. Pedir ao utilizador que repita o início de sessão com nome de utilizador e a palavra-passe |
| AADSTS50086 | SasNonRetryableError |
| AADSTS50087 | SasRetryableError |
| AADSTS50089 | O fluxo de token expirou - falha na autenticação. Pedir ao utilizador que repita o início de sessão com nome de utilizador e a palavra-passe |
| AADSTS50097 | DeviceAuthenticationRequired - é necessária autenticação de dispositivo. |
| AADSTS50099 | PKeyAuthInvalidJwtUnauthorized - a assinatura JWT é inválida. |
| AADSTS50105 | EntitlementGrantsNotFound - o utilizador com sessão iniciada não está atribuído a uma função para a aplicação com sessão iniciada. Atribua o utilizador para a aplicação. Para obter mais informações:[https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role). |
| AADSTS50107 | InvalidRealmUri - o objeto de realm de Federação solicitado não existe. Contacte o administrador do inquilino. |
| AADSTS50120 | ThresholdJwtInvalidJwtFormat - problema com o cabeçalho do JWT. Contacte o administrador do inquilino. |
| AADSTS50124 | ClaimsTransformationInvalidInputParameter - transformação de declarações contém parâmetro de entrada inválido. Contacte o administrador de inquilinos para atualizar a política. |
| AADSTS50125 | PasswordResetRegistrationRequiredInterrupt - início de sessão foi interrompido devido a uma reposição de palavra-passe ou a entrada de registo de palavra-passe. |
| AADSTS50126 | InvalidUserNameOrPassword - erro ao validar as credenciais devido a nome de utilizador inválido ou a palavra-passe. |
| AADSTS50127 | BrokerAppNotInstalled - utilizador tem de instalar uma aplicação de mediação para obter acesso a este conteúdo. |
| AADSTS50128 | Nome de domínio inválido - não foram encontradas informações de identificação do inquilino no pedido nem estão presentes de forma implícita em qualquer credencial fornecida|
| AADSTS50129 | DeviceIsNotWorkplaceJoined - associação à área de trabalho é necessária para registar o dispositivo. |
| AADSTS50131 | ConditionalAccessFailed - indica vários erros de acesso condicional, como o mau estado de dispositivo de Windows, pedido bloqueado devido a atividade suspeita, a política de acesso ou a decisões de política de segurança. |
| AADSTS50132 | SsoArtifactInvalidOrExpired - a sessão não é válida devido à expiração de palavra-passe ou a alteração de palavra-passe recentes. |
| AADSTS50133 | SsoArtifactRevoked - a sessão não é válida devido à expiração de palavra-passe ou a alteração de palavra-passe recentes. |
| AADSTS50134 | DeviceFlowAuthorizeWrongDatacenter |
| AADSTS50135 | PasswordChangeCompromisedPassword - alteração de palavra-passe é necessário devido ao risco de conta. |
| AADSTS50136 | RedirectMsaSessionToApp – foi detetada uma sessão única MSA. |
| AADSTS50139 | SessionMissingMsaOAuth2RefreshToken |
| AADSTS50140 | KmsiInterrupt - este erro ocorreu devido à interrupção "Manter sessão iniciada" quando o utilizador foi iniciar sessão. [Abra um pedido de suporte](../fundamentals/active-directory-troubleshooting-support-howto.md) com o ID de Correlação, o ID do Pedido e o código de Erro para obter mais detalhes. |
| AADSTS50143 | Erro de correspondência de sessão: a sessão é inválida porque o inquilino do utilizador não corresponde à sugestão de domínio devido a um recurso ser diferente. [Abra um pedido de suporte](../fundamentals/active-directory-troubleshooting-support-howto.md) com o ID de Correlação, o ID do Pedido e o código de Erro para obter mais detalhes. |
| AADSTS50144 | InvalidPasswordExpiredOnPremPassword - palavra-passe do utilizador do Active Directory expirou. Gerar uma nova palavra-passe para o utilizador ou o utilizador utilize a ferramenta de reposição de self-service para repor a palavra-passe. |
| AADSTS50146 | MissingCustomSigningKey - esta aplicação é necessária para ser configurado com uma chave de assinatura de aplicações específicas. Ou não está configurada com uma chave destas ou a chave expirou ou ainda não é válida. |
| AADSTS50147 | MissingCodeChallenge |
| AADSTS50155 | DeviceAuthenticationFailed - autenticação do dispositivo falhou para este utilizador. |
| AADSTS50158 | Não foi cumprida ExternalSecurityChallenge - desafio de segurança externas. |
| AADSTS50161 | InvalidExternalSecurityChallengeConfiguration - declarações enviadas por fornecedor externo não é suficiente ou em falta afirmação pediu para fornecedor externo. |
| AADSTS50166 | ExternalClaimsProviderThrottled - Falha ao enviar pedido para o fornecedor de afirmações. |
| AADSTS50168 | ChromeBrowserSsoInterruptRequired - o cliente é capaz de obter um token SSO através da extensão de contas do Windows 10, mas o token não foi encontrado no pedido ou o token fornecido expirou. |
| AADSTS50169 | InvalidRequestBadRealm - o realm não é um realm configurado do espaço de nomes de serviço atual. |
| AADSTS50170 | MissingExternalClaimsProviderMapping |
| AADSTS50177 | ExternalChallengeNotSupportedForPassthroughUsers - desafio externo não é suportada para os utilizadores de pass-through. |
| AADSTS50178 | SessionControlNotSupportedForPassthroughUsers - controlo de sessão não é suportada para os utilizadores de pass-through. |
| AADSTS50180 | É necessário WindowsIntegratedAuthMissing - autenticação integrada do Windows. Ativar o inquilino para SSO Totalmente Integrado. |
| AADSTS50187 | DeviceInformationNotProvided |
| AADSTS51000 | RequiredFeatureNotEnabled |
| AADSTS51001 | DomainHintMustbePresent - a sugestão de domínio tem de estar presente com o identificador de segurança no local ou o UPN no local. |
| AADSTS51004 | UserAccountNotInDirectory - a conta de utilizador não existe no diretório. |
| AADSTS51005 | TemporaryRedirect - equivalente ao estado HTTP 307, que indica que as informações pedidas estão localizadas no URI especificado no cabeçalho location. Quando receber este estado, siga o cabeçalho de localização associado com a resposta. Quando o método de pedido original foi POST, o pedido redirecionado também irá utilizar o método POST. |
| AADSTS51006 | É necessário ForceReauthDueToInsufficientAuth - autenticação integrada do Windows. Utilizador iniciado sessão com um token de sessão que está em falta a afirmação de autenticação integrada do Windows. Pedir ao utilizador para iniciar sessão novamente. |
| AADSTS52004 | DelegationDoesNotExistForLinkedIn - o utilizador não forneceu consentimento para acesso aos recursos do LinkedIn. |
| AADSTS53000 | DeviceNotCompliant - política de acesso condicional requer um dispositivo em conformidade e o dispositivo não está em conformidade. O utilizador tem de inscrever o dispositivo com um fornecedor de MDM aprovado, como o Intune. |
| AADSTS53001 | DeviceNotDomainJoined - política de acesso condicional requer um dispositivo associado ao domínio e o dispositivo não estiver associado a um domínio. Tem do utilizador utilize um domínio associado ao dispositivo. |
| AADSTS53002 | ApplicationUsedIsNotAnApprovedApp - a aplicação utilizada não é uma aplicação aprovada para o acesso condicional. Utilizador tem de utilizar uma das aplicações na lista de aplicações aprovadas para utilizar para obter acesso. |
| AADSTS53003 | BlockedByConditionalAccess - acesso foi bloqueado pelas políticas de acesso condicional. A política de acesso não permite a emissão de token. |
| AADSTS53004 | ProofUpBlockedDueToRisk - utilizador tem de concluir o processo de registo de autenticação multifator antes de aceder a este conteúdo. O utilizador deve registar-se na autenticação multifator. |
| AADSTS54000 | MinorUserBlockedLegalAgeGroupRule |
| AADSTS65001 | DelegationDoesNotExist - o utilizador ou o administrador não permitiu utilizar a aplicação com o ID X. envie um pedido de autorização interativo para este utilizador e o recurso. |
| AADSTS65004 | UserDeclinedConsent - o utilizador recusou-se dar consentimento para aceder à aplicação. Pedir ao utilizador que repita o início de sessão e que autorize o acesso à aplicação|
| AADSTS65005 | MisconfiguredApplication - a aplicação é necessário lista de acesso de recursos não contém aplicações Detetáveis pelo recurso ou a aplicação cliente pediu acesso a recursos, o que não foi especificado na sua lista de acesso de recurso necessário ou o serviço Graph devolvida incorreto pedido ou recurso não foi encontrado. Se a aplicação suporta SAML, poderá ter configurado a aplicação com o identificador (entidade) errado. Experimente a resolução indicada para SAML através da ligação abaixo: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list) |
| AADSTS67003 | ActorNotValidServiceIdentity |
| AADSTS70000 | InvalidGrant - falha na autenticação. O token de atualização não é válido. Erro pode ser devido ao seguinte:<ul><li>Cabeçalho de associação de token está vazio</li><li>Não corresponde ao hash de token de enlace</li></ul> |
| AADSTS70001 | UnauthorizedClient - a aplicação está desativada. |
| ERROR":"INVALID_GRANT","ERROR_DESCRIPTION":"AADSTS70002 | InvalidClient - erro ao validar as credenciais. O client_secret especificado não corresponde ao valor esperado para este cliente. Corrija o client_secret e tente novamente. Para mais informações, veja [utilizar o código de autorização para pedir um token de acesso](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token). |
| AADSTS70003 | UnsupportedGrantType - a aplicação devolveu um tipo de concessão não suportado. |
| AADSTS70004 | InvalidRedirectUri - a aplicação devolveu um URI de redirecionamento inválida. O endereço de redirecionamento especificado pelo cliente não corresponde a nenhum dos endereços configurados ou a nenhum dos endereços na lista de aprovações OIDC. |
| AADSTS70005 | UnsupportedResponseType - a aplicação devolveu um tipo de resposta não suportado devido ao seguinte:<ul><li>tipo de resposta "token" não está ativado para a aplicação</li><li>o tipo de resposta “id_token” requer o âmbito “OpenID”; contém um valor de parâmetro de OAuth não suportado no wctx codificado</li></ul> |
| AADSTS70007 | UnsupportedResponseMode - a aplicação devolveu um valor não suportado de `response_mode` quando solicitar um token.  |
| AADSTS70008 | O token de atualização expirou ExpiredOrRevokedGrant - devido a inatividade. O token foi emitido em XXX e estivesse inativo para um determinado período de tempo. |
| AADSTS70011 | InvalidScope - âmbito pedido pela aplicação é inválido. |
| AADSTS70012 | MsaServerError - Ocorreu um erro de servidor ao autenticar um utilizador MSA (consumidor). Tente novamente. Se continuar a falhar, [abra um pedido de suporte](../fundamentals/active-directory-troubleshooting-support-howto.md) |
| AADSTS70016 | AuthorizationPending - erro de fluxo de dispositivo do OAuth 2.0. Autorização está pendente. O dispositivo tentará novamente o pedido de consulta. |
| AADSTS70018 | BadVerificationCode - código de verificação inválida devido ao usuário digitando-se no código do usuário errado para o fluxo de código de dispositivo. Autorização não está aprovada. |
| AADSTS70019 | CodeExpired - código de verificação expirou. Peça ao utilizador repita o início de sessão. |
| AADSTS75001 | BindingSerializationError - Ocorreu um erro durante a vinculação de mensagem SAML. |
| AADSTS75003 | UnsupportedBindingError - a aplicação devolveu um erro relacionado com a ligação não suportada (resposta do protocolo SAML não pode ser enviada através de ligações que não seja o HTTP POST). |
| AADSTS75005 | Saml2MessageInvalid - Azure AD não suporta o pedido SAML enviado pela aplicação para SSO. |
| AADSTS75008 | RequestDeniedError - o pedido a partir da aplicação foi negado, uma vez que o pedido SAML tinha um destino inesperado. |
| AADSTS75011 | NoMatchedAuthnContextInOutputClaims - o método de autenticação através do qual o utilizador autenticado com o serviço não corresponde ao pedido de método de autenticação. |
| AADSTS75016 | Saml2AuthenticationRequestInvalidNameIDPolicy - pedido de autenticação SAML2 tem NameIdPolicy inválido. |
| AADSTS80001 | OnPremiseStoreIsNotAvailable - o agente de autenticação não conseguiu ligar ao Active Directory. Certifique-se de que os servidores de agente são membros da mesma floresta do AD os utilizadores cujas palavras-passe tem de ser validado e podem ligar ao Active Directory. |
| AADSTS80002 | OnPremisePasswordValidatorRequestTimedout - pedido de validação da palavra-passe foi excedido. Certifique-se de que os do Active Directory está disponível e está a responder aos pedidos dos agentes. |
| AADSTS80005 | OnPremisePasswordValidatorUnpredictableWebException - Ocorreu um erro desconhecido ao processar a resposta do agente de autenticação. Repita o pedido. Se continuar a falhar, [abra um pedido de suporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para obter mais detalhes sobre o erro. |
| AADSTS80007 | OnPremisePasswordValidatorErrorOccurredOnPrem - o agente de autenticação não conseguiu validar a palavra-passe do utilizador. Verifique os registos do agente para obter mais informações e certifique-se de que do Active Directory está a funcionar conforme esperado. |
| AADSTS80010 | OnPremisePasswordValidationEncryptionException - o agente de autenticação é não é possível desencriptar a palavra-passe. |
| AADSTS80012 | OnPremisePasswordValidationAccountLogonInvalidHours - os utilizadores tentaram iniciar sessão fora permitidos horas (isto é especificado no AD). |
| AADSTS80013 | OnPremisePasswordValidationTimeSkew - a tentativa de autenticação não foi possível concluir devido ao tempo distorção entre a máquina com o agente de autenticação e o AD. Hora de corrigir problemas de sincronização. |
| AADSTS81004 | DesktopSsoIdentityInTicketIsNotAuthenticated - falha na tentativa de autenticação de Kerberos. |
| AADSTS81005 | DesktopSsoAuthenticationPackageNotSupported - o pacote de autenticação não é suportada. |
| AADSTS81006 | DesktopSsoNoAuthorizationHeader - não foi encontrado nenhum cabeçalho de autorização. |
| AADSTS81007 | DesktopSsoTenantIsNotOptIn - o inquilino não está ativada para SSO totalmente integrado. |
| AADSTS81009 | DesktopSsoAuthorizationHeaderValueWithBadFormat - não é possível validar a permissão Kerberos do utilizador. |
| AADSTS81010 | DesktopSsoAuthTokenInvalid - o SSO integrado falhou porque a permissão Kerberos do utilizador expirou ou é inválido. |
| AADSTS81011 | DesktopSsoLookupUserBySidFailed - não é possível encontrar o objeto de utilizador com base nas informações na permissão Kerberos do utilizador. |
| AADSTS81012 | DesktopSsoMismatchBetweenTokenUpnAndChosenUpn - o utilizador tentar iniciar sessão com o Azure AD é diferente do utilizador com sessão iniciado no dispositivo. |
| AADSTS90002 | InvalidTenantName - o nome do inquilino não foi encontrado no arquivo de dados. Certifique-se de que tem o ID de inquilino correto. |
| AADSTS90004 | InvalidRequestFormat |
| AADSTS90005 | InvalidRequestWithMultipleRequirements |
| AADSTS90006 | ExternalServerRetryableError |
| AADSTS90007 | InvalidSessionId |
| AADSTS90008 | TokenForItselfRequiresGraphPermission |
| AADSTS90009 | TokenForItselfMissingIdenticalAppIdentifier |
| AADSTS90010 | NotSupported |
| AADSTS90012 | RequestTimeout |
| AADSTS90013 | InvalidUserInput |
| AADSTS90014 | MissingRequiredField - este código de erro pode aparecer em vários casos, quando um campo esperado não está presente na credencial. |
| AADSTS90015 | QueryStringTooLong |
| AADSTS90016 | MissingRequiredClaim |
| AADSTS90019 | MissingTenantRealm - Azure AD não foi possível determinar o identificador do inquilino no pedido. |
| AADSTS90022 | AuthenticatedInvalidPrincipalNameFormat - o formato de nome principal não é válido ou não cumpre a esperada `name[/host][@realm]` formato. O nome do principal não é necessário, o anfitrião e o realm são opcionais e podem ser definidos como nulo. |
| AADSTS90023 | InvalidRequest |
| AADSTS90024 | RequestBudgetExceededError |
| AADSTS90033 | MsodsServiceUnavailable |
| AADSTS90036 | MsodsServiceUnretryableFailure |
| AADSTS90038 | NationalCloudTenantRedirection - o inquilino especificado, 'Y' pertence a Cloud nacional 'X'. Instância de 'Z' não federar com o X da cloud atual. É devolvido um erro de redirecionamento de cloud. |
| AADSTS90043 | NationalCloudAuthCodeRedirection |
| AADSTS90051 | InvalidNationalCloudId |
| AADSTS90055 | TenantThrottlingError - existem demasiados pedidos recebidos. Essa exceção é lançada para inquilinos bloqueados. |
| AADSTS90056 | BadResourceRequest - para resgatar o código para um token de acesso, a aplicação deve enviar um pedido POST para o `/token` ponto final. Além disso, antes disso, deve fornecer um código de autorização e enviá-lo no pedido POST para o `/token` ponto final. Veja este artigo para obter uma descrição geral do fluxo de código de autorização de OAuth 2.0: [ https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code ](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code). Primeiro tem de direcionar o utilizador para o `/authorize` ponto final, que retornará um authorization_code. Através da publicação destacada de um pedido para o `/token` ponto final, o utilizador obtém o token de acesso. Inicie sessão no portal do Azure e verifique **registos de aplicações > pontos finais** para confirmar que os dois pontos de extremidade foram configurados corretamente. |
| AADSTS90072 | PassThroughUserMfaError |
| AADSTS90081 | OrgIdWsFederationMessageInvalid |
| AADSTS90082 | OrgIdWsFederationNotSupported |
| AADSTS90084 | OrgIdWsFederationGuestNotAllowed |
| AADSTS90085 | OrgIdWsFederationSltRedemptionFailed |
| AADSTS90086 | OrgIdWsTrustDaTokenExpired |
| AADSTS90087 | OrgIdWsFederationMessageCreationFromUriFailed |
| AADSTS90090 | GraphRetryableError |
| AADSTS90091 | GraphServiceUnreachable |
| AADSTS90092 | GraphNonRetryableError |
| AADSTS90093 | GraphUserUnauthorized - Graph devolveu com um código de erro proibido para o pedido. |
| AADSTS90094 | AdminConsentRequired |
| AADSTS90100 | InvalidRequestParameter |
| AADSTS90101 | InvalidEmailAddress |
| AADSTS90102 | InvalidUriParameter |
| AADSTS90107 | InvalidXml |
| AADSTS90114 | InvalidExpiryDate |
| AADSTS90117 | InvalidRequestInput |
| AADSTS90119 | InvalidUserCode |
| AADSTS90120 | InvalidDeviceFlowRequest |
| AADSTS90121 | InvalidEmptyRequest |
| AADSTS90123 | IdentityProviderAccessDenied |
| AADSTS90124 | V1ResourceV2GlobalEndpointNotSupported |
| AADSTS90125 | DebugModeEnrollTenantNotFound |
| AADSTS90126 | DebugModeEnrollTenantNotInferred |
| AADSTS90130 | NonConvergedAppV2GlobalEndpointNotSupported |
| AADSTS120000 | PasswordChangeIncorrectCurrentPassword |
| AADSTS120002 | PasswordChangeInvalidNewPasswordWeak |
| AADSTS120003 | PasswordChangeInvalidNewPasswordContainsMemberName |
| AADSTS120004 | PasswordChangeOnPremComplexity |
| AADSTS120005 | PasswordChangeOnPremSuccessCloudFail |
| AADSTS120008 | PasswordChangeAsyncJobStateTerminated |
| AADSTS120011 | PasswordChangeAsyncUpnInferenceFailed |
| AADSTS120012 | PasswordChangeNeedsToHappenOnPrem |
| AADSTS120013 | PasswordChangeOnPremisesConnectivityFailure |
| AADSTS120014 | PasswordChangeOnPremUserAccountLockedOutOrDisabled |
| AADSTS120015 | PasswordChangeADAdminActionRequired |
| AADSTS120016 | PasswordChangeUserNotFoundBySspr |
| AADSTS120018 | PasswordChangePasswordDoesnotComplyFuzzyPolicy |
| AADSTS120020 | PasswordChangeFailure |
| AADSTS120021 | PartnerServiceSsprInternalServiceError |
| AADSTS130004 | NgcKeyNotFound |
| AADSTS130005 | NgcInvalidSignature |
| AADSTS130006 | NgcTransportKeyNotFound |
| AADSTS130007 | NgcDeviceIsDisabled |
| AADSTS130008 | NgcDeviceIsNotFound |
| AADSTS135010 | KeyNotFound |
| AADSTS140000 | InvalidRequestNonce |
| AADSTS140001 | InvalidSessionKey |
| AADSTS165000 | InvalidRequestCanary |
| AADSTS165004 | InvalidApiCanary |
| AADSTS165900 | InvalidApiRequest |
| AADSTS220450 | UnsupportedAndroidWebViewVersion |
| AADSTS220501 | InvalidCrlDownload |
| AADSTS221000 | DeviceOnlyTokensNotSupportedByResource - o recurso não está configurado para aceitar tokens de dispositivo. |
| AADSTS240001 | BulkAADJTokenUnauthorized |
| AADSTS240002 | RequiredClaimIsMissing |
| AADSTS700020 | InteractionRequired |
| AADSTS700022 | InvalidMultipleResroucesScope |
| AADSTS700023 | InvalidResourcelessScope |
| AADSTS900000 | WebWatsonEnvironmentError |
| AADSTS1000000 | UserNotBoundError |
| AADSTS1000002 | BindCompleteInterruptError |

## <a name="next-steps"></a>Passos Seguintes

* Tem uma pergunta ou não é possível localizar o que está procurando? Criar um problema do GitHub ou veja [opções de ajuda e suporte para os desenvolvedores](active-directory-develop-help-support.md) para saber mais sobre outras formas pode obter ajuda e suporte.