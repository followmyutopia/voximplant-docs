# voximplant-docs
A new GitHub-based Voximplant documentation website  
Currently work in progress by Avi and Irina M.

## Voximplant documentation syntax guide
[Edit this file](https://github.com/followmyutopia/voximplant-docs/edit/main/README.md) to see the proper syntax:

### Basic markdown
Surround some text with double asterisks ** for **bold text**, with single underscore _ for _italics_.  
To make a new line, use double space at the end of the line; to make a new paragraph, use two enters.

Pound symbols # are the **headers**.
> Please note, that in Voximplant articles, there is only one level-one header #, and it is shown in the document tree. All the rest headers should be level-two ## or more (up to level-six).

**There is no need of empty line after a header!**

- To create a dotted list,
- use the hyphen "-" prefix

1. To create a numbered list,
1. use the 1. prefix for **each** item

Use the standard markdown syntax to create [a link](#voximplant-documentation-syntax-guide).  
> Note that all the links within the documentation are **relative**, so start the link with /docs/..., do not type the full address.

**For images**, also use the standard syntax _with alt text_.  
![Alt text](https://voximplant.com/_nuxt/img/83fcc31.svg)
> Please note, that images for Voximplant articles should be stored in the /assets/images/ folder and should have the following name pattern: section-subsection-article-whatisshown.png. For example:  
**guides-sms-phonenumber-buyanumber.png**

### Voximplant special syntax
This section explains what in Voximplant documentation differs from Markdown syntax.

#### Code blocks
We use special syntax for **code blocks**, for multi-tab code with tab titles, descriptions and links.  

> This section is WIP. I need sync with Andrey for all the possible variants of the snippet tags.

To see the proper syntax, please [edit this file](https://github.com/followmyutopia/voximplant-docs/edit/main/README.md):

```vox.multicode
env: voxengine
title:Test title
description:
====
highlight : 
link : 
name : 
lang : javascript
----
console.log("This is just a test code line")
```
```vox.multicode
env: voxengine
title:Test title
description:
====
highlight : 
link : 
name : 
lang : javascript
----
console.log("This is just a test code line")
```

This will result in a two-tab code snippet.

#### Tables
For **tables**, we use **simplified syntax**. Unlike markdown, which uses html table tag, Voximplant documentation uses vertical bar with a space "| " for left border of the table, a vertical bar surrounded by spaces " | " for all the middle borders, and a space and a vertical bar " |" for the right border. We support links and formatting within table cells. There should be no spaces before and after the line, and an empty line above and below the table.

To see the syntax in action, please [edit this file](https://github.com/followmyutopia/voximplant-docs/edit/main/README.md):

| **Lorem ipsum** | **Dolor sit amet** |
| Bacon ipsum dolor amet | frankfurter strip steak meatloaf |
| leberkas pork shank | bacon shoulder beef |

This syntax builds the following table (for demonstration purpose):

<table>
    <tr>
        <th>Lorem ipsum</th>
        <th>Dolor sit amet</th>
    </tr>
    <tr>
        <td>Bacon ipsum dolor amet</td>
        <td>frankfurter strip steak meatloaf</td>
    </tr>
    <tr>
        <td>leberkas pork shank</td>
        <td>bacon shoulder beef</td>
    </tr>
</table>