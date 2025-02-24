# **Meat Member Club API - Ontwikkelaarsdocumentatie**

Deze API stelt partners in staat om MMC-leden te valideren, kortingen toe te passen en loyaliteitspunten toe te kennen aan leden na voltooiing van een aankoop.

Alle API-aanroepen vereisen een **`secret_key`** in de headers om authenticatie te garanderen.

---

## **1. Ledenvalidatie en kortingen ophalen**
### **GET** `/api/partners-transactions/{partner_code}`

**Gebruik:** Controleert of de opgegeven `partner_code` geldig is en retourneert de kortingsinformatie voor de gebruiker.

**URL:**
```plaintext
GET https://partner.meatmemberclub.nl/api/partners-transactions/{partner_code}
```

**Headers:**
| Header Key  | Waarde |
|------------|--------|
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
| Header Key  | Waarde |
|------------|--------|
| `secret_key` | `JOUW_SECRET_KEY` |

**Body (JSON-formaat):**
```json
{
  "user_id": "PARTNER_CODE",
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

**URL:**
```plaintext
PATCH https://partner.meatmemberclub.nl/api/partners-transactions/
```

**Headers:**
| Header Key  | Waarde |
|------------|--------|
| `secret_key` | `JOUW_SECRET_KEY` |

**Body (JSON-formaat):**
```json
{
  "user_id": "PARTNER_CODE",
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

---

## **Veelvoorkomende Problemen en Oplossingen**
| Probleem | Oorzaak | Oplossing |
|----------|---------|-----------|
| `"user_not_found"` | `user_id` (partner_code) is ongeldig | Controleer of de partnercode correct is |
| `"invalid_secret_key"` | Secret key is fout of ontbreekt | Gebruik de juiste `secret_key` in de headers |
| `"transaction_not_found"` | `ref_number` bestaat niet | Controleer of het referentienummer correct is |
