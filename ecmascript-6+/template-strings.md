# Template Strings

### Exemple

```javascript
const appName = 'Wishtack';
const userName = 'Foo';
const greetings = `Hi ${userName},
Welcome to ${appName}!`

console.log(greetings);

// Result:
// Hi Foo,
// Welcome to Wishtack!
```

{% hint style="danger" %}
## Vulnérabilité Sécurité

N'utilisez jamais les template strings comme outil de templating HTML.  
Cela vous expose à des vulnérabilités de type XSS _\(Cross-Site Scripting\)_.

**Exemple :**

```javascript
/* userName is dynamically retrieved from malicious source:
 * query string, api, storage etc... */
const userName = '<img src=404 onerror=alert(1)>'; 
document.body.innerHTML = `<span>Hi ${userName}</span>`
```
{% endhint %}



