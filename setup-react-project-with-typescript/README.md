# Setup React Project with TypeScript

## Creating a Project

```sh
yarn create react-app react-app-starter --typescript
cd react-app-starter
git init
```
### Get Started Immediately

You **don’t** need to install or configure tools like Webpack or Babel.<br>
They are preconfigured and hidden so that you can focus on the code.

## Available Scripts

In the project directory, you can run:

```sh
yarn start
```

Runs the app in the development mode.<br>
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br>
You will also see any lint errors in the console.

```sh
yarn test
```

Launches the test runner in the interactive watch mode.<br>

```sh
yarn build
```

Builds the app for production to the `build` folder.<br>
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br>
Your app is ready to be deployed!

```sh
yarn eject
```

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (Webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Configuration 

Configure Lint

```sh
npm i -g eslint
eslint --init
yarn add @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-config-react-app --dev
```
Configure the .eslintrc.json like:

```sh
{
    "parser":"@typescript-eslint/parser",
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "react-app"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint",
        "react"
    ],
    "rules": {
        "@typescript-eslint/explicit-member-accessibility": 0,
        "@typescript-eslint/explicit-function-return-type": 0
    }
}
```

Add the lint command to the package.json:

```
     "scripts": {
      ...
       "lint": "eslint \"**/*.+(js|jsx|tsx|ts|graphql)\"", // --fix
      ...
    }
```

Now you can run: ```yarn lint``` to lint the files.

Configure Prettier

```sh
yarn add prettier eslint-config-prettier eslint-plugin-prettier --dev
```

Test Prettier

```sh
npx  prettier  --write  src/App.tsx // --write to save file
```

Configure the lint and prettier to work together at the .eslintrc.json :

```
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:prettier/recommended",
        "prettier/@typescript-eslint",
        "react-app"
    ],
    ...
      "rules": {
        ...
        "no-console": "off",
        ...
     }
```

Add prettier rules to the .prettierrc like :

```
    {
    "arrowParens": "avoid",
    "bracketSpacing": false,
    "jsxBracketSameLine": false,
    "printWidth": 80,
    "proseWrap": "always",
    "semi": false,
    "singleQuote": true,
    "tabWidth": 2,
    "trailingComma": "all",
    "useTabs": false
  }
```
Add prettier ignore rules to the .prettierignore:

```
    {
    node_modules
    coverage
    dist
    build
    .build
  }
```

Add the prettier command to the package.json:

```
     "scripts": {
      ....
         "format": "prettier  --write \"**/*.+(js|jsx|json|tsx|css|ts|md|graphql)\""
    }
```

Now you can run: ```yarn format``` to format according to the prettier rules and save the files.

Sometime we want to check if the files are already linted and formattered, to do that we will create the validate command to the package.json like:

```
     "scripts": {
      ....
        "validate": "yarn lint && yarn prettier --list-different \"**/*.+(js|jsx|json|tsx|css|ts|md|graphql)\"",
    }
```
Now you can run: ```yarn validate``` to lint and list the files that are not formatted.

We can validate the files before commiting to the git repository to prevent bad code by using the Husky.
Install the Husky to validate before commiting (The project has to be git init first):

```sh
    yarn add husky --dev
```

Add husky config to the to the package.json:
```
{
  "husky": {
    "hooks": {
      "pre-commit": "yarn validate"
    }
  }
}
```
Now when you commit files, the ```yarn validate``` will be run.

Considering linting the whole project is slow, we want to lint files that will be committed.
Configure to lint files that will be committed:

```sh
    yarn add lint-staged --dev
```

Create .lintstagedrc file:

```sh
{
    "linters":{
        "src/*.+(js|jsx|tsx|ts)":[
            "prettier --write",
            "eslint",
            "git add"
        ],
        "src/*.+(graphql|json|css)":[
            "prettier --write",
            "git add"
        ]
    }
}
```

Add the lint-staged config to husky:
```sh
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
  ```

Now when you commit files, lint-staged  will run according to its config.