---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 12/07/2018
ms.author: alkohli
ms.openlocfilehash: 0a9aaa86d44e71e429f2bfff13a56ddcb1ee2071
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550555"
---
1. Inicie sessão no dispositivo do Data Box. Certifique-se de que ele está desbloqueado.

    ![Dashboard de caixa de dados](media/data-box-add-device-ip/data-box-connect-via-rest-1.png)

2. Aceda a **definir interfaces de rede**. Tome nota do endereço IP do dispositivo para a interface de rede utilizada para ligar ao cliente.

    ![Dashboard de caixa de dados](media/data-box-add-device-ip/data-box-connect-via-rest-2.png)

3. Aceda a **Connect e de cópia** e clique em **Rest (pré-visualização)**.

    ![Dashboard de caixa de dados](media/data-box-add-device-ip/data-box-connect-via-rest-3.png)

4. Do **armazenamento de acesso da conta e carregar dados** caixa de diálogo, copie a **ponto final de serviço Blob**.

    ![Dashboard de caixa de dados](media/data-box-add-device-ip/data-box-connect-via-rest-4.png)

5. Inicie **bloco de notas** como administrador e, em seguida, abra o **anfitriões** arquivo localizado em `C:\Windows\System32\Drivers\etc`.
6. Adicione a seguinte entrada para sua **anfitriões** ficheiro: `<device IP address> <Blob service endpoint>`
7. Para referência, utilize a imagem seguinte. Guardar a **anfitriões** ficheiro.

    ![Dashboard de caixa de dados](media/data-box-add-device-ip/data-box-connect-via-rest-5.png)
