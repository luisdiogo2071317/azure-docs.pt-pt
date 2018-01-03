---
title: "Gerido identidade de serviço (MSI) para o Azure Active Directory"
description: "Uma descrição geral da identidade de serviço geridas para recursos do Azure."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ms.reviewer: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 53577c8da5f82235284d1cb9e48f2d47254aa6bd
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
#  <a name="managed-service-identity-msi-for-azure-resources"></a>Gerido identidade de serviço (MSI) para recursos do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Um desafio comum quando a criação de aplicações em nuvem como gerir as credenciais que têm de ser no seu código para autenticar a serviços em nuvem. Manter estas credenciais segura é uma tarefa importante. Idealmente, que nunca são apresentados em estações de trabalho de programador ou obterem marcadas para o controlo de origem. O Cofre de chaves do Azure fornece uma forma de armazenar com segurança as credenciais e outras chaves e segredos, mas o seu código tem de autenticar para o Cofre de chaves para recuperar. Identidade de serviço geridas (MSI) permite a resolver este problema mais simples, conferindo aos serviços do Azure uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, incluindo o Cofre de chaves, sem ter as credenciais no seu código.

## <a name="how-does-it-work"></a>Como funciona?

Quando utilizar uma identidade de serviço gerida numa instância de serviço do Azure, o Azure cria uma identidade no inquilino do Azure AD utilizado pela sua subscrição do Azure. Além disso, o Azure Aprovisiona as credenciais para a identidade, para a instância de serviço. Como resultado, o seu código, em seguida, pode efetuar um pedido de local para obter os tokens de acesso para os serviços que suportam a autenticação do Azure AD. Todos os enquanto o Azure encarrega-se de a anular as credenciais utilizadas pela instância do serviço.

## <a name="how-do-i-enable-my-resources-to-use-a-managed-service-identity"></a>Como ativar a meus recursos utilizar uma identidade de serviço geridas?

Existem dois tipos de identidades de serviço geridas disponíveis: *sistema atribuídas* e *utilizador atribuído*.

- A **sistema atribuídas** MSI é ativado diretamente numa instância de serviço do Azure. Durante o processo de ativar o Azure cria uma identidade para a instância de serviço no inquilino do Azure AD e Aprovisiona as credenciais para a identidade para a instância de serviço. O ciclo de vida de um sistema MSI atribuído diretamente está associado para o Azure instância de serviço está ativada no. Se a instância de serviço for eliminada, Azure limpa automaticamente as credenciais e da identidade no Azure AD.

- A **utilizador atribuído** MSI *(pré-visualização privada)* é criado como uma autónoma recursos do Azure. Durante o processo de criação, o Azure cria uma identidade no inquilino do Azure AD. Depois de criada a identidade, podem ser atribuído a um ou mais instâncias de serviço do Azure. Uma vez que um MSI utilizador atribuído pode ser utilizado por várias instâncias de serviço do Azure, o ciclo de vida de é gerido separadamente.

Eis um exemplo de como um MSI do sistema-atribuídas funciona com máquinas virtuais do Azure.

![Exemplo de MSI da máquina virtual](~/articles/active-directory/media/msi-vm-example.png)

1. O Azure Resource Manager recebe uma mensagem para ativar o MSI atribuído do sistema numa VM.
2. O Azure Resource Manager cria um Principal de serviço no Azure AD para representar a identidade da VM. O Principal de serviço é criado no inquilino do Azure AD, que é considerado fidedigno por esta subscrição.
3. O Azure Resource Manager configura os detalhes de Principal de serviço na extensão de VM do MSI da VM. Este passo inclui a configuração de ID de cliente e o certificado utilizado pela extensão para obter os tokens de acesso do Azure AD.
4. Agora que a identidade de Principal de serviço da VM é conhecida, pode conceder acesso aos recursos do Azure. Por exemplo, se o seu código tem de chamar o Azure Resource Manager, em seguida, é iria atribuir a VM Principal de serviço a função adequada, utilizando o controlo de acesso baseado em funções (RBAC) no Azure AD.  Se o seu código tem de chamar o Cofre de chaves, teria de conceder o acesso de código para o segredo específico ou a chave no Cofre de chaves.
5. O código em execução na VM pede um token de um ponto final local que está alojado pela extensão de VM de MSI: oauth2/http://localhost:50342/token. O parâmetro do recurso Especifica o serviço ao qual é enviado o token. Por exemplo, se pretender que o seu código para autenticar para o Azure Resource Manager, teria de utilizar recursos = https://management.azure.com/.
6. A extensão da VM MSI utiliza o respetivo ID de cliente configurada e o certificado para pedir um token de acesso do Azure AD.  Azure AD devolve um token de acesso de Token Web JSON (JWT).
7. O código envia o token de acesso numa chamada para um serviço que suporta a autenticação do Azure AD.

Utilizar o mesmo diagrama, aqui um exemplo de como um MSI utilizador atribuído funciona com máquinas virtuais do Azure.

![Exemplo de MSI da máquina virtual](~/articles/active-directory/media/msi-vm-example.png)

