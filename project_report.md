# End-of-Degree project report

- **Project name**: Renpath
- **Author**: Léo CHARTIER
- **School**: ALGOSUP
- **Degree**: Chef de projet en solutions logicielles pour l'internet des objets (IoT) - RNCP level 6 ([Website in French](https://www.francecompetences.fr/recherche/rncp/37046/))
- **Exam date**: September 11, 2024

## Abstract

<!-- TODO -->

## Table of Contents

- [Abstract](#abstract)
- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
  - [Visual Novel definition](#visual-novel-definition)
  - [Context](#context)
  - [Scope and objectives](#scope-and-objectives)
- [Specifications](#specifications)
  - [Current requirements](#current-requirements)
  - [Future requirements](#future-requirements)
  - [Technical requirements](#technical-requirements)
  - [Development guidelines](#development-guidelines)
  - [Testing and success criteria](#testing-and-success-criteria)
  - [Algorithms](#algorithms)

## Introduction

As part of the curiculum of ALGOSUP and its degree, the students must work on a personal software creation project. This official degree is split in two required levels: a level 6 and a level 7 (obtainable one year after the previous one).

### Visual Novel definition

First of all, I need to define a concept which is at the heart of this project: Visual Novels.

Visual Novels (VN for short) are a type of video game that could be described as "books for computers." Most of the time, they are accompanied by images, music, and sounds, as well as videos and interactive buttons.

What differentiates them the most from our usual books is that they are interactive. The player is provided with choices that change the story, offering a variety of scenarios.

### Context

The idea for this project came to me during my free time.

As I was playing a Visual Novel game, I was unexpectedly brought back to an earlier point in the game. I investigated the reason by looking at the code and found the issue. The reason was that the developer used a `jump` (`goto` in some languages) instead of a `call`.

This kind of mistake can easily be made in this field of video games as creators of Visual Novels are more knowledgable about arts than programming.

This led me to the idea for this project: Creating a tool that would assist both developers and players by analyzing the different paths and more.

### Scope and objectives

The end product of this project is a tool targeting both creators and users of Visual Novel games.

For this first version, the tool simply reads the content of a game and creates a visual representation of all the choices that can be made and the paths that links them.

Some additional functionalities are already in place to prepare for the next step of this project, which could not have been finished in time for this first version. The original target for this version was to also have basic analysis of the recovered data by finding the path to take to go to a specific point in the game.

## Specifications

### Current requirements

- The tool should be compatible with any version of Renpy.
  - *Many modern games still use old versions of the game engine.*
- The tool must be able to hook itself to the game without modifing the original code.
- The tool must be easily removed from the game.
- The tool should start when the game is launched and prevent it from starting normally.
- The tool must, for now, automatically create a file with a graph of the different paths.
- The file should be written using the DOT language and be placed at the root of the game.

### Future requirements

- The tool should allow the user to select a destination label from the game.
- The tool should be able to find the path and its requirements to reach this target.
- The tool should be able to write a file with the choices required to reach this target.
- The tool should allow the user to select a save file to change the starting point.

### Technical requirements

To ensure full compatibility between our tool and any game, the code will be run directly by the game engine as if it was part of the game.
For this reason, the code should be written either in Python, or in the Renpy scripting language for the interfaces.
Despite not being planned, another programming language could be used to speed up the analyses.

### Development guidelines

It is recommended to develop using Visual Studio Code along with the following extensions:
- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
- [SonalLint](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode)
- [Renpy language](https://marketplace.visualstudio.com/items?itemName=renpy.language-renpy)

Having Python installed is NOT required.

### Testing and success criteria

Since each game is different, no specific test environment has been set up. An example is provided [here](https://github.com/leo-chartier/renpath/tree/main/tests).
Each modification should be tested on multiple games to ensure full compatibility. You can find some for free on [itch.io](https://itch.io/games/free/tag-renpy).

For the graph creation, the success criteria are:
- The graph must correctly show all the labels and choices available.
- All the labels and choices must correctly be linked with their respective names and/or requirements.

A graph is a complex structure which can be rearanged without changing its meaning. This makes comparing two graphs in different format quite tricky and creating one by hand will likely lead to mistakes. For this reason, no automated testing has been done for this step.

For the walkthrough creation, the success criteria are:
- The output file must contain one choice per line.
- After selecting those choices in the game, the player must end up at the selected location.
- After selecting those choices in the game, the variables must contain the same content as outputed by the tool.

Automated testing will be written for this step as the choices always have the same names and are in the same order.

### Algorithms

<!-- TODO -->