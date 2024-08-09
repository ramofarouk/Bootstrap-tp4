# WebPack
Webpack est un module bundler pour les applications JavaScript. Il vous aide à regrouper, transformer et optimiser les fichiers JavaScript et autres ressources (comme les CSS, images, etc.) en un ou plusieurs fichiers (bundles) que votre navigateur peut charger.

Pour intégrer Bootstrap dans un projet utilisant Webpack, suivez ces étapes. Nous allons passer par l'installation de Bootstrap via npm, la configuration de Webpack, et l'utilisation de Bootstrap dans votre projet.

### Étape 1: Initialiser le Projet

1. **Créez un nouveau projet** :
   ```bash
   mkdir mon-projet-bootstrap
   cd mon-projet-bootstrap
   npm init -y
   ```

### Étape 2: Installer les Dépendances

Installez Webpack, le serveur de développement, Bootstrap, et le plugin HTML :

```bash
npm install --save bootstrap
npm install --save-dev webpack webpack-cli webpack-dev-server style-loader css-loader html-webpack-plugin
```

### Étape 3: Configurer Webpack

Créez un fichier `webpack.config.js` à la racine du projet avec la configuration suivante :

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  devServer: {
    static: path.join(__dirname, 'dist'),
    port: 9000,
    open: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html', // Chemin vers le fichier HTML source
      filename: 'index.html',       // Nom du fichier HTML généré
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
  mode: 'development', // Changez en 'production' pour la build de production
};
```

### Étape 4: Ajouter les Fichiers Source

1. **Créez le répertoire `src` et ajoutez les fichiers nécessaires** :

   ```bash
   mkdir src
   touch src/index.js src/index.html src/styles.css
   ```

2. **Contenu du fichier `src/index.html`** :

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Bootstrap avec Webpack</title>
   </head>
   <body>
       <div class="container mt-5">
           <h1>Bootstrap avec Webpack</h1>
           <div class="progress mt-3">
               <div id="progress-bar" class="progress-bar" role="progressbar" style="width: 0%;" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100">0%</div>
           </div>
           <button id="increase-progress" class="btn btn-primary mt-3">Augmenter la Progression</button>
       </div>
   </body>
   </html>
   ```

3. **Contenu du fichier `src/index.js`** :

   ```javascript
   import 'bootstrap/dist/css/bootstrap.min.css';
   import './styles.css';

   let progress = 0;

   function updateProgressBar() {
       progress += 25;
       if (progress > 100) progress = 100;
       const progressBar = document.getElementById('progress-bar');
       progressBar.style.width = progress + '%';
       progressBar.setAttribute('aria-valuenow', progress);
       progressBar.innerText = progress + '%';
   }

   document.getElementById('increase-progress').addEventListener('click', updateProgressBar);
   ```

4. **Contenu du fichier `src/styles.css`** (optionnel pour des styles personnalisés) :

   ```css
   .progress-bar {
       transition: width 0.6s ease;
   }
   ```

### Étape 5: Configurer les Scripts dans `package.json`

Ajoutez les scripts pour démarrer le serveur de développement et construire le projet :

```json
"scripts": {
  "start": "webpack serve",
  "build": "webpack"
}
```

### Étape 6: Démarrer le Projet

1. **Démarrer le serveur de développement** :

   ```bash
   npm start
   ```

   Le serveur Webpack Dev Server démarrera et ouvrira automatiquement votre application dans le navigateur. Vous devriez voir la page avec la barre de progression et le bouton pour augmenter la progression.

2. **Construire le projet pour la production** :

   ```bash
   npm run build
   ```

   Cela générera les fichiers optimisés dans le répertoire `dist`.
