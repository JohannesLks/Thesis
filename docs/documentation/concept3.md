Nachfolgend ein nochmals erweitertes Projektkonzept zum **BSI-Honeypot** (All-Port), das die **unlabeled T-Pot-Daten** als Ergänzung vorsieht und dabei eine **neue, nicht-lineare Fusionsformel** für den finalen Anomaliescore präsentiert. Diese Formel berücksichtigt synergetische Effekte aus verschiedenen Ebenen (Line-, Session-, Graph-Level) und verstärkt Fälle, in denen mehrere Indikatoren parallel hoch sind.

---

# Adaptives BSI-Honeypot-Konzept mit nicht-linearer Score-Fusion:
*All-Port-Erfassung, T-Pot-Datenintegration und neuartige Synergie-Formel*

## 1. Ausgangslage und Ziel

1. **BSI-Honeypot**:  
   - Realisiert ein All-Port-Prinzip (ähnlich MADCAT), erfasst sämtliche eingehende Verbindungen (ggf. hunderte GB Logs, zumeist unlabelt).  
   - Schränkt sich meist auf statische Minimal-Interaktion ein, will aber zukünftig **adaptive** Container-Dienste bei erhöhtem Verdacht starten.

2. **T-Pot als zusätzliche Datenbasis**:  
   - T-Pot vereint diverse Honeypot-Dienste (Cowrie, Dionaea, etc.) und liefert Interaktions- bzw. Session-Logs.  
   - Ebenfalls überwiegend unlabelt, jedoch nützlich für Vortraining auf **Session-Ebene** und tiefe Befehlssequenzen.  

3. **Ziel**:  
   - Ein ressourcenschonendes, stufenweises ML-System (Autoencoder → Session-Transformer-LSTM → optional GNN).  
   - Dynamische Container-Orchestrierung (heuristisch oder RL) zur Emulation auffälliger Verbindungen.  
   - **Neu**: Eine synergetische, nicht-lineare Fusionsformel, die Anomaliescores von line-, session- und graph-Level kombiniert.

---

## 2. Mehrstufige ML-Architektur

### 2.1 Line-Level Autoencoder (AE)
- **Eingabe**: Jede Log-Zeile \( l_i \) wird in Feature-Vektor \( x_i \) (Port, IP, Banner, Byte-Snippets etc.) umgewandelt.  
- **Ausgabe**:  
  - **Line Embedding** \( e_{l_i} \) (Bottleneck),  
  - **Anomaliescore** \( \mathrm{score}_{\mathrm{line}}(l_i) = \| x_i - \hat{x}_i \|_2 \).

### 2.2 Session-Level Transformer-LSTM
- **Session-Bildung**: Anomale Zeilen desselben IP/Protokolls, zeitlich nahe, bilden Session \( s_j \).  
- **Verarbeitung**: Hybrid (LSTM + Self-Attention).  
- **Ergebnis**: Session-Embedding \( e_{s_j} \) + Session-Anomaliescore \( \mathrm{score}_{\mathrm{session}}(s_j) \).

### 2.3 (Optional) Cross-Session Graph + GNN
- **Graph**: Knoten = Sessions, Kanten durch Ähnlichkeit in Embedding + Zeitfenster (z. B. IP-/Credential-Reuse).  
- **GNN** (z. B. GraphSAGE) erkennt multi-session Attacken, z. B. verteilte Bot-Welle.  
- **Graphscore** \( \mathrm{score}_{\mathrm{graph}}(s_j) \) für jede Session im Subgraph.

---

## 3. Neuartige nicht-lineare Fusionsformel

Um die **Synergie** zwischen mehreren hochgradigen Indikatoren (Line, Session, Graph) abzubilden und nicht nur linear zu summieren, definieren wir:

\[
\mathrm{score}_{\mathrm{fusion}}(s_j) 
= \bigl[(\mathrm{score}_{\mathrm{line}}(s_j) + 1)^{\alpha} \cdot (\mathrm{score}_{\mathrm{session}}(s_j) + 1)^{\beta} \cdot (\mathrm{score}_{\mathrm{graph}}(s_j) + 1)^{\gamma}\bigr] - 1
\]

**Interpretation**:  
- Alle Teil-Scores werden um 1 erhöht, damit 0-Anomaly keine Nullmultiplikation verursacht.  
- Die Exponenten \(\alpha, \beta, \gamma\) gewichten die Wichtigkeit der jeweiligen Ebene.  
- Ist \( \mathrm{score}_{\mathrm{line}} \approx 0\), aber \(\mathrm{score}_{\mathrm{session}}\) und \(\mathrm{score}_{\mathrm{graph}}\) hoch, so bleibt das Produkt dennoch groß.  
- **Synergetischer Effekt**: Wenn zwei oder drei Teil-Scores gleichzeitig hoch sind, multiplizieren sich ihre Abweichungen, was den resultierenden Fusion-Score deutlich steigert (stärker als in einer linearen Summe).

**Beispiel**:
- line=2, session=1, graph=0 => \((2+1)^\alpha (1+1)^\beta (0+1)^\gamma -1\).  
- Sind alle > 0 => starker Multiplikationseffekt.

