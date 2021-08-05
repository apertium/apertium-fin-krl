
# apertium-krl-fin: Finnish–Karelian rules for rule-based machine translation

This is a visualisation of some rules in apertium transfer.


## Categories (parts of chunks)
   
These are the categories Apertium is using in order to chunk, re-order and
transfer lexemes.
    
| Category | Items |
|:---------|:------|
| nom |  `<n.*>`  |
| sent |  `<sent>`  |

    
## Attributes

These are the morphological analysis value (tag) sets that can be processed in
the transfer.

| Attribute set name | Tags |
|:-------------------|:-----|
| a_cas | `<nom>`  |

    
## Macros

Macros are helper functions in apertium transfer files.



### test

Parametres: 1

1. **let** `$number` ≔ ""

## Rules
    
The actual rules concerning stuff.



### default rule
    
#### Matching pattern:
    

1. sent

#### Action:
    

1. Output: 
  1. sent``<SENT>``
    1. `tl[1]['whole']` 
    

- - -

Documentation for [apertium-krl-fin](//github.com/apertium/apertium-krl-fin/).
Generated with [Flammie’s apevis-xslt](https://github.com/flammie/apevis-xslt).
  