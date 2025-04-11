Perfekt – hier ist dein vollständiges, hochdetailliertes Paper-Konzept auf Deutsch, **struktur- und publikationstauglich**, mit allem, was dazugehört: von Motivation und Architekturanalyse über mathematische Modelle bis hin zu Risiken, Evaluation und Einordnung in den aktuellen Stand der Forschung.

---

# **Adaptives Multi-Layer Honeynet-Framework mit nichtlinearer Score-Fusion, RL-gesteuerter Emulation und Angriffsketten-Versionierung**

---

## **Abstract**

Dieses Paper stellt ein modular aufgebautes, KI-gestütztes Honeypot-Framework vor, das auf der MADCAT-Infrastruktur des BSI aufsetzt. Während MADCAT durch All-Port-DNS-Rewriting maximale Breitenabdeckung erzielt, beschränkt sich die aktuelle Auswertung auf isolierte First-Flight-Daten. Das vorgestellte System erweitert dieses Setup um eine dynamische Tiefe: Nur bei hinreichender Anomalie in der Erstverbindung werden containerisierte Dienste über Kubernetes bereitgestellt, deren Session-Daten dann mithilfe eines mehrschichtigen ML-Pipelines ausgewertet werden.

Kernelemente sind ein rekonstruktionsbasierter Autoencoder, ein Transformer-LSTM-Modul zur Sessionmodellierung, ein optionales GNN für cross-session Analyse sowie eine nichtlineare Fusionsformel zur Synergieerkennung. Ziel ist die automatisierte Erkennung typischer Angreiferverhalten (Bots, Scanner, Crawler), deren Versionierung auf Basis von Verhaltensabweichungen sowie die ressourcenschonende Steuerung von Emulationskapazitäten durch einen RL-gesteuerten Container-Orchestrator. Ein Expert-in-the-Loop-Modul erlaubt zusätzlich manuelle Rückmeldung zur Feinjustierung.

---

## **1. Einleitung**

Automatisierte Angreiferwerkzeuge wie SSH-Brute-Forcer, Port-Scanner oder Credential-Stuffer dominieren weite Teile des aktuellen Internet-Traffics. Ihre Modularität, Variabilität und ständige Weiterentwicklung erschweren die präzise Kategorisierung, Nachverfolgung und Bekämpfung. Zwar existieren Standards wie MITRE ATT&CK zur semantischen Beschreibung menschlicher Angriffssequenzen, doch fehlen datengetriebene Verfahren zur **Verhaltensversionierung** automatisierter Tools.

Honeypots wie MADCAT zeichnen vollständige Port-Interaktionen auf, liefern jedoch pro Verbindung nur ein First-Flight-Event. Dies genügt nicht zur Modellierung von Angriffsketten, bietet aber eine große Chance: Durch gezielte, selektive Emulation kann Tiefe genau dort erzeugt werden, wo statistisch relevante Abweichungen auftreten – und nur dort.

---

## **2. Zielsetzung & Beitrag**

Das System zielt darauf ab, Honeypot-Infrastrukturen durch folgende Elemente substanziell zu erweitern:

1. **Adaptive Containeremulation**: Dienste werden nur bei Anomalieverdacht auf Portebene gestartet.
2. **Mehrstufige Anomalieanalyse**: Kombination aus AE (Line-Level), Transformer-LSTM (Session), GNN (Graph).
3. **Nichtlineare Anomaliefusion**: Synergetische Verstärkung paralleler Abweichungen.
4. **Verhaltensbasierte Angriffsketten-Versionierung**: automatische Zuordnung zu Bot-/Tool-Familien.
5. **RL-gesteuerte Orchestrierung**: Bewertung des Informationsgewinns vs. Ressourceneinsatz.
6. **Feedback-Integration**: Analystenkommentare beeinflussen Versionsgrenzen und Policy-Anpassung.

---

## **3. Systemarchitektur & Datenfluss**

### **3.1 First-Flight Phase (Pre-Spawn)**

**Eingabe:** Unstrukturierte Logzeile aus MADCAT (z. B. `event.original`)

**Verarbeitung:**

- **AE-Modul:** Rekonstruktionsfehler → `score_line`
- **Heuristik:** Port-Rarität, IP-Reputation, Wiederholungsdichte
- **Protokollklassifikation:** Erkennung von SSH, HTTP, FTP etc.
- **RL-Agent:** Entscheidet Container-Start auf Basis von:
  - `score_line`, Heuristik, Protokoll
  - Aktive Pods, CPU-Auslastung
  - Historischer Reward

→ Bei Entscheidung: Kubernetes-Spawn eines passenden Containers (mit L7-Routing durch Proxy)

---

### **3.2 Session Phase (Post-Spawn)**

**Nur aktiv bei Containerbereitstellung.**

**Verarbeitung der Pod-Logs:**

