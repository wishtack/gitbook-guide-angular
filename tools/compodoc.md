# Compodoc

Compodoc est un outil open-source permettant de générer rapidement une documentation de votre application Angular.

## Installation

La commande suivante installe le module `@compodoc/compodoc`.

```bash
yarn add --dev @compodoc/compodoc
```

## Utilisation

Définissez ensuite un npm "script" dans votre fichier `package.json`

```json
"scripts": {
  "compodoc": "compodoc -p src/tsconfig.app.json"
}
```

Et lancez la ensuite dans votre terminal :

```bash
yarn compodoc
```

Vous obtiendrez ensuite la documentation générée dans le dossier `documentation`. Ouvrez la dans votre navigateur préféré.

## Example

Un example de documentation générée est disponible sur ce lien : https://compodoc.github.io/compodoc-demo-todomvc-angular/