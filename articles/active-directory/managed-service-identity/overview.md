---
title: O que é a Identidade de Serviço Gerida para os recursos do Azure
description: Uma descrição geral da Identidade de Serviço Gerida para recursos do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 03/28/2018
ms.author: daveba
ms.openlocfilehash: 3d6df04df8ceac1f868e64f0e8fbc7eb0fa317e3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38547978"
---
#  <a name="what-is-managed-service-identity-for-azure-resources"></a>O que é a Identidade de Serviço Gerida para os recursos do Azure?

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Um desafio comum inerente à criação de aplicações na cloud passa pela gestão das credenciais que têm de estar no código para autenticação nos serviços cloud. Manter essas credenciais protegidas é uma tarefa importante. Idealmente, nunca aparecem nas estações de trabalho dos programadores nem são verificadas no controlo de origem. O Azure Key Vault oferece uma forma de armazenar credenciais e outras chaves e segredos em segurança, mas o código tem de se autenticar no Key Vault para poder obtê-los. A Identidade de Serviço Gerida simplifica a resolução deste problema ao dar aos serviços do Azure uma identidade gerida automaticamente no Azure Active Directory (Azure AD). Pode utilizar esta identidade para autenticar em qualquer serviço que suporte a autenticação do Azure AD, incluindo o Key Vault, sem ser necessário ter credenciais no seu código.

A Identidade de Serviço Gerida está incluída no Azure Active Directory gratuito, que é a predefinição para as subscrições do Azure. Não existem custos adicionais.

## <a name="how-does-it-work"></a>Como funciona?

Existem dois tipos de Identidades de Serviço Geridas, **Atribuídas pelo Sistema** e **Atribuídas pelo Utilizador**.

- A **Identidade Atribuída pelo Sistema** é ativada diretamente numa instância de um serviço do Azure. Quando ativada, o Azure cria uma identidade para a instância do serviço no inquilino do Azure AD no qual a subscrição da instância do serviço confia. Assim que a identidade for criada, as respetivas credenciais são aprovisionadas na instância do serviço. O ciclo de vida das identidades atribuídas pelo sistema está diretamente ligado à instância do serviço do Azure nas quais estão ativadas. Se a instância do serviço for eliminada, o Azure limpa automaticamente as credenciais e a identidade no Azure AD.
- As **Identidades Atribuídas pelo Utilizador** são criadas como um recurso do Azure autónomo. Através de um processo de criação, o Azure cria uma identidade no inquilino do Azure AD no qual a subscrição que está a ser utilizada confia. Depois de criada, a identidade pode ser atribuída a uma ou mais instâncias do serviço do Azure. O ciclo de vida das identidades atribuídas pelo utilizador é gerido separadamente do ciclo de vida das instâncias do serviço do Azure ao qual estão atribuídas.

Como resultado, pode utilizar uma identidade atribuída pelo sistema ou pelo utilizador no seu código, para pedir tokens de acesso para serviços que suportem a autenticação do Azure AD. E tudo isto enquanto o Azure se encarrega da implementação das credenciais que a instância do serviço utiliza.

Eis um exemplo de como as Identidades Atribuídas pelo Sistema funcionam com as Máquinas Virtuais do Azure:

![Exemplo de Identidade Gerida da Máquina Virtual](overview/msi-vm-vmextension-imds-example.png)

1. O Azure Resource Manager recebe um pedido para ativar a identidade atribuída pelo sistema numa VM.
2. O Azure Resource Manager cria um Principal de Serviço no Azure AD para representar a identidade da VM. O Principal de Serviço é criado no inquilino do Azure AD no qual esta subscrição confia.
3. O Azure Resource Manager configura a identidade na VM:
    - Atualiza o ponto final da identidade do Azure Instance Metadata Service com o ID de cliente e o certificado do Principal de Serviço.
    - Aprovisiona a extensão de VM e adiciona o ID de cliente e o certificado do Principal de Serviço. (vai ser preterido)
