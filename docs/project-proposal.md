# Project proposal

## 1. The problem and goal

(Explain the problem you want to solve and why it matters. Describe what you
want to improve, how you will measure improvement, and which limits or
requirements must be respected. A short example can help someone outside your
field understand the problem.)

**The problem and goal in my project:**

Software testing is essential for detecting bugs and ensuring that programs behave correctly. However, writing effective test cases manually can be time-consuming, and developers may overlook unusual inputs and edge cases.

The goal of my project is to develop an AI-based agent that automatically generates and improves test cases for Python functions. The agent will analyze the function's code and expected behavior, identify scenarios that may not be adequately tested, and propose new test cases.

The main objective is to increase the effectiveness of a test suite while keeping the number of tests and execution time reasonable.

I will measure improvement using:

- **Code coverage:** The percentage of program statements or branches exercised by the tests.
- **Bug detection:** The number of deliberately introduced bugs that the tests successfully identify.
- **Execution cost:** The number of test cases generated and the time required to run them.

The project will initially focus on small, deterministic Python functions with clearly defined inputs and expected outputs. Tests must execute automatically and produce reproducible results.

For example, consider a function that calculates shipping costs based on package weight. A basic test suite might check only typical weights, while missing important boundary cases such as zero weight, negative inputs, or values exactly at pricing thresholds. The AI agent should identify these weaknesses and generate tests that reveal them.

## 2. The simulator

(Describe a program or model that could simulate your system and calculate what
happens under different conditions. Explain what inputs it takes and what
results it produces. State whether it can run on your computer and accept inputs
in a text file, such as JSON or YAML. If you have not chosen a simulator yet,
name an option you want to explore and explain what you need to check.)

**The simulator in my project:**

The simulator will be a deterministic Python-based testing environment that evaluates the effectiveness of test cases proposed by the AI agent.

I plan to use `pytest` to execute tests and `coverage.py` to measure statement and branch coverage. I will also create a small collection of Python functions with known specifications and deliberately introduced bugs to measure how well the generated tests detect incorrect behavior.

The simulator will receive a structured input file in JSON format containing the target function identifier and a list of test cases, including their input values and expected outputs.

For each proposed test suite, the simulator will:

1. Run the test cases against the correct reference implementation to verify their expected results.
2. Measure statement and branch coverage.
3. Run valid tests against faulty implementations and count how many bugs they detect.
4. Measure execution time and the number of tests.
5. Return a JSON report containing these measurements and any errors.

The simulator will run locally on my computer and will not require external services. Given the same function, test cases, and environment, its results should be reproducible, apart from small variations in measured execution time.

One aspect I still need to investigate is how to execute AI-generated tests safely, particularly if the agent proposes arbitrary Python code. Initially, I plan to accept only structured test inputs rather than executable code, which will make the evaluation safer and more predictable.

## 3. The agent's role

(Explain what the language-model-based agent will decide or change, and how it
will use the simulator's results to choose its next action. If known, list the
variables it may change and their allowed ranges. Explain what you expect the
language model to contribute: could a simple program that tries different
options and picks the best result achieve the same goal? Include one question
you still need to answer.)

**The agent in my project:**

The AI agent will be responsible for generating and iteratively improving test cases for Python functions using feedback from the testing simulator.

The agent will receive the target function's source code, its expected behavior, and the results of previously executed tests. Based on this information, it will identify potentially untested scenarios and propose new test cases.

The agent will follow an iterative process:

1. Analyze the function's logic, conditions, and input constraints.
2. Generate a set of test cases, including normal inputs, boundary values, and unusual combinations.
3. Send the proposed tests to the simulator for evaluation.
4. Examine the results, including coverage, detected bugs, and execution cost.
5. Generate improved tests targeting weaknesses identified in previous iterations.

The variables the agent can modify include the number of tests, their input values, and the expected outputs. Initially, I will limit each proposal to at most 20 test cases and allow up to 10 improvement iterations. The target functions will have bounded, JSON-compatible inputs, with specific valid ranges defined for each function.

The goal is to maximize bug detection and branch coverage while minimizing unnecessary tests and execution time.

Unlike a simple program that randomly generates inputs and keeps the best results, a language-model-based agent can reason about the meaning of the code, identify boundary conditions, and use previous feedback to generate more targeted test cases.

To evaluate whether the language model provides a meaningful advantage, I plan to compare its performance against a baseline that generates random test inputs using the same evaluation budget.

One open question is how much information the simulator should reveal about detected bugs without making the testing problem unrealistically easy for the agent.

## Explain your project to someone outside your field

(Now describe your whole project again, as if you were explaining it to someone
from another field who has no background in your topic. For example, if you
study chemical engineering, imagine explaining it to a physicist; if you study
aeronautical engineering, imagine explaining it to someone in medicine; if you
study data science, imagine explaining it to a materials engineer. In one or two
short paragraphs, connect all three parts: the problem you want to solve, how
the simulator helps you test ideas, and what the agent will decide or change.
Use everyday language, explain any technical terms you need, and avoid
unexplained abbreviations. Do not simply copy your answers above.)

**My project in simple words:**

When programmers create software, they need to check that it works correctly in many different situations. However, it is difficult to think of every possible case. A program might work perfectly with normal inputs but fail when given an unusual value or a combination that the programmer did not consider.

My project aims to build an AI assistant that helps programmers discover these hidden problems. The AI will examine a small program and suggest different inputs to test it. A separate testing tool will then run these tests and measure how much of the program was checked and whether any known bugs were detected. Based on the results, the AI will try to create better tests, focusing on parts of the program that were not tested sufficiently.

The goal is to determine whether an AI assistant can find software problems more effectively than simply trying random inputs, while keeping the testing process fast and manageable.
