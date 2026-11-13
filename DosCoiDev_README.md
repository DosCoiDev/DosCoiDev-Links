# DosCoiDev

## Game Experience Orchestration Middleware

> **Observation JSON in.**  
> **Validated Director Command JSON out.**  
> **The engine remains in control.**

DosCoiDev is an experimental Unreal Engine middleware that orchestrates encounter-level gameplay without replacing existing AI systems.

Rather than making individual enemies smarter, DosCoiDev coordinates groups of AI through a safe, validated External Director layer.

## 日本語概要

DosCoiDevは、Unreal Engineの既存AIの上に、戦闘シーン全体の演出を指揮するExternal Directorを追加する研究試作です。ゲーム状態を受け取ったDirectorが演出コマンドを提案し、Validatorがスキーマやゲーム内の制約に照らして検証します。移動・戦闘などの実行はUnreal Engineが担います。実装には生成AIの支援を活用し、テスト条件と合否基準は開発者自身が定めて動作を検証しました。

NVIDIA ACEの経路テストでは、閉鎖された左側ルートと直前の行動計画を同時に渡すと、ACEの出力が現在の経路制約を満たしたのは1/10件でした。DosCoiDevの検証・修復後は10/10件が制約を満たしました。

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
(RuleSet / local LLM / cloud LLM)
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

running inside the same Unreal Engine encounter. The Director also supports a local Ollama backend.

Current validated commands include:

- `flank_pressure_2`
- `shotgun_fallback`

## Representative response times

| Prototype path | Observed response time |
| --- | ---: |
| RuleSet Director | ~0.2 s |
| Local LLM Director (Ollama / Qwen3:8B) | ~2 s |
| Cloud LLM Director (earlier API, subprocess and file-polling path) | ~8 s |

These observations come from separate prototype runs. Each value describes its own setup.

---

# Validation and Benchmarks

## NVIDIA ACE route-conflict test

Each test provided the current route closure. Trials with plan history also provided a previous plan that still assigned the blocked route. A trial passed if the resulting plan avoided that route.

| Test condition | ACE output met the route constraint | After DosCoiDev validation and repair |
| --- | ---: | ---: |
| Left route blocked, with plan history | 1/10 | 10/10 |
| Right route blocked, with plan history | 0/10 | 10/10 |
| Left route blocked, without plan history | 10/10 | 10/10 |
| Right route blocked, without plan history | 10/10 | 10/10 |

Each row represents ten trials in the prototype integration.

## Battlefield 64 (BF64) virtual benchmark

BF64 evaluates squad-level plans as JSON. It models eight squads of eight soldiers and checks constraints such as objective coverage, shared-resource assignments and blocked routes.

| Scenario | Structurally valid plans | Observed result |
| --- | ---: | --- |
| Baseline battlefield | 10/10 | Plan churn 0.125 (about one squad's assignment changed) |
| Shared-resource contention | 10/10 | Plan churn 0.625 (about five squads' assignments changed) |
| Local repair and policy checks | 10/10 | An unnecessary artillery assignment remained in 10/10 trials |

Plan churn tracks how much the squad assignments changed. Structural validity and scenario policy are evaluated separately.

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

- Further latency measurement and optimization
- Multi-agent orchestration
- Behavior Tree / Blackboard adapters
- Broader evaluation of NVIDIA ACE integration

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

# Repository Contents

This repository contains the project documentation and demo links. The prototype source code is currently private.

## Development Process

The developer used generative AI to assist implementation and defined the test conditions and pass criteria used to verify the prototype.


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
