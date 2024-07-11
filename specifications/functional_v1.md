# Functional specifications - Renpath

`Functional version 1 - "Pathfinder"`

- [Context](#context)
  - [What are Visual Novels?](#what-are-visual-novels)
- [Scope](#scope)
- [Competitors](#competitors)
- [Use cases](#use-cases)
- [Requirements](#requirements)
- [Glossary](#glossary)

## Context

The modern world is ruled by two types of distractions: Social media and video games. In 2023, the sales of video games generated $184 billion ([source](https://newzoo.com/resources/blog/games-market-estimates-and-forecasts-2023)).

While the development of video games is always growing, the expectations of the players are increasing even faster. Many tools exist in this domain to ease the work of the developers of many fields. \
One category of games that lacks tools is the one of Visual Novel games. This market is important as the simplicity of these games makes them easily portable to multiple platforms (PC, Mobile, Web, ...).

Creating such a tool will be my End-of-Degree project.

> [!NOTE]
> The name "Renpath" reflects the state of the project for this first version. It will likely change in the future as restrictions on the scope decrease.

### What are Visual Novels?

Visual Novels (VN for short) are a type of video game that could be described as "books for computers." They are most of the time accompanied by images, music, and sounds, and sometimes videos and interactive buttons. What differentiates them the most is that they are interactive. The player is provided with choices that change the story, offering a variety of scenarios.

## Scope

The goal of this project is to provide a variety of tools for developers and players of Visual Novels.
The software will come as an extension that can be added to any existing games or in-development ones, displaying a custom interface when the game is launched.

For the first version of the program, the software will limit itself to games created with the Renpy game engine.
The initial features are as follows:
- A pathfinder to search for the most optimal choices that the user should make
- A minimalistic interface on startup containing
  - a button to launch the game as usual,
  - a button to start the search for the optimal path,
  - a dropdown menu to select the target destination.

Deliverables:
- A repository on GitHub containing the different specifications artifacts
- Both the source code and an extension that is ready to be installed
- A presentation of the project

Timeline:
| Event                      | Date               |
| -------------------------- | ------------------ |
| Start of the project       | April 17, 2023     |
| Working prototype          | N/A                |
| All deliverables           | August 27, 2024    |
| First version presentation | September 11, 2024 |

## Competitors

Many tools exist to help with other parts (localization, quest manager, extractors, etc.) of Visual Novels creation, yet I only found one for path analysis.

https://github.com/EwenQuim/renpy-graphviz
Pros:
- Existing for more than three years
- Popular (123 stars)
- Heavy customizability of the path
Cons:
- Only draws a graph but does not do further analysis
- Puts limitations on how to code the game
- The whole software is done from scratch, leaving out some features

## Use cases

## Requirements

## Glossary

Visual Novel: See [What are Visual Novels?](#what-are-visual-novels).
GitHub: Online platform to store and share code inside of "repositories" with version control and bug tracking, among other features.
Source code: The original, human-readable code for software before it has been transformed to be usable by a computer.