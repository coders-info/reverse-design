# [reverse-design](http://reverse-design.coders.info)


## What mean reverse design in software development?

Reverse design, also commonly referred to as **reverse engineering** in software development

Reverse design is focusing on logs and step by steps to unit test and code

How to call the design software focusing on creating logs file, integration test, unit tests, and on the end on writing the code?


## Log-Driven Development (LDD)

**LDD** align closely with Test-Driven Development (TDD) and behaviour-driven development (BDD), but with a particular focus on understanding system behavior through logs, is working based on such main principles:

+ TDD - Test-Driven Development
+ BDD - Behaviour-Driven Development
+ Reverse Engineering
+ Modular design to create monorepo
+ Continouous improvement with CI/CD integration

This method allows you to create an autonomous system that, starting from only the system logs, will be able to recreate it, step by step, using humans and machines


## Reverse modular design 

**Reverse modular design** align closely with Test-Driven Development (TDD) and behaviour-driven development (BDD), but with a particular focus on understanding system behavior through logs - **Log-Driven Development (LDD)**
    
#### Manual Analysis of Logs 
+ Understanding Expected and Unexpected Behaviors
Review system logs to understand how the existing software behaves. This step involves deducing requirements, specifications, and system functionality strictly from the information contained in logs, which may represent both expected and unexpected system behavior.

#### Mock Function Development 
+ Abstraction of Functionality
Developers create mock functions that produce the expected outputs for given inputs based on the analyzed behavior. 
These mocks serve as temporary stand-ins for the actual implementations until they are developed.

#### Writing Tests based on Behaviours
+ TDD/BDD Principles
Developers write tests for the expected functionality, essentially formalizing the behavior observed and expected from the logs. 
These tests are written to fail initially, as the actual functions are not implemented yet.

#### Implementing Code Within Mocks 
+ Iterative Development
Actual functional code is gradually introduced to replace the mock bodies. 
The development follows the previously established tests.



## Reverse Engineering

is the process of analyzing a system to identify the system's components and their interrelationships and to create representations of the system in another form or at a higher level of abstraction. 

Reverse engineering is a sensitive and often controversial technique because, in some cases, it can be used to copy or recreate someone else's work without the original creator's permission. Consequently, there are legal and ethical implications to consider, especially in relation to copyright, patents, and trade secrets. Developers must ensure that they are not violating intellectual property laws when engaging in reverse design or reverse engineering.

### How to build an software based on reverse desgin principles?

This is often done with the following purposes:

#### Context Understandin
To gain a better understanding of how a particular piece of software works, especially if the original source code is not available. This is common when working with legacy systems where the original designers are no longer available and documentation is lacking.

#### Debugging
To identify the source of a problem within a piece of software for which the source code may be difficult to read or understand, often because of poor documentation or complex interactions within the system.


## Why it's necessary?

#### Interoperability
To enable a product to interoperate with another product that is designed to be incompatible. An example could be writing a new piece of software to interoperate with a legacy system without changing it.

#### Security Analysis
To assess the security of a system by searching for vulnerabilities that can be used to attack the system.

#### Product Migration or Integration
To facilitate the transfer of the product from one hardware or software environment to another, or to integrate with another product with compatible functionalities.

#### Code Recovery
To recover lost source code or to convert code written in an obsolete or discontinued language to a modern language.

#### Documentation
To create or enhance documentation for legacy systems where documentation may be incomplete or outdated.

#### Innovation
To learn from existing solutions to build new, improved systems that do things differently or more efficiently.








## Log-Driven Development (LDD)

**Definition**:
Log-Driven Development (LDD) is a software development approach where the design and construction of software are primarily informed and driven by the analysis of system logs. In this approach, logs are used as the fundamental source of truth for determining how a system should behave in both routine and exceptional circumstances. Developers use logs to abstract system requirements and functionality, write targeted tests, and then develop system features iteratively while respecting the guidance provided by the logs.

**Specification**:
1. **Log Analysis**:
   - Collect and review logs from the system to examine successful operations, errors, warnings, and any other significant events.
   - Document system behavior as described by the logs, distinguishing between normal behavior and exceptions.

2. **Mock Implementation**:
   - Develop mock implementations or skeleton functions/classes that encapsulate the expected behavior inferred from the logs.
   - Ensure mocks provide expected outputs for various input scenarios as evidenced by the logs.

3. **Test Creation**:
   - Derive and write a comprehensive suite of test cases to verify the correct behavior of system components.
   - Adhere to classical TDD practices: tests should initially fail since the mocks only simulate real functionality.

4. **Real Implementation**:
   - Step by step, replace the mock implementations with actual code, ensuring at each step that the code passes all tests.
   - Optimize and refactor as necessary, always maintaining the integrity of the tests.

5. **Continuous Integration and Testing**:
   - Automatically build and test the system regularly — or even after every change — to reveal integration errors as soon as possible.
   - Keep logs for ongoing analysis to feed back into the development cycle for continuous improvement.

Using such an approach can lead to a thorough understanding of the system and its interactions, as well as how to handle various types of data and errors. However, care must be taken when using logs as the sole source of truth; logs can sometimes be incomplete or misrepresentative. Thus, LDD should be complemented by additional information and insights from other system documentation and stakeholder input where possible.












## How to write the code with modular reverse design principle?

Implement a basic logging system in Python, using the built-in logging module. 
Below I will outline each step to create this logging system and demonstrate usage with a mock function and tests:

