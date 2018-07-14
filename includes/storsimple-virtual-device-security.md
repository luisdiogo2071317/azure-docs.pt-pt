<!--v-sharos 10/13/2105 virtual device security-->

Mantenha as seguintes considerações de segurança em mente ao utilizar o dispositivo virtual StorSimple:

* O dispositivo virtual é protegido através da subscrição do Microsoft Azure. Isso significa que se estiver a utilizar o dispositivo virtual e a sua subscrição do Azure for comprometida, os dados armazenados no seu dispositivo virtual também são suscetíveis.
* A chave pública do certificado utilizado para encriptar dados armazenados no Azure StorSimple em segurança esteja disponível para o portal clássico do Azure, e a chave privada é mantida com o dispositivo StorSimple. No dispositivo virtual StorSimple, as chaves públicas e privadas são armazenadas no Azure.
* O dispositivo virtual está alojado no datacenter do Microsoft Azure.

