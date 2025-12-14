# Tema 18 – HTTP & Postman (hapifyMe)

Colecție Postman care automatizează fluxul complet de utilizator în hapifyMe:
register → login → get_profile → delete_profile, folosind variabile de mediu și chaining între cereri.

## Structură

- Colecție: `Tema 18 - DavidSafta`
- Environment: `HapifyMe Tema Env`

### Variabile de mediu

- `baseUrl` – `https://test.hapifyme.com/api`
- `email` – email generat dinamic în Pre-request la Register
- `api_key` – extras din răspunsul de la Register
- `user_id` – ID-ul userului din răspunsul de la Register / Login
- `user_id_from_jwt` – ID-ul userului extras din payload-ul JWT (Login)
- `token` – JWT folosit ca Bearer pentru Delete Profile

## Cereri din colecție

### 1️⃣ Register – `POST {{baseUrl}}/user/register.php`

**Body (JSON)** folosește emailul din environment:

```json
{
  "first_name": "Test",
  "last_name": "User",
  "email": "{{email}}",
  "password": "Password123!"
}
2️⃣ Login – POST {{baseUrl}}/user/login.php

Body (JSON):

{
  "username": "{{username}}",
  "password": "Password123!"
}


({{username}} corespunde userului creat la Register.)

Tests

Status code is 200.

status === "success" în JSON.

Extrage și salvează în Environment:

token (JWT) din răspuns.

user_id din user.id.

Decodează JWT (base64 payload) și:

extrage user_id din payload,

îl salvează ca user_id_from_jwt,

validează că user_id_from_jwt este egal cu user.id din răspuns.

3️⃣ Get Profile – GET {{baseUrl}}/user/get_profile.php?user_id={{user_id}}

Auth

Tip: API Key

Key: Authorization

Value: {{api_key}}

Add to: Header

Tests

Status code is 200.

Verifică faptul că email-ul din user.email din răspuns este identic cu email-ul generat la Register (pm.environment.get("email")).

4️⃣ Delete Profile – DELETE {{baseUrl}}/user/delete_profile.php

Auth

Type: Bearer Token

Token: {{token}}

Tests

Status code is 200.

Verifică mesajul de succes (ex. status === "success" sau textul din message).

Rulare (Collection Runner)

Deschizi Postman.

Selectezi environment-ul: HapifyMe Tema Env.

Deschizi Collection Runner pe colecția Tema 18 - DavidSafta.

Rulezi 1 iterație cu toate request-urile.

Rezultat: toate testele verzi (Register, Login, Get Profile, Delete Profile).


---

## 3. Text pentru platformă (Your Answer) – copy/paste

Editează doar linkul de repo dacă alegi alt nume.

```text
Link repository: https://github.com/DavidSafta/hapifyme-tema18-postman

Colecție / Environment:
- Colecție: `Tema 18 - DavidSafta`
- Environment: `HapifyMe Tema Env`
- Variabile: baseUrl, email (dinamic), api_key, user_id, user_id_from_jwt, token.

Flux implementat:
1) Register (POST /user/register.php)
   - Pre-request Script: generează un email unic pe baza timestamp-ului și îl salvează în environment (`email`).
   - Body folosește `{{email}}`.
   - Tests: status 201, `status = "success"`, salvează `api_key` și `user_id` în environment.

2) Login (POST /user/login.php)
   - Folosește username/email-ul creat la Register.
   - Tests:
     - verifică status 200;
     - verifică `status = "success"`;
     - extrage JWT `token` și `user.id`;
     - decodează payload-ul JWT, extrage `user_id` și îl salvează ca `user_id_from_jwt`;
     - compară `user_id_from_jwt` cu `user.id` (asert: sunt egale).

3) Get Profile (GET /user/get_profile.php)
   - Auth: API Key în header `Authorization` cu valoarea `{{api_key}}`.
   - Parametru `user_id = {{user_id}}`.
   - Tests:
     - verifică status 200;
     - verifică faptul că email-ul din `user.email` este identic cu email-ul generat la Register (variabila de mediu `email`).

4) Delete Profile (DELETE /user/delete_profile.php)
   - Auth: Bearer Token cu `{{token}}` (salvat la Login).
   - Tests:
     - verifică status 200;
     - verifică mesajul de succes (profil șters cu succes).

Rulare:
- Selectez environment-ul `HapifyMe Tema Env`.
- Rulez colecția `Tema 18 - DavidSafta` cu Collection Runner.
- Toate testele (Register, Login, Get Profile, Delete Profile) sunt verzi.
