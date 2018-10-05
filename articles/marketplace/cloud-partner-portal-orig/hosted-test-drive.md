---
title: Hospedado Test-Drive | Documentos da Microsoft
description: Como configurar uma mantenha uma versão de teste de hospedado do Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 1ab9fcd50ad7081f8047d62e545287fa75db75e4
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48812096"
---
# <a name="hosted-test-drive"></a>Hospedado de teste de unidade

Uma versão de teste alojado retira a complexidade da configuração pelo alojamento da Microsoft e manter o serviço que executa o utilizador de teste de unidade de aprovisionamento e desaprovisionamento. Este artigo destina-se os publicadores com a oferta no AppSource ou estiver a criar um novo e que pretendem oferecer uma versão de teste de alojado, que liga a um do Dynamics 365 for Customer Engagement, do Dynamics 365 para finanças e operações ou o Dynamics 365 Business Central instância.

## <a name="how-to-publish-a-test-drive"></a>Como publicar um Test Drive

Navegue para a oferta existente ou crie uma nova oferta.

Selecione a opção de Test-Drive do menu do lado.

Selecione \'Sim\' para \'ativar um Test Drive\' opção.

Fornecer os campos seguintes no \'detalhes\' secção.

- **Descrição**: forneça uma descrição geral da sua versão de teste. Esse texto será apresentado ao utilizador enquanto o Test-Drive está a ser aprovisionado. Este campo suporta HTML, se quiser fornecer conteúdo formatado.
- **Manual do usuário**: carregue um manual de utilizador detalhadas (ficheiro de. pdf do tipo) que ajuda os utilizadores de Test-Drive compreender como utilizar a sua aplicação.
- **Teste de unidade o vídeo de demonstração**: Opcionalmente, carregar um vídeo que demonstra a sua aplicação.

Permissão de concessão AppSource para aprovisionar e desaprovisionar utilizadores de Test-Drive do seu inquilino ao seguir as instruções localizadas [aqui](https://github.com/Microsoft/AppSource/blob/patch-1/Microsoft%20Hosted%20Test%20Drive/Setup-your-Azure-subscription-for-Dynamics365-Microsoft-Hosted-Test-Drives.md).

Neste passo, irá gerar o \'Id de aplicação do Azure AD\' e \'chave de aplicação do Azure AD\' valores mencionado abaixo.

Fornecer os campos seguintes no \'técnico configuração\' secção:

- **Tipo de Test-Drive**: escolha \'Microsoft Hosted (exemplo do Dynamics 365 Customer Engagement)' opção. Isto indica que a Microsoft irá alojar e manter o serviço que executa o utilizador de teste de unidade de aprovisionamento e desaprovisionamento.
- **Max em simultâneo versões de teste**: defina este campo para o número de utilizadores em simultâneo que podem ter um Test Drive do Active Directory num determinado período de tempo. Cada utilizador irá consumir uma licença do Dynamics, enquanto a unidade de teste está ativa, por isso terá de garantir que tenha, pelo menos, tantos Dynamics licenças disponíveis para os utilizadores da versão de teste. Valor recomendado de 3 a 5.
- **Teste de unidade de duração (horas)**: defina este campo para o número de horas, os utilizadores a versão de teste ficará ativa para. Depois de tantas horas, o utilizador irá ser desaprovisionado do seu inquilino. Valor de 2 a 24 horas, consoante a complexidade da sua aplicação de recomendado. O usuário sempre pode pedir outro teste se ficar sem tempo e deseja acessar a unidade de teste novamente.
- **URL de instância**: forneça um URL que o utilizador de teste de unidade será inicialmente direcionado durante o arranque a versão de teste. Isso normalmente é o URL da sua instância do Dynamics 365 que tenha a sua aplicação e dados de exemplo instalados no. Valor de exemplo:https://testdrive.crm.dynamics.com
- **ID de inquilino do Azure AD**: forneça o ID do inquilino do Azure para a sua instância do Dynamics 365. Para obter este valor, inicie sessão no portal do Azure e navegue para \'do Azure Active Directory\'  - \> selecione propriedades no painel de menu -\> copiar o ID de diretório. Valor de exemplo: 72f988bf-86f1-41af-91ab-2d7cd0111234
- **ID de aplicação do Azure AD**: ID da aplicação do AD Azure que criou no passo 7. \ valor de exemplo: 53852862-a2ae-4e43-9461-faa49650a096
- **Chave da aplicação do Azure AD**: segredo da aplicação do Azure AD criada no passo 7. \ valor de exemplo: IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk =
- **Nome de inquilino do Azure AD**: forneça o nome do inquilino do Azure para a sua instância do Dynamics 365. Utilize o formato \<tenantname.\> onmicrosoft.com. Valor de exemplo: testdrive.onmicrosoft.com
- **URL da Web API de instâncias**: forneça o URL de Web API para a sua instância do Dynamics 365. Pode obter este valor ao iniciar sessão na sua instância do Microsoft Dynamics 365 e navegar para a definição -\> personalização -\> recursos para desenvolvedores -\> instância Web API (copiar esta URL). Valor de exemplo:  https://testdrive.crm.dynamics.com/api/data/v9.0 
- **Nome da função**: forneça o nome da função de segurança do Dynamics 365 personalizada que criou para a versão de teste. Esta é a função que será atribuída aos utilizadores durante a sua versão de teste. Valor de exemplo: testdriverole

## <a name="next-steps"></a>Passos Seguintes

Quando pronto **publicar** sua oferta, depois da aplicação foi aprovada a certificação, terá um **pré-visualização** da sua oferta. Iniciar uma versão de teste na interface de Usuário e certifique-se de que as versões de teste estão sendo executadas corretamente. Assim que se sentir à vontade com a sua oferta de pré-visualização, agora é hora de **ir ao vivo!**
