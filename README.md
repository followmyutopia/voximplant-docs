# voximplant-docs
A new GitHub-based Voximplant documentation website

## What do we lack for the documentation?
1. Tables support — done, we can use HTML tables, easy
1. Multi-tab code snippets

Let's try to use html for a table.

<table>
  <tr>
    <th>Header One</th>
    <th>Header Two</th>
  </tr>
  <tr>
    <td>Lorem ipsum</td>
    <td>Dolor</td>
  </tr>
  <tr>
    <td>Sit amet</td>
    <td>Bacon Voximplant</td>
  </tr>
</table>

Done. Now let's try to create a multi-tab code snippets:

1. We can use [Readme Engine](https://rdmd.readme.io/docs/getting-started) to render multi-tab code blocks, but I don't know how to import this functionality to GitHub.

```javascript I'm A tab
console.log('Code Tab A');
```
```javascript I'm tab B
console.log('Code Tab B');
```
