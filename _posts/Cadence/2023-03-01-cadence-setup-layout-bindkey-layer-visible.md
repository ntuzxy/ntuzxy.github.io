---
title: Bindkey to make specific layers visible or not
date: 2023-05-12 11:33:00 +0800
categories: [Tools, Cadence]
tags: [cadence, setup, layout, visible, layer]
math: true
mermaid: true
---

This blog provides an example of bindkey setting to make specific layer(s) visible or not.

Save the following script as `layout.il`, and load it in your `.cdsinit` by `load("<path>/layout.il")`.

NOTICE: You need modify the layer name (eg. M1, Metal 1) and purpose name (eg. pin, label) according to different PDKs.

```
procedure(CCSpteSetVisible(layersList "l")
  let(()
    pteSetActiveLpp(car(layersList))
    pteSetNoneVisible(?panel "Layers")
    foreach(layer layersList
      pteSetVisible(layer t "Layers")
    ); foreach
  ); let
); procedure
　
procedure(CCSpteSetNotVisible(layersList "l")
  let(()
    pteSetAllVisible(?panel "Layers")
    foreach(layer layersList
      pteSetVisible(layer nil "Layers")
    ); foreach
  ); let
); procedure


;;;; Set Visible Bindkey for Each Layer
;;;; Modify the layer name and purpose name according to PDK
hiSetBindKey("Layout" "<Key>1" "CCSpteSetVisible(list(\"M1 drawing\" \"M1 pin\"))")
hiSetBindKey("Layout" "<Key>2" "CCSpteSetVisible(list(\"M2 drawing\" \"M2 pin\"))")
hiSetBindKey("Layout" "<Key>3" "CCSpteSetVisible(list(\"M3 drawing\" \"M3 pin\"))")
hiSetBindKey("Layout" "<Key>4" "CCSpteSetVisible(list(\"M4 drawing\" \"M4 pin\"))")
hiSetBindKey("Layout" "<Key>5" "CCSpteSetVisible(list(\"M5 drawing\" \"M5 pin\"))")
hiSetBindKey("Layout" "<Key>6" "CCSpteSetVisible(list(\"TM1 drawing\" \"TM1 pin\"))")

;;;; Set Visible Bindkey for Combination of Layers
;;;; Modify the layer name and purpose name according to PDK
hiSetBindKey("Layout" "Ctrl<Key>1" "CCSpteSetVisible(list(\"M1 drawing\" \"M1 pin\" \"M2 drawing\" \"M2 pin\"))")
hiSetBindKey("Layout" "Ctrl<Key>2" "CCSpteSetVisible(list(\"M2 drawing\" \"M2 pin\" \"M3 drawing\" \"M3 pin\"))")
hiSetBindKey("Layout" "Ctrl<Key>3" "CCSpteSetVisible(list(\"M3 drawing\" \"M3 pin\" \"M4 drawing\" \"M4 pin\"))")
hiSetBindKey("Layout" "Ctrl<Key>4" "CCSpteSetVisible(list(\"M4 drawing\" \"M4 pin\" \"M5 drawing\" \"M5 pin\"))")
hiSetBindKey("Layout" "Ctrl<Key>5" "CCSpteSetVisible(list(\"M5 drawing\" \"M5 pin\" \"TM1 drawing\" \"TM1 pin\"))")

hiSetBindKey("Layout" "Alt<Key>2" "CCSpteSetVisible(list(\"M1 drawing\" \"M1 pin\" \"M2 drawing\" \"M2 pin\" \"M3 drawing\" \"M3 pin\"))")
hiSetBindKey("Layout" "Alt<Key>3" "CCSpteSetVisible(list(\"M2 drawing\" \"M2 pin\" \"M3 drawing\" \"M3 pin\" \"M4 drawing\" \"M4 pin\"))")
hiSetBindKey("Layout" "Alt<Key>4" "CCSpteSetVisible(list(\"M3 drawing\" \"M3 pin\" \"M4 drawing\" \"M4 pin\" \"M5 drawing\" \"M5 pin\"))")
hiSetBindKey("Layout" "Alt<Key>5" "CCSpteSetVisible(list(\"M4 drawing\" \"M4 pin\" \"M5 drawing\" \"M5 pin\" \"TM1 drawing\" \"TM1 pin\"))")

;;;; Set NotVisible Bindkey for Each Layer
;;;; Modify the layer name and purpose name according to different PDK
hiSetBindKey("Layout" "Shfit<Key>1" "CCSpteSetVisible(list(\"M1 drawing\" \"M1 pin\"))")
hiSetBindKey("Layout" "Shfit<Key>2" "CCSpteSetVisible(list(\"M2 drawing\" \"M2 pin\"))")
hiSetBindKey("Layout" "Shfit<Key>3" "CCSpteSetVisible(list(\"M3 drawing\" \"M3 pin\"))")
hiSetBindKey("Layout" "Shfit<Key>4" "CCSpteSetVisible(list(\"M4 drawing\" \"M4 pin\"))")
hiSetBindKey("Layout" "Shfit<Key>5" "CCSpteSetVisible(list(\"M5 drawing\" \"M5 pin\"))")
hiSetBindKey("Layout" "Shfit<Key>6" "CCSpteSetVisible(list(\"TM1 drawing\" \"TM1 pin\"))")
```
