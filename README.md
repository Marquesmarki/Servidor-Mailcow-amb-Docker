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

---

## 3. Entorn i prerequisits

- Sistema: Ubuntu (WSL2 o VM)  
- Docker i Docker Compose instal·lats
<img src="https://github.com/user-attachments/assets/8e302f97-ce99-4650-9539-84204fd85e9c" />

- Portes i accés local segons requeriments de la pràctica
- Domini de laboratori: `alexmarques.local`

Notes:
- Si s’utilitza `.local`, és un domini de laboratori. Les proves DNS s’han de documentar igualment amb els registres generats (encara que no es publiquin a internet).

---

## 4. Desplegament de Mailcow

### 4.1 Obtenció del repositori

Carpeta de treball: `mailcow-alexmarques`.  
Repositori: `mailcow-dockerized`.
<img src="https://github.com/user-attachments/assets/ca6849c5-6028-4e75-be9e-3706b6fa51bd" />


### 4.2 Configuració bàsica

- `mailcow.conf` personalitzat per al domini `alexmarques.local`
- Ajustos de xarxa/ports segons l’entorn
<img src="https://github.com/user-attachments/assets/b0d9011d-03df-4882-a598-d1cc700a7aac" />
<img src="https://github.com/user-attachments/assets/4aa09003-d50d-4d69-876f-43bc142655fa" />


### 4.3 Arrencada de serveis

Objectiu: tenir tots els contenidors en estat `running` i `healthy`.
Imatge de meu mailcow funcionant amb el meu usuari `admin-alexmarques`.

<img src="https://github.com/user-attachments/assets/49b5fc16-bf60-43e4-995f-c285005f4f23" />

---

## 5. Personalització obligatòria

S’ha de personalitzar com a mínim:
- Domini: `alexmarques.local`
- 3 mailboxes: `alexmarques1@alexmarques.local`, `alexmarques2@alexmarques.local`, `alexmarques3@alexmarques.local`
  
---

## 6. DNS: SPF, DKIM i DMARC

En aquest apartat s’han d’incloure els registres generats i una breu explicació de què fa cadascun.

### 6.1 SPF

Registre:
- `alexmarques.local. IN TXT "v=spf1 mx ~all"`

Explicació:
- SPF defineix quins servidors poden enviar correu en nom del domini.

### 6.2 DKIM

Registre:
`dkim._domainkey.alexmarques.local. IN TXT "v=DKIM1;k=rsa;t=s;s=email;p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvOhIzBk5mvg5WVMoDUK7LIqYIbdKvi/ID+voXYrt1sZJBpYjaedB8HAAnn5KbdjNEGwPi9B2lupQGsBQqOPnjjW54ROXvBDUAWE+IN984C4cXXBtxJuP8hnuLmg5vxQeTEmquBFFb0i7+Zr3uJH6QNn1FtjG8r5BhM8ujTu5jD/WAh+NCe3LSNyZMks18cTJ5ZFpX9LyZRcChTjyhpEjlGJ7hzCEqVVZvrUurtzUQW/K3zKWbuibGrQTd43g20ekzjsYE/bmWKvUC2owSUlLePIEJBiMF2nWIWtq7PiDdPbyDJeI/40YNhaYf4M1wkDn9ErrNYijIn1MPl3SZgSNHwIDAQAB"
`

Explicació:
- DKIM signa els correus i permet verificar que el missatge no s’ha modificat i que prové d’un servidor autoritzat.

### 6.3 DMARC

Registre:
- `_dmarc.alexmarques.local. IN TXT "v=DMARC1; p=quarantine; rua=mailto:admin@alexmarques.local"`

Explicació:
- DMARC defineix la política quan SPF/DKIM fallen i proporciona reporting.

---

## 7. Proves d’enviament i recepció

Objectiu:
- Enviar correu entre usuaris del domini
- Rebre correu correctament
- Verificar headers amb DKIM i puntuació d’antispam

