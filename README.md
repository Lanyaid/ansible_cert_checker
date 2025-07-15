Este playbook sirve para checkear, analizar y sacar un reporte de todos los certificados usados en todas las plataformas de openAM (menos los certificados de firma).
  * certificados SSL para https de los tomcat
  * certificados de encryptacion de la communicacions (LDAPS 1636 o admin 4444)
  * certificados de encryptacion de la communicacions (Puerto de replicacion cuando la hay: 8989)

El playbook:
- lee un certificado en uso en la maquina de destino (openAM o openDS) segun que puerto configurado en el inventario
- saca la fecha de inicio del certificado, la fecha de caducidad, calcula la differencia de dias que quedaria valido entre la fecha de caducidad y la fecha de hoy(al momento de ejecutar).
- saca el subject del certificado (a controlar, hay un bug creo)
- saca el numero de serie, permitiendo comparar en pycportal y asegurar que es el mismo certificado instalado en la maquina que el observado en la vigilancia de pycportal.

Ejemplo de analisis que hace el playbook:

Hostname;Port;Not Before;Not After;Expire Days;Subject;Serial Number
axdesopenam3.central.inditex.grp;8443;25/06/2025;25/06/2027;713;;46:00:13:62:5f:c1:22:d0:7c:19:b5:be:f1:00:01:00:13:62:5f
axdesopenam4.central.inditex.grp;8443;25/06/2025;25/06/2027;713;;46:00:13:62:5f:c1:22:d0:7c:19:b5:be:f1:00:01:00:13:62:5f
axdesecgfam0201.central.inditex.grp;8443;24/01/2025;24/01/2027;561;;46:00:0d:57:ee:65:d3:61:bd:63:ea:66:f6:00:01:00:0d:57:ee
axdesgfam0101.central.inditex.grp;8443;25/06/2025;25/06/2027;713;;46:00:13:62:a2:f3:ce:8a:9f:14:47:36:3d:00:01:00:13:62:a2
...
axpregfdscf01.central.inditex.grp;8989;10/07/2019;05/07/2039;5106;;165300160
axpregfdscf02.central.inditex.grp;8989;10/07/2019;05/07/2039;5106;;626547341
axpregfdstk01.central.inditex.grp;8989;10/07/2019;05/07/2039;5106;;720721386
axpregfdstk02.central.inditex.grp;8989;10/07/2019;05/07/2039;5106;;1219937472
axpreectfdsc101.central.inditex.grp;8989;28/10/2021;23/10/2041;5947;;2137871746
...
axdesopends1.central.inditex.grp;1636;10/09/2024;10/09/2026;425;;46:00:07:38:9f:e4:b0:3f:e1:ff:d4:14:07:00:01:00:07:38:9f
axdesopends2.central.inditex.grp;1636;10/09/2024;10/09/2026;425;;46:00:07:38:a0:9b:3a:cb:75:50:8b:20:b9:00:01:00:07:38:a0
axdesecgfds0201.central.inditex.grp;1636;24/01/2025;24/01/2027;561;;46:00:0d:58:e9:35:f9:9b:af:f9:25:ad:46:00:01:00:0d:58:e9
axdesgfds0101.central.inditex.grp;1636;29/02/2024;28/02/2026;231;;46:00:02:1a:ca:c8:be:9d:ee:98:af:ca:18:00:00:00:02:1a:ca
...
axdesopends1.central.inditex.grp;4444;10/09/2024;10/09/2026;425;;46:00:07:38:9f:e4:b0:3f:e1:ff:d4:14:07:00:01:00:07:38:9f
axdesopends2.central.inditex.grp;4444;10/09/2024;10/09/2026;425;;46:00:07:38:a0:9b:3a:cb:75:50:8b:20:b9:00:01:00:07:38:a0
axdesecgfds0201.central.inditex.grp;4444;24/01/2025;24/01/2027;561;;46:00:0d:58:e9:35:f9:9b:af:f9:25:ad:46:00:01:00:0d:58:e9
axdesgfds0101.central.inditex.grp;4444;29/02/2024;28/02/2026;231;;46:00:02:1a:ca:c8:be:9d:ee:98:af:ca:18:00:00:00:02:1a:ca
axpreecgfdscf01.central.inditex.grp;4444;29/01/2025;29/01/2027;566;;46:00:0d:8b:8a:d8:69:89:dd:7c:dc:2f:5d:00:01:00:0d:8b:8a
axpreecgfdstk01.central.inditex.grp;4444;24/01/2025;24/01/2027;561;;46:00:0d:58:da:52:64:52:10:2f:9f:9f:0c:00:01:00:0d:58:da

SOLO LOCAL!!! 

se sincroniza desde github en local unicamente, ya que escribe un fichero en el /tmp de la maquina ejecutadora

ansible-playbook -i inventories/forgerock_inventory.yml ./check_cert.yml
