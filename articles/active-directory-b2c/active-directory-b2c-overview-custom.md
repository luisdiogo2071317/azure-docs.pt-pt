---
title: Políticas personalizadas do Active Directory B2C do Azure | Documentos da Microsoft
description: Saiba mais sobre as políticas personalizadas do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 0724227da425eb2faeee9ac4ae8449782e62a241
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445988"
---
# <a name="azure-active-directory-b2c-custom-policies"></a>Do Azure Active Directory B2C: As políticas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="what-are-custom-policies"></a>O que são políticas personalizadas?

As políticas personalizadas são ficheiros de configuração que definem o comportamento do seu inquilino do Azure AD B2C. Enquanto **políticas incorporadas** estão predefinidas no portal do Azure AD B2C para as tarefas mais comuns de identidade, as políticas personalizadas podem ser totalmente editadas por um programador de identidades para concluir um número quase ilimitado de tarefas. Continue a ler para determinar se as políticas personalizadas são adequados para si e o seu cenário de identidade.

## <a name="comparing-built-in-policies-and-custom-policies"></a>Comparação entre as políticas incorporadas e as políticas personalizadas

| | Políticas incorporadas | Políticas personalizadas |
|-|-------------------|-----------------|
|Utilizadores de destino | Todos os programadores de aplicações com ou sem conhecimentos de identidade | Os profissionais de identidade: integradores de sistemas, consultores e equipes internas de identidade. Eles se sente confortáveis OpenIDConnect fluxos e compreender os fornecedores de identidade e autenticação baseada em afirmações |
|Método de configuração | Portal do Azure com uma interface do Usuário amigável de utilizador | Editar diretamente os arquivos XML e, em seguida, carregar para o portal do Azure |
|Personalização da interface do Usuário | Personalização da interface do Usuário total, incluindo suporte a HTML, CSS e javascript (requer o domínio personalizado)<br><br>Suporte a vários idiomas com cadeias de caracteres personalizado | Mesmo |
| Personalização de atributo | Atributos padrão e personalizados | Mesmo |
|Gestão de token e de sessão | Token personalizado e várias opções de sessão | Mesmo |
|Fornecedores de Identidade| **Hoje em dia**: provedor predefinido de local de redes sociais<br><br>**Futuro**: OIDC baseada em normas, SAML, OAuth | **Hoje em dia**: OIDC baseada em normas, OAUTH, SAML<br><br>**Futuro**: WsFed |
|Tarefas de identidade (exemplos) | Inscrição ou início de sessão com o local e várias contas de redes sociais<br><br>Reposição Personalizada de Palavra-passe<br><br>Edição de perfil<br><br>Cenários de autenticação Multifatores<br><br>Personalizar Tokens e de sessões<br><br>Fluxos de Token de acesso | Conclua as mesmas tarefas como as políticas incorporadas usando provedores de identidade personalizada ou utilizar âmbitos de personalizado<br><br>Utilizador de aprovisionar em outro sistema no momento de registo<br><br>Enviar um e-mail de boas-vindos através de seu próprio fornecedor de serviços de e-mail<br><br>Utilize um arquivo de utilizador fora do B2C<br><br>Validar o utilizador fornecido informações com um sistema confiável por meio da API |

## <a name="policy-files"></a>Ficheiros de política

Uma política personalizada é representada como um ou vários ficheiros de formatada em XML que fazem referência entre si de uma cadeia de hierárquica. Definem os elementos XML: o esquema de afirmações, as transformações, definições de conteúdo, perfis de technical/fornecedores de afirmações e passos de orquestração de percurso do utilizador, entre outros elementos de afirmações.

Recomendamos a utilização dos três tipos de ficheiros de política:

