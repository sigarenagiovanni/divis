# Divi’s

Obiettivi giornalieri di coppia, pensati per cervelli ADHD. Sito statico su **GitHub Pages** (gratis) e dati sincronizzati con **Firebase** (piano gratuito Spark, senza carta di credito). Solo le vostre due email Google possono leggere e scrivere i dati.

Tempo totale: circa 20 minuti. Non serve installare niente: si fa tutto dal browser.

---

## 1. Metti il sito su GitHub Pages

1. Crea un account su [github.com](https://github.com) (se non ce l'hai).
2. In alto a destra: **+ → New repository**.
   - Nome: `divis`
   - Visibilità: **Public** (con l'account gratuito, Pages funziona solo con repository pubblici)
   - Clicca **Create repository**.
3. Nella pagina del repository vuoto clicca **uploading an existing file** (oppure **Add file → Upload files**).
4. Trascina dentro **tutti** i file di questa cartella: `index.html`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`, `firestore.rules`, `README.md`. Poi **Commit changes**.
5. Vai in **Settings → Pages**.
   - Source: **Deploy from a branch**
   - Branch: **main**, cartella **/ (root)** → **Save**
6. Dopo qualche minuto il sito è online su:
   `https://TUO-NOME-UTENTE.github.io/divis/`

A questo punto l'app funziona già, ma in **modalità locale** (i dati restano sul singolo telefono). I passi seguenti collegano il database condiviso.

## 2. Crea il progetto Firebase

1. Vai su [console.firebase.google.com](https://console.firebase.google.com) ed entra con il tuo account Google.
2. **Crea un progetto** → nome `divis` → Google Analytics puoi disattivarlo → **Crea**.
3. Il progetto parte sul piano gratuito **Spark**. Non serve aggiungere una carta.

## 3. Registra l'app web e incolla la configurazione

1. Nella panoramica del progetto clicca l'icona **Web** (`</>`).
2. Nickname: `divis`. **Non** spuntare Firebase Hosting. Clicca **Registra app**.
3. Firebase mostra un blocco `const firebaseConfig = { ... }`. Copia solo il contenuto tra le graffe.
4. Su GitHub apri `index.html` → icona della **matita** (Edit).
5. Cerca `window.FIREBASE_CONFIG` e sostituisci le righe `INCOLLA_QUI` con i tuoi valori. Deve venire così:

   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "divis-xxxx.firebaseapp.com",
     projectId: "divis-xxxx",
     storageBucket: "divis-xxxx.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abc123"
   };
   ```

6. **Commit changes**.

> Questi valori non sono segreti: identificano il progetto, non danno accesso ai dati. La protezione vera sono le regole del passo 4.

## 4. Crea il database e proteggilo

1. Nel menu di Firebase: **Build → Firestore Database → Crea database**.
2. Posizione: scegli una regione in Europa (es. `eur3` o `europe-west`). Non si può cambiare dopo.
3. Modalità: **produzione** (tutto bloccato finché non metti le regole).
4. Apri la scheda **Regole**, cancella quello che c'è e incolla il contenuto di `firestore.rules`.
5. Sostituisci `TUA_EMAIL@gmail.com` e `SUA_EMAIL@gmail.com` con le email Google con cui accederete, **tutto in minuscolo**.
6. **Pubblica**.

## 5. Attiva il login con Google

1. **Build → Authentication → Inizia**.
2. Scheda **Metodo di accesso** → **Google** → **Attiva** → scegli l'email di assistenza → **Salva**.
3. Scheda **Impostazioni → Domini autorizzati → Aggiungi dominio** e inserisci:
   `TUO-NOME-UTENTE.github.io`
   (senza `https://` e senza `/divis`).

## 6. Prova

1. Apri `https://TUO-NOME-UTENTE.github.io/divis/` → **Accedi con Google**.
2. Inserisci i vostri due nomi e aggiungi 2-3 abitudini.
3. Manda il link alla tua ragazza: entra con il suo account Google e sceglie il suo nome. Da lì in poi viene riconosciuta in automatico.
4. Sul telefono: **Condividi → Aggiungi a Home** (iPhone) oppure **⋮ → Installa app / Aggiungi a schermata Home** (Android). Si apre a schermo intero come un'app.

---

## Salvataggio e backup

- In alto a destra c'è l'indicatore di salvataggio: **Salvato** (verde) vuol dire che tutto è nel database condiviso. **Offline** o **Riprovo…** vuol dire che le modifiche sono già conservate sul telefono e verranno inviate da sole appena possibile, anche se chiudi l'app.
- Toccando l'indicatore o il tuo avatar apri **Account e dati**: da lì puoi forzare un nuovo tentativo, scaricare un **backup** completo in formato `.json` e **importarlo**.
- **Spostare i dati dalla versione su Claude a questa**: nella versione su Claude apri Account e dati → *Scarica backup*. Qui apri Account e dati → *Importa backup* → *Sostituisci tutto con il backup*. Poi ognuno sceglie il proprio nome.

## Problemi comuni

| Cosa vedi | Cosa fare |
|---|---|
| “Dominio non autorizzato” al login | Passo 5.3: aggiungi `tuonome.github.io` ai domini autorizzati. |
| “Questo account non è nella lista” | L'email non è nelle regole (passo 4) o c'è una maiuscola. Correggi, pubblica, ricarica. |
| Banner “Firebase non è ancora configurato” | La configurazione del passo 3 non è stata salvata in `index.html`, oppure c'è ancora `INCOLLA_QUI`. |
| Le modifiche al codice non si vedono | GitHub Pages può metterci fino a 10 minuti. Poi ricarica la pagina. |
| L'indicatore resta su “Non salvato” | L'email non è autorizzata nelle regole di Firestore, oppure le regole non sono state pubblicate (passo 4). Le modifiche restano sul telefono finché non sistemi. |
| Il popup di login non si apre | Consenti i popup per il sito, oppure prova da Chrome/Safari invece che dal browser interno di un'app (Instagram, Telegram…). |

## Limiti del piano gratuito

Firestore sul piano Spark include 50.000 letture e 20.000 scritture al giorno e 1 GiB di spazio: per due persone che segnano le loro giornate siete molto, molto sotto. Se una quota si esaurisse, il servizio si ferma fino al giorno dopo: non arriva nessuna bolletta.

## Modificare l'app

Tutto il codice è in `index.html`. Per cambiare testi o consigli: apri il file su GitHub → matita → modifica → **Commit changes**. I consigli sono nell'elenco `TIPS`, le idee di abitudini in `SUGG`, le strategie anti-blocco in `STUCK`, i messaggi di incoraggiamento in `CHEERS`, i livelli in `LEVELS`.
