# Markdown Guide for documentation

1. Number `# ` symbol followed by a space is used for marking headings. Additional number symbols allow for subheadings (`# ` then `## ` then `### `, etc.)

1. The number 1 then a period and a space (1. ) is used to create a numbered list. Each section of the list needs the 1. 
Some editors, such as Visual Studio will automatically number each selection.

1. App level markups are done with an accent a grave ``` on either side of the word(s) to be highlighted

`Apps`

1. Dropdown menus should be highlighted by italics marked up by using a single asterisk

*Dropdowns*

1. Radio buttons and other types of selectible items should be marked up in bold by using double asterix on either side

**Radio Buttons**

1. Notes can be added by using 3 aceent a grave (backticks) followed by the word note in brackets then the actual note and three closing backticks

```{note} this is an important note```
There are other callout types (structured the same as `note`):`attention`, `caution`, `danger`, `deprecated`, `error`, `hint`, `important`, `tip`, `warning`, `versionadded`, `versionchange`.

1. Notes and other directives can be nested by altering the number of back ticks, the top directive must have more back ticks than then the bottom directive (4 backticks vs. 3 `, etc. )

````{note} 
this is very important
```{warning}
always double check item call numbers
```
````

1. If working on documentation coloboratively, in-line comments maybe of use, to add a comment prefix the line with a percentage sign followed by the comment 

````{comment} % this is how to format a comment ````



