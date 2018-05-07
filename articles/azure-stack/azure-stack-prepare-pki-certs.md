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
ms.openlocfilehash: 934585082e2832c41885874c82ab43d64a1fa361
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="prepare-azure-stack-pki-certificates-for-deployment"></a>Preparar os certificados PKI de pilha do Azure para a implementação
Os ficheiros de certificado [obtido a partir da sua AC escolhidas](azure-stack-get-pki-certs.md) tem de ser importados e exportados com propriedades correspondentes requisitos de certificado da pilha do Azure.


## <a name="prepare-certificates-for-deployment"></a>Preparar os certificados para implementação
Utilize estes passos para preparar e validar os certificados PKI de pilha do Azure: 

### <a name="import-the-certificate"></a>Importe o certificado

1.  Copiar as versões do certificado original [obtido a partir da sua AC escolhidas](azure-stack-get-pki-certs.md) para um diretório no anfitrião de implementação. 
  > [!WARNING]
  > Não copie os ficheiros que já foram importados, exportados ou alterados de qualquer forma dos ficheiros fornecidos diretamente pela AC.

2.  Faça duplo clique no certificado e selecione **instalar certificado** ou **Instalar PFX** consoante a forma como o certificado foi entregue da AC.

3. No **Assistente Importar certificado**, selecione **máquina Local** como a localização de importação. Selecione **Seguinte**. No ecrã seguinte, clique em seguinte novamente.

    ![Localização de importação de máquina local](.\media\prepare-pki-certs\1.png)

4.  Escolha **local em todos os certificados no seguinte arquivo** e, em seguida, selecione **Enterprise confiar** como a localização. Clique em **OK** para fechar a caixa de diálogo de seleção de arquivo de certificados e, em seguida, **seguinte**.

    ![Configurar o arquivo de certificados](.\media\prepare-pki-certs\3.png)

    a. Se estiver a importar um PFX será apresentada uma caixa de diálogo adicionais. No **proteção por chave privada** página, introduza a palavra-passe para os ficheiros de certificado e, em seguida, ative o **marcar esta chave como exportável. Isto permite-lhe criar cópias de segurança ou transporte das chaves posteriormente** opção. Selecione **Seguinte**.

    ![Marcar chave como exportável](.\media\prepare-pki-certs\2.png)

5. Clique em Concluir para concluir a importação.

### <a name="export-the-certificate"></a>Exportar o certificado

Abra a consola do MMC do Gestor de certificados e ligar ao arquivo de certificados de computador Local.

1. Abra a consola de gestão da Microsoft, no Windows 10 com o botão direito no Menu Iniciar, clique em executar. Tipo **mmc** clique em ok.

2. Clique em ficheiro, adicionar/remover Snap-In, em seguida, selecionados de certificados, clique em Adicionar.

    ![Adicionar Snap-in de certificados](.\media\prepare-pki-certs\mmc-2.png)
 
3. Selecione a conta de computador, clique em seguinte, em seguida, selecione o computador Local, em seguida, concluir. Clique em ok para fechar a página Adicionar/Remover Snap-In.

    ![Adicionar Snap-in de certificados](.\media\prepare-pki-certs\mmc-3.png)

4. Procurar certificados > Enterprise confiar > localização do certificado. Certifique-se de que vê o certificado à direita.

5. Na consola de Gestor de tarefas barra de certificado, selecione **ações** > **todas as tarefas** > **exportar**. Selecione **Seguinte**.

  > [!NOTE]
  > Dependendo de quantos Azure pilha certificados que tem, poderão ter de concluir este processo mais do que uma vez.

4. Selecione **Sim, exportar a chave privada**e, em seguida, clique em **seguinte**.

5. Na secção de formato de ficheiro de exportação, selecione **exportar todas as propriedades expandidas** e, em seguida, clique em **seguinte**.

6. Selecione **palavra-passe** e fornecer uma palavra-passe para os certificados. Memorize esta palavra-passe porque é utilizado como um parâmetro de implementação. Selecione **Seguinte**.

7. Escolha um nome de ficheiro e a localização do ficheiro pfx exportar. Selecione **Seguinte**.

8. Selecione **Concluir**.

## <a name="next-steps"></a>Passos Seguintes
[Validar os certificados PKI](azure-stack-validate-pki-certs.md)