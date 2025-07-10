
# Adaptive Container-Orchestrierung mit Reinforcement Learning (RL)

## Motivation

Statische Honeypot-Ansätze wie T-Pot bieten dauerhaft eine Vielzahl von Services auf festen Ports an – unabhängig davon, ob ein Angreifer tatsächlich mit ihnen interagieren möchte. Diese Strategie erzeugt zwar große Mengen an Daten, führt jedoch zu:

- hoher Ressourcenlast durch permanent laufende Container
- reduzierter Tarnung (alle Ports offen = Honeypot-Indikator)
- vielen irrelevanten Sessions (z. B. durch einfache Scanner oder Bots)

Um diese Nachteile zu überwinden, setzt unser System auf **adaptive Containerbereitstellung mithilfe von Reinforcement Learning (RL)**. Ziel ist es, nur dann einen Emulationscontainer zu starten, wenn **First-Flight-Daten** einer Quell-IP ein hohes Potenzial für wertvolle Angreiferinteraktionen aufweisen.

---

## ✅ Vorteile des RL-basierten Deployments gegenüber statischen Systemen

| Kriterium                     | Statisches System (z. B. T-Pot) | RL-basiertes System              |
|------------------------------|----------------------------------|----------------------------------|
| Ressourceneffizienz          | Gering                           | Hoch                             |
| Tarnung                      | Gering (alle Ports offen)        | Hoch (on-demand-Emulation)       |
| Datenqualität                | Viel Bot-Rauschen                | Fokus auf wertvolle Interaktionen |
| Reaktion auf Angreiferverhalten | Keine                         | Host-spezifisch, lernfähig       |
| Skalierbarkeit               | Begrenzt durch Last              | Dynamisch skalierbar             |

---

## Entscheidungsgrundlage: First-Flight-Daten

Der RL-Agent trifft seine Entscheidungen auf Basis sogenannter **First-Flight-Daten** – also aller Merkmale, die direkt zu Beginn einer Netzwerkverbindung verfügbar sind, z. B.:

- Zielport, Protokoll, TCP-Flags
- Payload-Länge, Entropie, Keywords (z. B. „GET“, „root“)
- Zeit zwischen Paketen, Sessiondauer
- Herkunfts-IP, ASN, geographische Lage
- Häufigkeit und Art der Sessions in den letzten Minuten

Diese Daten werden zu einem Feature-Vektor verarbeitet und bilden den **Zustand (state)** für das RL-Modell.

---

##  Reinforcement Learning Aufbau (MDP)

### Markov Decision Process (MDP)

- **Agent**: Das Decision-Modul zur Containerwahl
- **Environment**: Das Honeypot-System mit dynamischer Orchestrierung
- **State (𝑠ₜ)**: Feature-Vektor der First-Flight-Daten
- **Action (𝑎ₜ)**: Auswahl eines Containers oder Verzicht (`Ignore`)
- **Reward (𝑟ₜ)**: Bewertung auf Basis von Session-Qualität und Ressourcenverbrauch
- **Policy (π)**: Funktion zur Entscheidung (z. B. Deep Neural Network)

### Möglicher Aktionsraum

```
Actions = {
  Deploy_HTTP,
  Deploy_SSH,
  Deploy_SMB,
  Deploy_Telnet,
  Deploy_MySQL,
  Ignore
}
```
It might be better to make the decision of the service dependent on other methods then the RL:
```
Actions = {
  Deploy,
  Ignore
}
```

---

## 💡 Belohnungsfunktion

Die Belohnung ist so definiert, dass sie gute Entscheidungen fördert, die zu **neuen, tiefen Interaktionen** führen – ohne Ressourcen zu verschwenden:

```math
Reward = (Informationsgewinn / Containerkosten) - Redundanzstrafe
```

**Komponenten:**
- Informationsgewinn: neue Payloads, Authentifizierungsversuche, Shell-Zugriffe etc.
- Containerkosten: CPU-Zeit, RAM, Laufzeit
- Redundanzstrafe: wenn Angreifer bereits bekannt / typisches Botverhalten

---

## 🧠 Modellwahl: Q-Learning vs. Deep RL

Für einfache Proof-of-Concepts kann tabellarisches Q-Learning verwendet werden.  
Für realistische Anwendungen mit kontinuierlichen, hochdimensionalen Zuständen wird **Deep Reinforcement Learning** empfohlen (z. B. DQN oder PPO).

---

## 🚀 Ergebnis

Mit diesem Ansatz entsteht ein **lernfähiges, ressourcenschonendes und realistisches Honeypot-System**, das:

- gezielt eskalierende oder zielgerichtete Angreifer erkennt
- adaptive Emulation bietet (→ schwerer zu enttarnen)
- Container dynamisch auf Nachfrage startet
- bekannte Bots ignoriert und neue Angriffsformen priorisiert

---
