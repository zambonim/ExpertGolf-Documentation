# Progetto GolfDatabaseUtility - Log di Sviluppo

## Panoramica del Progetto

### Nome Progetto
**GolfDatabaseUtility** - App utility standalone per la manutenzione del database campi di ExpertGolf

### Genesi del Progetto
**Data**: 12 Agosto 2025  
**Motivo**: ExpertGolf si basa su dati hardcoded dei campi da golf che sono incompleti e obsoleti rispetto al database ufficiale FIG (Federazione Italiana Golf)

### Definizione del Problema
- ExpertGolf contiene ~83 campi da golf con configurazioni incomplete
- Il database ufficiale FIG contiene 848 configurazioni su 220 campi da golf italiani
- Gli aggiornamenti manuali del database sono soggetti a errori e richiedono tempo
- I dati hardcoded creano un carico di manutenzione e limitano la scalabilità dell'app

### Strategia di Soluzione
Creare un'app utility separata che:
1. Importa i dati Excel ufficiali FIG (848 record)
2. Valida e mappa i dati nel formato ExpertGolf
3. Genera file Swift aggiornati per l'integrazione
4. Mantiene la qualità e consistenza dei dati
5. Consente aggiornamenti periodici senza disturbare l'app principale

---

## Architettura Tecnica

### Strategia Database: Approccio Ibrido
- **Tier 1 (Core)**: 83 campi esistenti popolari - Swift hardcoded (alte performance)
- **Tier 2 (Extended)**: ~137 campi aggiuntivi - formato JSON (lazy loading)
- **Copertura Totale**: 220 campi da golf italiani (database FIG completo)

### Fonti Dati
- **Primaria**: File Excel FIG da https://www.federgolf.it/attivita-agonistica/servizi-online/tabella-slope-course-rating/
- **Struttura**: Circolo, Percorso, PAR + 6 tipi di tee (Nero, Bianco, Giallo, Verde, Blu, Rosso, Arancio)
- **Contatto**: commissione.rating@federgolf.it (per aggiornamenti futuri)

---

## File ExpertGolf Impattati

### File Principali
- **CampiView.swift** - Vista principale campi da golf e servizio database
  - Contiene GolfCoursesService con dati campi hardcoded
  - Metodo getRawJavaScriptDatabase() con tutte le configurazioni campi
  - Sarà aggiornato per supportare caricamento ibrido Tier 1/Tier 2

### File di Supporto (Identificati)
- **CourseDetailCompactView** - Visualizzazione dettagli campo (referenziato in CampiView)
- **FIGService** - Servizio di integrazione FIG esistente
- **Models** - Enum GolfCourse, CourseLayout, TeeRating, TeeColor, Gender

### File da Creare
- **GolfDatabaseUtility** (progetto Xcode separato)
- **ExtendedCoursesLoader.swift** - Caricatore dati JSON Tier 2
- **courses-extended.json** - File database esteso

---

## Progresso Sviluppo

### Fase 1: Analisi e Pianificazione ✅
**Completata**: 12 Agosto 2025
- [x] Analizzata struttura database ExpertGolf attuale
- [x] Importati e analizzati dati Excel FIG ufficiali (848 record)
- [x] Identificate discrepanze (es. ASOLO: 7 config in app vs 9 in FIG)
- [x] Confermata strategia di mapping dati
- [x] Definito approccio architettura ibrida

**Risultati Chiave**:
- Database FIG ha 848 configurazioni campi vs ~300 nell'app attuale
- Configurazioni mancanti includono "Provvisorio 2024/2025" e tornei speciali
- Struttura dati è compatibile con formato app esistente
- 220 campi da golf unici in Italia vs 83 attualmente supportati

### Fase 2: Sviluppo App Utility 🔄
**Iniziata**: 12 Agosto 2025  
**Stato**: In Pianificazione

**Funzionalità Pianificate**:
- [ ] Importazione e parsing file Excel
- [ ] Motore di validazione e mapping dati
- [ ] Interfaccia di confronto side-by-side
- [ ] Generazione codice Swift per Tier 1
- [ ] Export JSON per Tier 2
- [ ] Report di quality assurance
- [ ] Sistema di backup e versioning

### Fase 3: Integrazione ExpertGolf 🔄
**Stato**: In Attesa

**Modifiche Pianificate**:
- [ ] Aggiornare GolfCoursesService per caricamento ibrido
- [ ] Implementare caricatore JSON Tier 2
- [ ] Aggiungere sistema notifiche aggiornamento in CampiView
- [ ] Testing con database esteso

---

## Insights Qualità Dati

### Analisi Database Attuale
- **Campi Totali in App**: 83 campi
- **Configurazioni Totali in App**: ~300 configurazioni
- **Database Ufficiale FIG**: 220 campi, 848 configurazioni
- **Gap di Copertura**: 137 campi aggiuntivi disponibili

### Esempio Discrepanze (ASOLO)
**In ExpertGolf**: 7 configurazioni
- Giallo-Giallo, Giallo-Verde, Percorso Giallo 9 buche, ecc.

**Nel Database FIG**: 9 configurazioni
- Stesso di sopra PIÙ:
- "Giallo-Verde Provvisorio 2024 Par 70"
- "Trofeo Rocca d'Asolo 2024"

### Necessità di Validazione Dati
- Validazione range Course Rating (CR)
- Validazione Slope Rating (range 55-155)
- Controlli consistenza PAR
- Verifica disponibilità colori tee
- Rilevamento configurazioni duplicate

---

## Manutenzione Futura

### Frequenza Aggiornamenti
- **Raccomandato**: Revisione annuale database FIG
- **Trigger**: Notifica utente dopo 12 mesi dall'ultimo aggiornamento
- **Metodo**: Download Excel FIG aggiornato → Esegui utility → Aggiorna app

### Quality Assurance
- Regole di validazione automatiche nell'utility
- Revisione manuale per configurazioni nuove/modificate
- A/B testing per cambiamenti critici dati campi
- Sistema backup per capacità di rollback

---

## Contesto Progetto per Nuove Sessioni di Sviluppo

### Riassunto Stato Attuale
Questo progetto utility risolve le limitazioni del database hardcoded dei campi da golf di ExpertGolf. Stiamo costruendo un'app utility separata che processa i dati FIG ufficiali e genera file Swift aggiornati. L'approccio ibrido mantiene le performance per i campi popolari estendendo la copertura a tutti i campi da golf italiani.

### File Chiave per Riferimento
- **CampiView.swift**: Contiene database attuale e necessiterà caricamento ibrido
- **File Excel FIG**: Fonte ufficiale con 848 configurazioni
- **Struttura Progetto**: Vista nel navigator Xcode con FIGService già presente

### Prossima Priorità di Sviluppo
Costruire l'app GolfDatabaseUtility con capacità di import Excel, mapping dati e generazione codice Swift.

---

*Versione Documento: 1.0*  
*Ultimo Aggiornamento: 12 Agosto 2025*  
*Prossima Revisione: Al completamento Fase 2*
