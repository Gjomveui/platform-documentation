---
id: glossary
name: Documentation Terminology
title: Documentation Terminology.
---
# Documentation Terminology

Consistent vocabulary allows readers to get information from our documentation quickly. This document is designed to standardize phrasing, and defaults to the names and labels that appear in the Core Editor wherever possible.

!!! note "If you are searching for terminology that is not here, please update this document!"

## General

| Term | Usage|
| :--- | :--- |
|  Core Account | The account you use to to log in on coregames.com and in the Core Launcher |
|  Core Launcher | The program that both runs Core Games and launches the Core Editor |
|  Core Editor | The interface that allows users to edit games. |
|  The Core Website | coregames.com - The web site from which you register an account, download the installer, access documentation |
|  Core Creator Discord | The [Discord server](https://discord.gg/FFA8tQe) used by Core Creators. Recommended place to ask for live help. |
|  Core Creator | Anyone who makes content using Core |
|  Core Game Frameworks | The cloneable games found in the GAMEPLAY FRAMEWORKS section of NEW PROJECT |
|  Community Shared Games | Games marked as open source for other users to edit |
|  the Core Lua API | [The API](core_api.md) |

## Components

| Term | Usage|
| --- | --- |
|  project | Catchall for things made in the Core Editor. Could be a game or template |
|  object | All the components that could be part of a project. Art objects and game objects, sounds, game components anything that would show up in Project Content |
|  child object(s) | Instead of "children". Object nested under another object in the Hierarchy.|
|  script | Code attached to an object. Written in Lua. Using Core Lua API |
|  mesh | Primitive 3D shape |
|  prop | 3d object made of meshes. Does not have functionality |
|  item | prop with functionality |
|  scene | The way a project looks through the Main Viewport, the arrangement of the objects in the Hierarchy. The static look of the game before any dynamic gameplay changes. |
|  template | Group of objects with an intended functionality. Can be published to share. Could just be a prop, or could include functionality. |

## Parts of the Core Editor

| Term | Usage|
| --- | --- |
|  window | Each of the different submenus available in the View menu |
|  game assets | Everything from Core Content and Community Content that you can add to a game |
|  Main Viewport | The visual output of the game while in editing mode. Appears with the name of the project as the name on the tab, not “Main Viewport” |
|  Hierarchy | Core Editor Window that displays all of the objects that are currently part of the scene. |
|  Properties | Core Editor Window that shows changeable values for Core objects, including transform, collision, visibility, and properties specific to different types of objects. |
|  Preview Mode | The way of testing a project with a controllable character |
|  Multiplayer Preview Mode | Testing mode that creates multiple instances of the game in separate windows. Main Viewport will show approximation of server view. |
|  vantage point | The perspective held by the Main Viewport |
|  Top Toolbar | Where the move, resize, and play buttons are |
|  Top Menu Bar | Where File Edit View etc are |
|  Search function | The way you find objects by typing words |

## Inputs

| Term | Usage|
| --- | --- |
|  left mouse button, right mouse button | The button on the mouse gets pressed. Use this term when the action is holding |
|  mouse wheel | Button and scroll between left and right on a mouse. **Click** if pressing, **scroll** if rolling |
|  <kbd>W</kbd> <kbd>Esc</kbd> | A keyboard key, referenced directly by name |
|  <kbd>Ctrl</kbd> <kbd>Shift</kbd><br/><kbd>Alt</kbd><br/> | Use abbreviations for keys, but be rendundant in describing their actions in context for abbreviated keys. E.g. mention "deleting" with the <kbd>Del</kbd> key |
|  click, left-click, right-click | How to open a menu, or explain selecting an object. Use left-click when right-click will also be used in the document. |
|  select | Used two different ways: Choosing an option from a menu by clicking, or highlighting objects in Main Viewport or Hierarchy to be active for editing operations. |

## Icons

When writing about Core icons and buttons, use the icon image in addition to the name. The [icons list](icons.md) has a list of these, along with the formatting that makes them visible in both light and dark theme.
