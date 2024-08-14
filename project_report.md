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
    - [Extraction/Conversion](#extractionconversion)
    - [Simplification](#simplification)
    - [Graph generation](#graph-generation)
    - [Analysis](#analysis)

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
- The graph file should be written using the DOT language and be placed at the root of the game.

Here is what a graph could look like:

![Example of graph](images/example_full.png)

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

#### Extraction/Conversion

Before being able to do anything, we must first extract the data from the game.

As mentioned earlier, I will leverage the power of the game engine for this task. Since it provides an easy access to the content of the game, doing so ensures the integrity of the data. This would not necessarily be true if I was to read the data manually from the files, and would take longer to code.

There are a few data types provided by the engine that will interest us in particular. Those data types represent nodes that make up the path that can be taken, one for each statement in the script.

Here is a list of those that are common with other languages and some of their attributes:
- `Label`: Represents a place in the script that allows it to be broken down for easier access.
  - `name`: A unique identifier that represents this label.
  - `block`: A list of nodes in the section of code refered by the label.
- `Jump`: Moves the execution of the script somewhere else.
  - `target`: Name of the label to go to. 
- `Call`: Temporarily transfers the execution of the script elsewhere.
  - `label`: Name of the label to go to.
- `Return`: After a `Call`, makes the execution of the script go back where it came from.
- `If`: Selects one of multiple possibilities depensing on some values.
  - `entries`: A list of pairs of conditions and blocks.
    - The block is a list of nodes from the statements to be run.
    - The condition defines whether this block should be run or not.

Some others are specific to the Renpy scripting language:
- `Menu`: Gives the player a choice to make and diverts the execution of the script accordingly.
- `Python`: Executes some arbitrary code in the Python language.
  - `code`: The code to be executed.
- `UserStatement`: Among other things, specify when to show or hide a screen. See the screen section more more details. <!-- TODO -->
  - `get_name()`: A function that returns "show screen", "hide screen" or "call screen".

One of those nodes also has a `next` attributes which itself a node. It represents the statement following the current one and is used to progress the story.

A `Node` wrapper will be created to hold more data, including:
- `origin`: The underlying Renpy node.
- `parents` and `children`: Two lists of `Edges` that connect to this node.
- `screens`: A list of screens visible when the game is running this statement.
- `callers`: A list of previous `Call`s to know where to return to later on.

The main reason for the wrapper's existance is that the Renpy nodes do not provide a direct way to their predecesors which will be important as described further in this document.

A `Graph` and `Edge` classes will also be written for completeness and make things easier to use.

The conversion of the data into the wrappers will take place using a Depth First Search (DFS) algorithm. The start of the graph is always the label named "start". The search stops when all the nodes have an empty `next` value or when there is a `Return` with an empty call stack.

The reason we must use DFS is because we cannot know in advance which nodes exists other than the starting one.

#### Simplification

The second step is to remove all the data we obtained from the game that is unnecessary or redundant. This will simplify the work of the following steps and make them faster as well.

There should be three possible levels of simplification:
- Keep everything
- Standard
- Minimalist

The "Keep everything" level will actually affect the conversion process, where it will also include the types of statement that were not discussed previously. It will also skip the simplification phase. This mode can be useful for debugging for example but is not recommended at all since the sheer amount of nodes will clutter and slow the rest of the process.

The "Standard" mode will only keep the nodes mentioned in the previous section, without modification.

For the "Minimalist" level, nodes will be removed following some rules over an iterative process lasting until any further modifications can be done.

The first part of removing nodes goes as follows:
1. By default, only actuators and branches should be kept, i.e. `Menu`, `If` and `Python` nodes.
2. The root of the graph must always be kept.
3. A `Python` node must be removed if the code is one of the following:
   - `volume(...)`
   - `renpy.music`
   - `renpy.pause(x)`
   - `renpy.end_replay()`
   - `set_mode_adv()`
   - `set_mode_nvl()`
4. A node with no parents and no children should be deleted.
   - This happens often as the <!-- TODO: Find the correct word --> nodes are undistinguishable from ones generated for the game.

Once this is done, conditionals statements and, if the user desires so, menus, should be removed as well.
In order for this to work without affecting the flow of the graph, their parents and children will have to be merged together like so:
1. Merge the children edges that point to a same node. If there are conditions, they must be combined with an `or` operator.
2. If the product of number of parent and children edges is less than 25, you may remove the node. Otherwise, leave it as it would make further algorithms too slow for what it's worth.

The procedure to remove a node is not really straightforward as there could be some data loss.
1. Unregister the node from the graph along with its parent and children edges. Keep a copy of the old edges.
2. If it is a `Return` node, remove any duplicate children edges (this minimizes the possibilities of call stacks).
3. For each pair of parent and children edges, create a new edge with the same parent and child node. The conditions will be combined with an `and` operator.
   - If one of the two conditions is `True` or `False`, simplify the resulting condition.

#### Graph generation

The graph generation is probably the simplest requirement here.

As the actual display of the graph is very complex and out of scope, we will use a third party program do to this.
The selected one is [Graphviz](https://graphviz.org/) for its simplicity of use. Due to some performance issues with more complex graphs rising from the improvements on Renpath, an other tool such as [Mermaid](https://mermaid.js.org/), [NetworkX](https://networkx.org/) or [Gephi](https://gephi.org/).

While there exists a Python library for Graphviz, importing external assets in Renpy is extremely difficult and has been rejected. Instead, a raw text file will be created in the DOT format and it will be up to the user to run the software on that file.

Here is how the file for the eariler graph should look like:
```grapviz
digraph path {
	0 [label="Label: 'label start:' (line 1)"]
	1 [label="Menu: 'menu:' (line 4)"]
	2 [label="Python: '$ sword = True' (line 6)"]
	3 [label="Jump: 'jump fight' (line 11)"]
	4 [label="Jump: 'jump start' (line 15)"]
	5 [label="Jump: 'jump fight' (line 8)"]
	6 [label="Label: 'label fight:' (line 17)"]
	7 [label="If: 'if sword:' (line 20)"]
	8 [label="Jump: 'jump win' (line 21)"]
	9 [label="Jump: 'jump death' (line 23)"]
	10 [label="Label: 'label win:' (line 25)"]
	11 [label="Label: 'label death:' (line 30)"]
	12 [label="Return: 'return' (line 28)"]
	13 [label="Return: 'return' (line 33)"]

	0 -> 1
	1 -> 2 [label="Path 1"]
	1 -> 3 [label="Path 2"]
	1 -> 4 [label="Path 3"]
	2 -> 5
	3 -> 6
	4 -> 0
	5 -> 6
	6 -> 7
	7 -> 8 [label="if sword"]
	7 -> 9 [label="else"]
	8 -> 10
	9 -> 11
	10 -> 12
	11 -> 13
}
```

The first half correspond to the nodes and the second to the edges conencting them.

Since an id is necessary, the nodes are represented by their index in the list of nodes.

For the edges, the label corresponds to the name of the choice and/or the requirement to take that path. If both are present, separate the two with a line feed (`\n`).

The label for the nodes is as follows:\
`NodeType: 'code that generated this node' (relative/path/to/file, line number)`

#### Analysis

<!-- TODO -->