Uma máquina virtual do Azure suporta anexar um número de discos de dados. Este artigo descreve as metas de escalabilidade e desempenho para discos de dados de uma VM. Utilize esses destinos para ajudar a decidir o número e tipo de disco que tem de satisfazer os seus requisitos de desempenho e a capacidade. 

> [!IMPORTANT]
> Para otimizar o desempenho, limite o número de discos muito utilizados anexados à máquina virtual para evitar uma possível limitação. Se todos os discos ligados não são muito utilizados ao mesmo tempo, em seguida, a máquina virtual pode suportar um grande número de discos.

* **Para o Azure Managed Disks:** 

> | Recurso | Limite Predefinido | Limite Máximo |
> | --- | --- | --- |
> | Managed Disks Standard | 10,000 | 50,000 |
> | Managed Disks SSD Standard | 10,000 | 50,000 |
> | Managed Disks Premium | 10,000 | 50,000 |
> | Instantâneos de Standard_LRS | 10,000 | 50,000 |
> | Instantâneos de Standard_ZRS | 10,000 | 50,000 |
> | Instantâneos de Premium_LRS | 10,000 | 50,000 |
> | Imagem gerida | 10,000 | 50,000 |

* **Para contas de armazenamento standard:** uma conta de armazenamento standard tem uma taxa de pedidos total máxima de 20.000 IOPS. O IOPS total em todos os discos da máquina virtual numa conta de armazenamento standard não deve exceder este limite.
  
    Pode calcular aproximadamente o número de discos muito utilizados suportados por uma única conta de armazenamento standard com base no limite da taxa de pedido. Por exemplo, para uma VM de escalão básico, o número máximo de discos muito utilizados é cerca de 66 (20.000/300 IOPS por disco) e para uma VM de escalão Standard, é cerca de 40 (20.000/500 IOPS por disco). 

* **Para contas de armazenamento premium:** uma conta de armazenamento premium tem uma taxa de débito total máximo de 50 Gbps. O débito total em todos os discos da VM não deve exceder este limite.