4. Agora que a VM tem uma identidade, vamos utilizar as informações do respetivo Principal de Serviço para lhe conceder acesso aos recursos do Azure. Por exemplo, se o código tiver de chamar o Azure Resource Manager, terá de atribuir ao Principal de Serviço da VM a função adequada através do Controlo de Acesso Baseado em Funções (RBAC) no Azure AD. Se o código tiver de chamar o Key Vault, terá de lhe conceder acesso ao segredo ou à chave específica no Key Vault.
5. O código em execução na VM pode pedir um token de dois pontos finais que só estão acessíveis a partir da mesma:

    - Ponto final do Azure Instance Metadata Service (IMDS): http://169.254.169.254/metadata/identity/oauth2/token (recomendado)
        - O parâmetro do recurso especifica o serviço para o qual o token é enviado. Por exemplo, se pretender que o código autentique no Azure Resource Manager, terá de utilizar resource=https://management.azure.com/.
        - O parâmetro de versão da API especifica a versão do IMDS; utilize api-version=2018-02-01 ou superior.
    - Ponto final de extensão de VM: http://localhost:50342/oauth2/token (vai ser preterido)
        - O parâmetro do recurso especifica o serviço para o qual o token é enviado. Por exemplo, se pretender que o código autentique no Azure Resource Manager, terá de utilizar resource=https://management.azure.com/.

6. É feita uma chamada para o Azure AD a pedir uma token de acesso, conforme especificado no passo 5, através da utilização do ID de cliente e do certificado configurados no passo 3. O Azure AD devolve um token de acesso JSON Web Token (JWT).
7. O código envia o token de acesso numa chamada para um serviço que suporte a autenticação do Azure AD.

Através do mesmo diagrama, eis um exemplo do funcionamento atribuído pelo utilizador com as Máquinas Virtuais do Azure.

1. O Azure Resource Manager recebe um pedido para criar uma identidade atribuída pelo utilizador.
2. O Azure Resource Manager cria um Principal de Serviço no Azure AD para representar a identidade atribuída pelo utilizador. O Principal de Serviço é criado no inquilino do Azure AD no qual esta subscrição confia.
3. O Azure Resource Manager recebe um pedido para configurar a identidade atribuída pelo utilizador numa VM:
    - Atualiza o ponto final da identidade do Azure Instance Metadata Service com o ID de cliente e o certificado do Principal de Serviço da identidade atribuída pelo utilizador.
    - Aprovisiona a extensão de VM e adiciona o ID de cliente e o certificado do Principal de Serviço da identidade atribuída pelo utilizador (vai ser preterido).
4. Agora que a identidade atribuída pelo utilizador está criada, vamos utilizar as informações do respetivo Principal de Serviço para lhe conceder acesso aos recursos do Azure. Por exemplo, se o código tiver de chamar o Azure Resource Manager, terá de atribuir ao Principal de Serviço da identidade atribuída pelo utilizador a função adequada através do Controlo de Acesso Baseado em Funções (RBAC) no Azure AD. Se o código tiver de chamar o Key Vault, terá de lhe conceder acesso ao segredo ou à chave específica no Key Vault. Nota: este passo também pode ser realizado antes do passo 3.
5. O código em execução na VM pode pedir um token de dois pontos finais que só estão acessíveis a partir da mesma:

    - Ponto final do Azure Instance Metadata Service (IMDS): http://169.254.169.254/metadata/identity/oauth2/token (recomendado)
        - O parâmetro do recurso especifica o serviço para o qual o token é enviado. Por exemplo, se pretender que o código autentique no Azure Resource Manager, terá de utilizar resource=https://management.azure.com/.
        - O parâmetro de ID de cliente especifica a identidade para a qual o token é pedido. É necessário para eliminar ambiguidades se uma única VM tiver mais de uma identidade atribuída pelo utilizador.
        - O parâmetro de versão da API especifica a versão do IMDS; utilize api-version=2018-02-01 ou superior.

    - Ponto final de extensão de VM: http://localhost:50342/oauth2/token (vai ser preterido)
        - O parâmetro do recurso especifica o serviço para o qual o token é enviado. Por exemplo, se pretender que o código autentique no Azure Resource Manager, terá de utilizar resource=https://management.azure.com/.
        - O parâmetro de ID de cliente especifica a identidade para a qual o token é pedido. É necessário para eliminar ambiguidades se uma única VM tiver mais de uma identidade atribuída pelo utilizador.
6. É feita uma chamada para o Azure AD a pedir uma token de acesso, conforme especificado no passo 5, através da utilização do ID de cliente e do certificado configurados no passo 3. O Azure AD devolve um token de acesso JSON Web Token (JWT).
7. O código envia o token de acesso numa chamada para um serviço que suporte a autenticação do Azure AD.
     
## <a name="try-managed-service-identity"></a>Experimentar a Identidade do Serviço Gerido

