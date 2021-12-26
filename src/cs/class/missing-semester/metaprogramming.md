## Metaprogramming

- Build systems
    - define a number of **dependencies**, a number of **targets**, and **rules** for going from one to the other.
    - `make` is one of the most common build systems.
        - `Makefile`

- Dependency management
    - `apt`/ RubyGems / PyPi / ...
    - semantic versioning

- Continuous integration systems
    - > runs whenever your code changes
    
    - Tests
      
      - **Test suite**: a collective term for all the tests
      - **Unit test**: a “micro-test” that tests a specific feature in isolation
      - **Integration test**: a “macro-test” that runs a larger part of the system to check that different feature or components work *together*.
      - **Regression test**: a test that implements a particular pattern that *previously* caused a bug to ensure that the bug does not resurface.
      - **Mocking**: to replace a function, module, or type with a fake implementation to avoid testing unrelated functionality. For example, you might “mock the network” or “mock the disk”.