Funcionalitats Avançades
1. Ves a Mailboxes → Edita un usuari
2. Canvia la quota a 256 MB
   <img src="https://github.com/user-attachments/assets/d1747861-622b-4c4f-8e78-d34c79472f54" />

3. Intenta enviar correus amb adjunts grans per provar el límit
   <img src="https://github.com/user-attachments/assets/9a0cc37f-5a86-4fa6-8fad-43439708c7e3" />

---

## 8. Captures de pantalla obligatòries

- docker-compose ps  
<img src="https://github.com/user-attachments/assets/e8daad61-6da8-46c3-b6a1-38015fb2fbd6" />

- Admin domini  
<img src="https://github.com/user-attachments/assets/8bebbe96-4e5c-43c4-8588-8adae8cd6ba0" />

- Mailboxes  
<img src="https://github.com/user-attachments/assets/935f1afe-ad69-438a-b1f8-a5514ca47ac6" />

- DKIM  
<img src="https://github.com/user-attachments/assets/4cbbf3b4-19b5-4d7f-979a-f171a7a83822" />

- Thunderbird  
<imgsrc="https://github.com/user-attachments/assets/81d712fe-d277-4e38-90de-30f9a2c4b2f4" />

- Inbox  
<img src="https://github.com/user-attachments/assets/71cc5f5c-de54-4888-922e-75692304cf68" />

- Àlies
<img src="https://github.com/user-attachments/assets/45ee23bc-083a-4537-bca8-09dc595cfebc" />

- Headers  
<img src="https://github.com/user-attachments/assets/a4141244-13e8-46bd-bb34-a677ef67e0df" />

- Postfix logs  
<img src="https://github.com/user-attachments/assets/ee5f0827-1c11-4897-82a2-c8e321008e7e" />

- SQL users i àlies
<img src="https://github.com/user-attachments/assets/fcc14b91-11ca-4a31-b586-e31cc12c465c" />


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

En aquesta pràctica he pogut muntar un servidor de correu complet amb Mailcow utilitzant Docker Compose, i la veritat és que m’ha ajudat a entendre millor com funciona un servei de correu  en un entorn semblant al d’una empresa. El que m’ha agradat més de Mailcow és que integra tots els components necessaris (Postfix, Dovecot, Rspamd, webmail, base de dades, etc.) i els deixa gestionats des d’una sola interfície. Si hagués hagut de configurar cada servei manualment, hauria estat molt més llarg i amb més punts on equivocar-me, sobretot amb certificats, ports i integració entre serveis.

Una part important ha estat la configuració d’SPF, DKIM i DMARC. Abans ho veia com “tres registres DNS i ja”, però ara entenc que són essencials per evitar suplantacions i perquè els correus no acabin a spam. SPF ajuda a definir quins servidors poden enviar per un domini, DKIM assegura la integritat del missatge amb una signatura, i DMARC uneix tot plegat amb una política clara quan alguna cosa falla. Mirar els headers i veure la DKIM-Signature i el X-Rspamd-Score m’ha servit per comprovar que realment funciona i no és només teoria.

També he vist diferències entre usar webmail (SOGo) i un client com Thunderbird. El webmail és molt ràpid per entrar des de qualsevol lloc i no depens d’instal·lar res, però Thunderbird és més còmode per treballar amb diversos comptes, tenir-ho tot sincronitzat i gestionar millor carpetes i filtres.

Els principals problemes han estat petits detalls: configuració de domini local, ajustar coses de Docker, i revisar bé captures i evidències perquè el treball quedi complet. Com a millores futures, afegiria regles Sieve per automatitzar filtres, provar greylisting més a fons i fer una documentació encara més ordenada per a l’entrega final.

- files/mailcow.conf amb contrasenyes amagades
- Captura de docker-compose.yml (primeres 50 línies)
- (Si es demana) PDF final amb portada, captures, DNS, respostes i reflexió

