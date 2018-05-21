---
title: Funcionalidades de pedido de dados de cliente no Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: iot-central
ms.openlocfilehash: 0e348ca9ca85da7d4734a57afac4e5bb27217eae
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados de cliente

Centro de IoT do Azure é uma solução de software-como-um-serviço de Internet das coisas (IoT) completamente gerida que torna mais fácil ligar, monitorizar, gerir os recursos de IoT à escala, criar conhecimentos aprofundados dos seus dados do IoT e tomar medidas informadas.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Identificar dados de cliente

Azure Active Directory-os ID do objecto são utilizados para identificar os utilizadores e atribuir funções. Os endereços de e-mail de utilizador do Azure IoT Central apresenta portal para atribuições de funções, mas apenas o Azure Active Directory-ID de objeto são armazenados, o endereço de e-mail dinamicamente está a ser consultado do Azure Active Directory. Administradores do Centro de IoT do Azure podem ver, exportar e eliminar utilizadores da aplicação na secção de administração de utilizador de uma aplicação do Azure IoT Central.

Na aplicação, os endereços de correio eletrónico podem ser configurados para receber alertas. Neste caso, os endereços de e-mail são armazenados no Centro de IoT e tem de ser geridos a partir da página de administração de conta na aplicação.

Sobre dispositivos, a Microsoft não mantém nenhuma informação e não tem acesso aos dados que permite a correlação de utilizador do dispositivo. Muitos dos dispositivos geridos no Azure IoT Central não são dispositivos pessoais, por exemplo, uma máquina de distribuidores automáticos ou café maker. Os clientes podem, no entanto, considere alguns dos dispositivos pessoais e os respetivos critério podem manter os seus próprios asset ou inventário de sistemas que associar dispositivos a indivíduos de controlo. Centro de IoT do Azure gere e armazena todos os dados associados a dispositivos como se fosse dados pessoais.

Quando utilizar os serviços do Microsoft enterprise, a Microsoft gera algumas informações, conhecidas como registos gerados pelo sistema. Estes registos constituem factual ações a realização de um serviço e dados de diagnóstico relacionadas com a dispositivos individuais e não estão relacionadas com a atividade do utilizador. Azure IoT Central gerado pelo sistema os registos não são acessíveis ou exportável por administradores de aplicação.

## <a name="deleting-customer-data"></a>Eliminar dados de cliente

A capacidade de eliminar dados de utilizador só é fornecida através da página de Administração Central de IoT. Os administradores de aplicação, podem selecionar o utilizador pretende eliminar e clique em **eliminar** no canto superior direito da aplicação a eliminar o registo. Administradores de aplicações podem também remover contas individuais que já não estão associadas a aplicação em questão.

Depois de um utilizador for eliminado, não existem mais alertas são enviados por e-mail aos mesmos. No entanto, os seus endereços de e-mail tem de ser individualmente removido do cada alerta configurada.

Para obter mais informações, consulte [configurar regras e ações para o seu dispositivo](tutorial-configure-rules.md).

## <a name="exporting-customer-data"></a>Exportar dados de cliente

A capacidade para exportar dados só é fornecida através da página de Administração Central de IoT. Dados de cliente, incluindo funções atribuídas, podem ser selecionados, copiar e colar por um administrador da aplicação.

## <a name="links-to-additional-documentation"></a>Ligações para documentação adicional

Para obter mais informações sobre a administração de conta, incluindo definições de funções, consulte [como administrar a sua aplicação](howto-administer.md).
