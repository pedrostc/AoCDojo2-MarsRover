# Art of Code Dojo 2 - The Mars Rover
## Table of contents
- [Getting started with this repository](#getting-started-with-this-repository)
- [Coding Challenge - Mars Rover](#coding-challenge---mars-rover-kata)
  - [Description](#description)
  - [Rules](#rules)
  - [Challenge Tasks](#challenge-tasks)
- [TDD Basics](#tdd-basics)
  - [Red-Green-Refactor](#red-green-refactor)
  - [Cycle steps as design opportunities](#cycle-steps-as-design-opportunities)
  - [TDD side effects](#tdd-side-effects)
- [Ping-Pong Pair Programming](#ping-pong-pair-programming)
- [JavaScript Testing](#javascript-testing)
  - [Writing tests](#writing-tests)
  - [Asserting](#asserting)
    + [Errors](#errors)
  - [Filenames](#filenames)
  

## Getting started with this repository
You'll need [Node.Js](https://nodejs.org/) installed to be able to utilize this repository.

After cloning, run `npm install` on the root of this repository to install all necessary dependencies.

To run tests, you can use the `npm run test` command. If you want to enable file watch, so the tests re-run automatically after saving a file, you can use the `npm run test:watch` command.

The test framework is configured to look for files inside of the `src` folder and to look for test files with the `.spec.js` extension.

## Coding Challenge - Mars Rover Kata
[Original kata](https://kata-log.rocks/mars-rover-kata)

### Description
You’re part of the team that explores Mars by sending remotely controlled vehicles to the surface of the planet. Develop an API that translates the commands sent from earth to instructions that are understood by the rover, and returns the rovers new position after the command execution is done.

### Rules.
- TDD all the way. Keep working on the Red-Green-Refactor cycle through the session.
- Do one task at a time. The trick is to learn to work incrementally.
- Make sure you only test for correct inputs. Incorrect inputs and exceptions can be handled as an extra after the main tasks are done.

### Challenge Tasks
- The rover starts on a given X and Y coordinates and facing a given direction (ex.: `X: 0, Y:0, Direction: North`).
- It should receive an string with a list of commands. The possible commands are:
    + Translation: `F` to move forwards, `B` to move backwards.
    + Rotation: `L` to rotate left and `R` to rotate right.
    + The command input will be something like `FFBBLRLR`.
- The rover should execute all commands in the informed string and, when its done, return its new position containing X and Y coordinates and the direction it's facing.
- Implement the wrap around behaviour, when the rover reaches the edge of the map it should appear on the other side like it's just finishing a round trip on the planet.
    + The map has a set Height and Width.
- Implement colision detection.
    + When a movement would make the rover hit an obstacle, the Rover should stop executing and inform that it found an obstacle.

## TDD Basics
Test-Driven Development (TDD) is a software development methodology where you write a test first and then write the code to make the test pass. The tests should reflect one of the requirements or parts of it.  It works on a cycle where first we write tests that express what your system should do; then you write your code to make it meet the expectations expressed in the test, and then refactor your code to enhance its design. This cycle is also known as the Red-Green-Refactor cycle.

### Red-Green-Refactor
- Red - We write a test that reflects the requirement, or part of it, that we will work on. The test will describe a behaviour that does not exist on the code if there's any code written whatsoever so that the test will fail.
- Green - We write the minimal amount of code required to make the test pass. This code can be as dirty as required. We're not focusing on quality or clean code at this point. We just want to make the test pass as fast as possible.
- Refactor - After we have a passing test, we will focus on organizing and cleaning the code up. Here we're thinking about the design and quality of our implementation, so take your time.

### Cycle steps as design opportunities
During the TDD cycle, we have separate opportunities to think about different aspects of the design of the code we're writing. Here's one way of thinking about these.
- On the Red step, we will make decisions about the API design: what its contract looks like and what to expect.
- On the Green stage, we define the bare minimum that we need to deliver the requirement specified by the test.
- On the Refactor step, we can focus on the design of the solution, organizing the code better, removing duplications and applying patterns.

### How to start - Zero, One, Many
Normaly it can be a bit dificult to figure how to start testing, one way of dealing with that is by using the Zero-One-Many approach.
- Zero: What's the behaviour when my input is nothing. This is a good place to start and gives the opportunity to think about the contract of our API.
- One: What's the behaviour when I provide one input to my API?
- Many: When I have list on inputs, how does my API behave?

### TDD side effects
This approach to development has some side effects:
- You have a faster feedback cycle to work with. The automated tests should fast so you can have near-immediate feedback on the impact of your code changes.
- It separates the concerns (steps) of the development process by having different moments to think about meeting requirements and good design at different moments.
- It gives developers a safe net of automated tests, which facilitates refactoring the code throughout its lifespan.
- Tests can be a good way to document the intended behaviour of the code.

## Ping-Pong Pair Programming
In this session, we're trying out the "Ping-Pong" style of pair programming.
The idea is that both participants in the pair take turns as the driver on every new failing test (Red step on the TDD cycle).

- A Writes a test and sees it failing
- B Writes the code needed to make the test pass
- B Refactors the code as needed
- B Writes a new failing test
- A Writes the necessary code to make the test pass
- ...

## TypeScript Testing
In this repository, we're using [Jest](https://jestjs.io/en/) as our test framework.

### Writing tests
To write a test, you should use the `test` function. It takes a string as its first argument and a function as the second. The string is the test description, and the function is the test code:
```ts
test('should receive a CSV string', function() { /* Test code here */ });
```

You can use the `describe` function to group your tests. It has the same signature as the `test` function, receiving a string and a function as arguments. You should add your tests to the body of the second argument:
```ts
describe('add', function() {
    test('should receive a csv string', function() { /* Test code here */ });
    test('should sum all comma separated elements in the string', function() { /* Test code here */ });
});
```
#### Re-using the same test for multiple test cases
You can use dynamic test to simplify your code.
```ts
var testCases = [
    { input: "input1", expected: "output1" },
    { input: "input2", expected: "output2" }
];

testCases.forEach(function({input, expected}) {
    test(`should output ${expected} for input ${input}.`, function() {
        const actual = methodUnderTest(input);

        expect(actual).toEqual(expected);
    });
});
```

### Asserting
For assertation this repo follows the convention of using the [expect](https://jestjs.io/docs/en/expect) notation for the tests assertations.
```ts
expect(value).toBeTruthy()
expect(value).toEqual(true);
expect(value).not.toEqual(false);
expect(value).not.toBeFalsy(false);
```

#### Errors
There are two styles to assert errors: one uses a try/catch block and asserts on the error object in the catch block. The second uses the [toThrow](https://jestjs.io/docs/en/expect#tothrowerror) method of the `expect` object. For this second method you need to wrap your code in a function for it to work.
```ts
// Using a try/catch block
test('should throw an error', function() {
    try {
        brokenFunction();
    } catch(error) {
        expect(error.message).toEqual('I am an error message');
    }
});
```

```ts
// Using the expect.toThrow method
test('should throw an error', function() {
    var wrapperFunction = function() {
        borkenFunction();
    }

    expect(wrapperFunction.toThrow();
    // or, if you want to be more specific
    expect(wrapperFunction).toThrow(new Error('I am an error message'));
});
```

### Filenames
This repository is using the following convention for test file names: `<name>.spec.ts`.
If you use the `.spec.ts` extension for your file, the test runner will pick it up automatically and execute all available tests.
