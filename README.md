# 📘 README — Service d’Authentification (JWT)

## 🧠 Description

Ce microservice gère :

- Authentification avec JWT  
- Inscription des utilisateurs  
- Gestion des rôles :  
  - patient  
  - dermatologue  
  - admin  
- Refresh des tokens  
- Logout (blacklist)  
- Vérification des tokens  

---

## ⚙️ Base URL

http://127.0.0.1:8000/api/auth/

---

## 🔐 1️⃣ Register (Inscription)

### 📌 Endpoint

POST http://127.0.0.1:8000/api/auth/register/

### 📥 Input

```json
{
  "username": "rayane",
  "password": "1234",
  "role": "patient"
}
📤 Output
json
{
  "message": "User created"
}
🔑 2️⃣ Login
📌 Endpoint
POST http://127.0.0.1:8000/api/auth/login/

📥 Input
json
{
  "username": "rayane",
  "password": "1234"
}
📤 Output
json
{
  "access": "ACCESS_TOKEN",
  "refresh": "REFRESH_TOKEN",
  "role": "patient"
}
🔁 3️⃣ Refresh Token
📌 Endpoint
POST http://127.0.0.1:8000/api/auth/token/refresh/

📥 Input
json
{
  "refresh": "REFRESH_TOKEN"
}
📤 Output
json
{
  "access": "NEW_ACCESS_TOKEN"
}
🚪 4️⃣ Logout (Blacklist)
📌 Endpoint
POST http://127.0.0.1:8000/api/auth/logout/

📥 Input
json
{
  "refresh": "REFRESH_TOKEN"
}
📤 Output
json
{
  "message": "Logout successful"
}
✔️ 5️⃣ Verify Token
📌 Endpoint
POST http://127.0.0.1:8000/api/auth/verify/

📥 Input
json
{
  "token": "ACCESS_TOKEN"
}
📤 Output
✅ Valide
json
{
  "valid": true
}
❌ Invalide
json
{
  "valid": false
}
🔐 6️⃣ Utilisation du token dans les autres services
📌 Header obligatoire
Authorization: Bearer ACCESS_TOKEN

📌 Exemple
GET /api/diagnostic/
Authorization: Bearer ACCESS_TOKEN

🔄 Flow complet
Register → créer utilisateur

Login → récupérer access + refresh

Utiliser access pour appeler APIs

Si access expire → utiliser refresh

Logout → invalider le refresh token

⚠️ Règles importantes
access_token → courte durée (ex: 30 min)

refresh_token → longue durée (ex: 1 jour)

Toujours envoyer Authorization header

Logout nécessite refresh token

🧪 Test avec Postman
Étapes :
Register

Login → copier access + refresh

Tester refresh

Tester logout

🗄️ Base de données
👤 Utilisateurs
Table : comptes_compte

🔐 Tokens blacklistés
Tables : token_blacklist_outstandingtoken / token_blacklist_blacklistedtoken

🧠 Notes importantes
Les access tokens ne sont pas stockés en base

Les refresh tokens peuvent être blacklistés

Chaque microservice valide le token indépendamment

🚀 Exemple avec Python (client)
python
import requests

url = "http://127.0.0.1:8000/api/auth/login/"

data = {
    "username": "rayane",
    "password": "1234"
}

response = requests.post(url, json=data)

tokens = response.json()

print(tokens)
🎯 Conclusion
Ce service permet :

🔐 Authentification sécurisée

🔄 Gestion des sessions via JWT

🔒 Contrôle des accès avec rôles

⚡ Intégration facile avec microservices