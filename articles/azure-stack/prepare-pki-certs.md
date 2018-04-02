---
title: Preparar os certificados de infraestrutura de chave pública do Azure pilha para a implementação de sistemas de pilha do Azure integrado | Microsoft Docs
description: Descreve como preparar os certificados PKI de pilha do Azure para sistemas de pilha do Azure integrado.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: dadb443f8b7739e3a18c0d3beb558d8c42e9d19c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Preparar os certificados PKI de pilha do Azure para a implementação
Os ficheiros de certificado [obtido a partir da sua AC escolhidas](azure-stack-get-pki-certs.md) tem de ser importados e exportados com propriedades correspondentes requisitos de certificado da pilha do Azure.


## <a name="prepare-certificates-for-deployment"></a>Preparar os certificados para implementação
Utilize estes passos para preparar e validar os certificados PKI de pilha do Azure: 

1.  Copiar as versões do certificado original [obtido a partir da sua AC escolhidas](azure-stack-get-pki-certs.md) para um diretório no anfitrião de implementação. 
  > [!WARNING]
  > Não copie os ficheiros que já foram importados, exportados ou alterados de qualquer forma dos ficheiros fornecidos diretamente pela AC.

2.  Importe os certificados no arquivo de certificados de computador Local:

    a.  Faça duplo clique no certificado e selecione **Instalar PFX**.

    b.  No **Assistente Importar certificado**, selecione **máquina Local** como a localização de importação. Selecione **Seguinte**.

    ![Localização de importação de máquina local](.\media\prepare-pki-certs\1.png)

    c.  Selecione **seguinte** no **Escolher ficheiro a importar** página.

    d.  No **proteção por chave privada** página, introduza a palavra-passe para os ficheiros de certificado e, em seguida, ative o **marcar esta chave como exportável. Isto permite-lhe criar cópias de segurança ou transporte das chaves posteriormente** opção. Selecione **Seguinte**.

    ![Marcar chave como exportável](.\media\prepare-pki-certs\2.png)

    e.  Escolha **local em todos os certificados no seguinte arquivo** e, em seguida, selecione **Enterprise confiar** como a localização. Clique em **OK** para fechar a caixa de diálogo de seleção de arquivo de certificados e, em seguida, **seguinte**.

    ![Configurar o arquivo de certificados](.\media\prepare-pki-certs\3.png)

  f.    Clique em **concluir** para concluir o Assistente de importação de certificados.

  g.    Repetir o processo para todos os certificados que está a fornecer para a sua implementação.

3. Exporte o certificado para o formato do ficheiro PFX com requisitos do Azure pilha:

  a.    Abra a consola do MMC do Gestor de certificados e ligar ao arquivo de certificados de computador Local.

  b.    Vá para o **Enterprise confiar** diretório.

  c.    Selecione um dos certificados que importou no passo 2 acima.

  d.    Na consola de Gestor de tarefas barra de certificado, selecione **ações** > **todas as tarefas** > **exportar**.

  e.    Selecione **Seguinte**.

  f.    Selecione **Sim, exportar a chave privada**e, em seguida, clique em **seguinte**.

  g.    Na secção de formato de ficheiro de exportação, selecione **exportar todas as propriedades expandidas** e, em seguida, clique em **seguinte**.

  h.    Selecione **palavra-passe** e fornecer uma palavra-passe para os certificados. Memorize esta palavra-passe porque é utilizado como um parâmetro de implementação. Selecione **Seguinte**.

  i.    Escolha um nome de ficheiro e a localização do ficheiro pfx exportar. Selecione **Seguinte**.

  j.    Selecione **Concluir**.

  k.    Repita este processo para todos os certificados importados para a sua implementação no passo 2 acima.

## <a name="next-steps"></a>Passos Seguintes
[Validar os certificados PKI](validate-pki-certs.md)