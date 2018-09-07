---
title: Descrição geral da segurança no Data Lake Store | Documentos da Microsoft
description: Compreender como o Azure Data Lake Store é um armazenamento de grandes volumes de dados mais seguro
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 3b836859dd2af45e84d0f53db06d2c7ab2828253
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057166"
---
# <a name="security-in-azure-data-lake-store"></a>Segurança no Azure Data Lake Store
Muitas empresas estão a tirar partido da análise de macrodados para as informações de negócio para os ajudar a tomar decisões inteligentes. Uma organização pode ter um ambiente complexo e regulado, com um número crescente de diversos usuários. É fundamental para uma empresa para se certificar de que a dados empresariais críticos são armazenados de forma mais segura, com o nível correto de acesso concedido a utilizadores individuais. Azure Data Lake Store foi concebido para ajudar a satisfazer esses requisitos de segurança. Neste artigo, saiba mais sobre as capacidades de segurança do Data Lake Store, incluindo:

* Autenticação
* Autorização
* Isolamento de rede
* Proteção de dados
* Auditoria

## <a name="authentication-and-identity-management"></a>Gestão de identidades e autenticação
A autenticação é o processo pelo qual a identidade do utilizador é verificada quando o usuário interage com o Data Lake Store ou com qualquer serviço que se liga ao Data Lake Store. Gerenciamento de identidade e autenticação, o Data Lake Store utiliza [do Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), uma identidade e acesso gestão solução na cloud completa que simplifica a gestão de utilizadores e grupos.

Cada subscrição do Azure pode ser associada uma instância do Azure Active Directory. Apenas os utilizadores e identidades de serviço são definidas no seu serviço do Azure Active Directory podem aceder à sua conta do Data Lake Store, com o portal do Azure, ferramentas de linha de comandos, ou por meio de aplicativos de cliente sua organização baseia-se ao utilizar o Azure Data Lake Store SDK. Principais vantagens da utilização do Azure Active Directory como um mecanismo de controlo de acesso centralizado são:

* Gestão de ciclo de vida de identidade simplificada. A identidade de um utilizador ou um serviço (uma identidade principal de serviço) pode ser criada rapidamente e rapidamente revogada simplesmente eliminando ou desabilitar a conta no diretório.
* Autenticação multifator. [Autenticação multifator](../active-directory/authentication/multi-factor-authentication.md) fornece uma camada adicional de segurança de inícios de sessão de utilizador e transações.
* Autenticação a partir de qualquer cliente por meio de um protocolo padrão aberto, como o OAuth ou OpenID.
* Federação com os serviços de diretório empresarial e fornecedores de identidade na cloud.

## <a name="authorization-and-access-control"></a>Controlo de acesso e autorização
Depois do Azure Active Directory autentica um usuário para que o utilizador pode aceder ao Azure Data Lake Store, controles de autorização permissões de acesso para Data Lake Store. Data Lake Store separa autorização para atividades relacionadas com a conta e dados relacionados com a da seguinte forma:

* [Controlo de acesso baseado em funções](../role-based-access-control/overview.md) (RBAC) fornecida pelo Azure para a gestão de conta
* POSIX ACL para aceder aos dados no arquivo

### <a name="rbac-for-account-management"></a>RBAC para a gestão de conta
Quatro funções básicas são definidas para o Data Lake Store, por predefinição. As funções permitem operações diferentes numa conta do Data Lake Store através do portal do Azure, cmdlets do PowerShell e REST APIs. As funções de proprietário e contribuinte podem realizar uma variedade de funções de administração na conta. Pode atribuir a função de leitor aos utilizadores que apenas ver os dados de gestão de conta.

![Funções RBAC](./media/data-lake-store-security-overview/rbac-roles.png "funções RBAC")

