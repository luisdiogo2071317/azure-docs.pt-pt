---
title: Códigos de erro dos relatórios de atividades de início de sessão no portal do Azure Active Directory | Microsoft Docs
description: Referência aos códigos de erro dos relatórios de atividades de início de sessão.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: dc01a775579455ae24c95ecc6f3858ce28149dea
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232448"
---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Códigos de erro dos relatórios de atividades de início de sessão no portal do Azure Active Directory

Com as informações fornecidas pelo relatório de inícios de sessão de utilizador, encontrará respostas a perguntas como:

- Quem iniciou sessão com o Azure Active Directory?
- Em que aplicações foi iniciada sessão?
- Que inícios de sessão falharam e porquê?

Este artigo lista os códigos de erro e as descrições associadas. 

## <a name="how-can-i-display-failed-sign-ins"></a>Como posso apresentar inícios de sessão com falha? 

O primeiro ponto de entrada de todos os dados de atividades de início de sessão é **[Inícios de sessão](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)**, na secção de **Atividade** do **Azure Active**.


![Atividade de início de sessão](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Atividade de início de sessão")

Nos relatórios de inícios de sessão, pode apresentar todos os inícios de sessão com falha ao selecionar **Falha** como o **Estado do início de sessão**.

![Atividade de início de sessão](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Atividade de início de sessão")

Clicar num item na lista apresentada abre o painel **Detalhes da Atividade: inícios de sessão**. Esta vista mostra-lhe todos os detalhes que o Azure Active Directory monitoriza relativamente a inícios de sessão, incluindo o **código de erro do início de sessão** e um **motivo para a falha**.

![Atividade de início de sessão](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Atividade de início de sessão")


Como alternativa à utilização do portal do Azure para aceder aos dados de inícios de sessão, também pode utilizar a [API de relatórios](active-directory-reporting-api-getting-started-azure-portal.md).


A secção seguinte proporciona-lhe uma descrição geral completa de todos os possíveis erros e as descrições associadas. 

## <a name="error-codes"></a>Códigos de erro


|Erro|Descrição|
|---|---|
|20001|Há um problema com o seu Fornecedor de Identidade federado. Contacte o seu IDP para resolver este problema.|
|20012|Há um problema com o seu Fornecedor de Identidade federado. Contacte o seu IDP para resolver este problema.|
|20033|Há um problema com o seu Fornecedor de Identidade federado. Contacte o seu IDP para resolver este problema.|
|40008|Há um problema com o seu Fornecedor de Identidade federado. Contacte o seu IDP para resolver este problema.|
|40009|Há um problema com o seu Fornecedor de Identidade federado. Contacte o seu IDP para resolver este problema.|
|40014|Há um problema com o seu Fornecedor de Identidade federado. Contacte o seu IDP para resolver este problema.|
|50000|Há um problema no nosso serviço de início de sessão. [Crie um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) para resolver este problema.|
|50001|O nome do principal de serviço não foi encontrado neste inquilino. Isto pode acontecer se o administrador do inquilino não tiver instalado a aplicação. Ou se o principal do Recurso não tiver sido encontrado no diretório ou for inválido.|
|50002|O início de sessão falhou devido a acesso limitado ao proxy no inquilino. Se for a política do seu próprio inquilino, pode alterar as definições restritas do mesmo para corrigir o problema.|
|50003|O início de sessão falhou devido à falta da chave de assinatura ou do certificado. Isto poderá dever-se ao facto de não existir nenhuma chave de assinatura configurada na aplicação. Veja as resoluções descritas em [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured). Se continuar a encontrar problemas, contacte o proprietário ou o administrador da aplicação.|
|50005|O utilizador tentou iniciar sessão num dispositivo a partir de uma plataforma que não é atualmente suportada através da política de acesso condicional|
|50006| A verificação da assinatura falhou devido a assinatura inválida. Veja a resolução descrita em [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery). Se continuar a encontrar problemas, contacte o proprietário ou o administrador da aplicação.|
|50007|O certificado de encriptação do parceiro não foi encontrado para esta aplicação. [Abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) junto da Microsoft para resolver esse problema.|
|50008|A asserção SAML está em falta ou não está devidamente configurada no token. Contacte o seu fornecedor de federação.|
|50010|A validação do URI de audiência da aplicação falhou, pois não foram configuradas audiências de token. Contacte o proprietário da aplicação.|
|50011|O endereço de resposta está em falta, está mal configurado ou não corresponde aos endereços de resposta configurados para a aplicação. Experimente a resolução indicada em [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Se continuar a encontrar problemas, contacte o proprietário ou o administrador da aplicação.|
|50013|A asserção é inválida por diversos motivos: o emissor do token não corresponde à versão da api dentro do intervalo de tempo válido; expirado; mal formatado; o token de atualização da asserção não é um token de atualização principal.|
|50017|A validação da certificação falhou devido a um dos seguintes motivos:<ul><li>O certificado da emissora não foi encontrado na lista de certificados fidedignos</li><li>Não foi possível encontrar o CrlSegment esperado</li><li>O certificado da emissora não foi encontrado na lista de certificados fidedignos</li><li>O ponto de distribuição de Delta CRL está configurado sem um ponto de distribuição de CRL correspondente</li><li>Não é possível obter segmentos de CRL válidos devido a um problema de tempo de ligação excedido</li><li>Não é possível transferir a CRL</li></ul>Contacte o administrador do inquilino.|
|50020|O utilizador não está autorizado; não é possível emitir tokens devido a problema de versão; o nome do emissor não foi especificado; problemas com o nome do emissor (nulo; comprimento máximo). Contacte o proprietário da aplicação.|
|50027|Token JWT inválido devido aos seguintes motivos:<ul><li>Não contém a afirmação nonce, subafirmação</li><li>erro de correspondência do identificador de requerente</li><li>afirmação duplicada nas afirmações idToken</li><li>emissor inesperado</li><li>audiência inesperada</li><li>não está dentro do intervalo de tempo válido </li><li>o formato do token não é adequado</li><li>a verificação de assinatura falhou no token de ID externo do emissor</li></ul>Contacte o proprietário da aplicação.|
|50029|URI inválido - o nome de domínio contém carateres inválidos. Contacte o administrador do inquilino.|
|50034|O utilizador não existe no diretório. Contacte o administrador do inquilino.|
|50042|O salt necessário para gerar um identificador em pares está em falta no principal. Contacte o administrador do inquilino.|
|50048|O requerente não corresponde à afirmação do Emissor na asserção do cliente. Contacte o administrador do inquilino.|
|50050|O formato do pedido é incorreto. Contacte o proprietário da aplicação.|
|50053|A aplicação foi bloqueada porque o utilizador tentou iniciar sessão demasiadas vezes com um ID de utilizador ou uma palavra-passe inválida.|
|50055|Palavra-passe inválida; palavra-passe expirada introduzida.|
|50056|Palavra-passe inválida ou nula; a palavra-passe não existe na loja para este utilizador|
|50057|A conta de utilizador está desativada. A conta foi desativada por um administrador.|
|50058|A aplicação tentou executar um início de sessão automático e não foi possível iniciar a sessão do utilizador de forma silenciosa. A aplicação tem de iniciar um fluxo interativo, concedendo aos utilizadores a opção para iniciar sessão. Contacte o proprietário da aplicação.|
|50059|O utilizador não existe no diretório. Contacte o administrador do inquilino.|
|50061|O pedido de fim de sessão é inválido. Contacte o proprietário da aplicação.|
|50072|O utilizador tem de se inscrever na autenticação de segundo fator (interativa)|
|50074|O utilizador não passou na submissão da MFA.|
|50076|O utilizador não passou na submissão da MFA (não interativa)|
|50079|O utilizador tem de se inscrever na autenticação de segundo fator (inícios de sessão não interativos)|
|50085|O token de atualização precisa de início de sessão de IDP social. Pedir ao utilizador que repita o início de sessão com nome de utilizador e a palavra-passe|
|50089|O fluxo de token expirou - falha na autenticação. Pedir ao utilizador que repita o início de sessão com nome de utilizador e a palavra-passe|
|50097|A autenticação do dispositivo é necessária; as afirmações DeviceId e DeviceAltSecId são nulas OU não existe nenhum dispositivo que corresponda ao identificador do dispositivo|
|50099|A assinatura JWT é inválida. Contacte o proprietário da aplicação.|
|50105|O utilizador com sessão iniciada não tem uma função atribuída na aplicação em que a sessão está iniciada. Atribua o utilizador à aplicação. Para obter mais informações: [https://docs.microsoft.com/en-us/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/en-us/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|O objeto de realm de federação pedido não existe. Contacte o administrador do inquilino.|
|50120|Emitir com o cabeçalho JWT. Contacte o administrador do inquilino.|
|50124|A Transformação de Afirmações contém um parâmetro de entrada inválido. Contacte o administrador de inquilinos para atualizar a política.|
|50125|O início de sessão foi interrompido devido a uma reposição de palavra-passe ou entrada de registo de palavra-passe|
|50126|O nome de utilizador ou a palavra-passe é inválida ou o nome de utilizador ou a palavra-passe no local é inválida.|
|50127|O utilizador precisa de instalar uma aplicação de mediador para obter acesso a este conteúdo.|
|50128|Nome de domínio inválido - não foram encontradas informações de identificação do inquilino no pedido nem estão presentes de forma implícita em qualquer credencial fornecida|
|50129|O dispositivo não está associado à área de trabalho - a Associação à área de trabalho é necessária para registar o dispositivo.|
|50130|Não é possível interpretar o valor da afirmação como um método de autenticação conhecido|
|50131|Utilizado em vários erros de acesso condicional. Por exemplo, Mau estado do dispositivo Windows, pedido bloqueado devido a atividades suspeitas, decisões de políticas de acesso e de políticas de segurança.|
|50132|As credenciais foram revogadas devido aos seguintes motivos:<ul><li>O artefacto de SSO é inválido ou expirou</li><li>A sessão não é suficientemente nova para a aplicação</li><li>Foi enviado um início de sessão silencioso, mas a sessão do utilizador com o Azure AD é inválida ou expirou.</li></ul>|
|50133|A sessão é inválida devido à palavra-passe ter expirado ou ter sido alterada recentemente.|
|50135|É necessário alterar a palavra-passe, porque a conta está em risco|
|50136|Redirecionar sessão de msa para a aplicação – foi detetada uma sessão única de MSA |
|50140|Este erro ocorreu devido à interrupção de "Manter sessão iniciada" quando o utilizador estava a iniciar sessão. [Abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) com o ID de Correlação, o ID do Pedido e o código de Erro para obter mais detalhes. |
|50143|Erro de correspondência de sessão - a sessão é inválida porque o inquilino do utilizador não corresponde à sugestão de domínio devido a um recurso ser diferente. [Abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) com o ID de Correlação, o ID do Pedido e o código de Erro para obter mais detalhes.|
|50144|A palavra-passe do Active Directory do utilizador expirou. Gerar uma palavra-passe nova para o utilizador ou pedir ao utilizador final que utilize a ferramenta de reposição personalizada de palavras-passe|
|50146|Esta aplicação tem de ser configurada com uma chave de assinatura específica da aplicação. Ou não está configurada com uma chave destas ou a chave expirou ou ainda não é válida. Contacte o proprietário da aplicação.|
|50148|O code_verifier não corresponde ao code_challenge fornecido no pedido de autorização do PKCE. Contacte o programador da aplicação. |
|50155|A autenticação do dispositivo para este utilizador falhou|
|50158|O desafio de segurança externo não foi cumprido|
|50161|As afirmações enviadas pelo fornecedor externo não são suficientes ou a afirmação que foi pedida ao fornecedor externo está em falta|
|50166|Falha ao enviar pedidos para o fornecedor de afirmações|
|50169|O realm não é um realm configurado do espaço de nomes do serviço atual.|
|50172|O fornecedor de afirmações externo não foi aprovado. Contacte o administrador do inquilino.|
|50173|É necessário um token de autenticação atualizado. Pedir ao utilizador que volte a iniciar sessão com credenciais novas|
|50177|O desafio externo não é suportado para utilizadores de pass-through|
|50178|O Controlo de Sessão não é suportado para utilizadores de pass-through|
|50180|É necessária a Autenticação Integrada do Windows Ativar o inquilino para SSO Totalmente Integrado.|
|51001|A Sugestão de Domínio não está presente com o Identificador de Segurança no Local - UPN no Local|
|51004|A conta de utilizador não existe no diretório.|
|51006|É necessária a Autenticação Integrada do Windows O utilizador iniciou sessão com um token de sessão no qual a afirmação wia está em falta. Pedir ao utilizador que volte a iniciar sessão.|
|52004|O utilizador não deu consentimento para acesso a recursos do LinkedIn. |
|53000|A política de acesso condicional requer um dispositivo em conformidade e o dispositivo não está em conformidade. Pedir ao utilizador que inscreva o dispositivo dele num fornecedor de MDM aprovado, como o Intune|
|53001|A política de acesso condicional requer um dispositivo associado a um domínio, o que não é o caso deste dispositivo. Pedir ao utilizador que utilize um dispositivo associado a um domínio|
|53002|A aplicação utilizada não é uma aplicação aprovada para acesso condicional. Para obter acesso, o utilizador tem de utilizar uma das aplicações na lista de aplicações aprovadas.|
|53003|O acesso foi bloqueado devido a políticas de acesso condicional.|
|53004|O utilizador tem de concluir o processo de registo na autenticação multifator antes de aceder a este conteúdo. O utilizador deve registar-se na autenticação multifator.|
|65001|A aplicação X não tem permissão para aceder à aplicação Y ou a permissão foi revogada. Ou o utilizador ou o administrador não permitiu utilizar a aplicação com o ID X. Envie um pedido de autorização interativo para este utilizador e este recurso. Ou o utilizador ou o administrador não permitiu utilizar a aplicação com o ID X. Envie um pedido de autorização ao administrador do seu inquilino para agir em nome da Aplicação :Y para o Recurso : Z.|
|65004|O utilizador recusou dar autorização para aceder à aplicação. Pedir ao utilizador que repita o início de sessão e que autorize o acesso à aplicação|
|65005|A lista de acesso a recursos necessária para a aplicação não contém aplicações detetáveis pelo recurso ou a aplicação cliente pediu acesso a um recurso que não foi especificado nesta lista de acesso a recursos necessária ou o serviço Graph devolveu um pedido inválido ou o recurso não foi encontrado. Se a aplicação suportar SAML, poderá ter configurado a aplicação com o identificador (Entidade) incorreto. Experimente a resolução indicada para SAML através da ligação abaixo: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list)|
|70000|Concessão inválida devido aos seguintes motivos:<ul><li>A asserção de SAML 2.0 pedida tem um Método de Confirmação de Requerente inválido</li><li>O fluxo OnBehalfOf da aplicação não é suportado em V2</li><li>O token de atualização principal não está assinado com a chave de sessão</li><li>Token de atualização externo inválido</li><li>A concessão de acesso foi obtida para outro inquilino.</li></ul>|
|70001|A aplicação com o nome X não foi encontrada no inquilino com o nome Y. Este erro pode acontecer se a aplicação com o identificador X não tiver sido instalada pelo administrador do inquilino ou não tiver sido permitida por qualquer utilizador do inquilino. Poderá ter configurado incorretamente o valor do Identificador da aplicação ou enviado o pedido de autenticação para o inquilino errado|
|70002|A aplicação devolveu credenciais de cliente inválidas. Contacte o proprietário da aplicação.|
|70003|A aplicação devolveu um tipo de concessão não suportada. Contacte o proprietário da aplicação.|
|70004|A aplicação devolveu um URI de redirecionamento inválido. O endereço de redireccionamento especificado pelo cliente não corresponde a nenhum dos endereços configurados ou a nenhum dos endereços na lista de aprovações OIDC. Contacte o proprietário da aplicação.|
|70005|A aplicação devolveu um tipo de resposta não suportada devido aos seguintes motivos:<ul><li>o tipo de resposta “token” não está ativado na aplicação</li><li>o tipo de resposta “id_token” requer o âmbito “OpenID”; contém um valor de parâmetro de OAuth não suportado no wctx codificado</li></ul>Contacte o proprietário da aplicação.|
|70007|A aplicação devolveu um valor não suportado de “response_mode” durante um pedido de token. Contacte o proprietário da aplicação.|
|70008|O código de autorização ou o token de atualização fornecido expirou ou foi revogado. Pedir ao utilizador que inicie sessão|
|70011|O âmbito que a aplicação pediu é inválido. Contacte o proprietário da aplicação.|
|70012|Ocorreu um erro de servidor ao autenticar um utilizador MSA (consumidor). Tente novamente. Se continuar a falhar, [abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) |
|70018|Código de verificação inválido devido ao Utilizador ter introduzido o código de utilizador errado no fluxo de códigos do dispositivo. A autorização não foi aprovada|
|70019|O código de verificação expirou. Pedir ao utilizador que volte a inicie sessão|
|70037|Foi fornecida uma resposta errada para o desafio. A sessão de autenticação remota foi recusada.|
|75001|Ocorreu um erro durante o enlace de mensagem SAML.|
|75003|A aplicação devolveu um erro relacionado com o Enlace não suportado (a resposta do protocolo SAML não pode ser enviada através de enlaces que não HTTP POST). Contacte o proprietário da aplicação.|
|75005|O Azure AD não suporta o Pedido SAML enviado pela aplicação para Início de Sessão Único. Contacte o proprietário da aplicação.|
|75008|O pedido da aplicação foi recusado porque o pedido SAML tinha um destino inesperado. Contacte o proprietário da aplicação.|
|75011|O método de autenticação através do qual o utilizador se autenticou no serviço não corresponde ao método de autenticação pedido. Contacte o proprietário da aplicação.|
|75016|O Pedido de Autenticação SAML2 tem uma NameIdPolicy inválida. Contacte o proprietário da aplicação.|
|80001|O Agente de Autenticação não se consegue ligar ao Active Directory. Confirme que o agente de autenticação está instalado num computador associado a um domínio que tenha linha de visão para um controlador de domínio que possa servir o pedido de início de sessão do utilizador.|
|80002|Erro interno. O pedido de validação da palavra-passe atingiu o tempo limite. Não foi possível enviar o pedido de autenticação para o serviço de identidade híbrido interno. [Abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) para obter mais detalhes sobre o erro.|
|80003|O Agente de Autenticação recebeu uma resposta inválida. Ocorreu um erro desconhecido ao tentar autenticar no Active Directory no local. [Abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) para obter mais detalhes sobre o erro.|
|80005|Agente de Autenticação: ocorreu um erro desconhecido ao processar a resposta do Agente de Autenticação. [Abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) para obter mais detalhes sobre o erro.|
|80007|O Agente de Autenticação não conseguiu validar a palavra-passe do utilizador.|
|80010|O Agente de Autenticação não conseguiu desencriptar a palavra-passe. |
|80011|O Agente de Autenticação não conseguiu obter a chave de desencriptação.|
|80012|Os utilizadores tentaram iniciar sessão fora do horário permitido (que é especificado no AD)|
|80013|Não foi possível concluir a tentativa de autenticação devido a um desfasamento de horas entre o computador que está a executar o agente de autenticação e o AD. Corrija os problemas de sincronização de hora.|
|80014|O agente de autenticação excedeu o tempo limite. [Abra um pedido de suporte](fundamentals/active-directory-troubleshooting-support-howto.md) com o código de erro, o ID de correlação e a Data/hora para obter mais detalhes sobre este erro.|
|81001|A permissão do Kerberos do utilizador é demasiado grande. Isto pode acontecer se o utilizador estiver em demasiados grupos e, por conseguinte, a permissão do Kerberos contém demasiadas adesões a grupos. Reduza as adesões a grupos do utilizador e tente novamente.|
|81005|O Pacote de autenticação não é suportado.|
|81007|O inquilino não está ativado para SSO Totalmente Integrado|


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, veja os [relatórios de atividades de início de sessão no portal do Azure Active Directory](active-directory-reporting-activity-sign-ins.md).
