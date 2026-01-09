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
<img width="945" height="193" alt="image" src="https://github.com/user-attachments/assets/ca6849c5-6028-4e75-be9e-3706b6fa51bd" />


### 4.2 Configuració bàsica

- `mailcow.conf` personalitzat per al domini `alexmarques.local`
- Ajustos de xarxa/ports segons l’entorn
<img width="944" height="203" alt="image" src="https://github.com/user-attachments/assets/b0d9011d-03df-4882-a598-d1cc700a7aac" />
<img width="650" height="203" alt="image" src="https://github.com/user-attachments/assets/4aa09003-d50d-4d69-876f-43bc142655fa" />


### 4.3 Arrencada de serveis

Objectiu: tenir tots els contenidors en estat `running` i `healthy`.
Imatge de meu mailcow funcionant amb el meu usuari `admin-alexmarques`.

<img width="1919" height="962" alt="image" src="https://github.com/user-attachments/assets/49b5fc16-bf60-43e4-995f-c285005f4f23" />

---

## 5. Personalització obligatòria

S’ha de personalitzar com a mínim:
- Domini: `alexmarques.local`
- 3 mailboxes: `alexmarques1@alexmarques.local`, `alexmarques2@alexmarques.local`, `alexmarques3@alexmarques.local`
  
---

## 6. DNS: SPF, DKIM i DMARC

En aquest apartat s’han d’incloure els registres generats i una breu explicació de què fa cadascun.

<img width="990" height="448" alt="image" src="https://github.com/user-attachments/assets/0be7564b-7b1f-4acc-b989-174cf64a2efd" />

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
   <img width="1170" height="288" alt="image" src="https://github.com/user-attachments/assets/d1747861-622b-4c4f-8e78-d34c79472f54" />

3. Intenta enviar correus amb adjunts grans per provar el límit
   <img width="1073" height="832" alt="image" src="https://github.com/user-attachments/assets/9a0cc37f-5a86-4fa6-8fad-43439708c7e3" />

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
<img width="1911" height="931" alt="image" src="https://github.com/user-attachments/assets/e8daad61-6da8-46c3-b6a1-38015fb2fbd6" />

- Admin domini  
<img width="1011" height="473" alt="image" src="https://github.com/user-attachments/assets/8bebbe96-4e5c-43c4-8588-8adae8cd6ba0" />

- Mailboxes  
<img width="1042" height="571" alt="image" src="https://github.com/user-attachments/assets/935f1afe-ad69-438a-b1f8-a5514ca47ac6" />

- DKIM  
<img width="990" height="448" alt="image" src="https://github.com/user-attachments/assets/4cbbf3b4-19b5-4d7f-979a-f171a7a83822" />

- Thunderbird  
<img width="1255" height="974" alt="image" src="https://github.com/user-attachments/assets/81d712fe-d277-4e38-90de-30f9a2c4b2f4" />

- Inbox  
<img width="1257" height="304" alt="image" src="https://github.com/user-attachments/assets/71cc5f5c-de54-4888-922e-75692304cf68" />

- Àlies
<img width="1262" height="329" alt="image" src="https://github.com/user-attachments/assets/45ee23bc-083a-4537-bca8-09dc595cfebc" />

- Headers  
<img width="1257" height="1017" alt="image" src="https://github.com/user-attachments/assets/a4141244-13e8-46bd-bb34-a677ef67e0df" />

- Postfix logs  
<img width="1257" height="1017" alt="image" src="https://github.com/user-attachments/assets/ee5f0827-1c11-4897-82a2-c8e321008e7e" />

- SQL users i àlies
<img width="772" height="483" alt="image" src="https://github.com/user-attachments/assets/fcc14b91-11ca-4a31-b586-e31cc12c465c" />


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

