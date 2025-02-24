# **Meat Member Club API**

Deze API stelt partners in staat om MMC-leden te valideren, kortingen toe te passen en loyaliteitspunten toe te kennen aan leden na voltooiing van een aankoop.

Alle API-aanroepen vereisen een **`secret_key`** in de headers om authenticatie te garanderen.

> **Opmerking:** De `secret_key` identificeert de webshop en moet in elke aanvraag worden meegegeven als header. De `user_id` is de identificatiecode die door de eindklant wordt opgegeven tijdens het afrekenproces in de webshop.

---

## **1. Ledenvalidatie en kortingen ophalen**
### **GET** `/api/partners-transactions/{user_id}`

**Gebruik:** Controleert of de opgegeven `user_id` geldig is en retourneert de kortingsinformatie voor de gebruiker.

**URL:**
```plaintext
GET https://partner.meatmemberclub.nl/api/partners-transactions/{user_id}
```

**Headers:**
| Header Key   | Waarde            |
| ------------ | ----------------- |
| `secret_key` | `JOUW_SECRET_KEY` |

### **Mogelijke Responses**
✅ **200 - Detail Gevonden**
```json
{
    "success": true,
    "code": 200,
    "message": "detail_found",
    "data": {
        "discount": 15
    }
}
```

❌ **404 - Gebruiker niet gevonden**
```json
{
    "success": false,
    "code": 404,
    "message": "user_not_found"
}
```

❌ **404 - Ongeldige secret key**
```json
{
    "success": false,
    "code": 404,
    "message": "invalid_secret_key"
}
```

---

## **2. Transactie aanmaken (loyaliteitspunten toekennen)**
### **POST** `/api/partners-transactions/`

**Gebruik:** Registreert een transactie door loyaliteitspunten toe te kennen aan een gebruiker.

**URL:**
```plaintext
POST https://partner.meatmemberclub.nl/api/partners-transactions/
```

**Headers:**
| Header Key   | Waarde            |
| ------------ | ----------------- |
| `secret_key` | `JOUW_SECRET_KEY` |

**Body (JSON-formaat):**
```json
{
  "user_id": "KLANT_ID",
  "credit_points": 0,
  "ref_number": "123456",
  "description": "test"
}
```

### **Mogelijke Responses**
✅ **200 - Transactie succesvol aangemaakt**
```json
{
    "success": true,
    "code": 200,
    "message": "transaction_created_successfully"
}
```

❌ **404 - Gebruiker niet gevonden**
```json
{
    "success": false,
    "code": 404,
    "message": "user_not_found"
}
```

❌ **404 - Ongeldige secret key**
```json
{
    "success": false,
    "code": 404,
    "message": "invalid_secret_key"
}
```

---

## **3. Transactie annuleren of bewerken**
### **PATCH** `/api/partners-transactions/`

**Gebruik:** Annuleert of wijzigt een bestaande transactie op basis van het `ref_number`.

> **Opmerking:** Het opgegeven `ref_number` moet overeenkomen met een bestaande transactie om deze te kunnen annuleren of bewerken.

**URL:**
```plaintext
PATCH https://partner.meatmemberclub.nl/api/partners-transactions/
```

**Headers:**
| Header Key   | Waarde            |
| ------------ | ----------------- |
| `secret_key` | `JOUW_SECRET_KEY` |

**Body (JSON-formaat):**
```json
{
  "user_id": "KLANT_ID",
  "ref_number": "123456",
  "description": "test"
}
```

### **Mogelijke Responses**
✅ **200 - Transactie succesvol geannuleerd**
```json
{
    "success": true,
    "code": 200,
    "message": "transaction_reverted_successfully"
}
```

❌ **404 - Transactie niet gevonden**
```json
{
    "success": false,
    "code": 404,
    "message": "transaction_not_found"
}
```

❌ **404 - Ongeldige secret key**
```json
{
    "success": false,
    "code": 404,
    "message": "invalid_secret_key"
}
```

❌ **404 - Gebruiker niet gevonden**
```json
{
    "success": false,
    "code": 404,
    "message": "user_not_found"
}
```

---

## **Authenticatie en Beveiliging**
- Elke aanvraag moet de header **`secret_key`** bevatten met een geldige waarde.
- Alle aanvragen gebruiken **HTTPS** om gegevensbeveiliging te garanderen.
- Indien de `secret_key` ongeldig is, wordt een **404** response teruggestuurd met `"invalid_secret_key"`.
