# FreeCAD tutorial
FreeCAD learning material.



## Contents

- [Introduction](#introduction)
- [Installing FreeCAD](#installing-freecad)
- [Getting started](#getting-started)
- [Contributing](#contributing)



# Introduction

The goal of this tutorial is to collect useful links and best practices we found during the developments for the Mater project. Therefore, it is not a general-purpose guide on how to use FreeCAD. We may classify the FreeCAD users into three groups. *End users* do every operation from the graphical user interface (GUI) of FreeCAD. *Power users* also make use of the Python interface to automate certain steps and to extend the capabilities of FreeCAD. Then we have the *developers*, who contribute to the FreeCAD source code and write the core functionalities in C++ and in Python. This tutorial targets power users. All you need to know is basic Python programming.



# How to read this manual

The main sections seen in the table of contents above are generally independent from each other, so you can use this manual as a reference (actually, this manual *is* more a reference than a step-by-step guide). However, we strongly suggest that you read the [**Installing FreeCAD**](#installing-freecad) section even if you already have FreeCAD installed. The reason is that power users need the proper version of FreeCAD to perform certain tasks. If you are a new user, you can greatly benefit from the [**Getting started**](#getting-started) section, which is meant to be a collection of advices to efficiently get started using FreeCAD.

The manual is rich in inline references, which are hyperlinks that bring you to the webpage where the information comes from. Apart from that, sections and subsections have some standalone references at the end, which let you further immerse in the topic.

If you notice typos, inaccurate or untrue information, or have questions or suggestions, please contact us (see section [**Contributing**](#contributing)) to improve this manual.



# Installing FreeCAD

Hosted on [GitHub](https://github.com/FreeCAD/FreeCAD), FreeCAD is an open-source project. As such, people modified the source code to adapt FreeCAD to their needs. We strongly recommend to use Zheng Lei's (aka realthunder) fork of FreeCAD, available on https://github.com/realthunder/FreeCAD_assembly3/releases.

FreeCAD has quite a few software requirements. Depending on your system, compiling FreeCAD from sources can be challenging. If you are a power user, you will probably not need to bother with the C++ interface to FreeCAD (unless you want to write performance-critical code). In that case, it is a better choice to download the binaries, corresponding to your system. Using a precompiled version of FreeCAD has numerous advantages.

- When you want to share your developed plugin with the world, it is vital that you test it on the same system as what your users have. When using the precompiled binary, you can be sure that it is exactly the same environment as the one your users download. Since the same FreeCAD version is packages for the different operating systems, you can be sure that what you develop on Linux for instance, will work seamlessly on Windows as well, providing a convenient cross-platform development workflow.
- The binaries contain all the necessary functionalities, giving you access to well-known programs and libraries such as Gmsh and Netgen for mesh generation, ParaView for scientific visualization, PySide2 for creating graphics widgets in Python, PythonOCC for accessing the OpenCASCADE geometrical kernel from Python.Netgen, etc. On the contrary, if you install FreeCAD from sources, you may come across the situation that you cannot compile certain modules due to incompatibilities of your system.

Drawbacks of using the precompiled binaries include the following.
- If you want to tinker with the C++ layer of FreeCAD, you will need a compilation step, therefore you must install FreeCAD from the source code.
- Adding additional Python modules that require compilation may fail as the `glibc` and `libstdc++` runtime libraries might be different on your machine and on the machine on which FreeCAD has been compiled. On the positive side, this ensures that you do not introduce difficult-to-satisfy dependencies when you write your plugin, making practitioners' lives easier.

From now on, we assume that you run FreeCAD on a Linux distribution. Then download the AppImage file from the link above, set it to executable, and you are ready to use FreeCAD. As a power user, you will greatly benefit from extracting the AppImage.
-  You can see the directory structure, thereby having a deeper understanding on FreeCAD.
-  The obtained directory is in fact a [*conda*](https://docs.conda.io/en/latest/) package. Using *conda*, you can install new packages to the existing environment.
-  You can set up a development environment for programming.

To extract your downloaded AppImage, follow the steps [explained online](https://stackoverflow.com/a/67525817/4892892):
1.  Download FreeCAD, e.g. the [realthunder version](https://github.com/realthunder/FreeCAD_assembly3/releases). We assume that you downloaded it as *FreeCAD-asm3-Daily-Cond-Py3-Qt5-20210615-glibc2.12-x86_64.AppImage* into your `~/Downloads` directory.
2.  Make the AppImage executable
    ```bash
    chmod +x FreeCAD-asm3-Daily-Cond-Py3-Qt5-20210615-glibc2.12-x86_64.AppImage
    ```
3.  Create a new directory, which will store the extracted program.
    ```bash
    mkdir freecad_appimage
    ```
4. Extract the AppImage into this directory
   ```bash
   cd freecad_appimage/
   ../FreeCAD-asm3-Daily-Cond-Py3-Qt5-20210615-glibc2.12-x86_64.AppImage --appimage-extract 
   ```

For Windows, you should download the archived file and extract it.

We will detail later what you can find in the extracted directories. For now, we just remark that whether you launch FreeCAD from the AppImage or from the extracted directory, you will see the same environment even if you install additional workbenches, macros, etc! The reason is that both versions of FreeCAD read the installed addons and your settings from the same directory, located in `~/.FreeCAD/` by default.

   



# Getting started

You can save a lot of time later if you spend some time on setting up your FreeCAD environment. These are the steps we found useful.



## Install third-party macros and workbenches

Here is a set of macros and workbenches that we found useful during the development of the Mater project. Depending on your task at hand, the external tools to be installed will vary.

### [Extension Manager](https://github.com/mnesarco/FreeCAD_ExtMan)

This workbench contains more advanced tools than the built-in [*Addon manager*](https://wiki.freecadweb.org/Std_AddonMgr)m see also the [corresponding forum page](https://forum.freecadweb.org/viewtopic.php?t=50265). If you use a precompiled FreeCAD, its dependencies are already included. So you only need to clone the repository to directory of your FreeCAD settings:
```bash
cd ~/.FreeCAD/Mod
git clone https://github.com/mnesarco/FreeCAD_ExtMan.git ExtMan
```
Once restarted FreeCAD, you can select the *Extension Manager* among the other workbenches. You can install all the other macros and workbenches we list below from within this tool.

### [Curves](https://wiki.freecadweb.org/Curves_Workbench)

This mature Python-based workbench is very useful for manipulating NURBS curves and surfaces. Highly recommended to be installed. You can browse information or address your own question on the [corresponding forum page](https://forum.freecadweb.org/viewtopic.php?f=8&t=22675). However, [*Curves* is meant to be used from the GUI](https://forum.freecadweb.org/viewtopic.php?f=3&t=22675&start=1020#p522718), not as a Python package.

### [Lattice2](https://github.com/DeepSOIC/Lattice2)

We used a single, but useful, feature of this workbench: [downgrade](https://github.com/DeepSOIC/Lattice2/wiki/Feature-Downgrade). A use case will be mentioned later in this guide.

### [Launcher](https://github.com/triplus/Launcher)

Don't remember where you can find a certain command, or want to access them faster than changing your current workbench and selecting them from the toolbar or from the menu? This little utility will make your workflow faster by restricting the choices via autocompletion. See the [corresponding forum page](https://forum.freecadweb.org/viewtopic.php?f=22&t=15564).



# Contributing

TBA
