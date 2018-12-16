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
ms.openlocfilehash: 3c6485406c67bf84b9e0fdfb9f4683abe5062d6c
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444307"
---
Depois de criar um certificado de raiz autoassinado, exporte o ficheiro. cer de chave pública do certificado de raiz (não a chave privada). Mais tarde irá carregar o ficheiro para o Azure. Os passos seguintes ajudam-na exportar o ficheiro. cer para o seu certificado de raiz autoassinado:

1. Para obter um ficheiro .cer a partir do certificado, abra **Gerir certificados de utilizador**. Localize o certificado de raiz autoassinado, normalmente em ''Certificates - Current User\Personal\Certificates'', e clique com o botão direito do rato. Clique em **Todas as Tarefas** e, em seguida, clique em **Exportar**. Esta ação abre o **Assistente para Exportar Certificados**. Se não é possível localizar o certificado em User\Personal\Certificates atual pode ser que abriu o Gestor de certificados para os certificados de computador local (título será "Certificados – Local computador" como em vez de "Certificados – utilizador atual"). Para abrir o Gestor de certificados no atual âmbito de utilizador inicie-o do PowerShell mesmo em que os certificados foram criados, escrevendo ```certmgr```.

   ![Exportar](./media/vpn-gateway-certificates-export-public-key-include/export.png)
2. No Assistente, clique em **Seguinte**.

   ![Exportar o certificado](./media/vpn-gateway-certificates-export-public-key-include/exportwizard.png)
3. Selecione **Não, não exportar a chave privada** e, em seguida, clique em **Seguinte**.

   ![Não exporte a chave privada](./media/vpn-gateway-certificates-export-public-key-include/notprivatekey.png)
4. Na página **Formato do Ficheiro de Exportação**, selecione **X.509 codificado com Base-64 (.CER).** e, em seguida, clique em **Seguinte**.

   ![Com codificação base 64](./media/vpn-gateway-certificates-export-public-key-include/base64.png)
5. Para **ficheiro a exportar**, **procurar** para a localização para o qual pretende exportar o certificado. Em **Nome do ficheiro**, atribua um nome ao ficheiro de certificado. Clique depois em **Seguinte**.

   ![Procurar](./media/vpn-gateway-certificates-export-public-key-include/browse.png)
6. Clique em **Concluir** para exportar o certificado.

   ![Concluir](./media/vpn-gateway-certificates-export-public-key-include/finish.png)
7. O certificado é exportado com êxito.

   ![Êxito](./media/vpn-gateway-certificates-export-public-key-include/success.png)
8. O certificado exportado será semelhante ao seguinte:

   ![Exportado](./media/vpn-gateway-certificates-export-public-key-include/exported.png)
9. Se abrir o certificado exportado usando o bloco de notas, verá algo semelhante a este exemplo. A seção em azul contém as informações que são carregadas para o Azure. Se abrir o certificado com o bloco de notas e não ser semelhante ao seguinte, normalmente, isso significa que não exportá-lo utilizando a Base 64 com codificação X.509 (. Formato CER). Além disso, se pretender utilizar um editor de texto diferente, compreenda que podem apresentar alguns editores de formatação não-intencionais em segundo plano. Isto pode provocar problemas quando carregado o texto deste certificado para o Azure.

   ![Abrir com o bloco de notas](./media/vpn-gateway-certificates-export-public-key-include/notepad.png)
