# GUIDE TECHNIQUE : CONFIGURATION DNS DU DOMAINE COMMERCIAL BEXY SHOP

Ce document detaille l'ensemble des enregistrements DNS a configurer chez votre registrar (OVH, Cloudflare, Namecheap, Gandi, Hostinger) pour lier le domaine commercial personnalisé `bexyshop.com` au site vitrine hébergé sur GitHub Pages (`bexyshop.github.io`).

---

## 1. Enregistrements DNS Obligatoires (Zone DNS Registrar)

Pour diriger le trafic de `bexyshop.com` vers les serveurs Edge de GitHub avec équilibrage de charge et redondance mondiale :

### A. Enregistrements IPv4 (Type A) - Domaine Racine (@ / Apex)
Créer 4 enregistrements de Type A pour l'hôte `@` (ou laisser vide selon l'interface de votre registrar) :

| Type | Hôte / Nom | Valeur / Cible IP | TTL |
|------|------------|-------------------|-----|
| A    | @          | 185.199.108.153   | 3600 |
| A    | @          | 185.199.109.153   | 3600 |
| A    | @          | 185.199.110.153   | 3600 |
| A    | @          | 185.199.111.153   | 3600 |

### B. Enregistrements IPv6 (Type AAAA) - Domaine Racine (@ / Apex)
Créer 4 enregistrements de Type AAAA pour assurer la compatibilité IPv6 native :

| Type | Hôte / Nom | Valeur / Cible IPv6 | TTL |
|------|------------|---------------------|-----|
| AAAA | @          | 2606:50c0:8000::153 | 3600 |
| AAAA | @          | 2606:50c0:8001::153 | 3600 |
| AAAA | @          | 2606:50c0:8002::153 | 3600 |
| AAAA | @          | 2606:50c0:8003::153 | 3600 |

### C. Enregistrement Sous-Domaine WWW (Type CNAME)
Créer l'alias canonique pour router `www.bexyshop.com` :

| Type  | Hôte / Nom | Valeur / Cible           | TTL |
|-------|------------|--------------------------|-----|
| CNAME | www        | bexyshop.github.io.      | 3600 |

---

## 2. Sécurité & Certification SSL/TLS (Let's Encrypt / DigiCert)

Pour garantir la délivrance automatique du certificat SSL sans blocage CAA :

| Type | Hôte / Nom | Valeur (Tag + CA)          | TTL |
|------|------------|----------------------------|-----|
| CAA  | @          | 0 issue "letsencrypt.org"  | 3600 |
| CAA  | @          | 0 issue "digicert.com"     | 3600 |

---

## 3. Procédure de Validation et Activation

1. **Propagation DNS** :
   Attendre 5 a 15 minutes que la propagation DNS s'effectue. Vous pouvez vérifier avec :
   ```powershell
   Resolve-DnsName -Name bexyshop.com -Type A
   Resolve-DnsName -Name www.bexyshop.com -Type CNAME
   ```

2. **Vérification sur le Dépôt GitHub** :
   - Le fichier `CNAME` contenant `bexyshop.com` est présent à la racine du dépôt `BexyShop/bexyshop.github.io`.
   - Dans GitHub : *Settings* > *Pages* > *Custom domain* : renseigner `bexyshop.com`.
   - Cocher l'option **Enforce HTTPS** dès que le certificat TLS est émis.

---

## 4. Persistance Graphique & Performance Frontend

Le fichier `index.html` est strictement calibré sur les normes visuelles industrielles :
- **Palette True Black** : Fond `#000000`, cartes `#0c0c0c`, bordures subtiles `rgba(255, 255, 255, 0.08)`.
- **Micro-interactions Emil Kowalski** : 
  - Courbes d'accélération physiques (`cubic-bezier(0.23, 1, 0.32, 1)` et `cubic-bezier(0.32, 0.72, 0, 1)`).
  - Micro-compression tactile `scale(0.965)` au clic sur tous les boutons CTA.
  - Spinner de chargement inline sans décalage de structure (zero layout shift).
  - Conformité totale avec `prefers-reduced-motion`.