---

## 4. Adaptives Container-Spawning

### 4.1 Heuristisches Grundmodell
- Falls \(\mathrm{score}_{\mathrm{fusion}}(s_j) > T\) und Containerlimit nicht erreicht, wird ein passender Container hochgefahren (z. B. SSH, HTTP).  
- **Ressourcenlimit**: max \(\mathrm{Pods} = M\).  
- Timeout: Nach \(\delta\) min ohne neue Interaktion wird Container beendet.

### 4.2 Reinforcement Learning (später)
- RL-Agent liest \(\mathrm{score}_{\mathrm{fusion}}\) + Containerstatus.  
- Aktionen: spawn/stop service, adjust Banner, etc.  
- Reward**( s_j )** = \(\mathrm{Datengewinn} - \mathrm{Ressourcenaufwand}\).  
- Expert-Override = negative/positive Korrektur in der Replay-Memory.

---

## 5. Versionierung von Angriffsverhalten

### 5.1 Bot & APT-Familien
- Jede Familie hat eine Repräsentation \( f_{\mathrm{fam}} \in \mathbb{R}^d \).  
- Neue Session \( e_{s_j} \) => Distanz \(\mathrm{dist}(f_{\mathrm{fam}}, e_{s_j})\).  
- Größer als \(\Delta\) => neue Version. z. B. “Bot_X_v2“.

### 5.2 Bulk-Step Summaries
- Große repetitive Steps (z. B. 500 bruteforce-Versuche) werden in der Kette als ein “Bulk-Knoten” aufgeführt. 
- Zeitliche Aggregation**( \(\Delta t\) )** => verschlankt die Visualisierung.

---

## 6. Datenlage: BSI-Logs & T-Pot

### 6.1 BSI-Honeypot (unlabeled, All-Port)
- Hauptquelle: Erfassung im realen Betrieb, massive Logs.  
- Kein oder kaum Ground-Truth => unsupervised Ansätze.

### 6.2 T-Pot-Daten (ebenfalls unlabeled, aber session-structured)
- Bieten Interaktionslogs (Cowrie, Dionaea etc.) => Längere Kommandosequenzen.  
- Optionales Vortraining (z. B. Transformer-LSTM auf “SSH Kommandos”).
- Auch hier kein direktes Label, aber man kann heuristische Pseudo-Labels (z. B. “sicherer vs. unsicherer Befehlssatz”) definieren.

### 6.3 Partial Testing: 
- Subsets von KDD Cup 99 / NSL-KDD, oder manuell gelabelte Ausschnitte => Basic Precision/Recall.  

---

## 7. Evaluationskonzept

1. **Anomalie-Bewertung**:
   - Unsupervised Metriken (z. B. Reconstruction error distribution).  
   - Clustering-Qualität (Silhouette Score) bei Session-Embeddings.  

2. **Fusion-Formel**: 
   - Variation der Exponenten \(\alpha, \beta, \gamma\).  
   - Test, ob synergy-Ansatz signifikant besser hochgradige Ketten markiert als lineare Summe.

3. **Ressourcen**: 
   - \(\#\text{Container}, \mathrm{CPU/Mem}\) pro Tag, RL-Policy vs. heuristische Policy.

4. **Versionierung**:
   - Manuelle Stichproben => Ab wieviel \(\Delta\) distanz entsteht eine neue Bot-Familie v2 vs. v3?  
   - Metrik: Frequenz unnötiger Version Splits vs. verpasster wirklicher Changes.

---

## 8. Stufenweise Realisierung

1. **Line-level AE** + simpler Container-Spawn (Score>Schwelle => Start).  
2. **Session-Modul** + synergy-Fusionsformel \((\dots)^\alpha (\dots)^\beta (\dots)^\gamma -1\).  
3. **Optionale GNN** im HPC- oder Batchmodus für top-verdächtige Sessions.  
4. **RL-Integration** → Ablösen heuristischer Container-Starts, Overriding durch Analysten.  
5. **Versionierung** und Bulk Steps → Kettenvisualisierung iterativ verfeinert.

---

## 9. Schlussbemerkung

Dieses Konzept nutzt die **BSI-Honeypot-Daten** als primären Treiber und **T-Pot-Logs** als zusätzlichen Motor, um z. B. Kommandosequenzen für Session-Modelle zu trainieren oder Modelle zu validieren. Dabei behält man die Komplexität im Griff, indem:

- (a) nur bei hohen Scores tiefergehende Analysen (Session, GNN) erfolgen,  
- (b) Container-Spawning streng limitiert ist,  
- (c) die neue synergy-Fusionsformel exponentiell ansteigt, sobald mehrere Ebenen gleichzeitig auffällig sind,  
- (d) ein schrittweises Rollout (z. B. MVP → GNN → RL → Versionierung) erfolgt.

Mit diesem Ansatz gelingt eine **fortschrittliche, aber praktikable** ML-basierte All-Port-Honeypot-Lösung, die durch die synergy-basierte Anomaliescore-Berechnung mehrdimensionale Abweichungen erkennt und zugleich ressourcenschonend in reale BSI-Prozesse integrierbar bleibt.
