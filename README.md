# Conceptual Aircraft Sizer

A simple web tool that makes a **first estimate of an airliner's size** from a few requirements.

You enter the number of passengers, range, cruise speed, runway length, wing shape and engine fuel consumption. The tool estimates:

- the **take-off weight**
- how much **fuel** is needed
- the **wing area and span**
- the **thrust per engine**

**Live demo:** https://aktepeeren3-crypto.github.io/conceptual-aircraft-sizer/

## Why I made this

I made this as a student project while taking the course *Digitalisation in Aeronautics* (TUM). The course explained the first steps of aircraft design. I wanted to turn those steps into a working tool and see how the numbers change.

## How it works

The tool follows four steps:

1. **Mission requirements:** passengers, range, speed and runway length.
2. **Weight estimate:** take-off weight = empty aircraft + fuel + passengers + crew. A heavier aircraft needs a heavier structure, so the calculation is repeated until the result stops changing.
3. **Wing and engine size:** a constraint diagram shows the take-off, cruise, engine-out climb and landing requirements. The design point is the smallest wing and engines that meet all of them.
4. **Comparison:** the result is compared with real aircraft such as the A320neo and the Boeing 787-9.

Fuel is calculated with the **Breguet range equation**. Wing efficiency comes from a simple drag model, where a longer, thinner wing (higher aspect ratio) gives a better lift-to-drag ratio.

## Example results

| Example | My tool | Real aircraft |
|---|---|---|
| 100 passengers, 3,700 km | 40.8 t | Embraer E190: 51.8 t |
| 150 passengers, 5,000 km | 63.8 t | Airbus A320neo: 79.0 t |
| 290 passengers, 13,500 km | 258.8 t, 60.7 m span | Boeing 787-9: 254 t, 60.1 m span |

The results are close to the real aircraft but not exact. A simple method like this is usually within about 10-20%, and the real aircraft fly slightly different missions.

## What I learned

- In aircraft design, everything is connected. More passengers means more weight, more fuel and a bigger wing.
- For long-haul aircraft, fuel is almost half of the take-off weight.
- The take-off runway length usually decides how big the engines need to be.

## Limitations

This is only a first estimate for learning. It does not design the fuselage, tail or landing gear, and it uses fixed assumptions (100 kg per passenger, 2 engines, cruise at 11,000 m).

## How to use

Open the live demo, or download `index.html` and open it in any browser. No installation needed.

## Built with

HTML, CSS and JavaScript in a single file.

## References

- D. P. Raymer, *Aircraft Design: A Conceptual Approach*
- J. Roskam, *Airplane Design, Part I*

## Author

Eren Aktepe
