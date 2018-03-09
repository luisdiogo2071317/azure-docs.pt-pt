---
title: "Base de dados SQL do Azure gerida configuração de VNet instância | Microsoft Docs"
description: "Este tópico descreve as opções de configuração para uma rede virtual (VNet) com uma instância de gerido da base de dados do Azure SQL."
services: sql-database
author: srdjan-bozovic
manager: cguyer
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 1a839a9bb2355da9451816828f6f9f0e99f43f5e
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Configurar uma VNet para instância gerida de base de dados SQL do Azure

Instância do Azure SQL da base de dados geridos (pré-visualização) tem de ser implementada dentro de um Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md). Esta implementação permite que os seguintes cenários: 
- Ligar a uma instância de geridos diretamente formam uma rede no local 
- Ligar uma instância de gerido para o servidor ligado ou para outro local arquivo de dados 
- Ligar uma instância gerida nos recursos do Azure  

## <a name="plan"></a>Planear

Planeie como implementar uma instância geridos na rede virtual com as suas respostas às seguintes perguntas: 
- Planeia implementar única ou várias instâncias gerido? 

  O número de instâncias geridas determina o tamanho mínimo da sub-rede para alocar para as instâncias geridas. Para obter mais informações, consulte [determinar o tamanho de sub-rede para a instância geridos](#create-a-new-virtual-network-for-managed-instances). 
- É necessário implementar a sua instância gerido numa rede virtual existente ou estiver a criar uma nova rede? 

   Se planeia utilizar uma rede virtual existente, tem de modificar a que a configuração da rede para acomodar a sua instância geridos. Para obter mais informações, consulte [modificar a rede virtual existente para a instância geridos](#modify-an-existing-virtual-network-for-managed-instances). 

   Se planear criar nova rede virtual, consulte o artigo [criar nova rede virtual para a instância geridos](#create-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Requisitos

Para a criação de instância geridos necessário dedicar sub-rede dentro da VNet que está em conformidade com os seguintes requisitos:
- **Estar em branco**: A sub-rede não pode conter qualquer outro serviço de nuvem associado à mesma e não pode ser sub-rede do Gateway. Não será possível criar instância geridos na sub-rede que contém os recursos que não seja gerido instância ou adicione outros recursos dentro da sub-rede mais tarde.
- **Nenhum NSG**: A sub-rede não pode ter um grupo de segurança de rede associado.
- **Tem a tabela de rotas específicas**: A sub-rede tem de ter uma tabela de rota de utilizador (UDR) com Internet de salto seguinte 0.0.0.0/0 como a rota apenas atribuída. Para obter mais informações, consulte [criar a tabela de rotas necessária e associe-a](#create-the-required-route-table-and-associate-it)
3. **DNS personalizado opcional**: Se não for especificado DNS personalizado na VNet, endereço IP do Azure recursiva resoluções (por exemplo, 168.63.129.16) tem de ser adicionado à lista. Para obter mais informações, consulte [configurar DNS de personalizado](sql-database-managed-instance-custom-dns.md).
4. **Nenhum ponto final de serviço**: A sub-rede não pode ter um ponto final de serviço (armazenamento ou Sql) associado ao mesmo. Certifique-se de que a opção de pontos finais de serviço está desativada quando criar a VNet.
5. **Endereços IP suficientes**: A sub-rede tem de ter pelo menos 16 endereços IP. Para obter mais informações, consulte [determinar o tamanho de sub-rede para as instâncias geridas](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Não poderá implementar a nova instância geridos se a sub-rede de destino não é compatível com todos os requisitos anteriores. O destino de Vnet e sub-rede têm de ser mantidos em conformidade com esses requisitos de instância geridos (antes e após a implementação) como qualquer violação pode fazer com que a instância para o estado defeituoso e ficar indisponíveis. Recuperar a partir de que o estado requer a criar uma nova instância numa VNet com as políticas de rede compatível, recrie dados ao nível da instância e restaurar as bases de dados. Isto apresenta um período de indisponibilidade significativo para as suas aplicações.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Determinar o tamanho de sub-rede para as instâncias geridas

Quando cria uma instância gerido, o Azure atribui um número de máquinas virtuais, consoante o tamanho de camada que selecionar durante o aprovisionamento. Porque estas máquinas virtuais associados à sub-rede, necessitam de endereços IP. Para garantir a elevada disponibilidade durante operações normais e manutenção do serviço Azure pode alocar máquinas virtuais adicionais. Como resultado, o número de endereços IP necessários numa sub-rede é maior do que o número de instâncias de geridos nessa sub-rede. 

Por predefinição, uma instância gerido necessita de um mínimo de 16 endereços IP numa sub-rede e pode utilizar até 256 endereços IP. Como resultado, pode utilizar máscaras de sub-rede /28 para /24 quando definir os intervalos IP da sub-rede. 

Se planeia implementar várias instâncias gerido dentro da sub-rede e precisa otimizar o tamanho da sub-rede, utilize estes parâmetros para formar um cálculo: 

- Azure utiliza cinco endereços IP na sub-rede para as suas próprias necessidades 
- Cada instância de objetivo geral tem dois endereços 

**Exemplo**: planear ter oito instâncias geridas. Que significa que terá de 5 + 8 * 2 = 21 endereços IP. Quando os intervalos de IP definidos no potência de 2, é necessário o intervalo de IP de 32 (2 ^ 5) endereços IP. Por conseguinte, terá de reservar a sub-rede com máscara de sub-rede de/27. 

## <a name="create-a-new-virtual-network-for-managed-instances"></a>Criar uma nova rede virtual para as instâncias geridas 

Criar uma rede virtual do Azure é um pré-requisito para a criação de uma instância geridos. Pode utilizar o portal do Azure, [PowerShell](../virtual-network/quick-create-powershell.md), ou [CLI do Azure](../virtual-network/quick-create-cli.md). A secção seguinte mostra os passos no portal do Azure. Os detalhes aqui descritos se aplicam a cada um destes métodos.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize e, em seguida, clique em **rede Virtual**, certifique-se a **Resource Manager** está selecionado como o modo de implementação e, em seguida, clique em **criar**.

   ![criar rede virtual](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Preencha o formulário de rede virtual com as informações pedidas, de forma semelhante a captura de ecrã seguinte:

   ![formulário de criar rede virtual](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Clique em **Criar**.

   O espaço de endereços e a sub-rede estão especificados na notação CIDR. 

   > [!IMPORTANT]
   > Os valores predefinidos criar sub-rede, que demora a todo o espaço de endereço VNet. Se escolher esta opção, não pode criar quaisquer outros recursos dentro da rede virtual que não sejam geridos por instância. 

   A abordagem recomendada seria o seguinte: 
   - Calcular o tamanho da sub-rede seguindo [determinar o tamanho de sub-rede para a instância geridos](#determine-the-size-of-subnet-for-managed-instances) secção  
   - Avaliar as necessidades para o resto da VNet 
   - Preencha os intervalos de endereços VNet e sub-rede em conformidade 

   Certifique-se de que os pontos finais de serviço opção permanece **desativado**. 

   ![formulário de criar rede virtual](./media/sql-database-managed-instance-tutorial/service-endpoint-disabled.png)

## <a name="create-the-required-route-table-and-associate-it"></a>Criar a tabela de rotas necessária e associe-a

1. Iniciar sessão no portal do Azure  
2. Localize e, em seguida, clique em **tabela de rotas**e, em seguida, clique em **criar** na página de tabela de rota.

   ![tabela de rotas criar formulário](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

3. Crie uma rota de Internet de salto seguinte 0.0.0.0/0, de forma como as capturas de ecrã seguintes:

   ![adicionar a tabela de rotas](./media/sql-database-managed-instance-tutorial/route-table-add.png)

   ![Rota](./media/sql-database-managed-instance-tutorial/route.png)

4. Associe esta rota de sub-rede para a instância geridos, de forma como as capturas de ecrã seguintes:

    ![sub-rede](./media/sql-database-managed-instance-tutorial/subnet.png)

    ![tabela de rotas de conjunto](./media/sql-database-managed-instance-tutorial/set-route-table.png)

    ![conjunto de tabela de rota guardado](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)


Quando tiver sido criada a sua VNet, está pronto para criar a sua instância geridos.  

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Modificar uma rede virtual existente para as instâncias geridas 

As perguntas e respostas nesta secção mostram como adicionar uma instância de gerido para a rede virtual existente. 

**Está a utilizar clássico ou do Gestor de recursos do modelo de implementação para a rede virtual existente?** 

Só pode criar uma instância geridos em redes virtuais do Gestor de recursos. 

**Gostaria de criar nova sub-rede para a instância de SQL geridos ou utilizar um existente?**

Se gostaria de criar uma nova: 

- Calcular o tamanho da sub-rede ao seguir as diretrizes no [determinar o tamanho de sub-rede para as instâncias geridas](#determine-the-size-of-subnet-for-managed-instances) secção.
- Siga os passos em [adicionar, alterar ou eliminar uma sub-rede de rede virtual](../virtual-network/virtual-network-manage-subnet.md). 
- Criar uma tabela de rota que contém uma entrada único, **0.0.0.0/0**, uma vez que o próximo salto Internet e associá-lo com a sub-rede para a instância geridos.  

No caso de que pretende criar uma instância gerido dentro de uma sub-rede existente: 
- Verifique se a sub-rede está vazia - não é possível criar uma instância gerido numa sub-rede que contém outros recursos, incluindo a sub-rede do Gateway 
- Calcular o tamanho da sub-rede ao seguir as diretrizes no [determinar o tamanho de sub-rede para as instâncias geridas](#determine-the-size-of-subnet-for-managed-instances) secção e certifique-se de que está corretamente dimensionado. 
- Verifique se os pontos finais de serviço não estão ativados na sub-rede.
- Certifique-se de que não existem grupos de segurança de rede associados à sub-rede 

**Tem servidores DNS personalizados configurados?** 

Se Sim, consulte o artigo [configurar um DNS personalizado](sql-database-managed-instance-custom-dns.md). 

- Criar a tabela de rotas necessária e associe-o: consulte [criar a tabela de rotas necessária e associe-a](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral, consulte [o que é uma instância geridos](sql-database-managed-instance.md)
- Para um tutorial que mostra como criar uma VNet, criar uma instância geridos e restaurar uma base de dados a partir de uma cópia de segurança da base de dados, consulte [criar uma instância do Azure SQL da base de dados geridos](sql-database-managed-instance-tutorial-portal.md).
- Para problemas DNS, consulte [configurar um DNS personalizado](sql-database-managed-instance-custom-dns.md)
