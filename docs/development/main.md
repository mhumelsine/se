<div align="center">
  <h1>Software Engineering Playbooks</h1>
  <p>
    Templates to bootstrap projects and give you guidance.
  </p>
</div>

<!-- Table of Contents -->

# :notebook_with_decorative_cover: Table of Contents

- [General Coding Standards](#triangular_ruler_general-coding-guidelines)
- [Repository Structure](#file_cabinet-repository-structure)
- [Database Standards](./database.md)
- [Source Code Management](./source-code-management.md)


## :triangular_ruler: General Coding Guidelines

- Follow the [clean architecture model](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) in the simplest form possible.
- Use [value objects](https://martinfowler.com/bliki/ValueObject.html) where possible.
  - Avoids changes to interfaces when new properties are added.
  - Provides guarantees that simplify code.
  - Make unit testing easier.
- All public members must have descriptive and helpful comments. This will be enforced by treating warnings and errors and will cause compilation to fail.
- &gt; 95% unit test coverage. Try to cover 100% of lines, but certain lines like configuration or bootstrapping in `Program.cs` are not valuable to test. We are not about hitting a number to hit a number. This will be enforced as a pull request policy. The delta of lines changes in the PR must be &gt; 95% coverage.
- Optimize for developers first, machines second.
- Write declarative, easy to reason about code.
- Use a functional approach when available. Functional approaches remove state mutation, to cause of many race
  conditions and bugs and makes the code easier to reason about.
- When using method chaining, place each method call on its own line.
- Avoid nested conditionals. Avoid walls of conditionals. Use your judgement, if it is easy to see what is going on, it
  is probably OK. If it is not obvious, comment or refactor.
- Avoid long methods. Prefer using short methods that label activities so the code your teammates have to read is short
  and easy to get a handle on. For complex processes, consider making each step a separate method.

```
public void Execute(Command command)
{
    UpdateNote(command);
    UpdateLab(command);
    UpdateCase(command);
    UpdateTaskList(command);
}
```

Developers coming behind you can ignore large portions of the code that are out of scope of the changes they are working
on. When in a larger method, they are forced to understand it all and worry about how their changes may cause unintended
consequences.

- Use the best data type / algorithm for the problem. Check: [Big O Cheatsheet](https://www.bigocheatsheet.com/).


## :file_cabinet: Repository Structure

```bash
# example apps, demos, examples of how to interact with the application
- demos
# scripts, kubernetes, docker, bicep, IaC, or other artifacts required to deploy the application
- deploy
# contains md files that document design choices, how to run the application, or other required documentation
- docs
# required artifacts to support CI/CD on supported platforms
- pipelines
# source code that makes up the application
- src
# unit and integration tests
- tests
- .gitignore
- .dockerignore
- azurepipelines-coverage.yml
- Solution.Sln
- README.md
```