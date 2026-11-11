# Projet : site web du club de karaté shotokan (Renoji_web)

> Document vivant. À enrichir à chaque étape. Dernière mise à jour : 11 novembre 2026 (programme du club 2023 intégré).
> Légende : ✅ confirmé · 🔶 à vérifier · ⏳ en attente d'information · ❌ non fait

---

## 1. Contexte

| Élément | Valeur |
|---|---|
| Structure | Association, club de karaté shotokan |
| Fédération | FFKDA (site fédéral : ffkarate.fr) |
| Règlements de référence | [REF-TECH](reglementation/CSDGE_Reglementation_Technique_2026-2027.pdf) et [REF-GEN](reglementation/CSDGE_Reglementation_Generale_2026-2027.pdf), indexés dans [INDEX_REGLEMENTATION_FFKDA.md](reglementation/INDEX_REGLEMENTATION_FFKDA.md) (voir 5.1) |
| Responsable du projet | Responsable digital du club |
| Effectif | 15 membres : 5 ceintures blanches, 2 jaunes, 1 verte, 7 noires |
| Cours | Mardi et jeudi |
| Hébergement | NAS Synology avec Web Station (installé) |
| Langages demandés | JavaScript (front) et Python (back) |

---

## 2. Fonctions du site

### F1. Page de garde publique (visibilité : tout le monde)

- Présentation du club (shotokan, valeurs, contact)
- Planning des cours **mardi et jeudi** : horaires et adresse exacte du dojo
- ⏳ **Les horaires et le lieu figurent sur les flyers du club : ils n'ont pas été transmis.** À fournir (photo ou PDF des flyers) ou à saisir directement ci-dessous.

| Jour | Horaire | Public / niveau | Lieu |
|---|---|---|---|
| Mardi | ⏳ | ⏳ | ⏳ |
| Jeudi | ⏳ | ⏳ | ⏳ |

### F2. Espace membres privé (visibilité : membres du club uniquement)

- Connexion par identifiant et mot de passe (15 comptes)
- Contenu réservé : à préciser (voir questions ouvertes)
- Idées possibles : annonces internes, calendrier des stages et passages de grades, documents (certificat médical, règlement intérieur), présences

### F3. Passages de grades (ceintures)

- Techniques à connaître par grade
- Base de départ : **programme du club 2023** (section 6)
- Règlements officiels de la fédération en PDF (dossier `reglementation/`), avec index par thème et par page : à proposer en téléchargement depuis la page des grades
- Prise en compte de la **nouvelle réglementation FFKDA 2026-2027** (détail en section 5), avec les écarts en section 7
- Visibilité à décider : public ou membres seulement (🔶 voir questions ouvertes)

---

## 3. Architecture technique proposée 🔶

Proposition de départ, à valider.

```
Navigateur (HTML + CSS + JavaScript)
        │  HTTPS
        ▼
Synology Web Station (Nginx)
   ├── Pages publiques statiques (F1)
   └── API Python (Flask) ── SQLite
          ├── authentification / sessions (F2)
          └── contenu des grades (F3)
```

