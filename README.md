# After project creation
1. verifica dipendenze per vulnerabilità di sicurezza note, OWASP Dependency-Check
aggiunta dipendenza a org.owasp.dependencycheck da eseguire con gradlew dependencyCheckAnalyze
2. com.github.ben-manes.versions per verificare se ci sono librerie piu' recenti tra quelle introdotte
nelle dipendenza 
aggiunta dipendenza a com.github.ben-manes.version da eseguire con gradlew dependencyUpdates


# MyLibrary - Documentazione del Progetto

**MyLibrary** è un'applicazione Android che consente di scansionare le coste dei libri in una libreria personale utilizzando la fotocamera del dispositivo. I dati vengono elaborati tramite OCR e inviati alle API di OpenAI per identificare i titoli e gli autori dei libri riconosciuti.

L'app è stata strutturata in diversi moduli per garantire manutenibilità, scalabilità e riutilizzabilità. Di seguito è fornita una descrizione dettagliata della struttura del progetto, delle funzionalità e dei moduli.

## Struttura del Progetto

La struttura del progetto **MyLibrary** è suddivisa in sei moduli principali:

1. **app**: Modulo principale dell'applicazione, gestisce l'interfaccia utente e la logica di presentazione.
2. **camera**: Gestione della fotocamera per acquisire immagini dei libri.
3. **ocr**: Elaborazione OCR delle immagini per estrarre il testo.
4. **network**: Gestione delle comunicazioni con le API di OpenAI.
5. **auth**: Gestione dell'autenticazione, inclusa la memorizzazione sicura delle chiavi API.
6. **data**: Gestione dei dati, inclusi i modelli dei libri e la logica per interfacciarsi con il network.

### Struttura delle Directory

La struttura del progetto segue un'organizzazione modulare. Ogni modulo è una cartella separata all'interno del progetto con il proprio file `build.gradle`:

```
MyLibrary/
├── app/
│   ├── src/
│   └── build.gradle
├── camera/
│   ├── src/
│   └── build.gradle
├── ocr/
│   ├── src/
│   └── build.gradle
├── network/
│   ├── src/
│   └── build.gradle
├── auth/
│   ├── src/
│   └── build.gradle
├── data/
│   ├── src/
│   └── build.gradle
└── settings.gradle
```

Ogni modulo contiene il proprio codice sorgente, test e risorse necessarie.

## Moduli del Progetto

### 1. Modulo `app`

- **Funzionalità**: Punto di ingresso dell'applicazione. Contiene la `MainActivity` che gestisce l'avvio dell'app e l'inizializzazione della UI.
- **Interfaccia Utente**: Definita con layout XML tradizionali. Utilizza `MainScreen` per mostrare la lista dei libri riconosciuti e la preview della fotocamera.
- **Dipendenze**: Utilizza tutti gli altri moduli (`camera`, `ocr`, `network`, `auth`, `data`).

### 2. Modulo `camera`

- **Funzionalità**: Gestione della fotocamera utilizzando Android CameraX per acquisire le immagini dei libri.
- **Componenti**:
    - `CameraPreview`: Fornisce l'interfaccia per la preview della fotocamera e invia le immagini acquisite al modulo OCR per l'elaborazione.
- **Tecnologie**: CameraX (`camera-core`, `camera-view`, `camera-lifecycle`).

### 3. Modulo `ocr`

- **Funzionalità**: Elaborazione delle immagini utilizzando l'OCR (Optical Character Recognition) per estrarre il testo dai libri inquadrati.
- **Componenti**:
    - `OCRProcessor`: Utilizza **Google ML Kit** per il riconoscimento del testo nelle immagini.
- **Dipendenze**: Utilizza Google ML Kit per il riconoscimento ottico dei caratteri.

### 4. Modulo `network`

- **Funzionalità**: Gestione delle comunicazioni di rete, in particolare per interagire con le API di OpenAI e ottenere informazioni sui libri riconosciuti.
- **Componenti**:
    - `OpenAIClient`: Effettua richieste API a OpenAI per identificare i titoli e gli autori.
- **Tecnologie**: Utilizza **Retrofit** per la gestione delle richieste HTTP e **Gson** per la serializzazione/deserializzazione dei dati JSON.

### 5. Modulo `auth`

