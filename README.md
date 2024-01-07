# CP3---securiser-l-infra
mise en place bastion d'administration teleport

Mise à jour du système : Mettez à jour votre système avec les derniers packages.

bash
sudo apt-get update && sudo apt-get upgrade -y
Téléchargez le binaire Teleport : Téléchargez le binaire à partir du site officiel de Teleport.

bash


curl -O https://get.gravitational.com/teleport-v8.2.0-linux-amd64-bin.tar.gz
Remplacez v8.2.0 par la version que vous souhaitez installer.

Extrayez le binaire Teleport :

bash


tar -xzf teleport-v8.2.0-linux-amd64-bin.tar.gz
Déplacez le binaire de Teleport :

bash


sudo mv teleport/tctl teleport/tsh teleport/teleport /usr/local/bin/
Créez un certificat auto-signé :

bash


openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
Vous devrez fournir des informations pour le certificat. Ces informations seront affichées lorsque vous accédez à votre serveur via HTTPS.

Générez un fichier de configuration :

bash


sudo teleport configure > /etc/teleport.yaml
Modifiez le fichier de configuration :

Ouvrez le fichier de configuration dans un éditeur de texte.

bash


sudo nano /etc/teleport.yaml
Modifiez le fichier pour qu'il ressemble à ceci :

yaml


teleport:
  nodename: linux-bastion-supervision
  data_dir: /var/lib/teleport
  log:
    output: stderr
    severity: INFO
  auth_token: cluster-join-token
  auth_servers:
    - 10.0.0.5:3025
  connection_limits:
    max_connections: 15000
    max_users: 250
  auth_service:
    enabled: "yes"
  ssh_service:
    enabled: "yes"
  proxy_service:
    enabled: "yes"
    listen_addr: 0.0.0.0:3023
    web_listen_addr: 10.0.0.5:3080
    tunnel_listen_addr: 0.0.0.0:3024
    https_keypairs:
    - key_file: /path/to/key.pem
      cert_file: /path/to/cert.pem


Remplacez /path/to/key.pem et /path/to/cert.pem par les chemins vers votre clé privée et votre certificat. Vous pouvez également modifier cluster-join-token par un token de votre choix.

Démarrez Teleport :

bash


sudo teleport start -c /etc/teleport.yaml
C'est tout ! Vous avez maintenant un serveur Teleport en cours d'exécution avec un certificat auto-signé. Vous pouvez accéder à l'interface web de Teleport en visitant https://10.0.0.5:3080 dans votre navigateur.