- **Um ficheiro BASE**, que contém a maioria das definições de e para que o Azure fornece um exemplo completo.  Recomendamos que faça um número mínimo de alterações a este ficheiro para o ajudar a resolver problemas de mensagens em fila e a manutenção a longo prazo das suas políticas
- **um arquivo de extensões** que contém as alterações de configuração exclusiva para o seu inquilino
- **um ficheiro de terceiros entidade Confiadora (RP)** ou seja o único centrados na tarefa ficheiro que é invocado diretamente pela aplicação ou serviço (também conhecido como terceira parte confiável).  Leia o artigo sobre as definições de ficheiro de política para obter mais informações.  Cada tarefa exclusiva requer seu próprio RP e consoante os requisitos de imagem corporativa, o número pode ser "total de aplicativos x o número total de casos de utilização."


Políticas incorporadas no Azure AD B2C seguem o padrão de arquivo de três descrito acima, mas o desenvolvedor só vê o ficheiro de terceiros entidade Confiadora (RP), enquanto o portal faz alterações em segundo plano para o arquivo de extensões.

## <a name="core-concepts-you-should-know-when-using-custom-policies"></a>Conceitos-chave que deve saber quando utilizar políticas personalizadas

### <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

Ao cliente acesso e identidades (CIAM) serviço de gestão do Azure. O serviço inclui:

1. Um diretório de utilizador na forma de uma finalidade especial do Azure Active Directory acessíveis através do Microsoft Graph e que contém dados de utilizador para contas locais e contas federadas 
2. Acesso para o **arquitetura de experiências de identidade** que orquestra a confiança entre utilizadores e entidades e transmite declarações entre eles para concluir uma tarefa de gestão de identidade/acesso 
3. Um token serviço de segurança (STS) emissora ID tokens, atualização de tokens e acesso tokens (e equivalentes asserções de SAML) e validá-las para proteger os recursos.

O Azure AD B2C interage com fornecedores de identidade, os utilizadores, outros sistemas e com o diretório de utilizador local em seqüência para alcançar uma tarefa de identidade (por exemplo início de sessão um utilizador registar um novo utilizador, repor uma palavra-passe). A plataforma subjacente, que estabelece a fidedignidade de múltiplos intervenientes e executa estes passos denomina-se a arquitetura de experiências de identidade e uma política (também denominada um percurso do utilizador ou uma política de estrutura de confiança) define explicitamente os atores, as ações, os protocolos e o sequência de passos para concluir.

### <a name="identity-experience-framework"></a>Estrutura de Experiência de Identidade

Formatos de uma plataforma do Azure totalmente configurável, orientada por diretivas, com base na cloud que orquestra a confiança entre entidades (amplamente fornecedores de afirmações) no protocolo padrão como OpenIDConnect, OAuth, SAML, WSFed e algumas que não padrão (por exemplo, REST Trocas de afirmações de API-based sistema para sistema). O I2E cria amigáveis com etiqueta white experiências que oferecem suporte a HTML, CSS e javascript.  Hoje em dia, o Framework de experiência de identidade está disponível apenas no contexto do serviço do Azure AD B2C e priorizado para tarefas relacionadas com a CIAM.

### <a name="built-in-policies"></a>Políticas incorporadas

Predefinidas configuração ficheiros orientar o comportamento do Azure AD B2C para efetuar mais comumente usados tarefas de identidade (ou seja, registro de usuário, de início de sessão, reposição de palavra-passe) e interagirem com as pessoas confiáveis cuja relação também é predefinida no (Azure AD B2C Por exemplo Facebook fornecedor de identidade, LinkedIn, Microsoft Account, contas do Google).  No futuro, também podem fornecer políticas incorporadas para personalização de fornecedores de identidade que normalmente se encontram no campo empresarial, como o Azure Active Directory Premium, do Active Directory/ADFS, Salesforce ID fornecedor etc.


### <a name="custom-policies"></a>Políticas personalizadas

