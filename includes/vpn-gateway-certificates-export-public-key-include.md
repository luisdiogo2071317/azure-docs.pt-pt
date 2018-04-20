---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 7e19837c1d16ddeea185f340305a0c9c52ce23ff
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
Depois de criar um certificado de raiz autoassinado, exporte o ficheiro de. cer de chave pública do certificado de raiz (não a chave privada). Mais tarde, irá carregar este ficheiro para o Azure. Os seguintes passos ajudam a exportar o ficheiro. cer para o seu certificado de raiz autoassinado:

1. Para obter um ficheiro .cer a partir do certificado, abra **Gerir certificados de utilizador**. Localize o certificado de raiz autoassinado, normalmente em ''Certificates - Current User\Personal\Certificates'', e clique com o botão direito do rato. Clique em **Todas as Tarefas** e, em seguida, clique em **Exportar**. Esta ação abre o **Assistente para Exportar Certificados**.

  ![Exportar](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. No Assistente, clique em **Seguinte**.

  ![Exportar certificado](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. Selecione **Não, não exportar a chave privada** e, em seguida, clique em **Seguinte**.

  ![Não exportar a chave privada](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. Na página **Formato do Ficheiro de Exportação**, selecione **X.509 codificado com Base-64 (.CER).** e, em seguida, clique em **Seguinte**.

  ![Com codificação base-64](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. Para **ficheiro a exportar**, **procurar** para a localização para o qual pretende exportar o certificado. Em **Nome do ficheiro**, atribua um nome ao ficheiro de certificado. Clique depois em **Seguinte**.

  ![Procurar](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Clique em **Concluir** para exportar o certificado.

  ![Concluir](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. O certificado seja exportado com êxito.

  ![Êxito](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. O certificado exportado semelhante a isto:

  ![Exportado](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Se abrir o certificado exportado a utilizar o bloco de notas, verá algo semelhante a este exemplo. A secção azul contém as informações que são carregadas para o Azure. Se abrir o certificado com o bloco de notas e não ter um aspeto semelhante a isto, normalmente, isto significa não exportá-lo utilizando a Base-64 codificado x. 509 (. Formato CER). Além disso, se pretender utilizar um editor de texto diferente, tenha em atenção que alguns editores podem dar origem a formatação indesejado em segundo plano. Isto pode criar problemas ao carregar o texto deste certificado para o Azure.

  ![Abrir com o bloco de notas](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)