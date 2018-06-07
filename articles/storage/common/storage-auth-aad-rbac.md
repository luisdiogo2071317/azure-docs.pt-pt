---
title: Utilizar o RBAC para gerir os direitos de acesso a contentores de armazenamento do Azure e filas (pré-visualização) | Microsoft Docs
description: Utilize o controlo de acesso baseado em funções (RBA) para atribuir funções para o acesso aos dados de armazenamento do Azure para utilizadores, grupos, principais de serviço de aplicações ou identidades de serviço geridas. Storage do Azure suporta funções incorporadas e personalizadas para os direitos de acesso a contentores e filas.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/29/2018
ms.author: tamram
ms.openlocfilehash: cb77bd4418e105c877202f0f1725350380ea2308
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661014"
---
# <a name="manage-access-rights-to-azure-storage-data-with-rbac-preview"></a>Gerir direitos de acesso aos dados de armazenamento do Azure com o RBAC (pré-visualização)

Azure Active Directory (Azure AD) autoriza direitos de acesso a recursos protegidos através do [controlo de acesso baseado em funções (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Armazenamento do Azure define um conjunto de funções incorporadas do RBAC que abranger comuns conjuntos de permissões utilizadas para aceder a contentores ou filas. Quando uma função RBAC é atribuído a uma identidade do Azure AD, que identity é concedido acesso a esses recursos, de acordo com o âmbito especificado. Acesso pode ser confinado para o nível de subscrição, o grupo de recursos, a conta de armazenamento ou um contentor individuais ou fila. Pode atribuir direitos de acesso para recursos de armazenamento do Azure utilizando o portal do Azure, as ferramentas da linha de comandos do Azure e APIs de gestão do Azure. 

Uma identidade do Azure AD pode ser um utilizador, grupo ou principal de serviço de aplicações, ou pode ser um *identidade do serviço gerido*. Um principal de segurança pode ser um utilizador, grupo ou principal de serviço de aplicações. A [identidade do serviço gerido](../../active-directory/managed-service-identity/overview.md) é uma identidade gerida automaticamente utilizada para autenticar a partir de aplicações em execução em máquinas virtuais do Azure, aplicações de função, conjuntos de dimensionamento de máquina virtual e outros utilizadores. Para obter uma descrição geral da identidade no Azure AD, consulte [soluções de identidade do Azure compreender](https://docs.microsoft.com/en-us/azure/active-directory/understand-azure-identity-solutions).

## <a name="rbac-roles-for-azure-storage"></a>Funções do RBAC para o Storage do Azure

Storage do Azure suporta funções do RBAC incorporadas e personalizadas. Storage do Azure oferece estas funções incorporadas do RBAC para utilização com o Azure AD:

- [Contribuinte de dados de BLOBs de armazenamento (pré-visualização)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Leitor de dados de BLOBs de armazenamento (pré-visualização)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Contribuinte de dados de fila de armazenamento (pré-visualização)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Leitor de dados de fila de armazenamento (pré-visualização)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Para obter mais informações sobre como as funções incorporadas são definidas para o Storage do Azure, consulte [compreender as definições de função](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

Também pode definir funções personalizadas para utilização com contentores e pelas filas. Para obter mais informações, consulte [criar funções personalizadas para controlo de acesso em funções do Azure](https://docs.microsoft.com/azure/role-based-access-control/custom-roles.md). 

> [!IMPORTANT]
> Esta pré-visualização destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estarão disponíveis até que a integração do Azure AD para o Storage do Azure está declarada geralmente disponível. Se a integração do Azure AD ainda não é suportada para o seu cenário, continue a utilizar a autorização de chave partilhada ou SAS tokens nas suas aplicações. Para obter informações adicionais sobre a pré-visualização, consulte [autenticar o acesso ao Storage do Azure utilizando o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
>
> Durante a pré-visualização, atribuições de funções RBAC podem demorar até cinco minutos para propagar.

## <a name="assign-a-role-to-a-security-principal"></a>Atribuir uma função a um principal de segurança

Atribua uma função RBAC para uma identidade do Azure para conceder permissões para contentores ou filas na sua conta de armazenamento. Pode definir o âmbito de atribuição de função para a conta de armazenamento ou para um contentor específico ou a fila. A tabela seguinte resume os direitos de acesso concedidos por funções incorporadas, consoante o âmbito: 

|                                 |     Contribuinte de dados de BLOBs                                                 |     Leitor de dados de BLOBs                                                |     Fila contribuinte de dados                                  |     Leitor de dados de fila                                 |
|---------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------|
|    No âmbito de subscrição       |    Acesso de leitura/escrita para todos os contentores e blobs na subscrição       |    Acesso de leitura a todos os contentores e blobs na subscrição       |    Acesso de leitura/escrita para todas as filas na subscrição       |    Acesso de leitura para todas as filas na subscrição         |
|    No âmbito do grupo de recursos     |    Acesso de leitura/escrita para todos os contentores e blobs no grupo de recursos     |    Acesso de leitura a todos os contentores e blobs no grupo de recursos     |    Acesso de leitura/escrita para todas as filas, no grupo de recursos     |    Acesso de leitura para todas as filas, no grupo de recursos     |
|    No âmbito de conta de armazenamento    |    Acesso de leitura/escrita para todos os contentores e blobs na conta de armazenamento    |    Acesso de leitura a todos os contentores e blobs na conta de armazenamento    |    Acesso de leitura/escrita para todas as filas na conta de armazenamento    |    Acesso de leitura para todas as filas na conta de armazenamento    |
|    No âmbito de fila de contentor /    |    Acesso de leitura/escrita para o contentor especificado e os respetivos blobs              |    Acesso de leitura para o contentor especificado e os respetivos blobs              |    Acesso de leitura/escrita para a fila especificada                  |    Acesso de leitura para a fila especificada                    |

> [!NOTE]
> Como um proprietário da sua conta do Storage do Azure, a não atribui automaticamente permissões para aceder a dados. Deve explicitamente atribuir manualmente uma função RBAC para o Storage do Azure. Pode atribuí-la ao nível da sua subscrição, grupo de recursos, conta de armazenamento, ou um contentor ou fila.

Para obter detalhes sobre as permissões necessárias para chamar as operações de armazenamento do Azure, consulte [permissões para chamar as operações REST](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

As secções seguintes mostram como atribuir uma função no âmbito da conta de armazenamento ou um âmbito para um contentor individuais.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Atribuir uma função no âmbito da conta do storage no portal do Azure

Para atribuir uma função incorporada conceder acesso a todos os contentores ou filas na conta do storage no portal do Azure:

1. No [portal do Azure](https://azure.portal.com/), navegue até à sua conta de armazenamento.
2. Selecione a sua conta de armazenamento, em seguida, selecione **controlo de acesso (IAM)** para apresentar as definições de controlo de acesso para a conta. Clique em de **adicionar** botão para adicionar uma nova função.

    ![Captura de ecrã que mostra as definições de controlo de acesso de armazenamento](media/storage-auth-aad-rbac/portal-access-control.png)

3. No **adicionar permissões** janela, selecione a função para atribuir a uma identidade do Azure AD. Procure para localizar a identidade para o qual pretende atribuir essa função. Por exemplo, a imagem seguinte mostra o **leitor de dados de BLOBs de armazenamento (pré-visualização)** função atribuída a um utilizador.

    ![Captura de ecrã que mostra como atribuir uma função do RBAC](media/storage-auth-aad-rbac/add-rbac-role.png)

4. Clique em **Guardar**. A identidade a quem é atribuída a função aparece listada nessa função. Por exemplo, a imagem seguinte mostra que os utilizadores adicionados agora tem permissões de leitura a todos os dados de BLOBs na conta de armazenamento.

    ![Captura de ecrã que mostra lista de utilizadores atribuídos a uma função](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Atribuir uma função no âmbito de um contentor ou uma fila no portal do Azure

Os passos para atribuir uma função incorporada no âmbito para um contentor ou para uma fila são semelhantes. O procedimento apresentado aqui atribui uma função no âmbito de um contentor, mas pode seguir os mesmos passos para atribuir uma função no âmbito de uma fila: 

1. No [portal do Azure](https://azure.portal.com/), navegue até à sua conta de armazenamento e apresentar o **descrição geral** para a conta.
2. Em serviço Blob, selecione **procurar Blobs**. 
3. Localize o contentor para o qual pretende atribuir uma função e visualizar as configurações do contentor. 
4. Selecione **controlo de acesso (IAM)** para apresentar as definições de controlo de acesso para o contentor.
5. No **adicionar permissões** janela, selecione a função que pretende atribuir a uma identidade do Azure AD. Em seguida, procure para localizar a identidade para o qual pretende atribuir essa função.
6. Clique em **Guardar**. A identidade a quem é atribuída a função aparece listada nessa função. Por exemplo, a imagem seguinte mostra que o utilizador adicionado agora tem permissões de leitura aos dados no contentor com o nome *exemplo contentor*.

    ![Captura de ecrã que mostra lista de utilizadores atribuídos a uma função](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Próximos Passos

- Para saber mais sobre o RBAC, veja [introdução ao controlo de acesso baseado em funções](../../role-based-access-control/overview.md).
- Para saber como atribuir e gerir atribuições de funções RBAC com o Azure PowerShell, CLI do Azure ou a API REST, veja estes artigos:
    - [Gerir o controlo de acesso baseado em funções (RBAC) com o Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
    - [Gerir o controlo de acesso baseado em funções (RBAC) com a CLI do Azure](../../role-based-access-control/role-assignments-cli.md)
    - [Gerir o controlo de acesso baseado em funções (RBAC) com a API REST](../../role-based-access-control/role-assignments-rest.md)
- Para saber como autorizar o acesso a contentores e filas de dentro das suas aplicações de armazenamento, consulte [utilizar o Azure AD com aplicações de armazenamento do Azure](storage-auth-aad-app.md).
- Para obter informações adicionais sobre a integração do Azure AD para contentores do Azure e as filas, consulte o blogue da equipa do Storage do Azure post, [anunciar a autenticação de pré-visualização do Azure AD para o Storage do Azure](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
- 