Ficheiros de configuração que definem o comportamento do Framework de experiência de identidade no seu inquilino do Azure AD B2C. Uma política personalizada é acessível como um ou vários arquivos XML (consulte as definições de política de ficheiros) que são executados pela estrutura de experiência de identidade quando invocado por uma terceira parte confiável (por exemplo, um aplicativo). As políticas personalizadas podem ser editadas diretamente por um programador de identidades para concluir um número quase ilimitado de tarefas. Os desenvolvedores configurar as políticas personalizadas tem de definir as relações fidedignas em detalhe cuidado para incluir pontos de extremidade de metadados, afirmações exatas definições do exchange e configurar certificados, chaves e segredos conforme necessário por cada provedor de identidade.

## <a name="policy-file-definitions-for-identity-experience-framework-trust-frameworks"></a>Definições de ficheiro de política para estruturas de confiança de arquitetura de experiências de identidade

### <a name="policy-files"></a>Ficheiros de política

Uma política personalizada é representada como um ou vários ficheiros de formatada em XML que fazem referência entre si de uma cadeia de hierárquica. Definem os elementos XML: o esquema de afirmações, as transformações, definições de conteúdo, perfis de technical/fornecedores de afirmações e passos de orquestração de percurso do utilizador, entre outros elementos de afirmações.  Recomendamos a utilização dos três tipos de ficheiros de política:

- **Um ficheiro BASE** contém a maioria das definições de e para que o Azure fornece um exemplo completo.  Recomendamos que faça um número mínimo de alterações a este ficheiro para o ajudar a resolver problemas de mensagens em fila e a manutenção a longo prazo das suas políticas
- **um arquivo de extensões** que contém as alterações de configuração exclusiva para o seu inquilino
- **um ficheiro de terceiros entidade Confiadora (RP)** é o único ficheiro centrados na tarefa que é invocado diretamente pela aplicação ou serviço (também conhecido como terceira parte confiável).  Leia o artigo sobre as definições de ficheiro de política para obter mais informações.  Cada tarefa exclusiva requer seu próprio RP e consoante os requisitos de imagem corporativa, o número pode ser "total de aplicativos x o número total de casos de utilização".

![Tipos de ficheiro de política](media/active-directory-b2c-overview-custom/active-directory-b2c-overview-custom-policy-files.png)

| Tipo de ficheiro de política | Nome de ficheiro de exemplos | Utilização recomendada | Herda de |
|---------------------|--------------------|-----------------|---------------|
| BASE |TrustFrameworkBase.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_BASE1.xml | Inclui o esquema de afirmações central, transformações de afirmações, fornecedores de afirmações e jornadas de utilizador configuradas pela Microsoft<br><br>Fizesse alterações mínimas para este ficheiro | Nenhuma |
| Extensão (EXT) | TrustFrameworkExtensions.xml<br><br>Mytenant.onmicrosoft.com-B2C-1A_EXT.xml | Consolidar as alterações para o ficheiro de BASE aqui<br><br>Fornecedores de afirmações modificado<br><br>Jornadas de utilizador modificado<br><br>Suas próprias definições de esquema personalizado | Ficheiro de BASE |
| Terceira parte confiável (RP) | B2C_1A_sign_up_sign_in.xml| Alterar token forma e sessão definições aqui| Ficheiro de Extensions(Ext) |

### <a name="inheritance-model"></a>Modelo de herança

Quando um aplicativo chama o arquivo de diretiva da RP, o Framework de experiência de identidade no B2C irá adicionar todos os elementos de BASE, em seguida, das EXTENSÕES e por fim do ficheiro de política RP para montar a política atual em vigor.  Elementos do mesmo tipo e nome no ficheiro de RP serão substituem as EXTENSÕES, e as EXTENSÕES substitui BASE.

**Políticas incorporadas** no Azure AD B2C seguem o padrão de 3-ficheiro descrito acima, mas o desenvolvedor só vê o ficheiro de terceiros entidade Confiadora (RP), enquanto o portal faz alterações em segundo plano para o ficheiro de extensões.  Todas as do Azure AD B2C partilha um ficheiro de política BASE que está sob o controlo da equipa do Azure B2C e é atualizado frequentemente.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Introdução às políticas personalizadas](active-directory-b2c-get-started-custom.md)