| Couche | Choix proposé | Remarque |
|---|---|---|
| Front | HTML, CSS, JavaScript natif | Léger, sans build |
| Back | Python 3 + Flask | Web Station permet d'héberger un service Python (profil à configurer) 🔶 |
| Base de données | SQLite | Largement suffisant pour 15 membres |
| Authentification | Sessions Flask, mots de passe hachés (bcrypt ou argon2) | Jamais de mot de passe en clair |
| Sécurité | HTTPS (certificat Let's Encrypt via le DSM), pare-feu, sauvegardes | Obligatoire dès qu'il y a une connexion |
| Données personnelles | Minimum nécessaire, conformité RGPD | Association : informer les membres |

### Arborescence cible

```
renoji_web/
├── PROJET_SITE_CLUB_KARATE.md
├── public/            # pages statiques, css, js, images
├── app/               # code Python (Flask)
├── data/              # base SQLite (hors dossier public)
└── requirements.txt
```

---

## 4. Plan de travail

| # | Étape | Statut |
|---|---|---|
| 1 | Rassembler les flyers (horaires, lieu, logo, charte du club) | ⏳ |
| 2 | Configurer Web Station (portail, HTTPS, nom de domaine ou DDNS) | ❌ |
| 3 | Page de garde publique avec planning | ❌ |
| 4 | Back-end Python : base, comptes, connexion | ❌ |
| 5 | Espace membres | ❌ |
| 6 | Section grades (contenu 2026-2027) | ❌ |
| 7 | Tests, sauvegardes, mise en ligne | ❌ |

---

## 5. Passages de grades : réglementation FFKDA 2026-2027

### 5.1 Documents de référence et niveau de confiance

Les deux règlements dont dépend le club sont conservés dans le dossier `reglementation/` du projet, avec un index (table des matières, index thématique, numéros de page).

| Réf. | Document | Fichier |
|---|---|---|
| **REF-TECH** | Règlement technique de la CSDGE 2026-2027 (karaté : pages 3 à 43) | [reglementation/CSDGE_Reglementation_Technique_2026-2027.pdf](reglementation/CSDGE_Reglementation_Technique_2026-2027.pdf) |
| **REF-GEN** | Règlement général de la CSDGE 2026-2027 (38 pages) | [reglementation/CSDGE_Reglementation_Generale_2026-2027.pdf](reglementation/CSDGE_Reglementation_Generale_2026-2027.pdf) |
| Index | Table des matières, index thématique, pages | [reglementation/INDEX_REGLEMENTATION_FFKDA.md](reglementation/INDEX_REGLEMENTATION_FFKDA.md) |

Dans ce fichier, `[REF-TECH p. 4]` désigne la page 4 du lecteur PDF de REF-TECH. Le seul texte qui fait foi est le règlement officiel de la fédération.

✅ **Contenu vérifié sur les documents officiels** transmis pour la saison 2026-2027 :
- **Règlement technique de la CSDGE** (Commission Spécialisée des Dans et Grades Équivalents), partie « Karaté » (articles 402K à 409K, annexes I à V).
- **Règlement général de la CSDGE** (articles 101 à 403-B, annexes générales).

Les synthèses de clubs (Karate Club Colombes, Imagin Arts Digital) ont servi de première base : elles ont été recoupées, et les corrections figurent en 5.9.

⏳ **Absent du PDF** : la **liste des 6 enchaînements de kihon du 1er Dan** (7 pour le 2e Dan, 10 pour le 3e Dan). Le règlement indique que cette liste est « envoyée dans les clubs en début de saison et disponible sur le site internet de la FFKDA ». À récupérer.

Les kyu (ceintures de couleur) ne dépendent pas de ces textes. Seule contrainte : le **1er kyu (ceinture marron)** est exigé pour se présenter au 1er Dan (voir 5.8).

### 5.2 Nouveautés annoncées : état de confirmation

Références : REF-TECH annexe I (p. 28 à 30), annexe IV (p. 39), art. 402K-1K (p. 4 à 6) ; REF-GEN art. 402 (p. 26 à 27).

Le caractère « nouveau » de ces points vient des synthèses secondaires. Leur **contenu** est vérifié dans le règlement 2026-2027, sauf mention contraire.

| # | Point | Grades | Confirmé |
|---|---|---|---|
| 1 | Kihon linéaire : 3 enchaînements tirés au sort le jour de l'examen dans une liste nationale (6 au 1er Dan, 7 au 2e, 10 au 3e) | 1er, 2e, 3e Dan | ✅ (annexe I) |
| 2 | Kihon en poursuite : les enchaînements restants de la liste, de 0° à 180° en garde gauche puis de 180° à 0° en garde droite | 1er, 2e, 3e Dan | ✅ |
| 3 | Bunkai : minimum 3 séquences issues d'un des deux kata exécutés | 1er, 2e, 3e Dan | ✅ |
| 4 | Erreur de kata : on peut recommencer, la note est divisée par 2 | Tous les grades | ✅ (annexe IV) |
| 5 | Voie compétition : 5 participations sur la saison du jour de l'examen, combat ou kata, sans cumul | 1er, 2e, 3e Dan | ✅ |
| 6 | Kihon sur cibles : 5 techniques parmi 7, choisies **par le jury** ; Uke par deux petits sursauts lents | 1er, 2e, 3e Dan | ✅ |
| 7 | Kihon sur cibles en enchaînements de 2 à 3 techniques, sur deux pas lents | 4e Dan | ✅ |
| 8 | Bonification de points : 1 point maximum | 1er au 5e Dan | ✅ (art. 402, 403-A) |
| 9 | Kihon de 5 enchaînements de 5 techniques | 6e Dan | ✅ ; 7e Dan non relu |
| 10 | Nouvel examen en 3 UV (kihon, kata et bunkai, entretien) | 8e Dan | ✅ |

### 5.3 1er Dan, voie traditionnelle (vérifié)

Références : **REF-TECH art. 402K-1K, p. 4 à 6** ; annexe I (kihon), p. 28 à 29 ; annexe III (kumite), p. 35 à 37 ; annexe IV (kata et bunkai), p. 39 à 40 ; REF-GEN art. 402, p. 26.

**Organisation**
- 1 jury de **3 juges**. 6 épreuves (UV), chacune notée sur 20. Obtention du grade avec **60/120**.
- Pas de note éliminatoire. Les UV avec **au moins 10/20** restent acquises définitivement ; seules les autres sont à repasser, avec un **nouveau dossier** (l'inscription n'est valable que pour un seul examen).
- Pour les épreuves à deux, le candidat est évalué avec un **partenaire choisi parmi les candidats**.

| UV | Épreuve | Avec partenaire |
|---|---|---|
| 1 | Kihon | En partie |
| 2 | Kihon ippon kumite | Oui |
| 3 | Kata | Non |
| 4 | Bunkai | Oui |
| 5 | Jiyu ippon kumite | Oui |
| 6 | Ju kumite (randori ou midare) | Oui |

**UV 1 : Kihon** (une seule note, 3 parties). Le jury n'évalue que **deux candidats à la fois** et emploie la **terminologie officielle en japonais**.

Critères : puissance et vitesse d'exécution, aisance des déplacements, équilibre et stabilité, bonne attitude corporelle, détermination.

1. **Kihon linéaire** : enchaînements de **3 techniques maximum**, exécutés en aller-retour sur **trois pas** en avançant ou en reculant. **3 enchaînements tirés au sort** le jour de l'examen par le responsable des grades, parmi **6**. Ils sont énoncés à tous les candidats.
2. **Kihon en poursuite** : les **3 enchaînements restants** de la liste de 6. Déroulement :
   - départ au point A face au jury, salut ;
   - tourner à gauche vers le point 0°, pied gauche en avant, exécuter l'enchaînement, revenir au point A en garde gauche ;
   - tourner de 90° vers la droite (toujours en garde gauche) et exécuter le même enchaînement vers le jury ;
   - tourner de 90° vers la droite, vers le point 180°, exécuter une troisième fois, revenir au point A ;
   - changer de jambe sur place (pied droit en avant vers 180°) et refaire l'exercice **de 180° à 0° en garde droite** ;
   - se remettre face au jury au point A, puis enchaînement suivant.
3. **Maîtrise de la technique sur cibles, à deux** : **5 techniques choisies par le juge** parmi **7**, exécutées à droite ou à gauche **au choix du candidat**.
   - Tori attaque, Uke présente la cible. Uke se positionne par **deux petits sursauts lents**, vers l'arrière ou sur le côté.
   - Tori adapte sa distance et répète la technique **au moins 3 fois**, avec précision. Uke est passif et immobile pendant la technique, puis se repositionne.
   - Critères : gestion de la distance, maîtrise du geste, précision.

Les 7 techniques sur cible :

| # | Technique |
|---|---|
| 1 | Mae geri, jambe arrière posée derrière, chudan |
| 2 | Mawashi geri, jambe arrière posée derrière, jodan ou chudan |
| 3 | Mae geri, jambe avant avec sursaut, chudan |
| 4 | Mawashi geri, jambe avant avec sursaut, jodan ou chudan |
| 5 | Gyaku zuki chudan |
| 6 | Kizami zuki ou Maete zuki jodan, suivi de Gyaku zuki chudan |
| 7 | Oï zuki jodan, retour à l'arrière |

**UV 2 : Kihon ippon kumite**
- **5 attaques**, chacune **une fois à droite puis une fois à gauche, en alternance** : Oï zuki jodan, Oï zuki chudan, Mae geri chudan, Mawashi geri jodan ou chudan, Yoko geri chudan. Les rôles s'inversent quand Tori a terminé toutes ses attaques.
- Procédure : partenaires à **1 mètre**, de profil par rapport au jury. Salut, Hachi ji dachi. Tori se met en garde en reculant la jambe droite. **Tori annonce son attaque**, se concentre un instant et la lance avec conviction. **La défense et la contre-attaque d'Uke sont libres.** Uke reste un instant sur sa contre-attaque. Retour en Hachi ji dachi après chaque attaque.
- L'attaque s'effectue **toujours avec la jambe ou le bras arrière**.

**UV 3 : Kata**
- **2 kata libres**, choisis dans la liste des kata de **1er Dan de son style** (Shotokan Ryu : Heian 1 à 5, Tekki shodan, voir 5.7) ou dans celle de **tout autre style**.
- **Erreur : on peut recommencer, la note est divisée par 2.**
- Le kata est **annoncé à voix haute** et commence et se termine par le salut. Critères (annexe IV) : présentation, cérémonial et étiquette, concentration, équilibre et stabilité, rythme et tempo, puissance, regard, respiration et kiai, respect des techniques et du diagramme original. Pour le 1er et le 2e Dan, on se réfère aux kata officiels du règlement.

**UV 4 : Bunkai**
- Avec un partenaire, **minimum 3 séquences de son choix issues d'un des deux kata exécutés**. Le candidat doit connaître les explications techniques et les applications de ses kata.

**UV 5 : Jiyu ippon kumite**
- **3 attaques, avec leur niveau, annoncées**, choisies **par le juge** parmi : Oï zuki, Mae geri, Mawashi geri, Yoko geri, Gyaku zuki, Maete zuki. Retour en Hachi ji dachi après chaque attaque.
- Détails en section 10.6.

**UV 6 : Ju kumite (randori ou midare)**
- **1 assaut souple de 2 minutes maximum**, pour juger les qualités techniques des deux candidats.
- Critères : travail en souplesse avec **contrôle absolu**, aisance dans les déplacements, variété des techniques, équilibre et stabilité, opportunité, zanshin.
- Le **midare** est spécifique au style Shotokaï (1 minute par rôle).

### 5.4 1er Dan, voie compétition (vérifié)

Référence : **REF-TECH art. 402-2K, p. 7 à 8**.

- **4 épreuves** : UV 1 Kihon, UV 2 Kihon ippon kumite, UV 3 Kata (contenu **identique** à la voie traditionnelle), plus **5 participations à des compétitions**.
- Obtention du grade avec **30/60 et les 5 participations**.
- Compétitions officielles des organes déconcentrés (département, zone inter-départementale, ligue, national, coupes de styles), **en individuel uniquement**. Au moins **un tour**, en combat ou en kata, à chaque participation.
- **Choix définitif** sur le dossier d'inscription entre combat et kata : pas de cumul.
- Les 5 participations comptent pour **la saison sportive du jour de l'examen** et sont reportées sur le **passeport sportif**.

### 5.5 Bonification de points (1er au 5e Dan, vérifié)

Références : **REF-GEN art. 402, p. 26 à 27** ; art. 403-A, p. 27 à 28 ; bonification en temps de pratique : art. 209, p. 19 à 20.

**1 point maximum** par passage de grade, au choix parmi :

| Mode | Détail |
|---|---|
| Stage de préparation aux grades | Dirigé par un haut gradé d'**au moins 6e Dan**, organisé par un organe déconcentré. 1 point par stage |
| Stage d'expert fédéral | Organisé par un organe déconcentré ou au niveau national. 1 point par stage |
| Diplôme d'enseignant | Instructeur fédéral, CQP, BEES 1er ou 2e degré, DEJEPS, DESJEPS. Seul le dernier diplôme compte, un nouveau diplôme ouvre un nouveau point |

- Le point est **certifié sur le passeport du candidat** par le haut gradé ou l'expert qui dirige le stage.
- Pour le 1er Dan, les stages sont **ouverts aux ceintures marron**.
- Aucune bonification de points pour les 6e, 7e et 8e Dan.
- À ne pas confondre avec la **bonification en temps de pratique** (art. 209), réservée aux titres sportifs (médaillés, champions) : demande à adresser au président de la CSDGE au plus tard **45 jours** avant l'examen.

### 5.6 Les autres grades Dan (vérifié, résumé)

Références : REF-TECH art. 403-1K (2e Dan) p. 9 à 11 ; 3e Dan p. 14 à 16 ; art. 405K (4e Dan) p. 18 à 19 ; art. 406K (5e Dan) p. 20 à 21 ; art. 407K (6e Dan) p. 22 à 23 ; art. 408K (7e Dan) p. 24 à 25 ; art. 409K (8e Dan) p. 26 à 27.

| Grade | Examen |
|---|---|
| 2e Dan | Mêmes 6 UV, 60/120. **Kihon** : linéaire 3 parmi **7**, poursuite 3 parmi les **4** restants, cibles 5 parmi 7. **Ippon kumite** : les deux partenaires en garde, attaques et niveau annoncés, **6 attaques**, droite puis gauche, toutes en **Ayumi ashi** (Oï zuki jodan, Oï zuki chudan, Mae geri chudan, Mawashi geri jambe avant, Mawashi geri jambe arrière, Yoko geri jambe arrière). **Kata** : 2 kata de la liste du 2e Dan. **Jiyu ippon kumite** : attaque libre, **niveau annoncé** (jodan ou chudan), **5 assauts** |
| 3e Dan | Mêmes 6 UV, 60/120. **Kihon** : linéaire 3 parmi **10**, poursuite 3 parmi les **7** restants. **Ippon kumite** : mêmes 6 attaques, perception de l'attaque supérieure à celle du 2e Dan. **Kata** : liste du 3e Dan. **Jiyu ippon kumite** : **5 attaques**, attaque libre, Tori ne fait **aucune annonce** (au-delà du 2e Dan) |
| 4e et 5e Dan | **4 épreuves** notées sur 20, 40/80 : Kihon (kihon libre dans différentes directions, puis enchaînements de 2 à 3 techniques sur cibles en deux pas lents), Kumité (Jiyu ippon kumite, **5 attaques**, on reste en garde et on reprend le maaï après chaque échange), Kata (2 kata, chacun noté sur 10), Bunkai (**plusieurs applications**) |
| 6e Dan | Kihon : prestation personnelle de 10 min, **5 enchaînements de 5 techniques** puis démonstration de leur utilité avec un partenaire (majeur, 1er Dan minimum). Kumite traditionnel : 5 attaques et 5 défenses. Kata et bunkai. Entretien. 40/80, pas de bonification de points |
| 7e Dan | 4 épreuves (entretien et 3 épreuves techniques), 40/80 (non détaillé ici) |
| 8e Dan | 3 UV : Kihon (**3 enchaînements de 3 techniques**), Kata et bunkai (**2 kata, 3 séquences de bunkai chacun**), Entretien. 30/60 |

### 5.7 Programme des kata Shotokan Ryu par Dan (annexe V, vérifié)

Référence : **REF-TECH annexe V, p. 41** (autres styles : p. 41 à 43).

| Dan | Kata |
|---|---|
| 1er Dan | Heian 1-2-3-4-5, Tekki shodan |
| 2e Dan | Bassai dai, En-pi, Jion, Hangetsu, Kanku dai |
| 3e Dan | Gankaku, Jitte, Kanku sho, Bassai sho, Tekki nidan |
| 4e Dan | Nijushi ho, Sochin, Jiin, Unsu, Tekki sandan |
| 5e Dan | Gojushi ho sho, Chinté, Gojushi ho dai, Wankan, Meikyo |

L'annexe V donne aussi les listes d'autres styles (Shotokaï, Wado ryu, Shito ryu, Goju ryu, etc.). **Au 1er Dan, un candidat peut choisir ses kata dans la liste du 1er Dan de n'importe quel style.**

### 5.8 Conditions d'inscription et organisation (règlement général, vérifié)

Références : REF-GEN art. 107 (1er Dan en club), p. 9 à 10 ; art. 204 (dossier), p. 16 à 17 ; art. 205 (âges), p. 17 ; art. 206 (temps de pratique), p. 18 ; art. 207 (fréquence), p. 19 ; art. 217 (arbitres), p. 24 ; art. 401 (jury), p. 26.

| Grade | Âge minimum | Délai et licences avant le passage |
|---|---|---|
| 1er Dan | 14 ans et **ceinture marron (1er kyu)** | 3 timbres de licence, dont la saison en cours |
| 2e Dan | 17 ans | 2 ans de pratique de date à date et 2 timbres après le 1er Dan |
| 3e Dan | 21 ans | 3 ans et 3 timbres après le 2e Dan |
| 4e Dan | 25 ans | 4 ans et 4 timbres après le 3e Dan |
| 5e Dan | 30 ans | 5 ans et 5 timbres après le 4e Dan |
| 6e Dan | 40 ans | 6 ans et 6 timbres après le 5e Dan (et 20 ans de pratique au 1er Dan) |

Une tolérance de **20 jours** est accordée sur le délai entre deux grades.

**Dossier et calendrier**
- Dossier à envoyer **30 jours francs** avant l'examen (45 jours pour la zone nationale et la CSDGE). Un envoi tardif rend le dossier irrecevable.
- Pièces : formulaire, photo d'identité, attestation d'historique des licences et grades, justificatifs de bonification s'il y a lieu. **Passeport sportif FFK** renseigné obligatoire.
- Le candidat se présente dans le **secteur géographique où il est licencié**.
- Au 1er Dan, **2 passages maximum par saison** pour un candidat ; au moins 2 passages par saison organisés par le comité départemental.
- Le grade n'est valable qu'une fois **validé par la CSDGE** et inscrit au fichier national et sur le diplôme officiel.

**1er Dan organisé en club (art. 107)**
- Le professeur doit être référencé dans la base fédérale et posséder **au minimum le 2e Dan**.
- Il faut au minimum **3 gradés 2e Dan minimum**, professeur compris (les autres peuvent venir d'autres clubs FFK).
- Un club de **moins de 100 licenciés** peut organiser **2 passages de 1er Dan au maximum** par saison.
- Dès qu'un club organise ce passage, **tout licencié du club** candidat au 1er Dan doit le passer dans ce club. Si le club ne l'organise pas, les candidats passent au comité départemental.
- Le professeur et les deux gradés signent le bordereau d'admission, et le président de la CODG paraphe les passeports.

**Jury** : 3 juges par table, de grade supérieur au grade examiné ; un seul peut être licencié dans le même club que le candidat. Contestation : écrite, au président de la CSDGE dans les **30 jours** après les résultats.

**Arbitres** (art. 217) : un arbitre en activité depuis au moins 1 an (départemental pour le 1er Dan, régional pour le 2e, national pour le 3e) peut être exempté d'une épreuve, notée alors **12/20**. Les arbitres kata sont exemptés de kata, les arbitres kumité du Ju kumite.

### 5.9 Corrections par rapport à la première version

| Point | Version provisoire (synthèses secondaires) | Règlement officiel |
|---|---|---|
| Kihon sur cibles | 5 techniques parmi 7 choisies par le candidat | Les 5 techniques sont choisies **par le juge** ; seul le côté (droite ou gauche) est au choix du candidat |
| Kata du 1er Dan Shotokan | Bassai dai, Kanku dai ou Tekki shodan possibles | **Heian 1 à 5 et Tekki shodan.** Bassai dai et Kanku dai sont des kata du **2e Dan** |
| Kihon ippon kumite | Contre-attaque codifiée | **Défense et contre-attaque libres** |
| Jiyu ippon kumite (1er Dan) | Go no sen / Sen no sen « selon le grade » | **3 attaques annoncées** par Tori, choisies par le juge parmi 6 techniques. Go no sen et Sen no sen sont des notions fondamentales à démontrer |
| Intitulés des UV 2 à 5 | À compléter | Confirmés : Kihon ippon kumite, Kata, Bunkai, Jiyu ippon kumite |
| Partenaire majeur et 1er Dan | Cité pour la voie compétition | Exigé pour les épreuves de **6e à 8e Dan**, pas au 1er Dan |
| Bonification | Stages et diplômes | Confirmée. Pour le 1er Dan, stages ouverts aux **ceintures marron** |

---

## 6. Programme du club (base de départ)

Source : fichier **PROGRAMME_2023.docx** (« Programme Karaté Renoji 2023 »), repris tel quel, orthographe du club conservée. Les catégories reprennent le code couleur du document : Posture (gris), Poing (rouge), Jambe (bleu), Blocage (vert), Kata (violet), Exercice (sans couleur).

La colonne **« Cerise sur le gâteau »** désigne les éléments bonus du club, au-delà du programme de base.

### Blanche → Jaune

| Technique à connaître | Catégorie | Cerise sur le gâteau |
|---|---|---|
| Yoi | Posture |  |
| Zazen | Posture |  |
| Zenkutsu | Posture |  |
| Kokutsu | Posture |  |
| Choku Tsuki | Poing |  |
| Gyaku Tsuki | Poing |  |
| Oi Tsuki | Poing |  |
| Mae giri | Jambe | Mawashi |
| Gedan Barai | Blocage |  |
| Age uke | Blocage |  |
| Teisho uke | Blocage | Soto uke |
| Gohon Kumite | Exercice |  |
| Heian Shodan | Kata | Taikyoku Shodan et Nidan |

### Jaune → Orange

| Technique à connaître | Catégorie | Cerise sur le gâteau |
|---|---|---|
| Kiba dachi | Posture |  |
| Tetsui | Poing |  |
| Uraken | Poing | Kage tsuki |
| Shuto ushi | Blocage |  |
| Soto uke | Blocage |  |
| Uchi uke | Blocage |  |
| Morote uke | Blocage |  |
| Shuto uke | Blocage |  |
| Ashi barai | Blocage |  |
| Uchi komi | Jambe | "Demi circulaire" |
| Mawashi geri | Jambe |  |
| Yoko geri kaege kekomi | Jambe | Ura mawashi |
| Changement de position Kokutsu / Zenkutsu | Exercice | Jyu kumite |
| Sambon kumite | Exercice |  |
| Taikyoku sandan | Kata |  |
| Heian shodan | Kata |  |

### Orange → Verte

| Technique à connaître | Catégorie | Cerise sur le gâteau |
|---|---|---|
| Fudo dachi | Posture |  |
| Nukite | Poing | "Patte d'ours" |
| Tate tsuki | Poing |  |
| Kizami tsuki | Poing |  |
| Tobikomi tsuki | Poing |  |
| Empi age, mawashi, ushiro, yoko, otoshi | Poing | Empi no kata |
| Ushiro geri | Jambe |  |
| Mikazuki geri | Jambe | Ura mikazuki |
| Sanbon kumite | Exercice |  |
| Jyu ippon kumite | Exercice |  |
| Taikyoku godan | Kata |  |
| Heian nidan | Kata | Jyu kumite |

### Verte → Bleue

| Technique à connaître | Catégorie | Cerise sur le gâteau |
|---|---|---|
| Fumikomi | Jambe | Ushiro mawashi |
| Hiza geri | Jambe |  |
| Ura mawashi | Jambe |  |
| Empi uke | Blocage |  |
| Juji uke | Blocage |  |
| Kakiwake uke | Blocage |  |
| Ura tsuki | Poing | Hiraken tsuki |
| Ipon kumite | Exercice |  |
| Ten no kata | Kata |  |
| Heian Sandan | Kata |  |

### Bleue → Marron

| Technique à connaître | Catégorie | Cerise sur le gâteau |
|---|---|---|
| Hangetsu dachi | Posture |  |
| Neko ashi dachi | Posture |  |
| Kage tsuki | Poing |  |
| Haito uchi | Poing |  |
| Teisho uchi | Poing |  |
| Haito uke | Blocage |  |
| Haishu uke | Blocage |  |
| Ushiro mawashi geri | Jambe |  |
| Kakato geri | Jambe | Casse de planche |
| Heian Yodan | Kata | Empi no kata |
| Jiyu kumite | Exercice |  |

### Marron → Noire

| Technique à connaître | Catégorie | Cerise sur le gâteau |
|---|---|---|
| Sanchin dachi | Posture |  |
| Morote tsuki | Poing |  |
| Yama tsuki | Poing |  |
| Tobikonde tsuki | Poing |  |
| Age tsuki | Poing |  |
| Kumite kata | Exercice | Casse de planche |
| Sen no sen | Exercice |  |
| Heian godan | Kata |  |
| Bassai dai | Kata |  |
| Kanku dai | Kata |  |
| Tekki shodan | Kata |  |

---

## 7. Écarts entre le programme du club et la réglementation FFKDA 2026-2027

✅ Analyse vérifiée sur les documents officiels (section 5 ; références dans [l'index](reglementation/INDEX_REGLEMENTATION_FFKDA.md)).

### 7.1 Périmètre

- Le règlement de la CSDGE régit les **examens de Dan**. Le programme du club couvre les ceintures **blanche → noire** : seul le dernier palier, **Marron → Noire (1er Dan)**, est directement concerné.
- Aucun écart n'est calculé pour les kyu (blanche à marron) : ce n'est pas l'objet de ce règlement.
- **Aucune ceinture marron** au club en ce moment : le 1er Dan n'est pas une échéance immédiate. Les **7 ceintures noires** sont, elles, concernées par les examens de **2e Dan et plus** (grade exact à préciser).

### 7.2 Déjà couvert par le programme du club

| Exigence officielle | Ce que le programme du club contient |
|---|---|
| Kata du 1er Dan Shotokan : Heian 1 à 5, Tekki shodan | ✅ Heian shodan à godan (une par palier) et Tekki shodan (marron → noire) |
| Techniques du Kihon ippon kumite : Oï zuki jodan et chudan, Mae geri, Mawashi geri, Yoko geri | ✅ Oi tsuki, Mae geri (jaune), Mawashi geri, Yoko geri kekomi (orange) |
| Jiyu ippon kumite | ✅ « Jyu ippon kumite » (orange → verte) |
| Ju kumite (assaut souple) | ✅ « Jyu kumite » et « Jiyu kumite » (orange et bleue → marron) |
| Sen no sen | ✅ Marron → noire |
| Techniques du kihon sur cibles : Mae geri, Mawashi geri, Gyaku zuki, Oï zuki | ✅ Présentes |
| Kata de niveau supérieur (2e Dan) : Bassai dai, Kanku dai | ✅ Marron → noire. Hors liste du 1er Dan, utiles au 2e Dan |

### 7.3 Écarts : à ajouter au programme (1er Dan)

| # | Exigence officielle | État dans le programme du club | Action proposée |
|---|---|---|---|
| 1 | **Kihon linéaire** : 3 enchaînements de 3 techniques tirés au sort parmi 6 | Absent : le programme liste des techniques isolées | ⏳ Récupérer la liste officielle (clubs, site FFKDA), créer une fiche |
| 2 | **Kihon en poursuite** : les 3 enchaînements restants, de 0° à 180° garde gauche puis de 180° à 0° garde droite | Absent | Apprendre la procédure (5.3), l'entraîner avec la liste officielle |
| 3 | **Kihon sur cibles** : 5 techniques parmi 7, choisies par le jury, Uke par sursauts lents | Techniques présentes, exercice absent | Entraînement à deux avec cibles. **Kizami zuki / Maete zuki** (technique n° 6) absent du programme |
| 4 | **Bunkai** : 3 séquences minimum d'un des deux kata exécutés | **Absent** | Associer des applications à chaque kata de 1er Dan du club |
| 5 | **Kata** : 2 kata dans la liste du 1er Dan, annoncés à voix haute, salut au début et à la fin | Kata couverts, cérémonial non décrit | Ajouter les critères de notation (annexe IV). Erreur = on recommence, note ÷2 |
| 6 | **Kihon ippon kumite** : format codifié (1 m, Hachi ji dachi, attaque annoncée, défense libre, côtés alternés) | Format non décrit | Ajouter la procédure d'examen |
| 7 | **Jiyu ippon kumite** : 3 attaques annoncées, 3 m, Hajime, retour en Hachi ji dachi | Format non décrit | Ajouter la procédure (10.6) |
| 8 | **Go no sen et Sen no sen** démontrés dans les défenses | Seul **Sen no sen** cité | Ajouter **Go no sen** |
| 9 | **Hachi ji dachi** (position d'attente à l'examen), **Ayumi ashi, Yori ashi, Tsugi ashi** (déplacements du Jiyu ippon kumite) | Absents du programme | Ajouter au programme des postures et déplacements |
| 10 | **Ju kumite** : 2 minutes maximum, contrôle absolu | Jiyu kumite présent | Formaliser le format de 2 minutes |
| 11 | **Bonification** : 1 point maximum (stage ≥ 6e Dan ou expert fédéral, diplôme), au passeport | Non couvert | Ajouter une rubrique « préparer son Dan » |
| 12 | **Voie compétition** : 5 participations sur la saison de l'examen | Non couvert | Mentionner comme alternative |
| 13 | **Conditions d'inscription** : 14 ans, ceinture marron, 3 timbres, dossier à 30 jours francs, passeport sportif | Non couvert | Page « Passer son grade » sur le site (5.8) |
| 14 | **Terminologie officielle en japonais** employée par le jury | Lexique partiel | Lexique complété en section 9 |

### 7.4 Pour les 7 ceintures noires (grade Dan à préciser)

Selon le Dan visé, le contenu change nettement :

| Dan visé | Kata à travailler (Shotokan) | Kihon | Kumite |
|---|---|---|---|
| 2e Dan | Bassai dai, En-pi, Jion, Hangetsu, Kanku dai | Linéaire 3 parmi 7, poursuite 3 parmi 4 restants, cibles | Ippon kumite (6 attaques annoncées), Jiyu ippon kumite (niveau annoncé, 5 assauts) |
| 3e Dan | Gankaku, Jitte, Kanku sho, Bassai sho, Tekki nidan | Linéaire 3 parmi 10, poursuite 3 parmi 7 restants, cibles | Ippon kumite (6 attaques), Jiyu ippon kumite (5 attaques sans annonce) |
| 4e Dan | Nijushi ho, Sochin, Jiin, Unsu, Tekki sandan | Kihon libre + enchaînements de 2 à 3 techniques sur cibles | Jiyu ippon kumite, 5 attaques, on reste en garde |
| 5e Dan | Gojushi ho sho, Chinté, Gojushi ho dai, Wankan, Meikyo | Idem 4e Dan | Idem 4e Dan |

Chaque examen inclut aussi le **bunkai** (3 séquences d'un des deux kata, plusieurs applications à partir du 4e Dan).

### 7.5 Observations sur le programme du club

À confirmer avec ton professeur (je n'ai rien corrigé dans la section 6) :
- **Heian shodan** apparaît deux fois (blanche → jaune, puis jaune → orange).
- **Taikyoku** : shodan, nidan, sandan, godan sont cités ; **yondan** n'apparaît pas.
- Doublons possibles : **Sanbon kumite** (jaune → orange et orange → verte), **Kage tsuki** (cerise jaune → orange et palier bleue → marron), **Ura mawashi** (cerise jaune → orange et palier verte → bleue).
- Fautes de frappe probables : « Mae giri » (Mae geri), « Shuto ushi » (Shuto uchi), « Yoko geri kaege kekomi » (keage ?), « Ipon » (Ippon), « Jyu » (Jiyu), « Sambon » (Sanbon).
- « **Kumite kata** » (marron → noire) : intitulé à préciser.

---

## 8. Organisation des séances d'entraînement

### 8.1 Composition du club

| Niveau | Effectif | Ceinture visée (programme du club) |
|---|---|---|
| Débutants | 5 blanches | Jaune |
| Débutants avancés | 2 jaunes | Orange |
| Intermédiaire | 1 verte | Bleue |
| Avancés | 7 noires | Dan supérieur (grade exact ⏳) |
| **Total** | **15** | |

Constats utiles :
- **8 ceintures de couleur pour 7 ceintures noires** : un encadrement en binômes (un gradé avec un moins gradé) est possible.
- La répartition est **très inégale** : 5 blanches, 1 seule verte, aucune orange, bleue ni marron.
- Les **7 noires** n'ont plus de contenu dans le programme actuel du club (qui s'arrête à la ceinture noire). Le contenu Dan est à construire, et c'est là que la nouvelle réglementation 2026-2027 pèse le plus (voir sections 5.3 à 5.7).

### 8.2 Proposition de structure (à valider avec le professeur) 🔶

Deux séances par semaine (mardi et jeudi). Une séance type en 3 temps :

| Temps | Contenu | Qui |
|---|---|---|
| 1. Socle commun | Échauffement, Kihon de base, kata fondamentaux | Tout le monde ensemble |
| 2. Ateliers par niveau | Programme de la ceinture visée | 3 groupes (voir ci-dessous) |
| 3. Application | Kumite et exercices à deux, avec binômes mixtes | Mixte : un gradé avec un moins gradé |

**Groupes d'ateliers**

| Groupe | Membres | Travail |
|---|---|---|
| A | 5 blanches + 2 jaunes (7) | Programme Blanche → Jaune, puis Jaune → Orange pour les jaunes. Postures, coups de base, Heian shodan |
| B | 1 verte | Programme Verte → Bleue. Pour ne pas la laisser isolée : l'associer au groupe A comme référente sur les bases, ou travailler avec une ceinture noire |
| C | 7 noires | Préparation des Dan : kihon par enchaînements, kata, bunkai, ippon kumite (contenu 2026-2027) |

**Alternance suggérée sur la semaine**

| Séance | Dominante |
|---|---|
| Mardi | Kihon et kata : programme technique de chaque groupe |
| Jeudi | Kumite et applications : ippon kumite, bunkai, assauts adaptés au niveau |

Principes pour des séances homogènes :
- garder un **temps commun** pour que le groupe reste soudé ;
- différencier par la **profondeur**, pas par le contenu : même exercice, exigence adaptée au grade ;
- les ceintures noires **encadrent** les débutants sur une partie de la séance, ce qui sert aussi leur propre préparation de grade (démonstration, explication) ;
- adapter le kumite : contrôle strict et distance plus grande entre noires et blanches.

### 8.3 Impact sur le site

- **Espace membres** : chaque membre voit **son groupe** et le **programme de son prochain passage**.
- **Section grades** : une page par niveau, alimentée par le programme du club (section 6).
- ⏳ La durée et les horaires des séances (flyers) conditionnent le découpage en temps 1, 2 et 3.

### 8.4 Séance 1 : jeudi 12 novembre 2026 (kumite et applications)

**Durée prévue : 1h30** 🔶 (hypothèse, à ajuster selon les horaires des flyers)
**Encadrement** : le responsable de séance + 2 ceintures noires assistantes
**Participants** : 5 blanches, 2 jaunes, 1 verte, 7 noires

| Temps | Durée | Contenu | Qui |
|---|---|---|---|
| Rei et échauffement | 10 min | Mobilité, gainage léger, déplacements | Tous |
| Kihon commun | 15 min | En lignes : Oi tsuki, Gyaku tsuki, Mae geri, Age uke, Soto uke. Exigence adaptée au grade | Tous |
| Ateliers par niveau | 30 min | Voir tableau ci-dessous | 3 groupes en parallèle |
| Application en binômes | 25 min | La noire attaque et contrôle vitesse et distance, le moins gradé défend. Montée progressive : attaque annoncée, Ippon kumite, puis Jiyu ippon kumite pour la verte et les noires. Changement de partenaire toutes les 5 min. 7 gradés pour 8 ceintures de couleur : 6 binômes et 1 trio | Binômes mixtes (assistants inclus) |
| Retour au calme | 10 min | Heian shodan lent et ensemble, étirements, mokuso, rei | Tous |

**Ateliers (30 min)**

| Groupe | Effectif | Encadrant | Travail |
|---|---|---|---|
| Blanches | 5 | Assistant 1 | 10 min postures et blocages (Zenkutsu, Kokutsu, Age uke, Gedan barai). 10 min Gohon kumite à deux (l'assistant fait binôme avec la 5e). 10 min Heian shodan |
| Intermédiaires | 3 (2 jaunes + 1 verte) | Assistant 2 | Jaunes : Sambon kumite ensemble. Verte : Ippon kumite avec l'assistant. Permutation à mi-atelier |
| Noires | 5 (+ le responsable) | Responsable de séance | Kihon ippon kumite : 5 attaques, une fois à droite puis une fois à gauche, **en alternance** (règle 2026-2027). Puis 3 séquences de bunkai tirées d'un kata (règle 2026-2027) |

**Briefing des assistants (5 min avant la séance)** : contenu de leur atelier minute par minute ; contrôle strict de la distance et de la vitesse face aux débutants ; retour court après chaque passage (un point positif, un point à corriger).

**Version 1h** : retour au calme réduit à 5 min, kihon commun à 10 min, ateliers à 20 min.

**Bilan (à remplir après la séance)** : ⏳

### 8.5 Séance 2 (classique, exercices à deux) : postures, renforcement, kata et bunkai

**Durée prévue : 1h30** 🔶 (hypothèse, à ajuster selon les horaires des flyers)
**Encadrement** : le responsable de séance (noires) + 1 assistante (intermédiaires) + 1 assistante (blanches)
**Intensité visée** : assez physique pour les noires, physique pour les intermédiaires, un peu soutenue pour les blanches
**Principe** : chacun travaille avec un partenaire, qui compte, observe et corrige. Chacun applique la dose de son propre niveau.
**Fin de séance** : le module kata et bunkai (section 8.6) clôt la séance.

| Temps | Durée | Contenu | Qui |
|---|---|---|---|
| Rei et échauffement | 20 min | 5 min : rei, mobilité articulaire (cou, épaules, hanches, genoux, chevilles). 5 min : montée en température (déplacements, pas chassés, sauts légers). 5 min : technique à vide sans intensité (Oi tsuki, Gyaku tsuki, Mae geri, Age uke, balancés de jambes). 5 min : en binômes, étirements dynamiques en miroir et gainage léger | Tous |
| Ateliers par niveau | 25 min | Postures à deux (10 min) puis renforcement à deux (15 min), voir tableau ci-dessous | 3 groupes en parallèle |
| **Carrousel des partenaires** | 20 min | Exercice où tout le monde tourne ensemble (voir ci-dessous) | Tous |
| **Kata et bunkai** | 20 min | Module de la section 8.6 : kata commun (3 min), ateliers par niveau (12 min), bunkai croisé en binômes mixtes (5 min) | Tous |
| Retour au calme | 5 min | Étirements à deux, doux et sans forcer, mokuso, rei | Tous |

**Ateliers (25 min)**

| Groupe | Effectif | Encadrant | Intensité | Travail |
|---|---|---|---|---|
| Noires | 5 (+ le responsable, soit 3 binômes) | Responsable | Assez physique | 10 min postures profondes à deux : Kiba dachi, Hangetsu dachi, Sanchin dachi, tenues de 45 à 60 s, l'autre vérifie genoux, bassin, dos et teste la stabilité par une poussée douce sur l'épaule. 15 min renforcement à deux : 3 tours de 4 exercices (40 s / 20 s) : planche face à face avec tape de main, pompes avec tape d'épaule, squats synchronisés, abdos pieds tenus |
| Intermédiaires | 3 (2 jaunes + 1 verte) + l'assistante, soit 2 binômes | Assistante | Physique | 10 min postures à deux : changement Kokutsu / Zenkutsu, Kiba dachi, Fudo dachi pour la verte. 15 min renforcement à deux : 3 tours de 4 exercices (30 s / 30 s), mêmes exercices que les noires, dose adaptée |
| Blanches | 5 + l'assistante, soit 3 binômes | Assistante | Un peu soutenue | 10 min postures à deux : Zenkutsu et Kokutsu, correction mutuelle. 15 min renforcement léger à deux : 3 tours de 4 exercices (20 s / 40 s) : planche face à face, squats synchronisés, pompes sur les genoux, abdos pieds tenus |

**Carrousel des partenaires (20 min)**

Deux lignes face à face. Au signal, la ligne extérieure se décale d'un cran et **tout le monde change de partenaire en même temps**. 5 manches de 4 min : 1 min 30 pour chacun des deux partenaires, 1 min de rotation. Avec 15 personnes, le 8e élève forme un trio, qui change de place à chaque rotation. Chacun applique la dose de son niveau, et l'autre compte et corrige.

| Manche | Exercice à deux | Blanches | Intermédiaires | Noires |
|---|---|---|---|---|
| 1 | Kiba dachi : tenue avec poussée douce de test par le partenaire | 30 s | 45 s | 60 s |
| 2 | Planche face à face avec tape de main alternée | 10 tapes | 15 | 20 |
| 3 | Zenkutsu dachi : aller-retour avec Gedan barai puis Oi tsuki, le partenaire corrige la posture | 4 aller-retours | 6 | 8, très bas |
| 4 | Mae geri sur paume (le partenaire tient la cible) | 5 par jambe | 8 | 10 |
| 5 | Squats synchronisés face à face | 10 | 15 | 20 |

L'intensité vient de la dose, jamais de la technique : tout le monde fait le même exercice.

**Sécurité** : poussées de test douces et contrôlées, jamais sur les genoux ni sur la tête ; choisir un partenaire de morphologie proche pour les tests de stabilité ; étirements à deux sans forcer ; s'hydrater ; s'arrêter en cas de douleur ; ne pas forcer sur les genoux en postures basses.

**Briefing des assistantes (5 min avant)** : dose par niveau, une seule correction à la fois, posture avant vitesse.

**Variante avec plus de temps** (par exemple 1h50) : remettre 10 min de retour au calme et 10 min de postures à deux en commun avant les ateliers.

**Bilan (à remplir après la séance)** : ⏳

### 8.6 Module de 20 minutes : kata et bunkai

Module autonome, à utiliser seul ou à intégrer dans une séance.
**Encadrement** : le responsable de séance (noires) + 1 assistante (intermédiaires) + 1 assistante (blanches)
**Principe** : exercices à deux, chacun avec le kata de son programme.

| Temps | Durée | Contenu | Qui |
|---|---|---|---|
| Kata commun | 3 min | Rei, puis Heian shodan lent et ensemble, en lignes, en soignant les postures | Tous |
| Ateliers par niveau | 12 min | Voir tableau ci-dessous | 3 groupes en parallèle |
| Bunkai croisé | 5 min | Binômes mixtes : la noire joue l'attaquant (Uke), la ceinture de couleur applique (Tori) le bunkai de Heian shodan, puis on inverse. Rei | Binômes mixtes |

**Ateliers (12 min)**

| Groupe | Encadrant | Kata (min) | Bunkai (min) |
|---|---|---|---|
| Blanches (5, binômes) | Assistante | 4 min : Heian shodan, l'un exécute, l'autre corrige un point, on inverse | 8 min : 2 séquences à deux, lent puis vitesse normale. Séquence 1 : Gedan barai contre une attaque de jambe (Mae geri), puis Gyaku tsuki en riposte. Séquence 2 : Age uke contre Oi tsuki jodan, puis Gyaku tsuki 🔶 |
| Intermédiaires (2 jaunes + 1 verte, avec l'assistante) | Assistante | 5 min : chacun son kata (jaunes : Heian shodan ou Taikyoku sandan ; verte : Heian nidan ou sandan), le partenaire corrige | 7 min : 2 séquences du kata choisi, une application chacune, lent puis vitesse normale |
| Noires (5 + le responsable, 3 binômes) | Responsable | 4 min : un kata de leur liste de Dan (voir 5.7), un passage sans erreur ; **toute erreur = on recommence** (règle 2026-2027 : note divisée par 2) | 8 min : **3 séquences issues du kata exécuté** (règle 2026-2027), démonstration lente puis à vitesse normale, le partenaire attaque, l'autre corrige |

🔶 Les applications proposées pour les blanches sont des exemples classiques à valider avec le professeur. Pour les noires, c'est à chaque binôme de choisir ses 3 séquences dans le kata.

**Sécurité** : bunkai à vitesse et distance contrôlées, jamais de frappe en contact sur le visage, le partenaire qui attaque reste précis et stable.

**Bilan (à remplir après la séance)** : ⏳

---

## 9. Lexique technique (français, japonais, écriture japonaise)

Ce lexique reprend les termes du programme du club (section 6) et ceux utilisés dans les séances et la réglementation. Les écritures japonaises sont données en kanji (ou kana). Les transcriptions suivent le système Hepburn.

🔶 **Établi de mémoire, sans source en ligne vérifiée.** Les éléments marqués 🔶 sont incertains. À relire par le professeur avant publication sur le site.

### 9.1 Postures (dachi)

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Yoi | 用意 | Prêt (position d'attente avant de commencer) |  |
| Zazen | 坐禅 | Méditation assise (zen) | Écrit aussi 座禅 |
| Dachi | 立ち | Posture, position debout | Se suffixe : Zenkutsu dachi |
| Zenkutsu dachi | 前屈立ち | Posture avant (jambe avant fléchie) |  |
| Kokutsu dachi | 後屈立ち | Posture arrière (poids sur la jambe arrière) |  |
| Kiba dachi | 騎馬立ち | Posture du cavalier |  |
| Fudo dachi | 不動立ち | Posture immobile, inébranlable |  |
| Hangetsu dachi | 半月立ち | Posture en demi-lune |  |
| Neko ashi dachi | 猫足立ち | Posture du pied de chat |  |
| Sanchin dachi | 三戦立ち | Posture des « trois combats » (tension et respiration) |  |
| Kamae | 構え | Garde, position d'attente de combat |  |

### 9.2 Attaques de poing et de main

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Tsuki (zuki) | 突き | Coup de poing, frappe directe | Après un autre mot, « tsuki » devient « zuki » : Oi zuki, Gyaku zuki |
| Choku tsuki | 直突き | Coup de poing direct (droit) |  |
| Gyaku tsuki | 逆突き | Coup de poing inversé (bras opposé à la jambe avant) |  |
| Oi tsuki | 追い突き | Coup de poing en poursuite (même côté que la jambe avant) |  |
| Kizami tsuki | 刻み突き | Coup de poing du bras avant (type direct avant) |  |
| Tate tsuki | 縦突き | Coup de poing vertical (poing de profil) |  |
| Tobikomi tsuki | 飛び込み突き | Coup de poing en bondissant vers l'avant | Le programme cite aussi « Tobikonde tsuki » 🔶 à vérifier si c'est la même technique |
| Ura tsuki | 裏突き | Coup de poing court, de près (paume vers le haut) |  |
| Hiraken tsuki | 平拳突き | Frappe avec les phalanges à plat |  |
| Morote tsuki | 諸手突き | Coup de poing à deux mains |  |
| Yama tsuki | 山突き | Coup de poing en « montagne » (double poing, haut et bas) |  |
| Age tsuki | 上げ突き | Coup de poing montant |  |
| Kage tsuki | 鉤突き | Coup de poing en crochet | Terme officiel (annexe II) : Kagi zuki |
| Uraken | 裏拳 | Revers du poing (dos du poing) |  |
| Tetsui | 鉄槌 | Marteau de fer (frappe avec le bas du poing) |  |
| Nukite | 貫手 | Frappe de la pointe des doigts (main en lance) |  |
| Kumade | 熊手 | Main d'ours (« patte d'ours ») | 🔶 Probable équivalent de « Patte d'ours » au programme |
| Shuto uchi | 手刀打ち | Frappe en tranchant de la main | Écrit « Shuto ushi » au programme |
| Haito uchi | 背刀打ち | Frappe avec le bord interne de la main (côté pouce) |  |
| Teisho uchi | 底掌打ち | Frappe avec la base de la paume |  |
| Empi (enpi) | 猿臂 | Coude, frappe de coude | 🔶 Variantes : age (montant), mawashi (circulaire), ushiro (arrière), yoko (latéral), otoshi (descendant) |

### 9.3 Blocages (uke)

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Uke | 受け | Blocage, parade (aussi : celui qui reçoit l'attaque) |  |
| Gedan barai | 下段払い | Balayage bas (blocage bas) |  |
| Age uke | 上げ受け | Blocage montant (haut) |  |
| Soto uke | 外受け | Blocage de l'extérieur vers l'intérieur |  |
| Uchi uke | 内受け | Blocage de l'intérieur vers l'extérieur |  |
| Teisho uke | 底掌受け | Blocage avec la base de la paume |  |
| Morote uke | 諸手受け | Blocage à deux mains (renforcé) |  |
| Shuto uke | 手刀受け | Blocage en tranchant de la main |  |
| Empi uke | 肘受け | Blocage du coude |  |
| Juji uke | 十字受け | Blocage en croix (deux mains croisées) |  |
| Kakiwake uke | 掻き分け受け | Blocage qui écarte (deux mains en V) |  |
| Haito uke | 背刀受け | Blocage avec le bord interne de la main |  |
| Haishu uke | 背手受け | Blocage avec le dos de la main |  |

### 9.4 Attaques de jambe (geri) et balayages

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Geri | 蹴り | Coup de pied | Peut devenir « geri » ou « keri » selon le mot |
| Mae geri | 前蹴り | Coup de pied de face | Écrit « Mae giri » au programme |
| Mawashi geri | 回し蹴り | Coup de pied circulaire |  |
| Yoko geri | 横蹴り | Coup de pied latéral | Deux formes officielles : Yoko keage 蹴上げ (remontant), Yoko kekomi 蹴込み (défonçant). Écrit « kaege » au programme |
| Ura mawashi geri | 裏回し蹴り | Coup de pied circulaire inversé |  |
| Ushiro geri | 後ろ蹴り | Coup de pied arrière |  |
| Ushiro mawashi geri | 後ろ回し蹴り | Coup de pied circulaire retourné (avec rotation) |  |
| Mikazuki geri | 三日月蹴り | Coup de pied en croissant de lune |  |
| Ura mikazuki geri | 裏三日月蹴り | Coup de pied en croissant inversé |  |
| Hiza geri | 膝蹴り | Coup de genou |  |
| Kakato geri | 踵蹴り | Coup de talon |  |
| Fumikomi | 踏み込み | Coup de pied écrasant, appui en frappant |  |
| Ashi barai | 足払い | Balayage de jambe |  |
| Uchi komi | 打ち込み | Frappe, entrée en attaque | 🔶 À confirmer avec le professeur (mention « demi circulaire » au programme) |

### 9.5 Niveaux, directions et mots qualificatifs

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Jodan | 上段 | Niveau haut (visage) |  |
| Chudan | 中段 | Niveau moyen (tronc) |  |
| Gedan | 下段 | Niveau bas (jambes, bas-ventre) |  |
| Migi | 右 | Droite |  |
| Hidari | 左 | Gauche |  |
| Mae | 前 | Avant, de face |  |
| Ushiro | 後ろ | Arrière |  |
| Yoko | 横 | Côté, latéral |  |
| Mawashi | 回し | Circulaire |  |
| Age | 上げ | Montant, qui monte |  |
| Otoshi | 落とし | Descendant, qui tombe |  |
| Soto | 外 | Extérieur |  |
| Uchi | 内 | Intérieur | Aussi 打ち (frapper) : Shuto uchi |
| Gyaku | 逆 | Inversé, opposé |  |
| Oi | 追い | Poursuite |  |
| Ura | 裏 | Envers, inversé |  |
| Morote | 諸手 | Deux mains |  |

### 9.6 Kata et bunkai

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Kata | 型 (形) | Forme, enchaînement codifié |  |
| Taikyoku shodan | 太極初段 | Taikyoku, premier niveau | Idem nidan 二段, sandan 三段, yondan 四段, godan 五段 |
| Heian shodan | 平安初段 | Heian (paix et tranquillité), premier niveau | Idem nidan 二段, sandan 三段, yodan 四段, godan 五段 |
| Ten no kata | 天の型 | Kata du ciel |  |
| Empi no kata (Enpi) | 燕飛 | Vol de l'hirondelle | 🔶 Le kata Enpi, à ne pas confondre avec le coude (empi) |
| Bassai dai | 抜塞大 | Prendre la forteresse (grande version) |  |
| Kanku dai | 観空大 | Regarder le ciel (grande version) |  |
| Tekki shodan | 鉄騎初段 | Cavalier de fer, premier niveau |  |
| Bunkai | 分解 | Analyse, application des techniques du kata |  |
| Embusen | 演武線 | Ligne de déplacement du kata |  |

### 9.7 Étiquette, commandements et notions

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Rei | 礼 | Salut |  |
| Seiza | 正座 | Position assise à genoux |  |
| Mokuso | 黙想 | Méditation silencieuse (yeux fermés) | Début et fin de séance |
| Mokuso yame | 黙想止め | Fin de la méditation |  |
| Kiotsuke | 気を付け | Garde-à-vous |  |
| Yasume | 休め | Repos |  |
| Hajime | 始め | Commencez |  |
| Yame | 止め | Arrêtez |  |
| Naore | 直れ | Reprendre la position |  |
| Mawatte | 回って | Tournez (demi-tour) |  |
| Shomen ni rei | 正面に礼 | Salut vers le front du dojo |  |
| Sensei ni rei | 先生に礼 | Salut au professeur |  |
| Otagai ni rei | お互いに礼 | Salut entre partenaires |  |
| Osu | 押忍 | Formule de salut et de respect, « oui, compris » |  |
| Kiai | 気合 | Cri qui concentre l'énergie |  |
| Kime | 決め | Focalisation, concentration de la force à l'impact |  |
| Zanshin | 残心 | Vigilance maintenue après la technique |  |
| Hikite | 引き手 | Main de retrait (poing au côté de la hanche) |  |
| Tori | 取り | Celui qui exécute la technique (attaquant ou défenseur selon l'exercice) |  |

### 9.8 Structure, grades et vocabulaire général

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Karate | 空手 | Main vide |  |
| Karate-do | 空手道 | La voie de la main vide |  |
| Shotokan | 松濤館 | Style Shotokan (« maison des pins qui ondulent ») |  |
| Dojo | 道場 | Lieu d'entraînement (« lieu de la voie ») |  |
| Sensei | 先生 | Professeur, enseignant |  |
| Sempai | 先輩 | Aîné, ceinture plus gradée |  |
| Kohai | 後輩 | Cadet, ceinture moins gradée |  |
| Karate-gi (gi) | 空手着 | Tenue de karaté |  |
| Obi | 帯 | Ceinture |  |
| Kyu | 級 | Grade de ceinture de couleur |  |
| Dan | 段 | Grade de ceinture noire |  |
| Kihon | 基本 | Bases, technique fondamentale |  |
| Waza | 技 | Technique |  |
| Kumite | 組手 | Combat conventionnel (« rencontre des mains ») |  |
| Tameshiwari | 試し割り | Casse (planches) | Terme de la « casse de planche » du programme |

### 9.9 Nombres et décompte des attaques

| Nombre | Japonais (romaji) | Écriture | Décompte des attaques (hon 本) |
|---|---|---|---|
| 1 | Ichi | 一 | Ippon 一本 (un coup) |
| 2 | Ni | 二 | Nihon 二本 |
| 3 | San | 三 | Sanbon 三本 |
| 4 | Shi / Yon | 四 | Yonhon 四本 |
| 5 | Go | 五 | Gohon 五本 |
| 6 | Roku | 六 |  |
| 7 | Shichi / Nana | 七 |  |
| 8 | Hachi | 八 |  |
| 9 | Ku / Kyu | 九 |  |
| 10 | Ju | 十 |  |

### 9.10 Grades

Correspondance usuelle des couleurs 🔶 (à confirmer pour la FFKDA).

| Grade | Japonais | Écriture | Ceinture | Couleur en japonais |
|---|---|---|---|---|
| 6e kyu | Rokkyu | 六級 | Blanche | Shiro 白 |
| 5e kyu | Gokyu | 五級 | Jaune | Ki-iro 黄色 |
| 4e kyu | Yonkyu | 四級 | Orange | Daidai-iro 橙色 |
| 3e kyu | Sankyu | 三級 | Verte | Midori 緑 |
| 2e kyu | Nikyu | 二級 | Bleue | Ao 青 |
| 1er kyu | Ikkyu | 一級 | Marron | Cha-iro 茶色 |
| 1er dan | Shodan | 初段 | Noire | Kuro 黒 |
| 2e dan | Nidan | 二段 | Noire |  |
| 3e dan | Sandan | 三段 | Noire |  |
| 4e dan | Yondan | 四段 | Noire |  |
| 5e dan | Godan | 五段 | Noire |  |

### 9.11 Orthographe du programme du club et forme usuelle

Utile pour la validation de la section 7.5 avec le professeur : rien n'a été corrigé dans la section 6.

| Écrit au programme | Forme usuelle |
|---|---|
| Mae giri | Mae geri |
| Shuto ushi | Shuto uchi |
| Yoko geri kaege kekomi | Yoko geri keage ou Yoko geri kekomi |
| Ipon kumite | Ippon kumite |
| Jyu ippon kumite, Jyu kumite | Jiyu ippon kumite, Jiyu kumite |
| Sambon kumite | Sanbon kumite |
| Kage tsuki | Kagi zuki (terme officiel) |
| Tobikonde tsuki | Tobikomi tsuki 🔶 |

### 9.12 Terminologie officielle du programme technique (annexe II du règlement)

Le jury emploie la terminologie officielle en japonais. Les termes ci-dessous proviennent de l'annexe II du règlement 2026-2027 (**REF-TECH p. 32 à 34**, romaji). Les écritures japonaises restent établies de mémoire.

| Terme | Japonais | Traduction | Remarque |
|---|---|---|---|
| Hachi ji dachi | 八字立ち | Posture en « 8 » : pieds pointés vers l'extérieur, écartés de la largeur des hanches | Position d'attente et de départ à l'examen |
| Heisoku dachi | 閉足立ち | Debout, pieds l'un contre l'autre |  |
| Musubi dachi | 結び立ち | Debout, talons joints, pointes des pieds écartées |  |
| Heiko dachi | 平行立ち | Debout, pieds écartés et parallèles |  |
| Uchi hachiji dachi | 内八字立ち | Pieds pointés vers l'intérieur |  |
| Reinoji dachi | レの字立ち | Un pied devant l'autre, formant un L |  |
| Teiji dachi | T字立ち | Un pied devant l'autre, formant un T |  |
| Shiko dachi | 四股立ち | Position du sumotori |  |
| Kosa dachi / Kake dachi | 交差立ち / 掛け立ち | Position pieds croisés | 🔶 Kake dachi |
| Moto dachi | 基立ち | Position fondamentale (petit Zenkutsu) | 🔶 |
| Tsuru ashi dachi / Sagi ashi dachi | 鶴足立ち / 鷺足立ち | Debout sur une jambe |  |
| Unsoku | 運足 | Déplacements |  |
| Ayumi ashi / De ashi | 歩み足 / 出足 | Avancer d'un pas | Déplacement de base des attaques du Ippon kumite |
| Hiki ashi | 引き足 | Reculer d'un pas |  |
| Yori ashi | 寄り足 | Pas glissé | Déplacement du Jiyu ippon kumite |
| Tsugi ashi | 継ぎ足 | Pas chassé | Déplacement du Jiyu ippon kumite |
| Okuri ashi | 送り足 | Double pas |  |
| Mawari ashi | 回り足 | Déplacement tournant autour du pied avant | 🔶 |
| Uke waza | 受け技 | Techniques de défense |  |
| Tsuki waza | 突き技 | Attaques directes de poing |  |
| Uchi waza | 打ち技 | Techniques de percussion |  |
| Keri waza | 蹴り技 | Attaques de pied |  |
| Soto ude uke | 外腕受け | Défense du bras, de l'extérieur vers l'intérieur |  |
| Uchi ude uke | 内腕受け | Défense du bras, de l'intérieur vers l'extérieur |  |
| Jodan age uke | 上段上げ受け | Défense haute, mouvement remontant |  |
| Osae uke | 押さえ受け | Défense par pression ou immobilisation avec la main |  |
| Sukui uke | 掬い受け | Défense en puisant |  |
| Nagashi uke | 流し受け | Défense brossée, qui accompagne l'attaque |  |
| Otoshi uke | 落とし受け | Défense en frappant l'avant-bras vers le bas |  |
| Heiko uke | 平行受け | Défense double, deux bras parallèles |  |
| Koken uke | 弧拳受け | Défense avec le poignet | 🔶 |
| Maete zuki | 前手突き | Coup de poing avec le poing avant | Utilisé au 1er Dan (cibles, Jiyu ippon kumite) |
| Kizami zuki | 刻み突き | Coup de poing avec le poing avant, en effaçant le buste |  |
| Nagashi zuki | 流し突き | Coup de poing avec le poing avant, en esquivant |  |
| Jun zuki | 順突き | Autre nom de l'Oï zuki |  |
| Mawashi zuki / Furi zuki | 回し突き / 振り突き | Coup de poing circulaire |  |
| Haishu uchi | 背手打ち | Attaque avec le dos de la main |  |
| Hiji uchi | 肘打ち | Attaque avec le coude | Autre nom : Enpi uchi |
| Yoko kekomi | 横蹴込み | Coup de pied latéral défonçant (chassé) |  |
| Yoko keage | 横蹴上げ | Coup de pied latéral remontant |  |
| Fumikiri | 踏み切り | Coup de pied bas (idée de couper) |  |
| Tobi geri | 飛び蹴り | Coup de pied sauté |  |
| Nami gaeshi | 波返し | Coup de pied « en vague », mouvement remontant avec la plante du pied |  |
| Maai | 間合い | Distance spatio-temporelle entre les adversaires, qui détermine l'action |  |
| Randori | 乱取り | Assaut libre et souple | Autre nom du Ju kumite |
| Midare | 乱れ | Mouvement continu | Exercice du style Shotokaï, 1 minute par rôle 🔶 |
| Shotokai | 松濤會 | Style Shotokaï | À distinguer du Shotokan |

---

## 10. Les types de kumite

Le **kumite** (組手) désigne le travail à deux. Il progresse du plus codifié au plus libre. Dans les formes où l'attaquant et le défenseur sont désignés, **Tori** (取り) attaque et **Uke** (受け) se défend.

✅ Les formes d'examen (10.4 à 10.7) sont vérifiées sur l'annexe III du règlement 2026-2027 (**REF-TECH p. 35 à 37**). Les formes de club (Gohon et Sanbon kumite) ne figurent pas dans le règlement fédéral.

### 10.1 Vue d'ensemble

| Type | Japonais | Attaques | Défense | Où le trouve-t-on |
|---|---|---|---|---|
| Gohon kumite | 五本組手 | 5, enchaînées en avançant | Recul et blocage à chaque pas, puis contre | Club : blanche → jaune |
| Sanbon kumite | 三本組手 | 3, enchaînées en avançant | Recul et blocage à chaque pas, puis contre | Club : jaune → orange et orange → verte |
| Kihon ippon kumite | 基本一本組手 | 1, annoncée, depuis Hachi ji dachi | **Libre** (défense et contre-attaque) | Examen 1er Dan, UV 2. Club : « Ipon kumite » (verte → bleue) |
| Ippon kumite | 一本組手 | 6, annoncées, les deux en garde | **Libre**, avec retour au maaï | Examen 2e et 3e Dan, UV 2 |
| Jiyu ippon kumite | 自由一本組手 | 3 à 6 selon le Dan, 1 à la fois | Libre, avec Go no sen ou Sen no sen | Examen 1er au 5e Dan. Club : orange → verte |
| Ju kumite | 柔組手 | Assaut souple, 2 min max au 1er Dan | Libre, souple, contrôle absolu | Examen 1er au 3e Dan, UV 6. Club : « Jiyu kumite » (bleue → marron) |

### 10.2 Gohon kumite (五本組手), forme de club

**Principe** : cinq attaques identiques, une par pas.

1. Salut (rei), mise en place à bonne distance, en garde (Zenkutsu dachi avec Gedan barai).
2. Tori avance et frappe **Oï zuki jodan**, en annonçant « Jodan ! ».
3. Uke recule d'un pas et bloque par **Age uke**.
4. On répète **5 fois**, Tori avançant à chaque pas.
5. Au 5e blocage, Uke contre-attaque par **Gyaku zuki chudan** avec un kiai.
6. Retour en position, rei, puis on inverse les rôles.

**Points d'attention** : distance (l'attaque s'arrête avant le contact), recul stable en Zenkutsu, blocage précis et au bon moment, kime et kiai sur la contre-attaque, rythme régulier.

### 10.3 Sanbon kumite (三本組手), forme de club

**Principe** : trois attaques enchaînées, même schéma que le Gohon kumite avec moins de répétitions : Uke recule en bloquant trois fois, puis contre-attaque.

🔶 Les attaques varient selon les écoles (même niveau, changement de niveau, ou Mae geri en finale). À préciser avec le professeur.

### 10.4 Kihon ippon kumite (基本一本組手), examen du 1er Dan

**Principe** : assaut fondamental basé sur **une attaque contrée par une seule défense et/ou contre-attaque**. On retient la forme commune à tous les styles.

**Déroulement officiel**
1. Les deux candidats sont à **1 mètre** l'un de l'autre, de profil par rapport au jury.
2. Salut, puis **Hachi ji dachi** en écartant successivement le pied gauche et le pied droit. Tori se met en garde en reculant la jambe droite.
3. Uke trouve la distance. **Tori annonce son attaque**, se concentre un instant et la lance avec le plus de conviction possible.
4. **La défense et la contre-attaque d'Uke sont libres.** Uke reste un instant sur sa contre-attaque (ou retire son poing en ikité).
5. Retour des deux candidats en **Hachi ji dachi**, Tori se place en position inverse pour répéter la même attaque du côté opposé.
6. L'attaque s'effectue **toujours avec la jambe ou le bras arrière**. Les rôles s'inversent quand Tori a terminé toutes ses attaques.

**Les 5 attaques**, chacune une fois à droite et une fois à gauche, **en alternance** :

| # | Attaque | Japonais |
|---|---|---|
| 1 | Oï zuki jodan | 追い突き上段 |
| 2 | Oï zuki chudan | 追い突き中段 |
| 3 | Mae geri chudan | 前蹴り中段 |
| 4 | Mawashi geri jodan ou chudan | 回し蹴り上段・中段 |
| 5 | Yoko geri chudan | 横蹴り中段 |

**Pourquoi cette forme ?** La défense, dans l'idée martiale, doit pouvoir s'exécuter depuis une position naturelle.

### 10.5 Ippon kumite (一本組手), examens du 2e et du 3e Dan

**Principe** : les deux partenaires sont **en garde** (Fudo dachi). La défense et le contre d'Uke sont **libres**. Après chaque contre-attaque, Uke doit **récupérer son maaï**. À chaque fois, les attaques et les contre-attaques doivent être **différentes**.

**6 attaques**, exécutées d'abord à droite puis à gauche, avec la jambe ou le bras arrière, toutes en **Ayumi ashi** :

| # | Attaque |
|---|---|
| 1 | Oï zuki jodan |
| 2 | Oï zuki chudan |
| 3 | Mae geri chudan (le pied reste posé devant) |
| 4 | Mawashi geri de la jambe avant |
| 5 | Mawashi geri de la jambe arrière |
| 6 | Yoko geri de la jambe arrière |

Au 2e Dan, les attaques et le niveau sont annoncés. La perception de l'attaque exigée au 3e Dan est supérieure à celle du 2e Dan. Le jury peut demander des attaques supplémentaires.

**Critères de notation** : bonne distance (les techniques doivent « porter »), stabilité et équilibre, puissance et détermination, variété des défenses (blocage puis contre-attaque, esquive puis contre-attaque, contre direct), maîtrise et précision de la contre-attaque, **kimé**, **zanshin**, attitude générale.

### 10.6 Jiyu ippon kumite (自由一本組手), examens du 1er au 5e Dan

**Principe** : ce n'est pas la forme extrême où attaquant et défenseur ne sont pas désignés. **Tori et Uke sont désignés d'avance.**

**Déroulement**
1. Tori et Uke se placent à **3 mètres**, se saluent et prennent **Hachi ji dachi**.
2. Au commandement **Hajimé** du jury, ils se mettent en garde et commencent.
3. **Tori cherche une opportunité pour attaquer, Uke adapte sa réaction.** Tori ne doit pas s'approcher trop près, Uke ne doit pas refuser le combat en reculant systématiquement.
4. Le nombre d'attaques est fixé par le jury (de 3 à 6).
5. Du 1er au 3e Dan, retour en Hachi ji dachi après chaque échange. Du **4e au 5e Dan**, les deux restent en garde et reprennent leur maaï. À la fin, salut et inversion des rôles.

| Dan | Annonce par Tori | Nombre d'attaques |
|---|---|---|
| 1er Dan | La technique **et** son niveau | 3, choisies par le juge parmi Oï zuki, Mae geri, Mawashi geri, Yoko geri, Gyaku zuki, Maete zuki |
| 2e Dan | Le niveau seulement (jodan ou chudan) | 5 |
| 3e Dan et au-delà | Aucune annonce | 5 |

**Déplacements** : de base (Yori ashi, Tsugi ashi, Ayumi ashi), pas de sautillements. Ils permettent au jury de juger la maîtrise du maaï.

**Les deux notions à démontrer**

| Notion | Japonais | Sens |
|---|---|---|
| Go no sen | 後の先 | Blocage puis contre-attaque (on répond après l'attaque) |
| Sen no sen | 先の先 | Contre direct, qui prend l'initiative au moment où l'adversaire s'apprête à attaquer |

**Critères** : aisance des déplacements, puissance et détermination, stabilité, précision, recherche de l'opportunité, kimé, zanshin (concentration maintenue avant et après l'attaque).

### 10.7 Ju kumite (柔組手) et Jiyu kumite (自由組手)

- **Ju kumite (randori ou midare)** : assaut libre et souple, non réglementé, qui permet de juger la maîtrise technique des candidats de la voie traditionnelle. **2 minutes** au 1er Dan, durée fixée par la table d'examen aux 2e et 3e Dan. Il faut le distinguer d'un combat libre ou arbitré.
  - Critères : travail en souplesse avec **contrôle absolu**, aisance dans les déplacements, variété des techniques, équilibre et stabilité, opportunité, zanshin.
  - Le **midare** (mouvement continu) est spécifique au style Shotokaï : 1 minute par rôle.
- **Jiyu kumite** : combat libre contrôlé, sans attaque annoncée (forme de club).
- **Shiai kumite** (試合組手) : combat de compétition, hors programme du club.

**Sécurité à tous les niveaux** : distance et vitesse contrôlées, pas de contact appuyé, surtout au visage ; le plus gradé s'adapte au moins gradé.

---

## 11. Questions ouvertes

1. Flyers : peux-tu les téléverser (horaires, adresse du dojo, logo) ?
2. ⏳ **Liste officielle des enchaînements de kihon** (6 pour le 1er Dan, 7 pour le 2e, 10 pour le 3e) : envoyée aux clubs en début de saison et disponible sur le site de la FFKDA. Peux-tu me la transmettre ?
3. Les observations de la section 7.5 (doublons, fautes de frappe) : à valider avec le professeur.
4. Quel est le grade Dan exact de chacune des 7 ceintures noires (1er, 2e, 3e Dan ou plus) ? Cela décide des kata (section 5.7) et du kumite à travailler.
5. Quels kata sont réellement présentés au club pour le 1er Dan ? La liste officielle est Heian 1 à 5 et Tekki shodan.
6. La section « grades » est-elle publique ou réservée aux membres ?
7. Que doit contenir l'espace membres (annonces, documents, présences, autre) ?
8. Le site doit-il être accessible depuis Internet (nom de domaine, DDNS Synology) ou seulement en réseau local ?
9. Qui crée les comptes des 15 membres (toi uniquement) ?
10. Quelle est la durée des séances du mardi et du jeudi ?
11. Le professeur du club est-il 2e Dan minimum ? C'est la condition pour organiser un 1er Dan en club (5.8).
12. Le lexique (section 9) et le contenu exact du Sanbon kumite au club : à relire avec le professeur, notamment les éléments marqués 🔶.

---

## 12. Journal des modifications

| Date | Modification |
|---|---|
| 2026-11-11 | Création du document : 3 fonctions, architecture proposée, synthèse de la réglementation de grades FFKDA 2026-2027 (sources secondaires, à vérifier) |
| 2026-11-11 | Ajout du programme du club 2023 (section 6) comme base de départ, et de l'analyse des écarts avec la réglementation 2026-2027 (section 7) |
| 2026-11-11 | Ajout de la composition du club (5 blanches, 2 jaunes, 1 verte, 7 noires) et d'une proposition d'organisation des séances (section 8) |
| 2026-11-11 | Ajout de la séance 1 (jeudi 12 novembre 2026, kumite et applications, avec 2 assistants) en section 8.4 |
| 2026-11-11 | Ajout de la séance 2 en section 8.5, en version classique avec beaucoup d'exercices à deux (carrousel des partenaires) |
| 2026-11-11 | Séance 2 : échauffement porté à 20 min (postures à deux 10 min, carrousel 20 min) |
| 2026-11-11 | Ajout d'un module de 20 minutes kata et bunkai en section 8.6 |
| 2026-11-11 | Séance 2 : module kata et bunkai placé en fin de séance, durée maintenue à 1h30 (ateliers recentrés sur postures et renforcement) |
| 2026-11-11 | Ajout du lexique technique (section 9) et d'un rappel des types de kumite (section 10) |
| 2026-11-11 | Intégration des PDF officiels FFKDA 2026-2027 : sections 5, 7, 9.12 et 10 vérifiées et corrigées (kata du 1er Dan, kihon sur cibles, Kihon ippon kumite, Jiyu ippon kumite) |
| 2026-11-11 | Ajout des deux PDF officiels dans le dossier `reglementation/`, création de leur index (pages, thèmes) et des références REF-TECH et REF-GEN dans ce fichier |

---

## 13. Sources

- **REF-TECH** : règlement technique de la CSDGE 2026-2027 (partie Karaté, articles 402K à 409K, annexes I à V), PDF officiel transmis : [reglementation/CSDGE_Reglementation_Technique_2026-2027.pdf](reglementation/CSDGE_Reglementation_Technique_2026-2027.pdf)
- **REF-GEN** : règlement général de la CSDGE 2026-2027, PDF officiel transmis : [reglementation/CSDGE_Reglementation_Generale_2026-2027.pdf](reglementation/CSDGE_Reglementation_Generale_2026-2027.pdf)
- **Index des deux règlements** (table des matières, index thématique, pages) : [reglementation/INDEX_REGLEMENTATION_FFKDA.md](reglementation/INDEX_REGLEMENTATION_FFKDA.md)
- Programme du club : PROGRAMME_2023.docx (« Programme Karaté Renoji 2023 »)
- Karate Club Colombes, « 10 changements pour les examens de grade karaté », 8 septembre 2026 : https://www.karatecolombes.fr/10-changements-examens-grades-karate/
- Imagin Arts Digital, « Passages de grades de karaté 2026-2027 : ce qui change », 9 septembre 2026 : https://www.imaginarts.digital/passages-de-grades-de-karate-2026-2027-ce-qui-change/
- FFKDA, Commission Spécialisée des Dans et Grades Équivalents : https://www.ffkarate.fr/grades/csdge/
- FFKDA, « Passer un grade » (documents officiels) : https://www.ffkarate.fr/espace-licencies/passer-un-grade/
- FFKDA, PDF de la réglementation technique 2026-2027 (non ouvert) : https://www.ffkarate.fr/wp-content/uploads/2026/09/CSDGE-REGLEMENTATION-TECHNIQUE-KARATE-2026-2027.pdf
