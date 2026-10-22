# PEO
# Estensione PEO Ontology per modellazione di ontologia per la rappresentazione di tecniche di prompt engineering

Questo repository contiene il codice sorgente e la documentazione relativi al lavoro di tesi volto all'estensione della **Prompt Engineering Ontology (PEO)**.

## Tecniche Modellate
L'estensione si concentra su tre paradigmi avanzati di Prompt Engineering:

1. **ReAct (Reasoning and Acting):** Modella il ciclo iterativo in cui il modello alterna un pensiero logico interno (`ReasoningTrace`), l'esecuzione di un comando verso l'esterno (`Action`) e la ricezione di un feedback dall'ambiente (`Observation`).
2. **Plan-and-Solve:** Struttura la capacità dell'LLM di affrontare task complessi generando preventivamente un piano strategico globale (`Plan`), scomposto in sotto-task sequenziali (`PlanStep`) che vengono poi risolti attivamente (`ExecutionPhase`).
3. **MRKL (Modular Reasoning, Knowledge and Language):** Definisce un'architettura neuro-simbolica in cui l'LLM non genera direttamente la risposta, ma funge da `Router`. Il suo compito è estrarre parametri testuali (`ExtractedArgument`) e instradare la query verso il modulo più adatto (`ExpertModule`, che può essere un `NeuralExpert` o un `SymbolicExpert`) per ottenere un risultato inconfutabile (`DeterministicResult`).

---

## Ontology Requirements Specification Document (ORSD)

Seguendo la metodologia **LOT (Linked Open Terms)**, i requisiti dell'ontologia sono stati definiti attraverso la formulazione di *Competency Questions (CQs)*. Di seguito la tabella ufficiale dei requisiti.

### Tabella delle Competency Questions (CQs)

| ID | Competency Question (Domanda) | Risposta Attesa (Elementi Ontologici) |
| :--- | :--- | :--- |
| **CQ_A1** | Da quali fasi logiche sequenziali è composto il ciclo iterativo della tecnica ReAct? | Il ciclo è composto dalle istanze delle classi `ReasoningTrace`, `Action` e `Observation`. |
| **CQ_A2** | In un prompt ReAct, a quale entità si affida una "Action" per essere eseguita? | Una `Action` richiede (`requiresTool`) l'utilizzo di un `ExternalTool`. |
| **CQ_A3** | Quale elemento viene restituito dall'ambiente esterno dopo l'esecuzione di un'Azione in ReAct? | Viene restituita un'istanza della classe `Observation`. |
| **CQ_P1** | Nella tecnica Plan-and-Solve, da quali elementi è composto il piano strategico globale? | Un `Plan` globale è composto (`hasPlanStep`) da una sequenza di molteplici `PlanStep`. |
| **CQ_P2** | Quale fase segue la generazione del piano per arrivare alla risposta finale in Plan-and-Solve? | I singoli `PlanStep` vengono processati in una `ExecutionPhase`. |
| **CQ_M1** | Quale ruolo specifico assume il Large Language Model all'interno dell'architettura MRKL? | L'LLM agisce in qualità di `Router` (Instradatore). |
| **CQ_M2** | In MRKL, verso quali categorie di moduli il Router può instradare una specifica richiesta? | Il `Router` instrada la richiesta (`routesTo`) verso le sottoclassi di `ExpertModule` (`SymbolicExpert` o `NeuralExpert`). |
| **CQ_M3** | Cosa deve estrarre il Router dal testo naturale dell'utente per poter interrogare correttamente un modulo simbolico? | Il Router estrae gli argomenti formali, rappresentati dalla classe `ExtractedArgument`. |
| **CQ_M4** | Che tipo di output viene generato da un modulo esperto di tipo simbolico in MRKL? | Viene restituito un `DeterministicResult` (risultato esatto e non allucinato). |

---

## Struttura del Repository
- `peo_ontology_extension.rdf`: File sorgente (TBox) contenente l'implementazione delle classi, delle Object Properties e delle relative restrizioni (Domain/Range) sviluppate in Protégé.
- `evaluation/`: *(Work in Progress)* Directory destinata a ospitare le query SPARQL e i file per la validazione dell'ontologia tramite Reasoner una volta popolata con le istanze (ABox).
