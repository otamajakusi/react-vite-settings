# how to setup configs

# eslint

```
npm init @eslint/config
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · standard-with-typescript
✔ What format do you want your config file to be in? · JavaScript
✔ Would you like to install them now? · No / Yes
✔ Which package manager do you want to use? · npm
```

```
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint typescript
```

# prettier integraion

```bash
npm i --save-dev prettier eslint-config-prettier
```

```diff
--- a/.eslintrc.cjs
+++ b/.eslintrc.cjs
@@ -5,7 +5,8 @@ module.exports = {
     },
     "extends": [
         "standard-with-typescript",
-        "plugin:react/recommended"
+        "plugin:react/recommended",
+        "prettier"
     ],
     "overrides": [
         {
```

## Parsing error: ESLint was configured to run on `<tsconfigRootDir>/.eslintrc.cjs` using `parserOptions.project`: <tsconfigRootDir>/tsconfig.json

.eslintrc.cjs says:

```text
Parsing error: ESLint was configured to run on `<tsconfigRootDir>/.eslintrc.cjs` using `parserOptions.project`: <tsconfigRootDir>/tsconfig.json
```

```diff
--- a/.eslintrc.cjs
+++ b/.eslintrc.cjs
@@ -21,8 +21,7 @@ module.exports = {
         }
     ],
     "parserOptions": {
-        "ecmaVersion": "latest",
-        "sourceType": "module"
+        "project":  ['./tsconfig.json', './tsconfig.node.json']
     },
     "plugins": [
         "react"
```

```diff
--- a/tsconfig.node.json
+++ b/tsconfig.node.json
@@ -6,5 +6,5 @@
     "moduleResolution": "bundler",
     "allowSyntheticDefaultImports": true
   },
-  "include": ["vite.config.ts"]
+  "include": ["vite.config.ts", ".eslintrc.cjs"]
 }
```

## This rule requires the `strictNullChecks` compiler option to be turned on to function correctly.

.eslintrc.cjs says:

```text
This rule requires the `strictNullChecks` compiler option to be turned on to function correctly.
```

```diff
--- a/tsconfig.node.json
+++ b/tsconfig.node.json
@@ -4,7 +4,8 @@
     "skipLibCheck": true,
     "module": "ESNext",
     "moduleResolution": "bundler",
-    "allowSyntheticDefaultImports": true
+    "allowSyntheticDefaultImports": true,
+    "strict": true
   },
   "include": ["vite.config.ts", ".eslintrc.cjs"]
 }
```

## 'React' must be in scope when using JSX

.tsx says:

```text
'React' must be in scope when using JSX
```

```diff
--- a/.eslintrc.cjs
+++ b/.eslintrc.cjs
@@ -27,5 +27,7 @@ module.exports = {
         "react"
     ],
     "rules": {
+        "react/jsx-uses-react": "off",
+        "react/react-in-jsx-scope": "off"
     }
 }
```

## eslint-plugin-simple-import-sort

```
npm i --save-dev eslint-plugin-simple-import-sort
```

```diff
--- a/.eslintrc.cjs
+++ b/.eslintrc.cjs
@@ -25,10 +25,16 @@ module.exports = {
         "project":  ['./tsconfig.json', './tsconfig.node.json']
     },
     "plugins": [
-        "react"
+        "react",
+        "simple-import-sort"
     ],
     "rules": {
         "react/jsx-uses-react": "off",
-        "react/react-in-jsx-scope": "off"
+        "react/react-in-jsx-scope": "off",
+        "simple-import-sort/imports": "error",
+        "simple-import-sort/exports": "error",
+        "import/first": "error",
+        "import/newline-after-import": "error",
+        "import/no-duplicates": "error"
     }
 }
```


## vscode

.vscode/settings.json

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

## resolve eslint and prettier settings


