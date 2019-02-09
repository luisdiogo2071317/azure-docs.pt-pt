---
title: Gestão de aplicações empresariais no Azure Active Directory de aprovisionamento de utilizadores | Documentos da Microsoft
description: Saiba como gerir o aprovisionamento da conta de utilizador para aplicações empresariais com o Azure Active Directory
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2018
ms.author: celested
ms.reviewer: asmalser
ms.openlocfilehash: ae4f92d9dc1671ab32a2a309da5095d59b9b5212
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961223"
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Gerir a conta de utilizador de aprovisionamento de aplicações empresariais no portal do Azure
Este artigo descreve como utilizar o [portal do Azure](https://portal.azure.com) para gerir a conta de utilizador automático provisionamento e desprovisionamento para aplicações que o suportam. Para saber mais sobre o aprovisionamento de contas de utilizadores automático e como ele funciona, veja [automatizar o aprovisionamento do utilizador e cancelar o aprovisionamento a aplicações SaaS com o Azure Active Directory](user-provisioning.md).

## <a name="finding-your-apps-in-the-portal"></a>Encontrar as suas aplicações no portal
Todas as aplicações que estão configuradas para início de sessão único num diretório pode ser exibida e gerenciada no [portal do Azure](https://portal.azure.com). Os aplicativos podem ser encontrados no **todos os serviços** &gt; **aplicações empresariais** seção do portal. Aplicações empresariais são as aplicações que são implementadas e utilizadas na sua organização.

![Painel de aplicações empresariais](./media/configure-automatic-user-provisioning-portal/enterprise-apps-pane.png)

Selecionar o **todos os aplicativos** ligação no lado esquerdo mostra uma lista de todas as aplicações que tenham sido configurados, incluindo aplicações que tinham sido adicionadas a partir da galeria. Selecionar uma aplicação carrega o painel de recursos para essa aplicação, onde relatórios podem ser visualizados para essa aplicação e uma variedade de definições pode ser gerenciada.

Definições de configuração de contas de utilizador podem ser geridas, selecionando **aprovisionamento** à esquerda.

![Painel de recursos de aplicação](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning.png)

## <a name="provisioning-modes"></a>Modos de aprovisionamento
O **aprovisionamento** painel começa com um **modo** menu, que mostra a quais modos de aprovisionamento são suportados para uma aplicação empresarial e permite que eles devem ser configuradas. As opções disponíveis incluem:

* **Automática** -esta opção será apresentada se o Azure AD suporta baseada em API aprovisionamento automático e/ou anular o aprovisionamento de contas de utilizador para esta aplicação. Selecionar este modo apresenta uma interface que orienta os administradores por meio de configurar o Azure AD para ligar à API de gestão de usuário do aplicativo, criação de mapeamentos de conta e fluxos de trabalho que definem a forma como os dados de conta de utilizador devem fluir entre o Azure AD e o aplicações e gerir o Azure AD que o serviço de aprovisionamento.
* **Manual** -esta opção é apresentada se o Azure AD não suporta o aprovisionamento automático de contas de utilizador para esta aplicação. Esta opção significa que os registos de conta de utilizador armazenados no aplicativo tem de ser geridos utilizando um processo externo, com base nas capacidades de gestão e aprovisionamento de utilizador fornecidas por essa aplicação (o que pode incluir o aprovisionamento de SAML ativaram).

## <a name="configuring-automatic-user-account-provisioning"></a>Configurar o aprovisionamento de conta de utilizador automático
Selecionar o **automática** opção exibe uma tela que está dividida em quatro seções:

### <a name="admin-credentials"></a>Credenciais de Administrador
Esta secção é onde as credenciais necessárias para o Azure AD ligar à gestão de utilizadores da aplicação API são introduzidos. A entrada necessária varia consoante a aplicação. Para saber mais sobre os requisitos para determinados aplicativos e tipos de credenciais, consulte a [tutorial de configuração para aquele aplicativo específico](user-provisioning.md).

Selecionar o **Testar ligação** botão permite-lhe as credenciais de teste fazendo com que do Azure AD tenta ligar-se para a aplicação do aprovisionamento de aplicação com as credenciais fornecidas.

### <a name="mappings"></a>Mapeamentos
Esta secção é onde os administradores podem ver e editar o fluxo de atributos de utilizador entre o Azure AD e o aplicativo de destino, quando as contas de utilizador são aprovisionadas ou atualizadas.

Existe um conjunto pré-configuradas de mapeamentos entre objetos de utilizador do Azure AD e objetos de utilizador de cada aplicação de SaaS. Algumas aplicações, gerir outros tipos de objetos, tais como grupos ou contactos. Selecionar um desses mapeamentos na tabela mostra o editor de mapeamento para a direita, onde podem ser visualizados e personalizados.

![Painel de recursos de aplicação](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning-mapping.png)

Personalizações suportadas incluem:

* Ativar e desativar mapeamentos para objetos específicos, como o objeto de utilizador do Azure AD para o objeto de utilizador da aplicação SaaS.
* Editar os atributos que o fluam a partir do objeto de utilizador do Azure AD para o objeto de utilizador da aplicação. Para obter mais informações sobre o mapeamento do atributo, consulte [Noções básicas sobre os tipos de mapeamento de atributos](customize-application-attributes.md#understanding-attribute-mapping-types).
* Filtre as ações de aprovisionamento do Azure AD realiza na aplicação de destino. Em vez de precisar do Azure AD totalmente sincronizar objetos, pode limitar as ações executadas. Por exemplo, ao selecionar apenas **atualização**, do Azure AD apenas as atualizações de contas num aplicativo de usuário existente e não criar novos. Ao selecionar apenas **criar**, apenas o Azure cria novas contas de utilizador mas não atualiza os existentes. Esta funcionalidade permite aos administradores criar mapeamentos diferentes para a criação de conta e atualizar os fluxos de trabalho.

### <a name="settings"></a>Definições
Esta secção permite aos administradores iniciar e parar o Azure AD para a aplicação selecionada, o serviço de aprovisionamento, bem como, opcionalmente, limpar a cache de aprovisionamento e reinicie o serviço.

Se o aprovisionamento está a ser ativado pela primeira vez para uma aplicação, ative o serviço, alterando a **estado de aprovisionamento** ao **no**. Esta alteração faz com que o Azure AD do serviço de aprovisionamento efetuar uma sincronização inicial, onde ele lê os utilizadores atribuídos a **utilizadores e grupos** secção, consulta o aplicativo de destino para os mesmos e, em seguida, executa as ações de aprovisionamento definido no Azure AD **mapeamentos** secção. Durante este processo, o serviço de aprovisionamento armazena em cache dados sobre as contas de utilizador que está a gerir, pelo que não gerida contas dentro as aplicações de destino que foram nunca no âmbito de atribuição não são afetadas pelo Desconfiguração de operações. Após a sincronização inicial, o serviço de aprovisionamento sincroniza automaticamente os objetos de utilizador e grupo num intervalo de dez minutos.

Alterar o **estado de aprovisionamento** ao **desativar** simplesmente interrompe o serviço de aprovisionamento. Neste estado, Azure não criar, atualizar ou remover qualquer utilizador ou os objetos de grupo na aplicação. Alterar o estado de volta para o on faz com que o serviço para começar onde terminou.

Selecionar o **Limpar estado atual e reiniciar a sincronização** caixa de verificação e ao salvamento para o serviço de aprovisionamento, despejos, os dados em cache sobre as contas do Azure AD está a gerir, reinicia os serviços e executa inicial sincronização novamente. Esta opção permite aos administradores iniciar o processo de implantação aprovisionamento novamente.

### <a name="synchronization-details"></a>Detalhes da Sincronização
Esta seção fornece detalhes adição da operação do serviço de aprovisionamento, incluindo os tempos de primeiros e últimos que serviço de aprovisionamento, executou a aplicação e o número de utilizadores e objetos de grupo estão a ser geridos.

São fornecidas hiperligações para o **relatório de atividade de aprovisionamento** que fornece um registo de todos os utilizadores e grupos criados, atualizado e removido entre o Azure AD e o aplicativo de destino e para o **derelatóriodeerrosdeaprovisionamento** que fornece mais mensagens de erro para o utilizador e grupo de objetos que falharam ao ser de leitura, criado, atualizado ou removido. 



