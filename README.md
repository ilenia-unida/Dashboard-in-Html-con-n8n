# 📊 Dashboard e AI Reportistica per Chatbot QA (Flusso 13 - n8n)

**Video di Spiegazione Dettagliata:** [https://youtu.be/gNxayFaFNeM](https://youtu.be/gNxayFaFNeM)

Questo workflow di n8n è un sistema di **Business Intelligence e Analisi Automatizzata** progettato per monitorare e valutare le performance del Chatbot di Question-Answering.

Il flusso svolge due funzioni principali:

1.  **Generazione di una Dashboard interattiva** tramite Webhook, che visualizza in tempo reale le domande e risposte registrate.
2.  **Generazione di un Report di Criticità** analizzato da Google Gemini e salvato in Google Docs, con notifica via email.

---

## ⚙️ Architettura del Flusso

### 1. Dashboard di Monitoraggio (Web View)

Questa sezione permette di visualizzare i dati in tempo reale accedendo all'URL del Webhook (path `/dashboard`):

1.  **`Webhook`:** Punto di accesso per la dashboard.
2.  **`Search records` (Airtable):** Recupera l'intero storico delle interazioni Domanda/Risposta loggate.
3.  **`Estrai solo Domanda e Risposta` (Code):** Normalizza i dati, creando un array con `domanda`, `risposta` e `data`. Calcola i **temi principali** e identifica il `temaPiuRilevante` (basato sulla prima parola della domanda).
4.  **`HTML`:** Utilizza i dati normalizzati per generare una pagina HTML contenente tabelle e due grafici (Chart.js): distribuzione dei temi e risposte per giorno.
5.  **`Respond to Webhook`:** Restituisce il codice HTML al browser, visualizzando la Dashboard.

### 2. Analisi e Reportistica AI (Report di Criticità)

Questa sezione utilizza l'intelligenza artificiale per analizzare i dati e generare un report automatizzato:

1.  **`Array -> string` (Code):** Converte la lista di Domande/Risposte in una stringa JSON per l'analisi AI.
2.  **`Google Gemini Chat Model` & `AI Agent`:** L'Agente AI agisce da "analista" per identificare criticità, errori o problemi nelle interazioni e produrre un report strutturato.
3.  **`Create a document` (Google Docs):** Crea un nuovo documento di Google Docs (`Report Criticità Risposte`).
4.  **`Merge`:** Combina l'ID del documento creato con l'output di analisi dell'Agente AI.
5.  **`Prepara report` (Code):** Normalizza i dati combinati.
6.  **`Update a document` (Google Docs):** Inserisce l'analisi di testo generata dall'AI all'interno del documento.
7.  **`Download file` (Google Drive):** Nodo incluso nel flusso.
8.  **`Send a message` (Gmail):** Invia una notifica via email (`ileniaunidawork@gmail.com`) indicando che il report è stato aggiornato e fornendo l'URL per accedervi.

---

## ⚠️ Credenziali Richieste

Questo workflow richiede la configurazione delle seguenti credenziali in n8n per operare:

1.  **Airtable Personal Access Token account**
2.  **Google Gemini (PaLM) Api account**
3.  **Google Docs account** (OAuth2)
4.  **Google Drive account** (OAuth2)
5.  **Gmail account** (OAuth2)