Observe que embora funções são atribuídas para gestão de conta, algumas funções de afetam o acesso aos dados. Tem de utilizar as ACLs para controlar o acesso para operações que um utilizador pode realizar no sistema de ficheiros. A tabela seguinte mostra um resumo dos direitos de gestão e direitos de acesso de dados para as funções predefinidas.

| Funções | Gestão de direitos | Direitos de acesso de dados | Explicação |
| --- | --- | --- | --- |
| Nenhuma função atribuída |Nenhuma |Regidos por ACL |O utilizador não é possível utilizar o portal do Azure ou cmdlets do Azure PowerShell para procurar o Data Lake Store. O utilizador pode utilizar apenas as ferramentas da linha de comandos. |
| Proprietário |Todos |Todos |A função de proprietário é um Superutilizador. Esta função pode gerir tudo e tem acesso total aos dados. |
| Leitor |Só de leitura |Regidos por ACL |A função de leitor pode ver tudo sobre a gestão de conta, por exemplo, o que é atribuída a que função do utilizador. A função de leitor não pode efetuar quaisquer alterações. |
| Contribuinte |Tudo, exceto adicionar e remover funções |Regidos por ACL |A função Contribuidor pode gerir alguns aspectos de uma conta, por exemplo, implementações e a criação e a gestão de alertas. A função Contribuidor não é possível adicionar ou remover funções. |
| Administrador de Acesso de Utilizador |Adicionar e remover funções |Regidos por ACL |A função de administrador de acesso de utilizador pode gerir o acesso de utilizador para as contas. |

