# Introduction
setting initial project.

using React, Docker, Vite, ESLint, Prettier

updated: 2022

# Template
## Docker

```zsh
$ docker-compose build
```

## React.js

```zsh
$ docker-compose run --rm app yarn create vite starter-kit-react-2022 --template=react-ts\
  && mv starter-kit-react-2022/* . && mv starter-kit-react-2022/.* . && rm -r starter-kit-react-2022
```

#### package.json
```diff
{
  "name": "starter-kit-react-2022",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
+   "dev": "vite --host",
    "build": "tsc && vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "@types/react": "^18.0.22",
    "@types/react-dom": "^18.0.7",
    "@vitejs/plugin-react": "^2.2.0",
    "typescript": "^4.6.4",
    "vite": "^3.2.0"
  }
}
```

```zsh
$ docker-compose run --rm app yarn add -D vite-tsconfig-paths
```

#### vite.config.ts
```diff
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
+ import tsconfigPaths from 'vite-tsconfig-paths';

// https://vitejs.dev/config/
export default defineConfig({
+  plugins: [react(), tsconfigPaths()],
});
```

#### tsconfig.json
```diff
{
  "compilerOptions": {
    "target": "ESNext",
    "useDefineForClassFields": true,
    "lib": ["DOM", "DOM.Iterable", "ESNext"],
    "allowJs": false,
    "skipLibCheck": true,
    "esModuleInterop": false,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
+   "baseUrl": ".",
+   "paths": {
+     "@/*": ["src/*"]
+   }
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```
## ESLint
```zsh
$ docker-compose run --rm app yarn add -D eslint
$ yarn eslint --init
```

#### terminal
```
You can also run this command directly using 'npm init @eslint/config'.
Need to install the following packages:
  @eslint/create-config@0.4.1
Ok to proceed? (y) y
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · Yes
✔ Where does your code run? · browser
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · standard-with-typescript
✔ What format do you want your config file to be in? · JSON
Checking peerDependencies of eslint-config-standard-with-typescript@latest
The config that you've selected requires the following dependencies:

eslint-plugin-react@latest eslint-config-standard-with-typescript@latest @typescript-eslint/eslint-plugin@^5.0.0 eslint@^8.0.1 eslint-plugin-import@^2.25.2 eslint-plugin-n@^15.0.0 eslint-plugin-promise@^6.0.0 typescript@*
✔ Would you like to install them now? · No / Yes
✔ Which package manager do you want to use? · yarn
```

#### .eslintrc.json
```diff
{
  "env": {
    "browser": true,
-   "es2021": true
+   "es2022": true
  },
  "extends": [
+   "eslint:recommended",
+   "plugin:@typescript-eslint/recommended",
+   "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "plugin:react/recommended",
    "standard-with-typescript"
  ],
  "overrides": [
  ],
  "parserOptions": {
    "ecmaVersion": "latest",
+   "tsconfigRootDir": ".",
+   "project": ["./tsconfig.json"],
    "sourceType": "module"
  },
  "plugins": [
+   "@typescript-eslint",
    "react"
  ],
  "rules": {
  },
+ "settings": {
+   "react": {
+     "version": "detect"
+   }
+ }
}
```

```zsh
$ yarn add -D eslint-plugin-jsx-a11y eslint-plugin-react-hooks
```

#### eslintrc.json
```diff
  …
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "standard-with-typescript",
+   "plugin:jsx-a11y/recommended",
    "plugin:react/recommended",
+   "plugin:react/jsx-runtime",
+   "plugin:react-hooks/recommended"
  ],
  …
  "plugins": [
    "@typescript-eslint",
+   "jsx-a11y",
    "react",
+   "react-hooks"
  ],
  …
```