Log-Driven Development (LDD) is form of reverse design where we start from logs manual written to descibe the expected and unexpected behaviors, and next the the mock function with expected inputs and outputs, writing test and on the end writing the code inside the mocks.

1. Create 2 logs file: info.log, error.log
2. Describe line by line with values, the outputs. So we should know how should work the software when should be seeing an error
3. Write the logs function or the decorator to build the logs on each starting the function in form of sentence
4. Write the mock function with just name, parameters and example value on return
5. Write integration, functional and unit test
6. Test each test with live data from function, and replace step by step the empty mocks to source code



### Create 2 logs file: `info.log`, `error.log`

```python
import logging

# Configure logger
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger()

# Create handlers for info and error
info_handler = logging.FileHandler('info.log')
info_handler.setLevel(logging.INFO)

error_handler = logging.FileHandler('error.log')
error_handler.setLevel(logging.ERROR)

# Create formatter and add it to handlers
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
info_handler.setFormatter(formatter)
error_handler.setFormatter(formatter)

# Add handlers to the logger
logger.addHandler(info_handler)
logger.addHandler(error_handler)
```

### Describe line by line with values, the outputs

Assuming the above logger is in place, let's create a scenario:

```python
def risky_division(x, y):
    try:
        result = x / y
        logger.info(f"Successful division: {x} / {y} = {result}")
        return result
    except ZeroDivisionError as e:
        logger.error(f"Error dividing {x} by zero: {str(e)}")
        return None

# Expected output in `info.log` after risky_division(10, 2) is called:
# 2023-03-17 10:00:00,000 - root - INFO - Successful division: 10 / 2 = 5.0

# Expected output in `error.log` after risky_division(10, 0) is called:
# 2023-03-17 10:01:00,000 - root - ERROR - Error dividing 10 by zero: division by zero
```

### Write the logs function or the decorator to build the logs on each starting the function in form of a sentence

```python
def log_function_call(func):
    def wrapper(*args, **kwargs):
        logger.info(f"Function {func.__name__} called with arguments: {args} and keyword arguments: {kwargs}")
        return func(*args, **kwargs)
    return wrapper
```

### Write the mock function 
with just name, parameters, and example value on return

```python
@log_function_call
def mock_function(x, y):
    # Placeholder for a complex operation
    return {"result": "example_value"}

# Example invocation:
mock_function(5, 3)  # This should log an info message about the function being called
```

### Write integration, functional, and unit tests
Using Python's unittest framework, we'd write tests as follows:
```python
import unittest

class TestLoggingFunctionality(unittest.TestCase):
    def test_risky_division_success(self):
        self.assertEqual(risky_division(10, 2), 5.0)  # Unit test

    def test_risky_division_failure(self):
        self.assertIsNone(risky_division(10, 0))  # Unit test

    def test_mock_function(self):
        self.assertEqual(mock_function(5, 3), {"result": "example_value"})  # Integration/functional test

# Run the tests
if __name__ == '__main__':
    unittest.main()
```

### Test function

Test each function with live data from function, and replace step by step the empty mocks to source code
Initially, one would run the tests with the implementation of `risky_division` and `mock_function` in place.
Once the tests pass as expected with mock implementations, you can gradually replace the mock functionality with the actual business logic you intend to implement and re-run the tests until all pass, ensuring that your code changes do not break existing functionality. This process is iterative and aligns with Test-Driven Development (TDD) practices.


Verifying the content of log files as a part of tests would involve reading the log files and checking that the appropriate entries have been made.
This can be done using Python's built-in `unittest` framework.

Here's how you can extend the previously defined `TestLoggingFunctionality` class to include test cases for file contents:

```python
import unittest
import os

class TestLoggingFunctionality(unittest.TestCase):

    @staticmethod
    def read_last_line(logfile):
        with open(logfile, "rb") as f:
            f.seek(-2, os.SEEK_END)
            while f.read(1) != b"\n":
                f.seek(-2, os.SEEK_CUR)
            last_line = f.readline().decode()
        return last_line

    def test_risky_division_success_log_entry(self):
        risky_division(10, 2)
        last_line = self.read_last_line('info.log')
        self.assertIn("Successful division: 10 / 2 = 5.0", last_line)

    def test_risky_division_failure_log_entry(self):
        risky_division(10, 0)
        last_line = self.read_last_line('error.log')
        self.assertIn("Error dividing 10 by zero", last_line)

    # Other tests can go here...

# Run the tests
if __name__ == '__main__':
    unittest.main()
```

Note that `read_last_line` is a utility method that reads the last line from a specified log file assuming it's where the latest log entry would be. 
This method opens the file, seeks to the end, and reads backwards until it finds a new line. It then reads and returns the last line of the file.

For a more comprehensive testing process, you would want to clear the log file content before each test or use a dedicated log file for each test to ensure you are only checking the latest entries. Additionally, handling potential file I/O errors would be important in a production-grade testing suite.

Remember to reset the log file's state after each test or use unique log file names for each test run to avoid one test's log entries affecting another's assertions.

Keep in mind that this testing method is quite fragile, especially for larger-scale applications, since it relies on the assumption that the last log entry corresponds to the latest action—which might not be the case in multi-threaded or heavily-logged applications. In such cases, specialized tools or more sophisticated logging handling may be required to ensure accurate tests.

