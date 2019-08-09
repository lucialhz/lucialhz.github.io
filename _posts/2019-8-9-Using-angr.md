---
layout: post
title: Installing and using angr
categories: [MScProject]
tags: [CFG]
---

## Introduction

angr (http://angr.io/) is a python framework for analysing binaries. It can be used to create CFGs from a binary.

## Installing angr

It is recommended to run angr in a virtualenv (https://virtualenv.pypa.io/en/latest/), as it enables the environment to remain the same for the usage of angr, e.g. if installed libraries change it will not affect the use of angr.

Create virtual environment:
`virtualenv newEnv`

(From https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)
Activate virtual environment:
`.\newEnv\Scripts\activate`

Prompt now looks like:
(newEnv) C:\Users\Luke>

Where command:
`where python`

Produces:
```
C:\Users\Luke\newEnv\Scripts\python.exe
C:\Users\Luke\AppData\Local\Programs\Python\Python37\python.exe
```

Trying again in Ubunto WLS

After fresh Ubunto WLS:
`sudo apt-get update`

Followed by:
```
sudo apt install python3-pip
sudo apt install python-pip
```

`pip install --user virtualenv`

add export PATH="~/.local/bin:$PATH" to .bashrc
source .bashrc (instead of logging out and in)
`source ~/.bashrc`

Start activate virtualenv
`source newEnv/bin/activate`

cd to cfg-explorer directory, run `pip install .`



To make graphs

import angr
p = angr.Project('fauxware', load_options={'auto_load_libs':False}) //where fauxware is the binary
cfg = p.analyses.CFGFast() // create CFG, can also use the other CFG method
list(cfg.graph.nodes) // see nodes, e.g. <CFGNode rejected+0x16 0x400713[10]>, <CFGNode main+0xa0 0x4007bdL[10]>, <CFGNode __libc_csu_init+0x50 0x400830L[13]>
list(cfg.graph.edges) // see edges, e.g. (<CFGNode rejected+0x16 0x400713[10]>, <CFGNode 0x400570L[6]>), (<CFGNode main+0xa0 0x4007bdL[10]>, <CFGNode accepted 0x4006ed[14]>),
import networkx as nx // to use networkx functionality
nx.write_multiline_adjlist(cfgb.graph,"test.adjlist") // creates adjlist - https://networkx.github.io/documentation/stable/reference/readwrite/adjlist.html or https://networkx.github.io/documentation/stable/reference/readwrite/multiline_adjlist.html
nx.write_edgelist(cfgb.graph, "test.edgelist") // creates edge list - https://networkx.github.io/documentation/stable/reference/readwrite/edgelist.html
nx.write_gpickle(cfgb.graph, "test.gpickle") // creates gpickle - https://networkx.github.io/documentation/stable/reference/readwrite/gpickle.html

Edgelist seems to make most amount of sense. Although we really need entire paths. Unless we build them ourselves...