#### .eslintrc.json
```json
{
  "env": {
    "browser": true,
    "es2022": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "standard-with-typescript",
    "plugin:jsx-a11y/recommended",
    "plugin:react/recommended",
    "plugin:react/jsx-runtime",
    "plugin:react-hooks/recommended"
  ],
  "parserOptions": {
    "ecmaVersion": "latest",
    "tsconfigRootDir": ".",
    "project": ["./tsconfig.json"],
    "sourceType": "module"
  },
  "plugins": [
    "@typescript-eslint",
    "jsx-a11y",
    "react",
    "react-hooks"
  ],
  "rules": {
    "padding-line-between-statements": [
      "error", {
        "blankLine": "always",
        "prev": "*",
        "next": "return"
      }
    ],
    "@typescript-eslint/consistent-type-definitions": "off",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/explicit-module-boundary-types": ["error"],
    "@typescript-eslint/no-misused-promises": [
      "error", {
        "checksVoidReturn": false
      }
    ],
    "@typescript-eslint/no-unused-vars": [
      "error", {
        "argsIgnorePattern": "^_",
        "varsIgnorePattern": "^_"
      }
    ],
    "@typescript-eslint/triple-slash-reference": [
      "error", {
        "types": "always"
      }
    ],
    "import/extensions": [
      "error", {
        "ignorePackages": true,
        "pattern": {
          "js": "never",
          "jsx": "never",
          "ts": "never",
          "tsx": "never"
        }
      }
    ],
    "import/order": [
      "error", {
        "groups": [
          "builtin",
          "external",
          "internal",
          "parent",
          "sibling",
          "object",
          "index"
        ],
        "pathGroups": [
          {
            "pattern": "{react,react-dom/**}",
            "group": "builtin",
            "position": "before"
          },
          {
            "pattern": "{[A-Z]*,**/[A-Z]*}",
            "group": "internal",
            "position": "after"
          },
          {
            "pattern": "./**.module.css",
            "group": "index",
            "position": "after"
          }
        ],
        "pathGroupsExcludedImportTypes": ["builtin"],
        "alphabetize": {
          "order": "asc"
        }
      }
    ],
    "react/display-name": "off"
  },
  "overrides": [
    {
      "files": ["*.tsx"],
      "rules": {
        "react/prop-types": "off"
      }
    }
  ],
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```

```zsh
$ touch .eslintignore
```

#### .eslintignore
```
vite.config.ts
```

#### package.json
```diff
  …
  "scripts": {
    "dev": "vite --host",
    "build": "tsc && vite build",
    "preview": "vite preview",
+  "lint": "eslint 'src/**/*.{js,jsx,ts,tsx}'",
+   "lint:fix": "eslint --fix 'src/**/*.{js,jsx,ts,tsx}'",
+   "preinstall": "npx typesync || :"
  },
  …
```

## Prettier
```zsh
$ docker-compose run --rm app yarn add -D prettier eslint-config-prettier
```

#### .eslintrc.json
```diff
  …
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "standard-with-typescript",
    "plugin:jsx-a11y/recommended",
    "plugin:react/recommended",
    "plugin:react/jsx-runtime",
    "plugin:react-hooks/recommended",
+   "prettier"
  ],
  …
```

```zsh
$ touch .prettierrc.json
```

#### .prettierrc.json
```json
{
  "singleQuote": true,
  "endOfLine": "auto"
}
```

#### check conflict
```zsh
$ docker-compose run --rm app yarn eslint-config-prettier 'src/**/*.{js,jsx,ts,tsx}'
```

#### package.json
```diff
  …
  "scripts": {
    "dev": "vite --host",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint 'src/**/*.{js,jsx,ts,tsx}'",
    "lint:fix": "eslint --fix 'src/**/*.{js,jsx,ts,tsx}'",
+   "format": "prettier --write --loglevel=warn 'src/**/*.{js,jsx,ts,tsx,html,json,gql,graphql}'",
+   "fix": "npm run --silent format; npm run --silent lint:fix",
    "preinstall": "npx typesync || :"
  },
  …
```