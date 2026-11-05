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

## 5. Competency Questions (CQs)

Per validare i requisiti del dominio sono state formulate le seguenti Competency Questions, suddivise per tecnica modellata. L'ontologia dovrà essere in grado di rispondere a queste domande attraverso query SPARQL.

| ID | Tecnica | Competency Question (Domanda) | Risposta Attesa (Elementi Ontologici) |
| :--- | :--- | :--- | :--- |
| **CQ_A1** | ReAct | Da quali elementi è composto il ciclo iterativo della tecnica ReAct, e in che ordine si susseguono? | Una `ReActTrajectory` contiene istanze di `ReasoningTrace`, `Action` e `Observation` (`hasTrajectoryElement`), ordinate tramite `directlyFollows` / `directlyPrecedes` del Sequence ODP. |
| **CQ_A2** | ReAct | A quale entità si affida un'azione ReAct per essere eseguita? | Una `ToolAction` è collegata tramite `requiresTool` a un `ExternalTool`. |
| **CQ_A3** | ReAct | Quale elemento viene restituito dall'ambiente esterno dopo l'esecuzione di un'azione? | Un'istanza di `Observation`, collegata all'azione tramite `producesObservation`. |
| **CQ_A4** | ReAct | Con quale elemento può iniziare una traiettoria ReAct? | Con una `ReasoningTrace` o una `Action`, mai con una `Observation` (`hasFirstElement`). |
| **CQ_A5** | ReAct | Come si ottiene la risposta finale in ReAct, e quali osservazioni vi hanno contribuito? | La `FinishAction` restituisce la `Response` finale (`returnsAnswer`); le `Observation` vi contribuiscono tramite `contributesTo`. |
| **CQ_P1** | Plan-and-Solve | Da quali elementi è composto il piano globale di Plan-and-Solve? | Un `Plan` è composto da istanze di `PlanStep` (`hasPlanStep`). |
| **CQ_P2** | Plan-and-Solve | Da quali fasi è composta la tecnica Plan-and-Solve, e in che ordine si susseguono? | Una `PlanningPhase`, che produce il `Plan` (`producesPlan`), seguita direttamente (`directlyFollows`) da una `ExecutionPhase`, che esegue i `PlanStep` (`executesStep`). |
| **CQ_P3** | Plan-and-Solve | In quale ordine devono essere eseguiti i passi di un piano? | A partire dal primo passo (`hasFirstStep`), seguendo la catena `directlyFollows` / `directlyPrecedes` tra i `PlanStep`. |
| **CQ_P4** | Plan-and-Solve | Quale risultato produce ciascun passo del piano, e come contribuisce alla risposta finale? | Ogni `PlanStep` produce uno `StepResult` (`hasStepResult`), che contribuisce alla `Response` finale (`contributesTo`). |
| **CQ_M1** | MRKL | Quale ruolo assume il Large Language Model all'interno dell'architettura MRKL? | Agisce come `Router` (`actsAsRouter`). |
| **CQ_M2** | MRKL | Verso quali categorie di moduli il router può instradare una richiesta? | Verso istanze di `ExpertModule`, cioè `NeuralExpert` o `SymbolicExpert` (`routesTo`). |
| **CQ_M3** | MRKL | Cosa deve estrarre il router dal testo naturale dell'utente per interrogare correttamente un modulo simbolico? | Istanze di `ExtractedArgument` (`extractsArgument`), ricevute dal `SymbolicExpert` tramite `receivesArgument`. |
| **CQ_M4** | MRKL | Che tipo di output produce un modulo esperto simbolico? | Un `DeterministicResult`, sottoclasse di `ExpertResult` (`returnsDeterministicResult`). |
| **CQ_M5** | MRKL | Da quale prompt è stato estratto un argomento, e a quale modulo esperto è stato passato? | Il `Prompt` di origine tramite `isExtractedFrom`; l'`ExpertModule` destinatario tramite `isPassedTo`. |
| **CQ_M6** | MRKL | Quale risultato restituisce un modulo esperto, anche neurale, e come contribuisce alla risposta finale? | Un `ExpertResult` (`returnsResult`), che contribuisce alla `Response` finale (`contributesTo`). |
| **CQ_G1** | Trasversale | Da quali risultati intermedi è stata costruita una risposta finale? | Le istanze di `IntermediateResult` (`StepResult`, `Observation`, `ExpertResult`) collegate alla `Response` tramite `isBuiltFrom`, per tutte e tre le tecniche. |

## Struttura del Repository
- `peo_ontology_extension.rdf`: File sorgente (TBox) contenente l'implementazione delle classi, delle Object Properties e delle relative restrizioni (Domain/Range) sviluppate in Protégé.
- `evaluation/`: *(Work in Progress)* Directory destinata a ospitare le query SPARQL e i file per la validazione dell'ontologia tramite Reasoner una volta popolata con le istanze (ABox).
