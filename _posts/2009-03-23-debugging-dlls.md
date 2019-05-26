---
layout: post
title: "Debugging DLLs"
date: 2009-03-23T21:48:29-00:00
draft: false
author: "Brian Kloppenborg"
tags: ["tools", "DLLs"]
categories: ["debugging"]
---
{% include JB/setup %}

If you are writing a DLL and need to know the detailed information about your
compiled library, check out [Dependency Walker](http://www.dependencywalker.com/).
From their website:

>  Dependency Walker is a free utility that scans any 32-bit or 64-bit Windows
>  module (exe, dll, ocx, sys, etc.) and builds a hierarchical tree diagram of
>  all dependent modules. For each module found, it lists all the functions that
>  are exported by that module, and which of those functions are actually being
>  called by other modules.

In addition to a GUI product, MS Visual Studio provides `dumpbin`. Dumpbin will
dump the contents of a binary file. For DLLs, the following command is useful:
`dumpbin /exports SomeFile.dll`. Dumpbin is easily accessed through the Visual
Studio Command Prompt found under the Tools menu in VS or in the VS start menu
folder under "Visual Studio Tools." Sample output with /exports option ran on a
simple DLL I created the other day.

    > dumpbin /exports SALAPI.dll
    
    Microsoft (R) COFF/PE Dumper Version 8.00.50727.42  
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    Dump of file SALAPI.dll
    
    File Type: DLL
    
    Section contains the following exports for
    SALAPI.dll
    
    00000000 characteristics<br/>49C82DEE time date
    stamp Mon Mar 23 18:48:46 2009<br/>0.00 version<br/>1 ordinal base<br/>2 number
    of functions<br/>2 number of names
    
    ordinal hint RVA name
    
    1 0 000012BC DllMain = @ILT+695(_DllMain@12)
    
    Summary
    
    5000 .data<br/>2000 .idata<br/>B000
    .rdata<br/>4000 .reloc<br/>1000 .rsrc<br/>58000 .text
