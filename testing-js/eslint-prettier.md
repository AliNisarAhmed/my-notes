## ESLint

- We dont have .eslintrc file which is the best way to configure eslint, we can use eslint:recommended settings and override any other settings that we dislike.

- we dont have eslint in dev dependencies

- Remember to add --ignore-path if linting by npm scriptsm ignore-path is usually provided the .gitignore file

## Prettier

- add prettier as dev dependencies

- Go to Prettier playground and copy the settings from there

- paste the settings in .prettierrc file

- To make ESLint and Prettier work together, use eslint-prettier-config

- configure it as last `extends`