1. O Azure Resource Manager recebe uma mensagem para criar um MSI utilizador atribuído.
2. O Azure Resource Manager cria um Principal de serviço no Azure AD para representar a identidade do MSI. O Principal de serviço é criado no inquilino do Azure AD, que é considerado fidedigno por esta subscrição.
3. O Azure Resource Manager recebe uma mensagem para configurar os detalhes de Principal de serviço na extensão de VM do MSI da VM. Este passo inclui a configuração de ID de cliente e o certificado utilizado pela extensão para obter os tokens de acesso do Azure AD.
4. Agora que a identidade de Principal de serviço do MSI é conhecida, pode conceder acesso aos recursos do Azure. Por exemplo, se o seu código tem de chamar o Azure Resource Manager, em seguida, é iria atribuir o MSI Principal de serviço a função adequada, utilizando o controlo de acesso baseado em funções (RBAC) no Azure AD. Se o seu código tem de chamar o Cofre de chaves, teria de conceder o acesso de código para o segredo específico ou a chave no Cofre de chaves. Nota: Não é necessário para concluir passo 4 passo 3. Assim que existe um MSI, pode conceder acesso a recursos, independentemente da que está a ser configurado numa VM ou não.
5. O código em execução na VM pede um token de um ponto final local que está alojado pela extensão de VM de MSI: oauth2/http://localhost:50342/token. O parâmetro de ID de cliente especifica o nome da identidade MSI para utilizar. Além disso, o parâmetro do recurso Especifica o serviço ao qual é enviado o token. Por exemplo, se pretender que o seu código para autenticar para o Azure Resource Manager, teria de utilizar recursos = https://management.azure.com/.
6. A extensão da VM MSI verifica se o certificado para o ID de cliente pedida está configurado e os pedidos de um token de acesso do Azure AD. Azure AD devolve um token de acesso de Token Web JSON (JWT).
7. O código envia o token de acesso numa chamada para um serviço que suporta a autenticação do Azure AD.

Cada serviço do Azure que suporta uma identidade de serviço geridas tem o seu próprio método para o seu código obter um token de acesso. Consulte os tutoriais para cada serviço determinar o método específico para obter um token.

## <a name="try-managed-service-identity"></a>Tente a identidade de serviço geridas

Experimente um tutorial de identidade de serviço geridas para saber mais cenários ponto a ponto para aceder a diferentes recursos do Azure:
<br><br>
| Do recurso de MSI-ativado | Saiba como |
| ------- | -------- |
| VM do Azure (Linux)   | [Identidade do serviço do Gestor de acesso a recursos do Azure com uma VM com Linux gerido](msi-tutorial-linux-vm-access-arm.md) |
|                    | [Armazenamento de Azure acesso através de chave de acesso com uma identidade de serviço geridas do Linux VM](msi-tutorial-linux-vm-access-storage.md) |

## <a name="which-azure-services-support-managed-service-identity"></a>Os serviços do Azure suportam uma identidade de serviço geridas?

Serviços do Azure que suportam a identidade de serviço geridas podem utilizar MSI para se autenticarem em serviços que suportam a autenticação do Azure AD.  Estamos no processo de integração autenticação MSI e o Azure AD através do Azure.  Verifique back frequentemente para atualizações.

### <a name="azure-services-that-support-managed-service-identity"></a>Serviços do Azure que suportam a identidade de serviço geridas

Os seguintes serviços do Azure suportam uma identidade de serviço geridas.

| Serviço | Estado | Data | Configurar | Obter um token |
| ------- | ------ | ---- | --------- | ----------- |
| Máquinas Virtuais do Azure | Pré-visualização | Setembro de 2017 | [CLI do Azure](msi-qs-configure-cli-windows-vm.md)<br>[Modelos Azure Resource Manager](msi-qs-configure-template-windows-vm.md) | [Bash/Curl](msi-how-to-use-vm-msi-token.md#get-a-token-using-curl)<br>[/ REST DE HTTP](msi-how-to-use-vm-msi-token.md#get-a-token-using-http) |

### <a name="azure-services-that-support-azure-ad-authentication"></a>Suporte do Azure AD que a autenticação de serviços do Azure

Os seguintes serviços de suportam a autenticação do Azure AD e foi testados com serviços de cliente que utilizam a identidade de serviço geridas.

| Serviço | ID do Recurso | Estado | Data | Atribuir acesso |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://Management.Azure.com/ | Disponível | Setembro de 2017 | [CLI do Azure](msi-howto-assign-access-CLI.md) |
| Azure Key Vault | https://Vault.Azure.NET/ | Disponível | Setembro de 2017 | |
| Azure Data Lake | https://datalake.Azure.NET/ | Disponível | Setembro de 2017 | |
| SQL do Azure | https://Database.Windows.NET/ | Disponível | Outubro de 2017 | |

## <a name="how-much-does-managed-service-identity-cost"></a>Quantidade de identidade de serviço geridas custos?

Identidade de serviço geridas inclui gratuito do Azure Active Directory, que é a predefinição para as subscrições do Azure.  Não há sem custos adicionais para a identidade de serviço geridas.

## <a name="support-and-feedback"></a>Suporte e comentários

Gostaríamos de ouvir da!

* Coloque questões procedimentos no Stack Overflow com a etiqueta [azure msi](http://stackoverflow.com/questions/tagged/azure-msi).
* Efetuar pedidos de funcionalidade ou fazer comentários sobre o [fórum de comentários do Azure AD para os programadores](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences).






