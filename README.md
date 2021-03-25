# voximplant-docs
A new GitHub-based Voximplant documentation website

Currently work in progress by Avi and Irina M.

## Voximplant documentation syntax guide
[Edit this file](https://github.com/followmyutopia/voximplant-docs/edit/main/README.md) to see the proper syntax:

### Basic markdown
Surround some text with double asterisks ** for **bold text**, with single underscore _ for _italics_.  
To make a new line, use double space; to make a new paragraph, use two enters.

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
We use special syntax for code blocks, for multi-tab code with 
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

For tables, we use simplified syntax
| something | something |
| something | something |
