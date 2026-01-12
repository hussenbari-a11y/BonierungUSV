# Bonierung - Fußballmannschaft Getränke-Tracking

Eine Web-Applikation zur Verwaltung von Getränke- und Speisenbestellungen für Fußballmannschaften mit Firebase-Backend.

## 📁 Dateien

- **admin.html** - Admin-Oberfläche für Bonierung, Team- und Item-Verwaltung
- **ranking.html** - Öffentliche Ranking-Ansicht (Alkohol-Umsatz pro Person)

## 🔧 Firebase Einrichtung

### 1. Firebase-Projekt erstellen
1. Gehe zu [Firebase Console](https://console.firebase.google.com/)
2. Klicke auf "Projekt hinzufügen"
3. Gib einen Projektnamen ein und folge den Anweisungen

### 2. Firestore-Datenbank aktivieren
1. Im Firebase-Projekt: **Build → Firestore Database**
2. Klicke auf "Datenbank erstellen"
3. Wähle den **Testmodus** (für einfachen Start)
4. Wähle einen Standort (z.B. `europe-west3` für Frankfurt)

### 3. Firebase-Konfiguration kopieren
1. Gehe zu **Projekteinstellungen** (Zahnrad-Icon)
2. Unter "Ihre Apps" klicke auf **Web** (</>)
3. Registriere die App mit einem Namen
4. Kopiere die Konfiguration:

```javascript
const firebaseConfig = {
    apiKey: "AIza...",
    authDomain: "dein-projekt.firebaseapp.com",
    projectId: "dein-projekt",
    storageBucket: "dein-projekt.appspot.com",
    messagingSenderId: "123456789",
    appId: "1:123456789:web:abc123"
};
```

### 4. Konfiguration in die HTML-Dateien eintragen
Ersetze in **beiden** Dateien (admin.html und ranking.html) den Platzhalter:

```javascript
// Firebase Konfiguration - HIER EIGENE DATEN EINTRAGEN!
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",           // ← Hier ersetzen
    authDomain: "YOUR_PROJECT...",    // ← Hier ersetzen
    projectId: "YOUR_PROJECT_ID",     // ← Hier ersetzen
    ...
};
```

### 5. Firestore-Regeln anpassen (optional)
Für Produktion solltest du die Regeln anpassen. Gehe zu **Firestore → Regeln**:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;  // Testmodus - für Produktion anpassen!
    }
  }
}
```

## 🚀 Verwendung

### Admin-Oberfläche (admin.html)

#### Tab 1: Bonierung (Standard)
- Team aus Dropdown auswählen
- Getränk oder Speise anklicken
- Anzahl mit +/- einstellen
- "Bonieren" klicken

#### Tab 2: Teams verwalten 🔒
- Neue Teams hinzufügen (Name + Personenanzahl)
- Bestehende Teams löschen
- **Passwortgeschützt** (Standard: `admin123`)

#### Tab 3: Speisen/Getränke verwalten 🔒
- Neue Items hinzufügen:
  - Name
  - Preis
  - Typ (Getränk/Speise)
  - Alkoholisch (Ja/Nein)
- **Passwortgeschützt** (Standard: `admin123`)

#### Tab 4: Übersicht
- Zeigt alle Teams mit ihren Bestellungen
- Gesamtsumme pro Team

### Ranking-Seite (ranking.html)
- Zeigt alle Teams sortiert nach Alkohol-Umsatz pro Person
- Aktualisiert sich automatisch alle 30 Sekunden
- Zeigt nur **alkoholische Getränke** im Umsatz!

## ⚙️ Konfiguration

### Passwortschutz deaktivieren
In `admin.html` Zeile 193:
```javascript
const PASSWORD_CHECK_ENABLED = false;  // Auf false setzen
```

### Passwort ändern
In `admin.html` Zeile 194:
```javascript
const ADMIN_PASSWORD = "dein-neues-passwort";
```

## 📊 Datenstruktur (Firestore Collections)

### teams
```json
{
  "name": "FC Beispiel",
  "persons": 11,
  "createdAt": "..."
}
```

### items
```json
{
  "name": "Bier 0.5l",
  "price": 3.50,
  "type": "drink",        // "drink" oder "food"
  "alcoholic": true,
  "createdAt": "..."
}
```

### consumptions
```json
{
  "teamId": "abc123",
  "itemId": "xyz789",
  "itemName": "Bier 0.5l",
  "itemPrice": 3.50,
  "itemType": "drink",
  "itemAlcoholic": true,
  "quantity": 2,
  "total": 7.00,
  "createdAt": "..."
}
```

## 🎨 Design
- Violettes Farbschema
- Mobile-optimiert (responsive)
- Touch-freundliche Buttons

## 📱 Hosting-Optionen

### Option 1: Firebase Hosting (empfohlen)
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

### Option 2: Lokal testen
Einfach die HTML-Dateien im Browser öffnen (funktioniert mit Firebase).

### Option 3: Beliebiger Webserver
Die Dateien können auf jedem Webserver gehostet werden (z.B. Apache, Nginx, Netlify, Vercel).

---

**Viel Spaß beim Bonieren! 🍺⚽**