- **AE (erneut)** auf neue Logzeilen (aus Interaktion)
- **Session Aggregation** via IP, Zeitfenster, Protokoll
- **Transformer-LSTM Modellierung**
- **Graph-Konstruktion:** Session-Graph basierend auf Ähnlichkeit & Zeit
- **GNN-Analyse:** z. B. mit GraphSAGE
- **Nichtlineare Fusionsformel**:

$$
\mathrm{score}_{\mathrm{fusion}}(s) = \left[(\mathrm{score}_{\text{line}} + 1)^\alpha \cdot (\mathrm{score}_{\text{session}} + 1)^\beta \cdot (\mathrm{score}_{\text{graph}} + 1)^\gamma \right] - 1
$$

---

### **3.3 Angriffsketten-Versionierung**

- **Clusterung nach Session-Embedding**
- **Distanzprüfung zu vorhandenen Verhaltenszentren \( f_{\text{fam}} \)**
  - Bei Überschreitung von Schwelle \( \Delta \): neue Version
- **Bulk-Knoten:** bei > n gleichen Requests (z. B. 500x Passwortversuch)
- **Speicherung & Visualisierung:** als Graphstruktur mit Metainformationen
- **Feedback:** Experten können Familien markieren, splitten oder zusammenführen

---

### **3.4 Reward-Funktion des RL-Agents**

$$
R(s_j) = \lambda_1 \cdot \frac{D_{\text{neu}}}{D_{\text{gesamt}}} - \lambda_2 \cdot \frac{T_{\text{runtime}}}{T_{\text{max}}}
$$

- \( D_{\text{neu}} \): Anzahl neuer Command-/Payload-Typen
- \( T_{\text{runtime}} \): Containerlaufzeit
- \( \lambda_1, \lambda_2 \): Gewichtungen

---

## **4. Vergleich mit Stand der Technik**

| System/Ansatz | Abdeckung | Tiefe | Dynamik | KI-Integration | Versionierung |
|---------------|-----------|-------|---------|----------------|----------------|
| T-Pot         | hoch      | statisch | nein    | nein           | nein           |
| MADCAT        | maximal (All-Port) | keine | nein | nein | nein |
| DeepLog       | strukturiert, Einzel-Session | LSTM | nein | teilw. | nein |
| AutoLog       | AE-only | nein | nein | ja | nein |
| GraphSAGE     | Graphverhalten | ja | ja | ja | nein |
| **Dieses System** | All-Port + Session | tief & adaptiv | RL/Heuristik | voll integriert | **ja** |

---

## **5. Relevanz zu MITRE ATT&CK**

- **Anwendbarkeit auf APT-Ketten** bei komplexen Szenarien gegeben
- **Begrenzte Nutzbarkeit** für automatische Bots
- **Lösungsansatz hier:** datengetriebenes Verhaltensmodell → optionales Mapping auf TTPs

---

## **6. Evaluation & Validierung**

### **Datensätze:**

- MADCAT Logs (500 GB unlabeled)
- T-Pot (Cowrie-Sessiondaten zur Vorinitialisierung)
- Optional: KDD99 / NSL-KDD für Metrik-Tests

### **Metriken:**

| Komponente         | Bewertungsmaß                         |
|--------------------|----------------------------------------|
| AE                 | ROC, Precision, Recall                 |
| LSTM               | Cluster-Kohärenz, Silhouette-Score     |
| GNN                | Modularity, Anomaly Density Ratio      |
| Fusion             | Vergleich linear / max / nichtlinear   |
| RL                 | Reward-Stabilität, Effizienz / CPU     |
| Versionierung      | Recall vs. Drift vs. Oversplitting     |

---

## **7. Risiken & Limitationen**

| Risiko                        | Maßnahme                             |
|-------------------------------|--------------------------------------|
| RL überfitzt auf triviale Pattern | Replay-Buffer + heuristische Policy |
| GNN zu rechenintensiv         | Batchmodus auf Top-N Sessions        |
| Label-Mangel                  | Expertenfeedback + Semi-Supervised   |
| Pod-Überlastung               | Max-Pods + RL-Kostenfunktion         |
| False-Positives               | Fusionsformel dämpft Einzelsignale   |

---

## **8. Fazit & Ausblick**

Dieses Konzept verbindet die **kompromisslose Sichtbarkeit** von MADCAT mit einer **tiefgreifenden, selektiven Analysepipeline**, die nur dort Interaktion initiiert, wo es sich lohnt. Der modulare Aufbau erlaubt schrittweise Erweiterung:

- **Start:** AE + einfache Containersteuerung  
- **Phase 2:** Sessionmodell + Fusion  
- **Phase 3:** GNN + Versionierung + Expertenfeedback

Langfristig ergibt sich so ein verhaltensbasiertes, versionierbares Repositorium für automatisierte Angreifer – ein zentraler Baustein für die KI-gestützte Verteidigung.

---

Wenn du willst, bereite ich diesen Text direkt für Konferenzeinreichungen (LaTeX), als PDF oder DOCX auf – mit visualisierbarer Architektur und ggf. Beispieldatenflussdiagramm. Sag einfach Bescheid ✅
