---
title: Utilizar o RBAC para gerir os direitos de acesso a contentores de armazenamento do Azure e filas (pré-visualização) | Documentos da Microsoft
description: Utilize o controlo de acesso baseado em funções (RBA) para atribuir funções para acesso aos dados de armazenamento do Azure para utilizadores, grupos, os principais de serviço de aplicações ou identidades de serviço geridas. O armazenamento do Azure suporta funções incorporadas e personalizadas para os direitos de acesso a contentores e filas.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/16/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: be3fd304e04a66e6564141b6a20efea88cf62553
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42058208"
---
# <a name="manage-access-rights-to-azure-storage-data-with-rbac-preview"></a>Gerir os direitos de acesso a dados de armazenamento do Azure com o RBAC (pré-visualização)

Azure Active Directory (Azure AD) autoriza direitos de acesso a recursos protegidos por meio [controlo de acesso baseado em funções (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). O armazenamento do Azure define um conjunto de funções RBAC incorporadas que abrangem conjuntos comuns de permissões utilizados para aceder aos contentores ou filas. Quando uma função RBAC é atribuído a uma identidade do Azure AD, que é concedido acesso a esses recursos, identidade, de acordo com o âmbito especificado. Acesso pode ser confinado ao nível da subscrição, o grupo de recursos, a conta de armazenamento, ou um contentor individual ou fila. Pode atribuir direitos de acesso para recursos de armazenamento do Azure com o portal do Azure, ferramentas de linha de comandos do Azure e APIs de gestão do Azure. 

Uma identidade do Azure AD pode ser um utilizador, grupo ou principal de serviço de aplicações, ou pode ser um *identidade do serviço gerido*. Uma entidade de segurança pode ser um utilizador, grupo ou principal de serviço de aplicações. R [identidade do serviço gerido](../../active-directory/managed-service-identity/overview.md) é uma identidade gerida automaticamente utilizada para autenticar a partir de aplicações em execução em máquinas virtuais do Azure, aplicações de funções, os conjuntos de dimensionamento de máquinas virtuais e outros. Para uma descrição geral de identidade no Azure AD, consulte [soluções de identidade do Azure compreender](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions).

## <a name="rbac-roles-for-azure-storage"></a>Funções RBAC do armazenamento do Azure

O armazenamento do Azure suporta funções do RBAC incorporadas e personalizadas. O armazenamento do Azure oferece essas funções RBAC incorporadas para utilização com o Azure AD:

- [Contribuinte de dados de Blob de armazenamento (pré-visualização)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Leitor de dados de Blob de armazenamento (pré-visualização)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Contribuinte de dados de fila de armazenamento (pré-visualização)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Leitor de dados de fila de armazenamento (pré-visualização)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Para obter mais informações sobre como as funções incorporadas são definidas para o armazenamento do Azure, consulte [compreender as definições de função](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

Também pode definir funções personalizadas para utilização com contentores e filas. Para obter mais informações, consulte [criar funções personalizadas para controlo de acesso](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). 

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o armazenamento do Azure é declarada em disponibilidade geral. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a usar a autorização de chave partilhada ou SAS tokens em seus aplicativos. Para obter mais informações sobre a pré-visualização, veja [autenticar o acesso ao armazenamento do Azure com o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
>
> Durante a pré-visualização, as atribuições de funções do RBAC podem demorar até cinco minutos para propagar.

## <a name="assign-a-role-to-a-security-principal"></a>Atribuir uma função a uma entidade de segurança

Atribua uma função RBAC para uma identidade do Azure para conceder permissões para contentores ou filas na conta de armazenamento. Pode definir o âmbito de atribuição de função para a conta de armazenamento ou para um contentor específico ou a fila. A tabela seguinte resume os direitos de acesso concedidos por funções incorporadas, consoante o âmbito: 

|                                 |     Contribuinte de dados de blob                                                 |     Leitor de dados de blob                                                |     Contribuinte de dados de fila                                  |     Leitor de dados de fila                                 |
|---------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------|
|    No âmbito de subscrição       |    Acesso de leitura/escrita a todos os contentores e blobs na subscrição       |    Acesso de leitura a todos os contentores e blobs na subscrição       |    Acesso de leitura/gravação para todas as filas na subscrição       |    Acesso de leitura para todas as filas na subscrição         |
|    No âmbito do grupo de recursos     |    Acesso de leitura/escrita a todos os contentores e blobs, no grupo de recursos     |    Acesso de leitura a todos os contentores e blobs, no grupo de recursos     |    Acesso de leitura/gravação para todas as filas no grupo de recursos     |    Acesso de leitura para todas as filas no grupo de recursos     |
|    No âmbito de conta de armazenamento    |    Acesso de leitura/escrita a todos os contentores e blobs na conta de armazenamento    |    Acesso de leitura a todos os contentores e blobs na conta de armazenamento    |    Acesso de leitura/gravação para todas as filas na conta de armazenamento    |    Acesso de leitura para todas as filas na conta de armazenamento    |
|    O âmbito contentor/fila    |    Acesso de leitura/gravação para o contentor especificado e respetivos blobs              |    Acesso de leitura para o contentor especificado e respetivos blobs              |    Acesso de leitura/escrita para a fila especificada                  |    Acesso de leitura para a fila especificada                    |

> [!NOTE]
> Como proprietário da conta de armazenamento do Azure, não é atribuídos automaticamente permissões para aceder aos dados. Tem explicitamente de atribuir-se uma função RBAC do armazenamento do Azure. Pode atribuí-la no nível da sua subscrição, grupo de recursos, conta de armazenamento, ou um contentor ou fila.

Para obter detalhes sobre as permissões necessárias para chamar operações de armazenamento do Azure, consulte [permissões para chamar as operações REST](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

As secções seguintes mostram como atribuir uma função no âmbito para a conta de armazenamento ou um âmbito para um contentor individual.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Atribuir uma função de âmbito para a conta de armazenamento no portal do Azure

Para atribuir uma função incorporada conceder acesso a todos os contentores ou filas na conta de armazenamento no portal do Azure:

1. Na [portal do Azure](https://portal.azure.com), navegue até à sua conta de armazenamento.
2. Selecione a sua conta de armazenamento, em seguida, selecione **controlo de acesso (IAM)** para apresentar as definições de controlo de acesso para a conta. Clique nas **adicionar** botão para adicionar uma nova função.

    ![Captura de ecrã que mostra as definições de controlo de acesso de armazenamento](media/storage-auth-aad-rbac/portal-access-control.png)

3. Na **adicionar permissões** janela, selecione a função para atribuir a uma identidade do Azure AD. Procure para localizar a identidade a quem pretende atribuir essa função. Por exemplo, a imagem seguinte mostra os **leitor de dados de Blob de armazenamento (pré-visualização)** função atribuída a um utilizador.

    ![Captura de ecrã que mostra como atribuir uma função RBAC](media/storage-auth-aad-rbac/add-rbac-role.png)

4. Clique em **Guardar**. A identidade a quem é atribuída a função aparece listada sob essa função. Por exemplo, a imagem seguinte mostra que os utilizadores adicionados agora possuem permissões de leitura a todos os dados de BLOBs na conta de armazenamento.

    ![Captura de ecrã a mostrar lista de utilizadores atribuídos a uma função](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Atribuir uma função no âmbito de um contentor ou a fila no portal do Azure

Os passos para atribuir uma função incorporada de âmbito para um contentor ou a uma fila são semelhantes. O procedimento apresentado aqui atribui uma função no âmbito de um contentor, mas pode seguir os mesmos passos para atribuir uma função no âmbito de uma fila: 

1. Na [portal do Azure](https://portal.azure.com), navegue para a sua conta de armazenamento e exibir o **descrição geral** para a conta.
2. Em serviço de BLOBs, selecione **procurar Blobs**. 
3. Localizar o contentor para o qual pretende atribuir uma função e apresentar as definições do contentor. 
4. Selecione **controlo de acesso (IAM)** para apresentar as definições de controlo de acesso para o contentor.
5. Na **adicionar permissões** janela, selecione a função que pretende atribuir a uma identidade do Azure AD. Procure para localizar a identidade para o qual pretende atribuir essa função.
6. Clique em **Guardar**. A identidade a quem é atribuída a função aparece listada sob essa função. Por exemplo, a imagem seguinte mostra que o utilizador adicionado agora tem permissões de leitura aos dados no contentor com o nome *exemplo-container*.

    ![Captura de ecrã a mostrar lista de utilizadores atribuídos a uma função](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Próximos Passos

- Para saber mais sobre o RBAC, veja [introdução ao controlo de acesso baseado em funções](../../role-based-access-control/overview.md).
- Para saber como atribuir e gerir atribuições de funções RBAC com o Azure PowerShell, CLI do Azure ou da API REST, veja estes artigos:
    - [Gerir o controlo de acesso baseado em funções (RBAC) com o Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
    - [Gerir o controlo de acesso baseado em funções (RBAC) com a CLI do Azure](../../role-based-access-control/role-assignments-cli.md)
    - [Gerir o controlo de acesso baseado em funções (RBAC) com a API REST](../../role-based-access-control/role-assignments-rest.md)
- Para saber como autorizar o acesso a contentores e filas de dentro dos seus aplicativos de armazenamento, veja [utilize o Azure AD com aplicações de armazenamento do Azure](storage-auth-aad-app.md).
- Para obter mais informações sobre a integração do Azure AD para contentores do Azure e filas de mensagens em fila, consulte a postagem no armazenamento do Azure team blog [a autenticação de pré-visualização do Azure AD para o armazenamento do Azure a anunciar](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
- 
