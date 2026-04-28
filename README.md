# Ostrovaci – Pruzkum verejneho mineni

## Soubory
| Soubor | Popis |
|---|---|
| `index.html` | Hlavni stranka pruzkumu (verejne) |
| `vysledky.html` | Dashboard s vysledky (chraneno tokenem) |
| `vercel.json` | Routing pro Vercel (ciste URL) |

---

## 1. Firebase – zalozeni projektu

1. Jdete na **https://console.firebase.google.com**
2. Kliknete **"Add project"** → zadejte nazev (napr. `ostrovaci-pruzkum`)
3. Zapnete / vypnete Google Analytics dle libosti → **Continue**
4. V levm menu kliknete **"Firestore Database"** → **Create database**
   - Zvolte region: `europe-west3 (Frankfurt)`
   - Zvolte **"Start in test mode"** (upravime pravidla nize)
5. V levm menu kliknete **"Project settings"** (ozubene kolo)
6. Dolu najdete sekci **"Your apps"** → kliknete ikonu `</>`  (web)
7. Zaregistrujte app → zkopirujte objekt `firebaseConfig`

---

## 2. Vyplneni konfigurace v HTML souborech

V **obou** souborech (`index.html` i `vysledky.html`) najdete sekci:

```javascript
const FIREBASE_CONFIG = {
  apiKey:            "VYPLNTE_API_KEY",
  authDomain:        "VYPLNTE_PROJECT_ID.firebaseapp.com",
  projectId:         "VYPLNTE_PROJECT_ID",
  ...
};
```

Nahradte `VYPLNTE_*` skutecnymi hodnotami z Firebase console.

---

## 3. Doplneni projektu

V obou souborech najdete (musi byt shodne!):

```javascript
const PROJEKTY = [
  { id: "p1", nazev: "Projekt 1 – doplnte nazev" },
  { id: "p2", nazev: "Projekt 2 – doplnte nazev" },
  ...
];
```

Doplnte nazvy svych projektu z volebniho programu. ID `p1`, `p2`... ponechte.

---

## 4. Token dashboardu

V `vysledky.html` zmente token na neco tajneho:

```javascript
const DASHBOARD_TOKEN = "Ostrovaci2026";  // <-- zmente
```

URL dashboardu bude: `https://vas-web.vercel.app/vysledky?token=Ostrovaci2026`

---

## 5. Firestore – pravidla zabezpeceni

V Firebase console → Firestore → **Rules** nastavte:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /pruzkum/{doc} {
      allow create: if true;           // kdokoli muze odeslat
      allow read:   if true;           // dashboard cte data
      allow update, delete: if false;  // nikdo nemaze
    }
  }
}
```

---

## 6. Nasazeni na Vercel

1. Nahrajte vsechny soubory do GitHub repozitare (nebo jen zip)
2. Jdete na **https://vercel.com** → **New Project**
3. Propojte GitHub repo / uploadujte slozku
4. Deploy → hotovo

URL bude napr. `https://ostrovaci-pruzkum.vercel.app`

---

## Pouziti

| Akce | URL |
|---|---|
| Verejny pruzkum | `https://vas-web.vercel.app/` |
| Vas dashboard | `https://vas-web.vercel.app/vysledky?token=Ostrovaci2026` |
| Export CSV | tlacitko na dashboardu |

---

## Poznamky

- Kazdy prohlizec muze odeslat jen jednu odpoved (localStorage)
- Odpovedi jsou anonymni – bez IP, bez jmena
- Word cloud automaticky filtruje ceske pomocne slovicka
- CSV export obsahuje vsechny surove odpovedi
