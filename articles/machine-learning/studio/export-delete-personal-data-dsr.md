---
title: Exportar e eliminar os dados do Machine Learning Studio - Azure | Microsoft Docs
description: Os dados no produto armazenados pelo Azure Machine Learning Studio estão disponíveis para exportar e eliminação através do portal do Azure bem como através de APIs REST do autenticado. Os dados telemétricos podem ser acedidos através do Portal de privacidade do Azure. Este artigo mostra-lhe como.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: 6317d4baba5775c1e5a4fda66de80dd8299d8fed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655609"
---
# <a name="export-and-delete-in-product-user-data-from-machine-learning-studio"></a>Exportar e eliminar dados de utilizador no produto do Machine Learning Studio

Pode eliminar ou exportação no produto dados armazenados pelo Azure Machine Learning Studio através do portal do Azure, a interface de Studio, o PowerShell e autenticado REST APIs. Este artigo mostra-lhe como. 

Os dados telemétricos podem ser acedidos através do portal do Azure privacidade. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-collect"></a>Os tipos de dados de utilizador recolher Studio?

Para este serviço, dados de utilizador é composta por informações sobre os utilizadores autorizados a aceder às áreas de trabalho e registos de telemetria de interações do utilizador com o serviço.

Existem dois tipos de dados de utilizador no Machine Learning Studio:
- **Dados de conta pessoal:** IDs de conta e endereços de e-mail associados uma conta.
- **Dados de cliente:** dados que carregou para analisar.

## <a name="studio-account-types-and-how-data-is-stored"></a>Tipos de conta Studio e o modo como os dados são armazenados

Existem três tipos de contas no Machine Learning Studio. O tipo de conta que tenha determina a forma como os dados são armazenados e como pode eliminar ou exportá-lo.

- A **área de trabalho de convidado** é uma conta gratuita, anónima. Inscrever-se sem fornecer as credenciais, tais como um endereço de e-mail ou palavra-passe.
    -  Os dados são removidos depois de expira a área de trabalho de convidado.
    - Os utilizadores convidados podem exportar dados de cliente através do pacote de IU, REST APIs ou PowerShell.
- A **área de trabalho gratuita** é uma conta gratuita que iniciar sessão com a Microsoft de credenciais - um endereço de correio eletrónico e palavra-passe de contas.
    - Pode exportar e eliminar os dados pessoais e de cliente, que estão sujeitos aos pedidos de direitos (DSR) do requerente de dados.
    - Pode exportar dados de cliente através do pacote de IU, REST APIs ou PowerShell.
    - Gratuitamente áreas de trabalho não utilizar contas do Azure AD, telemetria pode ser exportada através do Portal de privacidade.
    - Ao eliminar a área de trabalho, tem de eliminar todos os dados de cliente pessoal.
- A **área de trabalho standard** é uma conta paga aceder com credenciais de início de sessão.
    - Pode exportar e eliminar os dados pessoais e de cliente, que estão sujeitos a DSR pedidos.
    - Pode aceder a dados através do portal do Azure privacidade
    - Pode exportar os dados pessoais e de cliente através do pacote de IU, APIs ou o PowerShell
    - Pode eliminar os dados no portal do Azure.

## <a name="delete-workspace-data-in-studio"></a>Eliminar dados de área de trabalho no Studio 

### <a name="delete-individual-assets"></a>Eliminar recursos individuais

Os utilizadores podem eliminar recursos numa área de trabalho, selecionando-los e, em seguida, selecionar o botão Eliminar.

![Eliminar recursos no Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Eliminar uma área de trabalho completa

Os utilizadores também podem eliminar os respetivos área de trabalho completa:
- Paga área de trabalho: eliminar através do portal do Azure.
- Área de trabalho gratuita: Utilize o botão Eliminar no **definições** painel.

![Eliminar uma área de trabalho gratuita no Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-data-with-powershell"></a>Exportar dados do Studio com o PowerShell
Utilize o PowerShell para exportar todas as suas informações num formato portable de utilizar comandos do Azure Machine Learning Studio. Para informações, consulte o [módulo do PowerShell do Azure Machine Learning](powershell-module.md) artigo.

## <a name="next-steps"></a>Passos Seguintes

Para obter documentação que abrangem a serviços web e o plano de compromisso de faturação, consulte [referência da API de REST do Azure Machine Learning](https://docs.microsoft.com/en-us/rest/api/machinelearning/). 
