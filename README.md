# Holberton School - Network

## Description

Ce repository contient les projets sur les fondamentaux des réseaux informatiques réalisés dans le cadre de la formation Holberton School. L'objectif est de comprendre et maîtriser les concepts clés du networking, du modèle OSI aux protocoles TCP/UDP, en passant par la configuration réseau sous Linux.

## Structure du repository

```
holbertonschool-network/
├── README.md
├── basics_0/       # Networking basics #0
│   ├── README.md
│   ├── 0-OSI_model
│   ├── 1-types_of_network
│   ├── 2-MAC_and_IP_address
│   ├── 3-UDP_and_TCP
│   ├── 4-TCP_and_UDP_ports
│   └── 5-is_the_host_on_the_network
└── basics_1/       # Networking basics #1
    ├── README.md
    ├── 0-change_your_home_IP
    ├── 1-show_attached_IPs
    └── 2-port_listening_on_localhost
```

## Projets

### Networking basics #0
Introduction aux concepts fondamentaux : modèle OSI, types de réseaux, adresses MAC et IP, protocoles TCP/UDP, ports réseau et commande ping.

### Networking basics #1
Approfondissement : localhost, 0.0.0.0, fichier /etc/hosts, interfaces réseau actives, écoute de ports avec netcat.

## Concepts clés abordés

- Modèle OSI (7 couches)
- Protocoles TCP vs UDP
- Adressage IP et MAC
- Ports réseau (SSH:22, HTTP:80, HTTPS:443)
- Commandes : `ifconfig`, `netstat`, `ping`, `nc`, `telnet`

## Environnement

- OS : Ubuntu 22.04 LTS
- Shell : Bash
- Tous les scripts passent `shellcheck` sans erreur
