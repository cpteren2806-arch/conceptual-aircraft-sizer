# Conceptual Aircraft Sizer

A browser-based tool for the **first step of aircraft design**: turning customer requirements into a first estimate of aircraft size.

You enter the mission (passengers, range, cruise speed and altitude) and a few technology assumptions (wing shape, drag, engine fuel consumption, runway lengths). The tool then estimates:

- **Maximum take-off weight (MTOW)**
- **Fuel weight** and **empty weight**
- **Wing area** and **wing span**
- **Engine thrust** per engine
- The **design point** on a constraint diagram

Every result updates instantly when you change an input, so you can see how the requirements affect each other.

**Live demo:** https://aktepeeren3-crypto.github.io/conceptual-aircraft-sizer/

---

## Why I built this

I built this while taking the course *Digitalisation in Aeronautics* (TUM / Munich Aerospace), which covered the aircraft design process. Instead of only describing the steps of preliminary design, I wanted to implement them and see how the numbers behave.

The main lesson: in aircraft design, nothing is independent. Adding passengers increases payload, which increases take-off weight, which needs more fuel and a bigger structure, which increases weight again. The tool makes this loop visible.

---

## Features

- **Three example missions**: regional jet (100 pax), narrow-body (150 pax), long-haul wide-body (290 pax)
- **Iterative weight solver**, with every iteration shown in a table
- **Mission fuel fractions**, segment by segment (take-off, climb, cruise, reserve, landing)
- **Constraint diagram** (T/W vs. W/S) with take-off, landing, cruise and one-engine-out climb requirements, the feasible region and the design point
- **Sensitivity check**: how MTOW changes with +10 passengers, +500 km range, 5% better fuel burn or 5% less drag
- **Comparison with real aircraft** (E190, A320neo, A321neo, 787-9, A350-900)
- Works on desktop and mobile, light and dark mode
- Single HTML file, no installation, no server

---

## How it works

The tool follows the classic **Class I sizing** method from aircraft design textbooks.

### 1. Weight equation

Take-off weight is split into four parts:

```
W0 = W_crew + W_payload + W_fuel + W_empty
```

Fuel and empty weight are expressed as fractions of W0, which gives:

```
W0 = (W_crew + W_payload) / (1 - Wf/W0 - We/W0)
```

The empty-weight fraction depends on W0 itself (statistical fit for jet transports, Raymer):

```
We/W0 = 0.97 * W0^(-0.06) * K      (W0 in kg, K = structure technology factor)
```

Because of this, the equation is solved by **iteration** until the guess and the result agree.

### 2. Fuel

The mission is divided into segments. Each segment has a weight fraction (weight at end / weight at start):

| Segment | Method |
|---|---|
| Take-off, climb, landing | Historical values (0.970, 0.985, 0.995) |
| Cruise | Breguet range equation: `exp(-R * C / (V * L/D))` |
| Reserve loiter | Endurance equation: `exp(-E * C / (L/D)max)` |

6% is added for reserves and trapped fuel.

### 3. Aerodynamics

A parabolic drag polar links wing geometry to efficiency:

```
(L/D)max = 0.5 * sqrt(pi * A * e / CD0)
```

Cruise uses `L/D = 0.866 * (L/D)max`, the best-range condition for a jet. This is how a longer, thinner wing (higher aspect ratio A) directly reduces fuel weight.

### 4. Constraint diagram

Each performance requirement gives a minimum thrust-to-weight ratio (T/W) for a given wing loading (W/S):

| Constraint | Method |
|---|---|
| Take-off field length | Loftin/Roskam FAR 25 correlation |
| Landing field length | Approach speed = 1.23 x stall speed |
| Cruise | Thrust = drag at cruise altitude, with thrust lapse |
| One-engine-out climb | FAR 25 second-segment climb gradient |

The design point uses the **highest wing loading** the landing limit allows (smallest wing), then the **lowest T/W** that satisfies all other constraints (smallest engines).

---

## Example results

| Mission | Tool MTOW | Real aircraft | Real MTOW |
|---|---|---|---|
| 100 pax, 3,700 km | 43.6 t | Embraer E190 | 51.8 t |
| 150 pax, 5,000 km | 65.0 t | Airbus A320neo | 79.0 t |
| 290 pax, 13,500 km | 233 t | Boeing 787-9 | 254 t |

For the wide-body case, the tool also predicts a wing span of 60.3 m (787-9: 60.1 m).

The differences are expected. A Class I estimate is typically within 10-20%, and the real aircraft fly slightly different missions (more seats, longer range).

---

## How to use it

**Online:** open the live demo link above.

**Locally:** download `index.html` and open it in any web browser.

1. Pick an example mission, or enter your own values on the left.
2. Read the key results at the top.
3. Look at the constraint diagram to see which requirement sets the engine size.
4. Use the sensitivity section to see which input matters most.

---

## Limitations

This is a first-pass estimate for learning and early trade studies. It does **not**:

- size the fuselage, tail or landing gear
- model compressibility drag at high Mach numbers
- account for detailed certification rules or engine-specific data

Results are approximate and should not be used for real aircraft design decisions.

---

## References

- D. P. Raymer, *Aircraft Design: A Conceptual Approach*, AIAA
- J. Roskam, *Airplane Design, Part I: Preliminary Sizing of Airplanes*
- L. K. Loftin, *Subsonic Aircraft: Evolution and the Matching of Size to Performance*, NASA RP-1060

---

## Tech

Plain HTML, CSS and JavaScript in a single file. No frameworks, no build step. Charts are drawn as SVG.

## Author

Eray Aktepe
