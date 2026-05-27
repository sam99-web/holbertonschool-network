
# Networking basics 0

# Description

Introduction aux fondamentaux du réseau informatique. Ce projet couvre le modèle OSI, les types de réseaux, l'adressage MAC/IP, les protocoles TCP/UDP et les outils de diagnostic réseau sous Linux.

# Learning Objectives

- Comprendre le modèle OSI et ses 7 couches
- Distinguer les types de réseaux (LAN, WAN, Internet)
- Comprendre les adresses MAC et IP
- Différencier TCP et UDP
- Utiliser `netstat` et `ping`

# Fichiers

| Fichier | Description |
|---------|-------------|
| `0-OSI_model` | Réponses sur le modèle OSI |
| `1-types_of_network` | Réponses sur les types de réseaux |
| `2-MAC_and_IP_address` | Réponses sur les adresses MAC et IP |
| `3-UDP_and_TCP` | Réponses sur TCP et UDP |
| `4-TCP_and_UDP_ports` | Script affichant les ports en écoute |
| `5-is_the_host_on_the_network` | Script qui ping une IP 5 fois |

## Problèmes rencontrés

### Problème 1 — `sed -i` sur `/etc/hosts` (task 4)

**Symptôme :** `sed: cannot rename /etc/sedXXXX: Device or resource busy`

**Cause :** `sed -i` fonctionne en 3 étapes :
1. Lit le fichier original
2. Crée un fichier temporaire avec les modifications
3. **Renomme** le fichier temporaire pour remplacer l'original

Sur le serveur du checker (environnement Docker/LXC), `/etc/hosts` est **monté en lecture spéciale** — le renommage est interdit même avec sudo.

**Solution :** Travailler dans `/tmp` et utiliser `cp` pour écraser le fichier :
```bash
cp /etc/hosts /tmp/hosts_temp
sed 's/...' /tmp/hosts_temp > /tmp/hosts_final
cp /tmp/hosts_final /etc/hosts
```
`cp` écrit **dans** le fichier (pas de renommage) → autorisé.

---

### Problème 2 — Output incomplet de `netstat` (task 4)

**Symptôme :** Le checker attendait TCP + UDP + UNIX sockets mais n'en recevait qu'une partie.

**Cause :** Les flags `-t` (TCP) et `-u` (UDP) **limitaient** l'output aux seuls protocoles internet, excluant les UNIX domain sockets.

**Solution :** Supprimer `-t` et `-u` :
```bash
netstat -lp   # affiche tout : TCP + UDP + UNIX sockets
```

---

### Problème 3 — `sudo` dans le script (task 4)

**Symptôme :** Comportement inattendu sur le serveur du checker.

**Cause :** Le checker tourne déjà en **root**. Appeler `sudo` depuis root peut bloquer l'exécution selon la configuration sudoers du serveur.

**Solution :** Ne jamais mettre `sudo` dans les scripts Holberton — c'est l'utilisateur qui lance le script avec `sudo` si nécessaire.

---

### Problème 4 — Permission denied (tasks 4 et 5)

**Symptôme :** `bash: ./script: Permission denied`

**Cause :** Sur **WSL (Windows Subsystem for Linux)**, le système de fichiers est NTFS. `chmod +x` fonctionne localement mais **git ne détecte pas** le changement de permission car NTFS ne gère pas les permissions Unix nativement.

**Solution :** Forcer git à enregistrer la permission exécutable :
```bash
git update-index --chmod=+x nom_du_script
```

---

## Leçons retenues

| Problème | Cause | Solution |
|----------|-------|----------|
| `sed -i` échoue sur /etc/hosts | Fichier monté en lecture spéciale | Passer par `/tmp` + `cp` |
| Output netstat incomplet | Flags `-tu` trop restrictifs | Utiliser `netstat -lp` |
| `sudo` dans le script | Checker déjà root | Supprimer `sudo` du script |
| Permission denied sur WSL | NTFS ne gère pas chmod | `git update-index --chmod=+x` |
