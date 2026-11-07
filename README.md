Este playbook sirve para checkear, analizar y sacar un reporte de todos los certificados usados en todas las plataformas de openAM (menos los certificados de firma).
  * certificados SSL para https de los tomcat
  * certificados de encryptacion de la communicacions (LDAPS 1636 o admin 4444)
  * certificados de encryptacion de la communicacions (Puerto de replicacion cuando la hay: 8989)

El playbook:
- lee un certificado en uso en la maquina de destino (openAM o openDS) segun que puerto configurado en el inventario
- saca la fecha de inicio del certificado, la fecha de caducidad, calcula la differencia de dias que quedaria valido entre la fecha de caducidad y la fecha de hoy(al momento de ejecutar).
- saca el subject del certificado (a controlar, hay un bug creo)
- saca el numero de serie, permitiendo comparar en pycportal y asegurar que es el mismo certificado instalado en la maquina que el observado en la vigilancia de pycportal.

SOLO LOCAL!!! 

se sincroniza desde github en local unicamente, ya que escribe un fichero en el /tmp de la maquina ejecutadora

ansible-playbook -i inventories/forgerock_inventory.yml ./check_cert.yml