Para obter instruções, consulte [atribuir utilizadores ou grupos de segurança para contas do Data Lake Store](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Usando as ACLs para operações em sistemas de ficheiros
Data Lake Store é um sistema de ficheiros hierárquico, como Hadoop Distributed File System (HDFS) e suporta [POSIX ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Ele controla a leitura (r), escrita (w) e executar (x) permissões para recursos para a função de proprietário para o grupo proprietários e para outros utilizadores e grupos. No Data Lake Store, as ACLs podem ser ativadas na pasta raiz, em subpastas e em ficheiros individuais. Para obter mais informações sobre como as ACLs funcionam no contexto do Data Lake Store, veja [Controlo de acesso no Data Lake Store](data-lake-store-access-control.md).

Recomendamos que definir ACLs para vários utilizadores através da utilização [grupos de segurança](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Adicionar utilizadores a um grupo de segurança e, em seguida, atribua as ACLs para um ficheiro ou pasta para o grupo de segurança. Isto é útil quando deseja fornecer permissões atribuídas, uma vez que é limitado a um máximo de 28 entradas de permissões atribuídas. Para obter mais informações sobre como proteger melhor os dados armazenados no Data Lake Store utilizando grupos de segurança do Azure Active Directory, consulte [atribuir utilizadores ou grupo de segurança como ACLs ao sistema de ficheiros do Azure Data Lake Store](data-lake-store-secure-data.md#filepermissions).

![Lista de permissões de acesso](./media/data-lake-store-security-overview/adl.acl.2.png "permissões de acesso de lista")

## <a name="network-isolation"></a>Isolamento de rede
Utilize o Data Lake Store para ajudar a controlar o acesso ao seu arquivo de dados ao nível da rede. Pode estabelecer firewalls e definir um intervalo de endereços IP para os seus clientes fidedignos. Com um intervalo de endereços IP, apenas os clientes que tenham um endereço IP no intervalo definido podem ligar ao Data Lake Store.

![As definições da firewall e IP de acesso](./media/data-lake-store-security-overview/firewall-ip-access.png "endereço IP e definições da Firewall")

## <a name="data-protection"></a>Proteção de dados
Azure Data Lake Store protege os dados em todo o seu ciclo de vida. Para dados em circulação, o Data Lake Store utiliza o protocolo Transport Layer Security (TLS 1.2) de norma da indústria para proteger dados através da rede.

![Encriptação no Data Lake Store](./media/data-lake-store-security-overview/adls-encryption.png "encriptação no Data Lake Store")

O Data Lake Store também fornece encriptação para dados armazenados na conta. Pode optar por encriptar os seus dados ou por não utilizar encriptação. Se optar por encriptação, os dados armazenados no Data Lake Store são encriptados antes de serem armazenados em suportes de dados persistentes. Nesse caso, o Data Lake Store automaticamente encripta os dados antes de persistir e desencripta-os antes da obtenção, pelo que é completamente transparente para o cliente a aceder aos dados. Não há nenhuma alteração de código necessária do lado do cliente para dados de encriptação/desencriptação.

Gestão de chaves, o Data Lake Store disponibiliza dois modos para gerir as chaves de encriptação mestra (MEKs), que são necessárias para desencriptar todos os dados que são armazenados na Store de Lake dados. Pode optar por permitir que os Data Lake Store faça a gestão de MEKs para, ou optar por manter a propriedade de MEKs com a sua conta do Azure Key Vault. Especifique o modo de gestão de chaves enquanto ao criar uma conta do Data Lake Store. Para obter mais informações sobre como fornecer configuração relacionada com encriptação, veja [Introdução ao Azure Data Lake Store utilizando o Portal do Azure](data-lake-store-get-started-portal.md).

## <a name="activity-and-diagnostic-logs"></a>Atividade e os registos de diagnóstico
Pode usar a atividade ou registos de diagnóstico, dependendo se estiver à procura de registos de atividades relacionadas com a gestão de conta ou atividades relacionadas com dados.

* Atividades relacionadas com a gestão da conta utilizam APIs do Azure Resource Manager e são apresentadas no portal do Azure através de registos de atividades.
* Dados relacionados com atividades utilizar WebHDFS REST APIs e são apresentadas no portal do Azure através de registos de diagnóstico.

### <a name="activity-log"></a>Registo de atividades
Para estar em conformidade com as normas, uma organização poderá necessitar de registos de auditoria adequada das atividades de gestão de conta se precisar de se aprofundar em incidentes específicos. Data Lake Store tem monitorização incorporada e regista todas as atividades de gestão de conta.

Para registos de auditoria da gestão de conta, ver e escolha as colunas que pretende iniciar sessão. Também pode exportar os registos de atividades para o armazenamento do Azure.

![Registo de atividades](./media/data-lake-store-security-overview/activity-logs.png "registo de atividades")

Para obter mais informações sobre como trabalhar com registos de atividades, consulte [ver registos de atividades para auditar as ações em recursos](../azure-resource-manager/resource-group-audit.md).

### <a name="diagnostics-logs"></a>Registos de diagnósticos
Pode ativar a auditoria de acesso de dados e log de diagnóstico no portal do Azure e enviar os registos para uma conta de armazenamento de Blobs do Azure, um hub de eventos ou do Log Analytics.

![Os registos de diagnóstico](./media/data-lake-store-security-overview/diagnostic-logs.png "os registos de diagnóstico")

Para obter mais informações sobre como trabalhar com registos de diagnóstico com o Azure Data Lake Store, consulte [aceder a registos de diagnóstico para o Data Lake Store](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Resumo
Os clientes empresariais exigem uma plataforma de cloud de análise de dados que é seguro e fácil de usar. Azure Data Lake Store é projetado para ajudar a solucionar esses requisitos através da gestão de identidade e autenticação através de integração do Active Directory do Azure, com base na ACL autorização, isolamento de rede, a encriptação de dados em trânsito e em inatividade e auditoria.

Se quiser ver os novos recursos do Data Lake Store, envie-nos seus comentários [fórum do UserVoice do Data Lake Store](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Consulte também
* [Descrição geral do Azure Data Lake Store](data-lake-store-overview.md)
* [Introdução ao Data Lake Store](data-lake-store-get-started-portal.md)
* [Secure data in Data Lake Store (Proteger dados no Data Lake Store)](data-lake-store-secure-data.md)

