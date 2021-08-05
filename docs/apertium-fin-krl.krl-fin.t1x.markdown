
# apertium-fin-krl: Finnish–Karelian rules for rule-based machine translation

This is a visualisation of some rules in apertium transfer.


## Categories (parts of chunks)
   
These are the categories Apertium is using in order to chunk, re-order and
transfer lexemes.
    
| Category | Items |
|:---------|:------|
| noun |  `<n.*>`  |
| inf |  `<vblex.inf.*>`  `<vaux.inf.*>`  |
| infa |  `<vblex.inf>`  `<vaux.inf>`  `<vblex.inf.*>`  `<vaux.inf.*>`  |
| punct |  `<punct>`  |

    
## Attributes

These are the morphological analysis value (tag) sets that can be processed in
the transfer.

| Attribute set name | Tags |
|:-------------------|:-----|
| verbtype | `<vblex>` `<vaux>`  |
| case | `<nom>` `<par>` `<gen>` `<ess>` `<acc>` `<tra>` `<ine>` `<ela>` `<ill>` `<ade>` `<abl>` `<all>`  |
| number | `<sg>` `<pl>` `<sp>` `<ND>`  |
| infform | `<inf>` `<infa>` `<infma>` `<infe>`  |
| possessive | `<px1sg>` `<px2sg>` `<px3sp>` `<px1pl>` `<px2pl>`  |

    
## Macros

Macros are helper functions in apertium transfer files.



### infmangler1

Parametres: 1


1. **if** `sl[1]['infform']`  ≟ `<inf>``sl[1]['case']`  ≟ `<ill>` **then**:
  1. **let** `tl[1]['infform']`  ≔ `<infma>`
1. **elseif** `sl[1]['infform']`  ≟ `<inf>``sl[1]['case']`  ≟ `<ine>` **then**:
  1. **let** `tl[1]['infform']`  ≔ `<infe>`
1. **else**:
  1. **let** `tl[1]['infform']`  ≔ `<infa>`1. **let** `tl[1]['number']`  ≔ `<sg>`1. **let** `tl[1]['case']`  ≔ `<lat>`

## Rules
    
The actual rules concerning stuff.



### infa only: -> sg.lat
    
#### Matching pattern:
    

1. infa

#### Action:
    

1. infmangler1($1)
1. Output: 
  1. inf``<INFA>``
    1. `tl[1]['lem']` `tl[1]['verbtype']` `<actv.infa.sg.lat>`
    

### infs: map cases and so
    
#### Matching pattern:
    

1. inf

#### Action:
    

1. infmangler1($1)
1. Output: 
  1. inf``<INF>``
    1. `tl[1]['lem']` `tl[1]['verbtype']` `<actv>``tl[1]['infform']` `<sg>``tl[1]['case']` `tl[1]['possessive']` 
    

### puncts fallback: copy
    
#### Matching pattern:
    

1. punct

#### Action:
    

1. Output: 
  1. punct``<PUNCT>``
    1. `tl[1]['whole']` 
    

- - -

Documentation for [apertium-fin-krl](//github.com/apertium/apertium-fin-krl/).
Generated with [Flammie’s apevis-xslt](https://github.com/flammie/apevis-xslt).
  