- **Funzionalità**: Gestione dell'autenticazione dell'utente e memorizzazione sicura della chiave API di OpenAI.
- **Componenti**:
    - `AuthUtils`: Funzioni per salvare e recuperare la chiave API utilizzando `EncryptedSharedPreferences`.
    - `ApiKeyActivity`: Attività per consentire all'utente di inserire e salvare la propria chiave API.
- **Sicurezza**: Utilizza **AndroidX Security** per la crittografia sicura delle preferenze.

### 6. Modulo `data`

- **Funzionalità**: Gestione dei dati dell'applicazione, inclusi i modelli per i libri e la logica di business per interagire con la rete.
- **Componenti**:
    - `Book`: Modello dati per rappresentare i libri.
    - `BookViewModel`: Logica di business per gestire la lista dei libri riconosciuti.
- **Architettura**: Segue l'architettura **MVVM** (Model-View-ViewModel) per la gestione della logica di business.

## Requisiti di Sistema

- **Android Studio**: Versione 4.0 o successiva.
- **SDK Minimo**: 21 (Lollipop)
- **SDK Target**: 33 (Tiramisu)

## Configurazione del Progetto

1. **Clonare il Repository**:

   ```bash
   git clone <URL_del_repository>
   ```

2. **Importare il Progetto in Android Studio**:

    - Aprire Android Studio e selezionare "Open an existing Android Studio project".
    - Navigare alla cartella del progetto e selezionarla.

3. **Configurare la Chiave API**:

    - L'applicazione richiede una chiave API di **OpenAI**.
    - Al primo avvio, l'applicazione chiederà di inserire la chiave API, che verrà salvata in modo sicuro tramite `EncryptedSharedPreferences`.

## Utilizzo dell'Applicazione

1. **Avvio**: Lancia l'applicazione su un dispositivo Android o emulatore.
2. **Inserimento Chiave API**: Al primo avvio, inserisci la tua chiave API di OpenAI nella schermata di autenticazione.
3. **Scansione dei Libri**:
    - Punta la fotocamera verso i libri nella tua libreria personale.
    - L'app effettuerà il riconoscimento dei testi sulle coste dei libri e invierà i dati a OpenAI per l'identificazione.
    - I libri riconosciuti verranno mostrati in una lista nella schermata principale.

## Dipendenze Principali

- **AndroidX**: Utilizzato per le componenti UI, il ciclo di vita e il ViewModel.
- **CameraX**: Utilizzato per l'accesso alla fotocamera e l'acquisizione delle immagini.
- **Google ML Kit**: Utilizzato per l'elaborazione OCR delle immagini.
- **Retrofit**: Utilizzato per le chiamate API a OpenAI.
- **AndroidX Security**: Utilizzato per la gestione sicura delle chiavi API.

## Testing

Il progetto è stato strutturato per supportare sia **test unitari** che **test strumentati**:

- **Test Unitari** (`test`): Test locali che vengono eseguiti sulla JVM del computer per verificare la logica di business dei moduli `network` e `data`.
- **Test Strumentati** (`androidTest`): Test eseguiti su un dispositivo o emulatore Android, utili per verificare l'interazione tra i componenti Android e la fotocamera (`camera`).

Per eseguire i test, utilizza Android Studio:

- **Test Unitari**: Vai sul file di test e clicca con il tasto destro sul metodo di test che vuoi eseguire, quindi seleziona "Run...".
- **Test Strumentati**: Collega un dispositivo o avvia un emulatore, quindi esegui i test dalla cartella `androidTest`.

## Contributi

I contributi sono benvenuti! Segui questi passaggi per contribuire:

1. **Fork del Repository**
2. **Crea un Branch** per la tua funzionalità:
   ```bash
   git checkout -b feature/nuova-funzionalita
   ```
3. **Fai il Commit** delle tue modifiche:
   ```bash
   git commit -m "Aggiunta di una nuova funzionalità"
   ```
4. **Pusha il Branch** su GitHub:
   ```bash
   git push origin feature/nuova-funzionalita
   ```
5. **Crea una Pull Request**

## Licenza

Questo progetto è rilasciato sotto la **MIT License**. Consulta il file `LICENSE` per ulteriori dettagli.

## Contatti

Per qualsiasi domanda o problema, puoi contattare il maintainer del progetto all'indirizzo email: [maintainer@example.com](mailto\:maintainer@example.com).

