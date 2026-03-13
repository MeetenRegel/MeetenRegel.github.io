---
layout: post
title: "Hoe werkt een PID-regelaar?"
date: 2026-03-13
categories: regeltechniek
---

Een PID-regelaar is een van de meest gebruikte regelaars in de industrie. Van temperatuurregeling in een oven tot het handhaven van druk in een leiding — de PID-regelaar zit bijna overal. Maar hoe werkt hij eigenlijk?

## Het idee achter regelen

Stel: je wilt een tanktemperatuur op 80°C houden. Je hebt een verwarmer die je kunt aansturen en een temperatuursensor die meet wat de werkelijke temperatuur is. Het verschil tussen wat je wilt (de **setpoint**) en wat je meet (de **proceswaarde**) noem je de **fout** (of error).

Een simpele regelaar zou zeggen: "Is de fout groot? Zet de verwarmer dan harder." Dat is het basisidee. De PID-regelaar doet dit slimmer door de fout op drie manieren te bekijken.

## De drie termen

### P — Proportioneel

De P-term reageert direct op de huidige fout. Hoe groter de fout, hoe harder de regelaar ingrijpt. De versterkingsfactor heet **Kp**.

**Voordeel:** Snelle reactie.  
**Nadeel:** Alleen een P-regelaar heeft bijna altijd een blijvende afwijking (steady-state error). De regelaar ingrijpt minder hard naarmate de fout kleiner wordt, maar stopt nooit helemaal met afwijken.

### I — Integraal

De I-term kijkt naar de **optelsom van alle fouten in de tijd**. Als er al lang een kleine afwijking is, bouwt de integraalterm op en duwt het proces naar de setpoint toe. De versterkingsfactor heet **Ki**.

**Voordeel:** Elimineert de blijvende afwijking.  
**Nadeel:** Kan leiden tot overshoot en oscillatie als Ki te hoog is. Ook kan de integraalterm "oprollen" (integrator windup) als de actuator verzadigd is.

### D — Differentiaal

De D-term reageert op hoe **snel** de fout verandert. Als de fout snel kleiner wordt, remt de D-term alvast af om overshoot te voorkomen. De versterkingsfactor heet **Kd**.

**Voordeel:** Remt overshoot, maakt de regeling stabieler.  
**Nadeel:** Gevoelig voor ruis op het meetsignaal. Wordt in de praktijk vaak beperkt of weggefilterd.

## De formule

De uitgang van een PID-regelaar is:

```
u(t) = Kp * e(t) + Ki * ∫e(t)dt + Kd * de(t)/dt
```

Waarbij:
- `u(t)` = stuuruitgang (bijv. positie van een klep, vermogen van een verwarmer)
- `e(t)` = fout op tijdstip t (setpoint − proceswaarde)
- `Kp`, `Ki`, `Kd` = de instelparameters

## Instellen van een PID (tuning)

Het instellen van de drie parameters heet **tuning**. Er zijn meerdere methodes:

- **Trial and error** — Kp verhogen tot het systeem reageert, Ki toevoegen om de afwijking weg te werken, Kd toevoegen als er te veel overshoot is.
- **Ziegler-Nichols methode** — Een klassieke methode waarbij je het systeem op de grens van oscillatie brengt en daaruit de parameters berekent.
- **Auto-tuning** — Veel moderne regelaars hebben een ingebouwde auto-tune functie.

## Praktische tips

- Zet **Ki op nul** als je snel wilt testen — een puur P-regelaar is makkelijker te begrijpen.
- Wees voorzichtig met **Kd** bij ruizige meetsignalen. Voeg een filter toe of gebruik helemaal geen D-term.
- **Integrator windup** is een veelvoorkomend probleem. Begrens de integraalterm als de actuator verzadigd raakt.
- In de praktijk wordt de D-term vaak berekend op de **proceswaarde** in plaats van de fout, om grote uitschieters bij setpointwijzigingen te voorkomen.

## Samenvatting

| Term | Reageert op | Effect | Risico |
|------|-------------|--------|--------|
| P | Huidige fout | Snelle reactie | Blijvende afwijking |
| I | Opgebouwde fout | Elimineert afwijking | Overshoot, windup |
| D | Snelheid van de fout | Remt overshoot | Ruisgevoelig |

Een PID-regelaar is eenvoudig in concept maar vraagt inzicht om goed in te stellen. In een volgende post ga ik dieper in op concrete tuningmethodes.
