---
title: Configurar a validação do Azure Stack como uma conta de serviço | Documentos da Microsoft
description: Saiba como configurar a sua validação como uma conta de serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 3f87721dcf7485e15a473e82597f6bf4baeca659
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "42139552"
---
# <a name="set-up-your-validation-as-a-service-account"></a>Configurar a sua validação como uma conta de serviço

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

A validação como um serviço (VaaS) é um serviço do Azure que é disponibilizado para os parceiros do Microsoft Azure Stack que tem um contrato de engenharia conjuntas com a Microsoft para criar, desenvolver, validar, vender, implementar e suportar soluções do Azure Stack no mercado.

Saiba como obter o seu sistema pronto para a validação como serviço. Configurar a instância do Azure Active Directory e executar outras tarefas necessárias para a preparação para utilizar VaaS. 

As tarefas incluem:

- Criar um blob de armazenamento do Azure para armazenar os registos
- Implementar o agente local
- Baixe as máquinas de virtuais de imagem de teste na instância do Azure Stack para ser testada

## <a name="create-an-azure-active-directory-tenant-id"></a>Criar um ID de inquilino do Azure Active Directory

1. Criar um inquilino do Azure Active Directory no [portal do Azure](https://portal.azure.com) ou utilizar um inquilino existente.

    É recomendado para criar um inquilino especificamente para utilização com VaaS com um nome descritivo, como, por exemplo ContosoVaaS@onmicrosoft.com. Os recursos de controle (RBAC) de acesso baseado em função do inquilino serão utilizados para gerir quem na organização do parceiro pode utilizar VaaS pelo parceiro.  
    
    Obter mais informações, consulte [gerir o seu diretório do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

    > [!Note]  
    > Para obter mais informações sobre a criação de novos inquilinos do Azure Active Directory, consulte [começar com o Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad).

2. Adicione membros da sua organização que serão responsáveis por utilizar o serviço para o inquilino. Atribua a cada utilizador no inquilino uma das seguintes funções para controlar o nível de acesso para VaaS:

    | Nome da Função | Descrição |
    |---------------------|------------------------------------------|
    | Proprietário | Tem acesso total a todos os recursos. |
    | Leitor | Pode ver todos os recursos, mas não editar. |
    | Contribuinte de teste | Pode gerir os recursos de teste. |
    | Contribuinte de catálogo | Pode gerir os recursos de publicação de solução. |

## <a name="set-up-your-tenant"></a>Configurar o seu inquilino

Configurar o seu inquilino no **serviço de validação do Azure Stack** aplicação. 

1. Enviar as seguintes informações sobre o inquilino para a Microsoft em vaashelp@microsoft.com.

    | Dados | Descrição |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Nome da Organização | Nome da organização oficial. |
    | Nome de diretório do inquilino do Azure AD | A ser registado um nome de diretório de inquilino do AD do Azure. |
    | Id de diretório do inquilino do Azure AD | O Azure AD inquilino Directory GUID associada ao diretório.<br> Para obter informações sobre como encontrar o ID do inquilino do Azure AD Directory pode ser encontrado, consulte "[obter ID de inquilino](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id)." |

    

2. A equipa do Azure Stack fornece a confirmação de que o seu inquilino pode utilizar o portal de VaaS.

3. Utiliza as credenciais de administrador global do inquilino para iniciar sessão para o [VaaS portal](https://azurestackvalidation.com/
). Selecione **minha conta**.

    ![Inicie sessão no portal do VaaS](media/vaas_portalsignin.png)

3. O site será solicitado a conceder acesso para VaaS. Aceite os termos para continuar.

## <a name="assign-user-roles"></a>Atribuir funções de utilizador

Para atribuir uma função de utilizador:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **do Azure Active Directory** no **identidade** grupo.
3. Selecione **aplicações empresariais** > **serviço de validação do Azure Stack** aplicação.
4. Selecionar **Utilizadores e grupos**. O **serviço do Azure Stack validação - os utilizadores e grupo** painel mostra os utilizadores com permissão para utilizar a aplicação.
5. Selecione **+ adicionar utilizador** para adicionar uma atribuição.

## <a name="create-an-azure-storage-blob-to-store-logs"></a>Criar um blob de armazenamento do Azure para armazenar os registos

VaaS cria registos de diagnóstico ao executar os testes de validação. Tem uma localização de um URL de SAS para armazenar os registos do serviço de Blobs do Azure. A conta de armazenamento também pode ser utilizadas para os pacotes de personalização do arquivo de OEM.

Estes passos irão guiá como configurar e gerar um URI de armazenamento-como-serviço (SAS) para uma conta de armazenamento do Azure e onde especificar a conta de armazenamento no portal do VaaS ao iniciar os testes no portal.

### <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure

1. Para criar uma conta de armazenamento, siga as instruções [criar uma conta de armazenamento](../../storage/common/storage-quickstart-create-account.md).

2. Ao selecionar o tipo de conta de armazenamento, selecione o **armazenamento de BLOBs** tipo de conta.

### <a name="generate-a-sas-url-for-the-storage-account"></a>Gerar um URL de SAS para a conta de armazenamento

1. Navegue para a conta de armazenamento que criou acima.

2. No painel, em **configurações**, selecione **assinatura de acesso partilhado**.

3. Apenas verifique **Blob** partir **opções de serviços permitidos** (desmarque os restantes).

4. Verifique **serviço**, * * contêiner, e **objeto** da **tipos de recurso permitidos**.

5. Verifique **leitura**, **escrever**, **lista**, **adicionar**, **criar** de **permissões**  (desmarque os restantes).

6. Definir **hora de início** para a hora atual, e **hora de fim** para três meses a partir da hora atual.

7. Selecione **cadeia de ligação e gerar SAS** e guarde o **URL de SAS de serviço Blob** cadeia de caracteres.

> [!Note]  
> O URL de SAS expira ao fim na hora de fim definida quando o URL foi gerado. Certifique-se de que o URL é suficientemente válido antes de partilhá-lo com a equipa de produto para depuração, ou que o URL é válido durante mais de 30 dias, durante o agendamento de testes.

## <a name="next-steps"></a>Passos Seguintes

- Utilize o agente local VaaS para verificar se o seu hardware. Para obter instruções, consulte [implementar as máquinas de virtuais locais do agente e o teste](azure-stack-vaas-test-vm.md).
- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).