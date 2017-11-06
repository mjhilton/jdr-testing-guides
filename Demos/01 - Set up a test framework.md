*App contains a basic Javascript class for a calculator with add, subtract, multiply, divide*

`git checkout Prac01-StartPoint -b Prac01`

1. Pull the practicals repository from `https://github.com/juniordevio/testing-testing-123`
1. It's currently pretty bare-bones: we'll work up to a fuller app later
1. For now, let's establish some test files
1. Add Mocha and Chai from NPM
    - `npm install --save-dev Mocha`
    - `npm install --save-dev chai`
    - Add .gitignore: https://raw.githubusercontent.com/github/gitignore/master/Node.gitignore
1. Add a `test` folder
1. Add a `calculator-tests.js` file to the `test` folder
1. Set up an npm test script in the `package.json` to point to Mocha
    - Under scripts, add one called "test" with value `mocha`
1. Run your (empty) test suite with `npm test`. You should get a green result :)
