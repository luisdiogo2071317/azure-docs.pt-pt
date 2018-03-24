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
ms.openlocfilehash: f860a0cebefaebe16c40abf008bd24e333eb80d5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
Se quiser criar uma ligação P2S a partir de um computador cliente sem ser o utilizado para gerar os certificados de cliente, tem de instalar um certificado de cliente. Ao instalar um certificado de cliente, vai precisar da palavra-passe que foi criada quando o certificado de cliente foi exportado.

1. Localize e copie o ficheiro *.pfx* para o computador cliente. No computador cliente, faça duplo clique no ficheiro *.pfx* para o instalar. Deixe a **Localização do Arquivo** como **Utilizador Atual** e, em seguida, clique em **Seguinte**.
2. Na página **Ficheiro a importar**, não efetue nenhuma alteração. Clique em **Seguinte**.
3. No **proteção por chave privada** página, introduza a palavra-passe para o certificado ou certifique-se de que o principal de segurança está correto e clique em **seguinte**.
4. Na página **Arquivo de Certificados**, mantenha a localização predefinida e, em seguida, clique em **Seguinte**.
5. Clique em **Concluir**. Em **Aviso de Segurança** para a instalação do certificado, clique em **Sim**. Pode sentir-se à vontade para clicar em ''Sim'', uma vez que gerou o certificado. O certificado foi agora importado com êxito.