---
title: simultaneously replace library name and cell name of cells in ADE schematic
date: 2023-04-2 11:33:00 +0800
categories: [Tools, Cadence]
tags: [cadence, replace, library, cell]
math: true
mermaid: true
---

In ADE schematic, you can replace library name or cell name of cells by `Edit` -> `Replace`. But you cannot replace both library name and cell name simultaneously.

For example, we want to automatically replace cells with lib name of `hiscs40ulph07svt40_ana` and cell name of `xxx_ana` to cells with lib name of `hiscs40ulph07svt40` and cell name of `xxx`.

|         | Old       | New           |
|---------|-----------|:--------------|
| Lib     | `hiscs40ulph07svt40_ana` | `hiscs40ulph07svt40` |
| Cell    | `xxx_ana`  | `xxx`  |

The following skill script provides this function.

```skill
procedure(CCFreplaceStdCellsPat(@key 
        (cvId geGetEditCellView()) 
        (fromLib "hiscs40ulph07svt40_ana") 
        (toLib "hiscs40ulph07svt40") 
        (pattern "40_ana") ;comment this if the cell name keeps
	(with "40") ;comment this if the cell name keeps
      )
  let((newCellName pat)
    pat=pcreCompile(pattern)
    foreach(instHeader cvId~>instHeaders
      when(instHeader~>libName==fromLib && instHeader~>instances
        newCellName=pcreReplace(pat instHeader~>cellName with 1)
        dbSetInstHeaderMasterName(
          instHeader
          toLib
          newCellName
          instHeader~>viewName
        )
      )
    )
    t
  )
)
```


Then all you need to do is (with the schematic open and in the current window):

`CCFreplaceStdCellsPat()`

You might also want to change the library names it is looking for, so you could do:

`CCFreplaceStdCells(?fromLib "origLibName" ?toLib "newLibName")`

There's also the ability to pass ?cvId so it doesn't have to be a schematic open in a window (it could have been opened with dbOpenCellViewByType), or to use a different suffix via ?suffix.

# Reference
[https://community.cadence.com/cadence_technology_forums/f/custom-ic-skill/47295/replace-library-name-and-cell-name-of-standard-cells-in-schematic] (https://community.cadence.com/cadence_technology_forums/f/custom-ic-skill/47295/replace-library-name-and-cell-name-of-standard-cells-in-schematic)
