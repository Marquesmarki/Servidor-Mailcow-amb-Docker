# Servidor Mailcow amb Docker

Documentació del desplegament d’un servidor de correu complet amb Mailcow utilitzant Docker Compose.

Autor: Alex Marques
Data: 2026/01/08 
Repositori: Servidor-Mailcow-amb-Docker  
Domini: alexmarques.local

## Índex

- [1. Objectiu](#1-objectiu)
- [2. Requisits i criteris d’avaluació](#2-requisits-i-criteris-davaluació)
- [3. Entorn i prerequisits](#3-entorn-i-prerequisits)
- [4. Desplegament de Mailcow](#4-desplegament-de-mailcow)
- [5. Personalització obligatòria](#5-personalització-obligatòria)
- [6. DNS: SPF, DKIM i DMARC](#6-dns-spf-dkim-i-dmarc)
- [7. Proves d’enviament i recepció](#7-proves-denviament-i-recepció)
- [8. Captures de pantalla obligatòries](#8-captures-de-pantalla-obligatòries)
- [9. Fitxers a incloure](#9-fitxers-a-incloure)
- [10. Consultes SQL](#10-consultes-sql)
- [11. Logs](#11-logs)
- [12. Qüestions i explicacions](#12-qüestions-i-explicacions)
- [13. Reflexió final (250-350 paraules)](#13-reflexió-final-250-350-paraules)
- [14. Checklist final d’entrega](#14-checklist-final-dentrega)

---

## 1. Objectiu

Desplegar Mailcow amb Docker Compose i deixar un servidor de correu funcional per al domini `alexmarques.local`, amb mínim 3 usuaris, enviament/recepció operatius, i configuració i documentació de SPF, DKIM i DMARC. S’ha d’aportar evidència amb captures personalitzades (nom visible) i fitxers de configuració (sense contrasenyes en clar).

---

## 2. Requisits i criteris d’avaluació

### Criteris d’avaluació

| Criteri | Puntuació |
|---|---:|
| Mailcow desplegat correctament amb tots els contenidors operatius | 2,0 |
| Domini i mínim 3 usuaris creats amb personalització | 1,5 |
| SPF, DKIM i DMARC configurats i documentats | 1,5 |
| Enviament i recepció de correus funcional | 2,0 |
| Client de correu (Thunderbird o webmail) configurat | 1,0 |
| Captures de pantalla completes amb personalització | 1,0 |
| Respostes a qüestions correctes | 1,0 |
| TOTAL | 10,0 |

### Penalitzacions

- -2,0 punts si no hi ha personalització amb `alexmarques`
- -1,5 punts si DKIM no està configurat o no funciona
- -1,0 punt si no es poden enviar/rebre correus entre usuaris
- -0,5 punts si contrasenyes no amagades al document lliurat

### Bonificacions opcionals (màxim +1,5; nota màxima 10,0)

- +0,5 punts per configuració de greylisting funcional
- +0,5 punts per anàlisi avançada dels headers amb totes les capçaleres explicades
- +0,5 punts per configuració de regles Sieve per filtrar correus

---

## 3. Entorn i prerequisits

- Sistema: Ubuntu (WSL2 o VM)  
- Docker i Docker Compose instal·lats
<img width="647" height="136" alt="image" src="https://github.com/user-attachments/assets/8e302f97-ce99-4650-9539-84204fd85e9c" /> 
- Portes i accés local segons requeriments de la pràctica
- Domini de laboratori: `alexmarques.local`

Notes:
- Si s’utilitza `.local`, és un domini de laboratori. Les proves DNS s’han de documentar igualment amb els registres generats (encara que no es publiquin a internet).
- Si el professor demana DNS “real”, s’ha d’usar un domini propi o un subdomini d’un DNS controlat i publicar els registres.

---

## 4. Desplegament de Mailcow

### 4.1 Obtenció del repositori

Carpeta de treball: `mailcow-alexmarques`.  
Repositori: `mailcow-dockerized`.
<img width="945" height="193" alt="image" src="https://github.com/user-attachments/assets/ca6849c5-6028-4e75-be9e-3706b6fa51bd" />


### 4.2 Configuració bàsica

- `mailcow.conf` personalitzat per al domini `alexmarques.local`
- Ajustos de xarxa/ports segons l’entorn
<img width="944" height="203" alt="image" src="https://github.com/user-attachments/assets/b0d9011d-03df-4882-a598-d1cc700a7aac" />
<img width="933" height="203" alt="image" src="https://github.com/user-attachments/assets/4aa09003-d50d-4d69-876f-43bc142655fa" />


### 4.3 Arrencada de serveis

Objectiu: tenir tots els contenidors en estat `running` i `healthy`.

Evidència obligatòria:
- captura de `docker-compose ps` amb tots els contenidors `running/healthy`

---

## 5. Personalització obligatòria

S’ha de personalitzar com a mínim:
- Domini: `alexmarques.local`
- 3 mailboxes: `VOSTRENOM1@alexmarques.local`, `VOSTRENOM2@alexmarques.local`, `VOSTRENOM3@alexmarques.local` (amb els vostres noms)
- Captures amb el nom complet visible a la sessió o a l’escriptori quan es pugui

---

## 6. DNS: SPF, DKIM i DMARC

En aquest apartat s’han d’incloure els registres generats i una breu explicació de què fa cadascun.

### 6.1 SPF

Registre (exemple; substituir pel que generi Mailcow o el vostre escenari):
- Tipus: TXT
- Nom/Host: `alexmarques.local`
- Valor: `v=spf1 ... -all`

Explicació:
- SPF defineix quins servidors poden enviar correu en nom del domini.

### 6.2 DKIM

Evidència obligatòria:
- captura de configuració DKIM mostrant la clau pública generada

Registre:
- Tipus: TXT
- Nom/Host: `selector._domainkey.alexmarques.local`
- Valor: `v=DKIM1; k=rsa; p=...`

Explicació:
- DKIM signa els correus i permet verificar que el missatge no s’ha modificat i que prové d’un servidor autoritzat.

### 6.3 DMARC

Registre:
- Tipus: TXT
- Nom/Host: `_dmarc.alexmarques.local`
- Valor: `v=DMARC1; p=none/quarantine/reject; rua=mailto:...`

Explicació:
- DMARC defineix la política quan SPF/DKIM fallen i proporciona reporting.

---

## 7. Proves d’enviament i recepció

Objectiu:
- Enviar correu entre usuaris del domini
- Rebre correu correctament
- Verificar headers amb DKIM i puntuació d’antispam

Evidències obligatòries:
- correu rebut a la safata d’entrada mostrant el teu nom complet
- headers del correu mostrant `DKIM-Signature` i `X-Rspamd-Score`

---

## 8. Captures de pantalla obligatòries

Guardar totes les captures dins `captures/` i referenciar-les aquí.

1) docker-compose ps mostrant tots els contenidors Mailcow en running/healthy  
Imatge: `captures/01_docker_compose_ps.png`

2) Interfície web d'administració mostrant el domini alexmarques.local creat  
Imatge: `captures/02_admin_domini.png`

3) Llista de mailboxes amb els 3 usuaris creats (amb els teus noms)  
Imatge: `captures/03_mailboxes.png`

4) Configuració DKIM mostrant la clau pública generada  
Imatge: `captures/04_dkim_public_key.png`

5) Thunderbird configurat amb el compte VOSTRENOM1@alexmarques.local  
Imatge: `captures/05_thunderbird_config.png`

6) Correu rebut a la safata d'entrada mostrant el teu nom complet  
Imatge: `captures/06_inbox_nom.png`

7) Headers del correu mostrant DKIM-Signature i X-Rspamd-Score  
Imatge: `captures/07_headers_dkim_rspamd.png`

8) Webmail SOGo amb sessió iniciada i correu visible  
Imatge: `captures/08_sogo_ok.png`

9) Logs de Postfix mostrant l'enviament d'un correu  
Imatge: `captures/09_postfix_logs.png`

10) Consulta SQL mostrant els usuaris de la base de dades  
Imatge: `captures/10_sql_users.png`

(Quan tinguis les imatges, pots incrustar-les així:)

- docker-compose ps  
![](captures/01_docker_compose_ps.png)

- Admin domini  
![](captures/02_admin_domini.png)

- Mailboxes  
![](captures/03_mailboxes.png)

- DKIM  
![](captures/04_dkim_public_key.png)

- Thunderbird  
![](captures/05_thunderbird_config.png)

- Inbox  
![](captures/06_inbox_nom.png)

- Headers  
![](captures/07_headers_dkim_rspamd.png)

- SOGo  
![](captures/08_sogo_ok.png)

- Postfix logs  
![](captures/09_postfix_logs.png)

- SQL users  
![](captures/10_sql_users.png)

---

## 9. Fitxers a incloure

Dins la carpeta `files/` (recomanat) o a l’arrel del repo:

### 9.1 mailcow.conf personalitzat

Fitxer: `files/mailcow.conf`

Requisit:
- contrasenyes amagades (substituir per `********`)

### 9.2 Captura de docker-compose.yml (primers 50 línies)

Fitxer:
- `files/docker-compose_yml_50linies.png` (captura)
o bé
- `files/docker-compose_yml_50linies.txt` (si el professor accepta text)

---

## 10. Consultes SQL

Objectiu:
- mostrar usuaris de la base de dades de Mailcow (evidència amb captura)

Incloure:
- consulta executada
- resultat visible amb personalització/entorn

Evidència:
- `captures/10_sql_users.png`

---

## 11. Logs

Objectiu:
- evidenciar l’enviament d’un correu a nivell de MTA (Postfix)

Evidència:
- `captures/09_postfix_logs.png`

Explicació breu:
- indicar què es veu al log (origen, destí, cua, status, entrega)

---

## 12. Qüestions i explicacions

Incloure respostes amb explicacions detallades (adaptar a les preguntes reals del professor si n’hi ha de concretes). Recomanat:

### 12.1 Què és Mailcow i quins components integra?

Explicació:
- components típics (MTA, IMAP/POP, antispam, webmail, admin UI, etc.)
- avantatge de tenir-ho integrat i orquestrat

### 12.2 Què valida SPF, DKIM i DMARC i per què són necessaris?

Explicació:
- SPF (autorització d’IP/servidor)
- DKIM (integritat i autenticitat del missatge)
- DMARC (política i reporting combinant SPF/DKIM)

### 12.3 Què és Rspamd i què significa X-Rspamd-Score?

Explicació:
- puntuació antispam
- com interpretar valors típics
- impacte en la entrega

### 12.4 Diferències entre webmail i client de correu (Thunderbird)

Explicació:
- avantatges i inconvenients
- ús en entorns corporatius
- suport de protocols i cache local

---

## 13. Reflexió final (250-350 paraules)

(Escriu aquí la reflexió final amb 250-350 paraules. Ha d’incloure obligatòriament:)

- Avantatges de Mailcow respecte a configurar cada component manualment
- Importància de SPF, DKIM i DMARC en l’actualitat
- Diferències entre webmail i client de correu tradicional
- Desafiaments trobats durant la pràctica
- Possibles millores o configuracions addicionals

Text:

[ESCRIU AQUÍ LA TEVA REFLEXIÓ (250-350 paraules)]

---

## 14. Checklist final d’entrega

- Repo públic amb GitHub Pages actiu
- README amb tota la documentació en una sola pàgina
- Captura docker-compose ps (running/healthy)
- Admin UI: domini alexmarques.local creat
- 3 mailboxes creats amb els vostres noms
- DKIM configurat i clau pública documentada
- Thunderbird configurat amb VOSTRENOM1@alexmarques.local
- Correu rebut amb el nom complet visible
- Headers amb DKIM-Signature i X-Rspamd-Score
- SOGo: sessió iniciada i correu visible
- Logs Postfix d’enviament
- Consulta SQL d’usuaris
- files/mailcow.conf amb contrasenyes amagades
- Captura de docker-compose.yml (primeres 50 línies)
- (Si es demana) PDF final amb portada, captures, DNS, respostes i reflexió

