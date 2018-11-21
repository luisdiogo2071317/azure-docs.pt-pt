---
title: Exportar e eliminar os dados do Machine Learning Studio - Azure | Documentos da Microsoft
description: Dados de produto armazenados pelo Azure Machine Learning Studio estão disponíveis para a exportação e eliminação através do portal do Azure e também através de APIs de REST autenticados. Dados de telemetria podem ser acedidos através do Portal de privacidade do Azure. Este artigo mostra-lhe como.
services: machine-learning
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: 04b26dc9ad9f4b7a4feb03039d92854385d75465
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262378"
---
# <a name="export-and-delete-in-product-user-data-from-machine-learning-studio"></a>Exportar e eliminar dados de utilizador no produto de Machine Learning Studio

Pode eliminar ou exportar no produto dados armazenados por Azure Machine Learning Studio através do portal do Azure, a interface do Studio, PowerShell e autenticado REST APIs. Este artigo mostra-lhe como. 

Dados de telemetria podem ser acedidos através do portal de privacidade do Azure. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-collect"></a>Quais são os tipos de dados de utilizador recolher o Studio?

Para este serviço, dados de utilizador consiste em informações sobre os utilizadores autorizados a aceder a áreas de trabalho e registos de telemetria de interações de utilizador com o serviço.

Existem dois tipos de dados de utilizador no Machine Learning Studio:
- **Dados de conta pessoal:** IDs de conta e endereços de e-mail associados com uma conta.
- **Dados do cliente:** dados carregados para analisar.

## <a name="studio-account-types-and-how-data-is-stored"></a>Tipos de conta de Studio e o modo como os dados são armazenados

Existem três tipos de contas no Machine Learning Studio. O tipo de conta que tiver determina como os dados são armazenados e como pode eliminar ou exportá-lo.

- R **área de trabalho de convidado** é uma conta gratuita e anônima. Inscrever-se sem fornecer credenciais, tal como um endereço de e-mail ou palavra-passe.
    -  Os dados são removidos depois de expira a área de trabalho de convidado.
    - Os utilizadores convidados podem exportar dados de clientes através do pacote da interface do Usuário, as APIs REST ou do PowerShell.
- R **área de trabalho gratuita** é uma conta gratuita que iniciar sessão no Microsoft credenciais - um endereço de e-mail e palavra-passe da conta.
    - Pode exportar e eliminar os dados pessoais e de cliente, que estão sujeitos a pedidos de (DSR direitos) dos dados.
    - Pode exportar dados de clientes através do pacote da interface do Usuário, as APIs REST ou do PowerShell.
    - Gratuitamente as áreas de trabalho, não a utilizar contas do Azure AD, telemetria pode ser exportada com o Portal de privacidade.
    - Ao eliminar a área de trabalho, excluir todos os dados de pessoal de cliente.
- R **área de trabalho standard** é uma conta paga aceder com credenciais de início de sessão.
    - Pode exportar e eliminar os dados pessoais e de cliente, que estão sujeitos a pedidos DSR.
    - Pode acessar dados através do portal de privacidade do Azure
    - Pode exportar os dados pessoais e de cliente através do pacote da interface do Usuário, as APIs REST ou o PowerShell
    - Pode eliminar os dados no portal do Azure.

## <a name="delete-workspace-data-in-studio"></a>Eliminar dados de área de trabalho do Studio 

### <a name="delete-individual-assets"></a>Eliminar recursos individuais

Os utilizadores podem eliminar os recursos numa área de trabalho selecionando-os e, em seguida, selecionar o botão de eliminação.

![Eliminar recursos no Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Eliminar uma área de trabalho inteira

Os utilizadores também podem eliminar a sua área de trabalho inteira:
- Paga a área de trabalho: eliminar através do portal do Azure.
- Área de trabalho gratuita: Utilize o botão Eliminar no **definições** painel.

![Eliminar uma área de trabalho gratuita no Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-data-with-powershell"></a>Exportar dados do Studio com o PowerShell
Utilize o PowerShell para exportar todas as suas informações num formato portáteis de utilizar comandos do Azure Machine Learning Studio. Para obter informações, consulte a [módulo do PowerShell para o Azure Machine Learning](powershell-module.md) artigo.

## <a name="next-steps"></a>Passos Seguintes

Para obter documentação que abrangem serviços da web e o plano de compromisso de faturação, consulte [referência da API de REST do Azure Machine Learning](https://docs.microsoft.com/rest/api/machinelearning/). 
