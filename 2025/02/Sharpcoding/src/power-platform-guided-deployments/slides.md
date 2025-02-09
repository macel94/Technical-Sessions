---
theme: seriph
background: https://cover.sli.dev
title: "Guided Deployments: Power Platform sotto controllo con Azure DevOps"
author: "macel94"
date: "2025-02-28"
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

<!-- Slide 1: Titolo -->
# Guided Deployments  
**Power Platform sotto controllo con Azure DevOps**

---

<!-- Slide 2: Introduzione -->
# Introduzione

- **Obiettivo**: Illustrare un sistema di guided deployments per Power Platform  
- **Contesto**: Ambienti enterprise complessi (es. Gruppo Wuerth)  
- **Focus**:
  - Integrazione con Azure DevOps (CI/CD, work items, retention degli artifact)
  - Custom Extension che sfrutta l’autenticazione nativa e le policy di Azure DevOps
  - Sicurezza e audit trail tramite validazione token e versioning JSON

---

<!-- Slide 3: Contesto e Motivazioni -->
# Contesto e Motivazioni

- **Sfide principali**:
  - Ciclo di vita delle soluzioni complesso
  - Necessità di validazioni in tempo reale su Power Platform
- **Soluzione proposta**:
  - UI guidata per semplificare le richieste di deployment
  - Integrazione nelle pipeline CI/CD per garantire controllo e sicurezza

---

<!-- Slide 4: Architettura del Sistema -->
# Architettura del Sistema

*Il diagramma illustrativo è stato rimosso per ora.*

---

<!-- Slide 5: La Custom Azure DevOps Extension -->
# La Custom Azure DevOps Extension

- Sfrutta l’autenticazione e le policy native di Azure DevOps  
- Gestione automatica dei work items (naming convention, versioning JSON)  
- UI centralizzata "installabile una volta per tutte"  
- **Benefici**:
  - Collaborazione semplificata
  - Comunicazione sicura tramite token

---

<!-- Slide 6: Frontend - Integrazione con Azure DevOps SDK -->
# Frontend: Integrazione con Azure DevOps SDK

Utilizziamo React (o altro framework) e l’SDK di Azure DevOps per:
- Ottenere il token di accesso
- Passare il token al backend per la validazione

*Esempio di integrazione frontend omesso.*

---

<!-- Slide 7: Flusso di Autenticazione -->
# Flusso di Autenticazione

1. **Inizializzazione**: Il frontend si inizializza con l’SDK di Azure DevOps  
2. **Richiesta Token**: Viene ottenuto un token valido per le API  
3. **Invio al Backend**: Il token viene inviato tramite una richiesta HTTP  
4. **Validazione**: Il backend verifica la firma e l’integrità del token

*Questo garantisce una comunicazione sicura in linea con le policy di Azure DevOps.*

---

<!-- Slide 8: Backend API in .NET 9 -->
# Backend API: Sicurezza e Validazione

- Implementata in **.NET 9**
- **JWT**: Validazione del token in arrivo
- Utilizzo di **service principal** per operazioni di deployment

*Esempio di implementazione backend omesso.*

---

<!-- Slide 9: Integrazione nelle Pipeline CI/CD -->
# Integrazione nelle Pipeline CI/CD

- **Automazione**: Deployment avviato direttamente dalla custom extension  
- **Tracciamento**: Ogni richiesta crea un work item con allegato JSON  
- **Pipeline**: Integrazione per:
  - Export della soluzione
  - Validazioni automatiche
  - Deploy controllato tramite service principal

---

<!-- Slide 10: Vantaggi del Guided Deployment -->
# Vantaggi del Guided Deployment

- **Sicurezza**: Autenticazione centralizzata e validazione token  
- **Tracciabilità**: Audit trail completo tramite work items e versioning JSON  
- **Efficienza**: Riduzione degli errori grazie alle validazioni in tempo reale  
- **Scalabilità**: Gestione dei deployment cross-organization in ambienti enterprise

---

<!-- Slide 11: Confronto Tecnologico -->
# Confronto Tecnologico

**Opzioni UI:**
- **React/Typescript con ViteJS**
  - Pro: Ecosistema maturo, grande flessibilità
  - Contro: Configurazioni avanzate possono risultare complesse
- **Blazor WASM**
  - Pro: Integrazione nativa con l’ecosistema .NET
  - Contro: Minor supporto per interazioni dinamiche rispetto a React

*La scelta dipende dalle competenze del team e dai requisiti del progetto.*

---

<!-- Slide 12: Sicurezza e Validazione del Token -->
# Sicurezza e Validazione del Token

- **Validazione JWT**: Controllo dell’integrità e autenticità del token  
- Utilizzo di chiavi simmetriche per la firma  
- **Service principal**: Operazioni con permessi elevati

*Approccio token-based per garantire sicurezza e compliance.*

---

<!-- Slide 13: Caso d'Uso: Gruppo Wuerth -->
# Caso d'Uso: Gruppo Wuerth

- **Ambiente Enterprise**: Deployment cross-organization  
- **Risultati**:
  - Automazione completa del ciclo di vita delle soluzioni
  - Audit trail per ogni deployment
  - Risoluzione di criticità di compliance e sicurezza

---

<!-- Slide 14: Considerazioni Architetturali -->
# Considerazioni Architetturali

- **Custom Extension**: Sfrutta funzionalità native di Azure DevOps  
- **Pipeline CI/CD**: Integrazione automatizzata per export, validazione e deploy  
- **Sicurezza**: Approccio token-based con validazione backend e service principal  
- **Tecnologie UI**: Valutazione tra React e Blazor in base alle competenze

---

<!-- Slide 15: Conclusioni e Prossimi Passi -->
# Conclusioni

- La custom Azure DevOps Extension ha semplificato e reso più sicuro il deployment  
- Integrazione completa nelle pipeline CI/CD per garantire controllo e tracciabilità  
- **Prossimi passi**:
  - Approfondire la configurazione del service principal
  - Sperimentare con differenti tecnologie UI
  - Estendere il sistema per scenari cross-organization

---

<!-- Slide 16: Domande & Risposte -->
# Domande & Risposte

*Spazio dedicato alle domande del pubblico*  
- Quali sono le sfide principali affrontate?  
- Come viene gestita la sicurezza nel passaggio dei token?  
- Approfondimenti sul confronto tra le tecnologie UI?

---

<!-- Slide 17: Ringraziamenti e Riferimenti -->
# Grazie!

**Contatti:**  
[LinkedIn](https://www.linkedin.com/in/francesco-belacca-dev/) • [GitHub](https://github.com/macel94)

**Riferimenti Utili:**
- [Slidev Syntax Guide](https://sli.dev/guide/syntax)
- [Estensione e sicurezza in Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/extend/overview)
- [Power Platform ALM](https://learn.microsoft.com/en-us/power-platform/alm/devops-build-tools)
