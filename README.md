# Installation d'un serveur OpenVPN

Ce script permet d'installer un serveur OpenVPN sur Debian, Ubuntu, Fedora, CentOS, Arch Linux, Oracle Linux, Rocky Linux et AlmaLinux. Basé sur le travail de [angristan](https://github.com/angristan/openvpn-install), ce script simplifie le processus d'installation d'un serveur VPN sécurisé.

## Prérequis

Pour installer OpenVPN sur un serveur, exécutez les commandes suivantes :

```bash
apt-get update &&
apt-get install -y $(tasksel --task-packages standard) openssh-server net-tools curl openvpn easy-rsa
```

---

## Installation

Téléchargez et exécutez le script d'installation (assurez-vous d’avoir les privilèges root) :

```bash
cd /root/
curl -O https://raw.githubusercontent.com/jchartoire/openvpn-server/master/openvpn-install.sh
chmod +x openvpn-install.sh
./openvpn-install.sh
```

## Modification du fichier de configuration

À la fin de l'installation, modifiez le fichier de configuration `/etc/openvpn/server.conf` pour ajouter ou modifier les lignes suivantes (adaptez les IPs à votre sous-réseau) :

```bash
nano /etc/openvpn/server.conf
```

```ini
#push "dhcp-option DNS 192.168.x.254"
#push "redirect-gateway def1 bypass-dhcp"
push "route 192.168.x.0 255.255.255.0"
```

---

## Utilisation

Une fois OpenVPN installé, réexécutez le script pour effectuer des modifications ou ajouter de nouveaux utilisateurs :

```bash
cd /root/
./openvpn-install.sh
```

```bash
Welcome to OpenVPN-install!
The git repository is available at: https://github.com/jchartoire/openvpn-server/
It looks like OpenVPN is already installed.
What do you want to do?
 1) Add a new user
 2) Revoke existing user
 3) Remove OpenVPN
 4) Exit
Select an option [1-4]: 1
```

### Création d'un nouvel utilisateur

Choisissez l'option `1) Add a new user` et suivez les instructions. Il est recommandé d'utiliser un nom d'utilisateur comprenant uniquement :

* Lettres minuscules (`a` à `z`)
* Chiffres (`0` à `9`)
* Tirets (`-`), tirets bas (`_`), et points (`.`)

### Génération d'un mot de passe

Choisissez ensuite l'option `2) Use a password for the client`.
Générez un mot de passe fort, avec au moins 25 caractères comprenant :

* Lettres minuscules (`a` à `z`)
* Lettres majuscules (`A` à `Z`)
* Chiffres (`0` à `9`)
* Caractères spéciaux (`!`, `@`, `#`, `$`, `%`, `^`, `&`, `*`, `(`, `)`, `_`, `+`, `-`, `=`, `{`, `}`, `[`, `]`, `|`, `\`, `:`, `;`, `"`, `'`, `<`, `>`, `,`, `.`, `?`, `/`)

Entrez ce mot de passe lorsqu'il est demandé (deux fois).

### Profils clients générés

Les fichiers `.ovpn` sont générés dans le dossier `/etc/openvpn/client/[$user.ovpn]`.

## Installation sans interaction (Headless)

Il est possible d'exécuter le script sans interaction pour une installation automatisée :

```bash
AUTO_INSTALL=y ./openvpn-install.sh
```

Personnalisez les paramètres d'installation en exportant des variables avant d'exécuter le script.

Exemple :

```bash
export APPROVE_INSTALL=y
export APPROVE_IP=y
export IPV6_SUPPORT=n
export PORT_CHOICE=1
export PROTOCOL_CHOICE=1
export DNS=1
export COMPRESSION_ENABLED=n
export CUSTOMIZE_ENC=n
export CLIENT=clientname
export PASS=1
./openvpn-install.sh
```

## Fonctions principales

- Installer et configurer un serveur OpenVPN prêt à l'emploi
- Gèrer les règles `iptables` et le forwarding de manière transparente
- Possibilité de supprimer proprement OpenVPN et ses configurations
- Paramètres de chiffrement personnalisables avec des configurations par défaut améliorées
- Support des fonctionnalités OpenVPN 2.4, notamment les améliorations de chiffrement
- Large choix de résolveurs DNS
- Support de la compression LZ4 et LZ0 (désactivée par défaut pour éviter les attaques VORACLE)
- Mode non-privilégié : exécution en tant que `nobody`/`nogroup`
- Protection des clients avec un mot de passe (chiffrement de la clé privée)
- Support NAT IPv6

## Compatibilité

Le script supporte les distributions Linux suivantes :

|                    | Support |
| ------------------ | ------- |
| AlmaLinux 8        | ✅      |
| Amazon Linux 2     | ✅      |
| Arch Linux         | ✅      |
| CentOS 7           | ✅      |
| CentOS Stream >= 8 | ✅      |
| Debian >= 10       | ✅      |
| Fedora >= 35       | ✅      |
| Oracle Linux 8     | ✅      |
| Rocky Linux 8      | ✅      |
| Ubuntu >= 18.04    | ✅      |

## Crédits et Licence

Ce projet est basé sur le script original d'installation d'OpenVPN développé par [angristan](https://github.com/angristan). Vous pouvez trouver le projet original ici : https://github.com/angristan/openvpn-install. Ce projet est distribué sous la licence MIT. Voir le fichier [LICENSE](https://raw.githubusercontent.com/jchartoire/openvpn-server/main/LICENSE) pour plus d'informations.
