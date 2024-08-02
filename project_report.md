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
