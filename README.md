# Configurando o projeto

Para configurar um projeto React de forma automática, sem precisar configurar manualmente o webpack, babel e etcs, podemos utilizar o comando abaixo

```powershell
create-react-app my-project --template=typescript
```
delete todos os arquivos de forma que a estrutura de pasta e arquivos fique da forma como está exibida abaixo.
```
|- node_modules
|- public
   |- index.html
   |- robots.txt
|- src
   |- App.tsx
   |- index.tsx
   |- react-app-env.d.ts
   |- setupTests.ts
|- .gitignore
|- package.json
|- tsconfig.json
|- yarn.lock
```

e então vamos corrigir o código nos arquivos abaixo

### index.html

````html
<!DOCTYPE  html>
<html  lang="en">
	<head>
		<meta  charset="utf-8" />
		<meta  name="viewport"  content="width=device-width, initial-scale=1" />
		<meta  name="theme-color"  content="#000000" />
		<title>React App</title>
	</head>
	<body>
	<noscript>You need to enable JavaScript to run this app.</noscript>
	<div  id="root"></div>
</body>
</html>
````

### App.tsx

```javascript
import React from  "react";

function App() {
	return <h1>Hello World</h1>;
}

export default App;
```

### index.js

```javascript
import React from  'react';
import ReactDOM from  'react-dom';
import App from  './App';

ReactDOM.render(
	<React.StrictMode>
	<App  />
	</React.StrictMode>,
	document.getElementById('root')
);
```

com todas essas alterações feitas executamos o comando abaixo

```
yarn start
```

e uma página web contento o Hello World deve ser exibida.

# Configurando o EditoConfig, ESLint e Prettier

Com o projeto base criado vamos configurar as ferramentas EditorConfig, ESLint e Prettier que farão a validação das boas práticas de código.

## EditorConfig

Para configurar o editor config basta clicar com o direito no vscode e selecionar ***generate .editorconfig*** e configurar como mostrado abaixo.

### .editorconfig
```
root  = true

[*]
end_of_line  = lf
indent_style  = space
indent_size  = 2
end_of_line  = lf
charset  = utf-8
trim_trailing_whitespace  = true
insert_final_newline  = true
```
## Eslint

Após configurar o ***EditorConfig*** vamos instalar o ***ESLint*** como dependência de desenvolvimento com o comando abaixo:

```
yarn add eslint -D
```
apos a instalação remova ***eslintConfig*** do ***packages.json***

```json
{
	"name":  "my-project",
	"version":  "0.1.0",
	"private":  true,
	"dependencies": {
		"@testing-library/jest-dom":  "^4.2.4",
		"@testing-library/react":  "^9.3.2",
		"@testing-library/user-event":  "^7.1.2",
		"@types/jest":  "^24.0.0",
		"@types/node":  "^12.0.0",
		"@types/react":  "^16.9.0",
		"@types/react-dom":  "^16.9.0",
		"eslint":  "^6.8.0",
		"react":  "^16.13.1",
		"react-dom":  "^16.13.1",
		"react-scripts":  "3.4.1",
		"typescript":  "~3.7.2"
	},

	"scripts": {
		"start":  "react-scripts start",
		"build":  "react-scripts build",
		"test":  "react-scripts test",
		"eject":  "react-scripts eject"
	},

	"browserslist": {
		"production": [
			">0.2%",
			"not dead",
			"not op_mini all"
		],
		"development": [
			"last 1 chrome version",
			"last 1 firefox version",
			"last 1 safari version"
		]
	}
}
```

e então executamos 

```
yarn eslint --init
```
Uma série de perguntas serão feitas através da cli:

- **To check syntax, find problems, and enforce code style**, 
- **JavaScript modules (import/export)**, 
-  **React**, 
-  **Does your project use TypeScript?** responda ***Y***
-  **Browser**,
-  **Use a popular style guide**,
-  **Airbnb (https://github.com/airbnb/javascript)**,
-  **JSON**, 

Quando a cli perguntar se deseja instalar o resto das deps com o npm responda não e copie os packages sugeridos removendo o eslint que já foi instalado e remover também as referencias de versões anteriores (está identificada com um **||**.

```powershell
yarn add eslint-plugin-react@^7.19.0 @typescript-eslint/eslint-plugin@latest eslint-config-airbnb@latest eslint-plugin-import@^2.20.1 eslint-plugin-jsx-a11y@^6.2.3 eslint-plugin-react-hooks@^2.5.0 @typescript-eslint/parser@latest -D
```
instale também como dependência de desenvolvimento o seguinte pacote

```powershell
yarn add eslint-import-resolver-typescript -D
```

### .eslintignore

crie um arquivo chamado **.eslintignore** na raiz do projeto

```json
**/*.js
node_modules
build
react-app-env.d.ts
```

edite o arquivo **.eslintrc.json** para ficar igual ao abaixo

### .eslintrc.json

```javascript
{
	"env": {
		"browser":  true,
		"es6":  true
	},

	"extends": [
		"plugin:react/recommended",
		"airbnb",
		"plugin:@typescript-eslint/recommended",
		"prettier/@typescript-eslint",
		"plugin:prettier/recommended"
	],

	"globals": {
		"Atomics":  "readonly",
		"SharedArrayBuffer":  "readonly"
	},

	"parser":  "@typescript-eslint/parser",
	"parserOptions": {
		"ecmaFeatures": {
			"jsx":  true
		},

	"ecmaVersion":  2018,
	"sourceType":  "module"
	},

	"plugins": ["react", "react-hooks", "@typescript-eslint", "prettier"],
	"rules": {
		"prettier/prettier":  "error",
		"react-hooks/rules-of-hooks":  "error",
		"react-hooks/exhaustive-deps":  "warn",
		"react/jsx-one-expression-per-line":  "off",
		"react/jsx-filename-extension": [1, { "extensions": [".tsx"] }],
		"import/prefer-default-export":  "off",
		"@typescript-eslint/explicit-function-return-type": [
			"error",
			{
				"allowExpressions":  true
			}
		],
		"import/extensions": [
			"error",
			"ignorePackages",
			{
				"ts":  "never",
				"tsx":  "never"
			}
		]
	},

	"settings": {
		"import/resolver": {
			"typescript": {}
		}
	}
}
```

## Prettier

Após configurar o ESlint vamos configurar uma extensão chamada ***Prettier***.

```
yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
```

e depois vamos criar o arquivo .prettier.config.js

### .prettier.config.js

```javascript
module.exports  = {
	singleQuote:  true,
	trailingComma:  'all',
	allowParens:  'avoid'
}
```
