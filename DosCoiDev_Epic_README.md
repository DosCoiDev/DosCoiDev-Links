# DosCoiDev

## Game Experience Orchestration Middleware

> **Observation JSON in.**  
> **Validated Director Command JSON out.**  
> **The engine remains in control.**

DosCoiDev is an experimental Unreal Engine middleware that orchestrates encounter-level gameplay without replacing existing AI systems.

Rather than making individual enemies smarter, DosCoiDev coordinates groups of AI through a safe, validated External Director layer.

---

# What Problem Does It Solve?

Modern games already have excellent local AI systems.

- Behavior Trees
- GOAP
- Navigation
- Animation
- Combat
- Cover systems

These systems control **individual AI**.

DosCoiDev explores a reusable middleware layer that coordinates the **encounter as a whole**.

Instead of replacing local AI, it provides high-level intent such as:

- Apply flank pressure
- Increase engagement distance
- Change encounter pacing
- Open a recovery window

---

# Core Principle

The project follows one simple rule.

> **The experience can change.**  
> **The engine remains in control.**

The External Director proposes.

The Validator verifies.

Unreal Engine owns execution.

---

# Architecture

```text
Unreal Engine
        │
Observation JSON
        │
External Director
(RuleSet / LLM)
        │
Validator
        │
Validated Director Command JSON
        │
Unreal Engine Execution
```

Individual enemies continue using native Unreal Engine AI.

DosCoiDev only selects encounter-level intent.

---

# Current Proof

Current prototype demonstrates:

- Baseline AI
- RuleSet Director
- LLM Director

running inside the same Unreal Engine encounter.

Current validated commands include:

- `flank_pressure_2`
- `shotgun_fallback`

Measured prototype latency:

| Path | Latency |
|------|---------|
| RuleSet | ~0.2 sec |
| LLM | ~8 sec |

---

# Safety First

DosCoiDev intentionally limits external control.

The current prototype does **not** allow:

- unrestricted pawn control
- teleportation
- arbitrary enemy spawning
- encounter reset
- bypassing Unreal Engine validation

Only validated commands are executable.

---

# Current Scope

The current demonstration uses a **one-versus-many FPS encounter** inspired by FEAR.

FPS is the **current proof domain**, not the final product boundary.

---

# Future Applications

Potential future applications include:

- PvE shooters
- Squad combat
- Horde modes
- AI teammates
- Strategy games
- RPG encounters
- Turn-based combat
- Multi-agent orchestration

---

# Why External?

DosCoiDev does not replace Unreal Engine AI.

Instead, it provides a reusable orchestration layer above existing game logic.

This separation keeps:

- gameplay implementation
- AI implementation
- encounter orchestration

cleanly decoupled.

---

# Roadmap

## Near-term

- Schema freeze
- Validator refinement
- Policy cleanup
- UE adapter cleanup
- Structured logging
- Latency optimization

## Proof Build

- Reusable Unreal Engine sample
- Public documentation
- Safe preset library
- Additional encounter scenarios

Planned preset families:

- Flank Pressure
- Recovery Window
- Range Response

## Future Investigation

- Lower-latency local LLM backends
- Multi-agent orchestration
- Behavior Tree / Blackboard adapters
- Investigation of integration points with NVIDIA ACE

---

# Current Status

Current maturity:

**Research Prototype / Proof Build**

This repository demonstrates a working middleware boundary for encounter orchestration.

It is **not** intended as a production-ready AI framework.

---

# Epic MegaGrants

DosCoiDev is being developed as an Unreal Engine middleware prototype exploring safe external encounter orchestration.

Requested Epic MegaGrants support will be used for:

- engineering time
- schema freeze
- reusable Unreal Engine sample
- documentation
- preset library expansion
- latency improvements

---

# License

License information will be added before public release.


# DosCoiDev ASCII Logo

> Game Experience Orchestration Middleware  
> Observation JSON in. Director Command JSON out. The engine remains in control.

## Pure ASCII

```text
                                      ###################
                                  ######                #####
                                ####       #########        ####
                              ###      ##################      ###
                            ###      ######################      ###
                           ##      #########################      ###
                          ##       ##########################       ##
                         ##       # ####    #######     ######       ##
                        ##        ####   ##    ##   #     ## #       ###
                        ##        # ##        ####       #####        ##
                        #####      ##########################      #####
                         #####     ##########################     #####
                       ###       ##   ####################   #        ##
                        ##     ####   ####################  #####     ##
                        ###  ######## ## ############## ## ########  ##
                         ## # #########  ##############  ## ######## ##
                          ## #########   #           ##  #############
                          # ##########  ################ ############
                          ########   ###    ##    ##    ###  ########
                          # ######  ####  # ##  # ## ## ##   ########
                           #######   ###    ##    ##    ##   ########
                                  ######    ##    ##    ######
                                  ###########################
                                   ######################## #
                                   ###########    ###########

            #########                  ########          #  ########
            ##     ### ######  ###### ###       #######  #  ##     ## ###### #    #
            ##     ##  ##   ## ###### ###       ##   ## ##  ##     ## ###### ##  ##
            #########  ##   ##  #####  #######  ##   ## ##  ## ###### #####   ####
             #######    #####  #####    #######  #####   #  #######    #####   ##

                                     ######## ###  ########
```