Experimente um tutorial sobre a Identidade de Serviço Gerida para obter cenários completos para aceder a diferentes recursos do Azure:
<br><br>
| A partir de recursos habilitados para identidade gerida | Saiba como |
| ------- | -------- |
| VM do Azure (Windows) | [Access Azure Data Lake Store with a Windows VM Managed Service Identity](tutorial-windows-vm-access-datalake.md) (Aceder ao Azure Data Lake Store com uma Identidade de Serviço Gerida de VM do Windows) |
|                    | [Access Azure Resource Manager with a Windows VM Managed Service Identity](tutorial-windows-vm-access-arm.md) (Aceder ao Azure Resource Manager com uma Identidade de Serviço Gerida de VM do Windows) |
|                    | [Access Azure SQL with a Windows VM Managed Service Identity](tutorial-windows-vm-access-sql.md) (Aceder ao Azure SQL com uma Identidade de Serviço Gerida de VM do Windows) |
|                    | [Access Azure Storage via access key with a Windows VM Managed Service Identity](tutorial-windows-vm-access-storage.md) (Aceder ao Armazenamento do Azure através de uma chave de acesso com uma Identidade de Serviço Gerida de VM do Windows) |
|                    | [Access Azure Storage via SAS with a Windows VM Managed Service Identity](tutorial-windows-vm-access-storage-sas.md) (Aceder ao Armazenamento do Azure através de SAS com uma Identidade de Serviço Gerida de VM do Windows) |
|                    | [Access a non-Azure AD resource with a Windows VM Managed Service Identity and Azure Key Vault](tutorial-windows-vm-access-nonaad.md) (Aceder a recursos não Azure com uma Identidade de Serviço Gerida de VM do Windows e o Azure Key Vault) |
| VM do Azure (Linux)   | [Access Azure Data Lake Store with a Linux VM Managed Service Identity](tutorial-linux-vm-access-datalake.md) (Aceder ao Azure Data Lake Store com uma Identidade de Serviço Gerida de VM do Linux) |
|                    | [Access Azure Resource Manager with a Linux VM Managed Service Identity](tutorial-linux-vm-access-arm.md) (Aceder ao Azure Resource Manager com uma Identidade de Serviço Gerida de VM do Linux) |
|                    | [Access Azure Storage via access key with a Linux VM Managed Service Identity](tutorial-linux-vm-access-storage.md) (Aceder ao Armazenamento do Azure através de uma chave de acesso com uma Identidade de Serviço Gerida de VM do Linux) |
|                    | [Access Azure Storage via SAS with a Linux VM Managed Service Identity](tutorial-linux-vm-access-storage-sas.md) (Aceder ao Armazenamento do Azure através de SAS com uma Identidade de Serviço Gerida de VM do Linux) |
|                    | [Access a non-Azure AD resource with a Linux VM Managed Service Identity and Azure Key Vault](tutorial-linux-vm-access-nonaad.md) (Aceder a recursos não Azure com uma Identidade de Serviço Gerida de VM do Linux e o Azure Key Vault) |
| Serviço de Aplicações do Azure  | [Use Managed Service Identity with Azure App Service or Azure Functions](/azure/app-service/app-service-managed-service-identity) (Utilizar a Identidade de Serviço Gerida com o Serviço de Aplicações do Azure ou as Funções do Azure) |
| Funções do Azure    | [Use Managed Service Identity with Azure App Service or Azure Functions](/azure/app-service/app-service-managed-service-identity) (Utilizar a Identidade de Serviço Gerida com o Serviço de Aplicações do Azure ou as Funções do Azure) |
| Service Bus do Azure  | [Use Managed Service Identity with Azure Service Bus](../../service-bus-messaging/service-bus-managed-service-identity.md) (Utilizar a Identidade de Serviço Gerida com o Azure Service Bus) |
| Azure Event Hubs   | [Use Managed Service Identity with Azure Event Hubs](../../event-hubs/event-hubs-managed-service-identity.md) (Utilizar a Identidade de Serviço Gerida com os Hubs de Eventos do Azure) |

## <a name="which-azure-services-support-managed-service-identity"></a>Que serviços do Azure suportam a Identidade de Serviço Gerida?

As identidades geridas podem ser utilizadas para autenticação em serviços que suportem a autenticação do Azure AD. Para obter uma lista dos serviços do Azure que suportam a Identidade de Serviço Gerida, veja o artigo seguinte:
- [Serviços que suportam a Identidade de Serviço Gerida](services-support-msi.md)

## <a name="next-steps"></a>Passos seguintes

Introdução à Identidade de Serviço Gerida do Azure com os inícios rápidos seguintes:

* [Utilizar uma Identidade de Serviço Gerida do Windows para aceder ao Resource Manager - VM do Windows](tutorial-windows-vm-access-arm.md)
* [Utilizar uma Identidade de Serviço Gerida de VM do Windows para aceder ao Azure Resource Manager - VM do Linux](tutorial-linux-vm-access-arm.md)
