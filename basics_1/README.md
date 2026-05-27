# Networking basics #1

## Description

Approfondissement des concepts réseau sous Linux : localhost, adresse 0.0.0.0, fichier `/etc/hosts`, interfaces réseau actives et écoute de ports avec netcat.

## Learning Objectives

- Comprendre localhost / 127.0.0.1
- Comprendre 0.0.0.0
- Comprendre et modifier `/etc/hosts`
- Afficher les interfaces réseau actives avec `ifconfig`
- Écouter sur un port avec `nc`

## Fichiers

| Fichier | Description |
|---------|-------------|
| `0-change_your_home_IP` | Modifie localhost → 127.0.0.2 et facebook.com → 8.8.8.8 |
| `1-show_attached_IPs` | Affiche toutes les IPv4 actives de la machine |
| `2-port_listening_on_localhost` | Écoute sur le port 98 de localhost |

## Problèmes rencontrés

### Problème 1 — `sed -i` impossible sur `/etc/hosts` (task 0)

**Symptôme :** `sed: cannot rename /etc/sedXXXX: Device or resource busy`

**Cause :** `sed -i` tente de **remplacer** le fichier via un renommage. Sur le serveur du checker (Docker/LXC), `/etc/hosts` est un fichier monté spécialement — le renommage est interdit.

**Solution :** Passer par des fichiers temporaires dans `/tmp` :
```bash
cp /etc/hosts /tmp/hosts_temp
sed 's/127.0.0.1\(.*\)localhost/127.0.0.2\1localhost/' /tmp/hosts_temp > /tmp/hosts_new
grep -v 'facebook.com' /tmp/hosts_new > /tmp/hosts_final
echo "8.8.8.8 facebook.com" >> /tmp/hosts_final
cp /tmp/hosts_final /etc/hosts
```

**Pourquoi `cp` fonctionne et pas `sed -i` ?**
- `sed -i` → remplace le fichier (renommage)  interdit
- `cp` → écrit dans le fichier existant (overwrite)  autorisé

---

### Problème 2 — Permission denied sur WSL (tasks 1 et 2)

**Symptôme :** `bash: ./script: Permission denied` sur le checker.

**Cause :** Sur WSL, le système de fichiers Windows (NTFS) ne transmet pas les permissions Unix à git. `chmod +x` fonctionne localement mais git ne voit pas le changement.

**Solution :**
```bash
git update-index --chmod=+x nom_du_script
git add nom_du_script
git commit -m "fix: make script executable"
git push
```

---

### Problème 3 — GitHub Internal Server Error 500 (task 1)

**Symptôme :**
```
remote: Internal Server Error
! [remote rejected] main -> main (Internal Server Error)
```

**Cause :** Erreur temporaire côté serveurs GitHub — pas liée au code.

**Solution :** Attendre 1-2 minutes et relancer `git push`.

**Règle générale :**
| Code erreur | Origine | Solution |
|-------------|---------|----------|
| `4xx` | Toi — mauvaise requête | Corriger le code/config |
| `5xx` | Le serveur | Attendre et réessayer |

---

## Commandes clés apprises

```bash
# Voir les interfaces réseau actives
ifconfig
ip a

# Extraire les IPs IPv4
ifconfig | grep 'inet ' | awk '{print $2}'

# Écouter sur un port
nc -lp 98

# Se connecter à un port
telnet localhost 98

# Modifier /etc/hosts sans sed -i
cp /etc/hosts /tmp/h && sed '...' /tmp/h > /tmp/hf && cp /tmp/hf /etc/hosts
```

## Note importante

Après avoir testé `0-change_your_home_IP`, remettre localhost à sa valeur originale :
```bash
sudo sed -i 's/127.0.0.2/127.0.0.1/' /etc/hosts
```
Sinon de nombreux programmes cesseront de fonctionner !
