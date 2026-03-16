### Project Overview 

Tandem is a shared payment card platform that automatically splits transactions among group members in real time.
When a purchase is made on the shared card, each member is instantly charged their proportional share to their own linked card.

This project is a mono-repo that contain many services that are independently deployed but all work towards Tandem's goal.
Since this is a mono-repo the following principles should be adhered:
- Do NOT run tests at root level; prefer tests at the highest level appropriate for a change.
- Do NOT run linting at root level, only run linting for files modified
- Do NOT build the project at the root level, only build the project that is being modified

### Agents to use
When being prompted, if a project is not specified in the prompt then clarify which project the task should be in. If
a programming task is prompted then use the programming-expert agent. If an API generation/design task is prompted then
use the api-architect agent. When creating database models or working with code that builds on top of the database / storage 
layer, then use the database-architect agent. 

### Development Workflow

- Make any necessary code changes after planning and asking for clarification 
- Run linting with the programming language installed linter
- Run tests
- When builds fail, use verbose logging to easily identify what the root cause of the build failure is

### Code Styles & Conventions
- Project structure is to be broken up into individual modules where all logic for each module (aka service)
  exists. That means that a module contains DTOs/DAOs/Commands needed to accomplish certain logic. The project should
  NOT be structured into service/daos but rather by services.
- For API facing/interface-like data models, use enums over boolean values. Every enum should have a default value that 
  is the name of the enum suffixed by `INVALID`.
- Use descriptive variable/class naming. Rules for class/struct naming are the following:
  - Command entities do not need to have the name Command in name (but still be under
    the command sub-module) but the name should still describe the action the entity represents.
  - DAOs should have the DAO suffix in it. 
- All database/storage related code should exist in a DAO object. This DAO layer is the only layer that should interact with 
  storage systems. Service layer (aka Command layer) components that require data accesses should call appropriate DAO methods.
- DAO logic should exist in its appropriate module. If one command needs to call logic that requires database access for models in
  a different module then that should be exposed by a command that the caller calls. Essentially, a command from 1 module should NOT
  call DAO methods that exist in different modules.
- Follow Command pattern approach for code structuring/architecture. Make sure to re-use commands when possible. Do NOT
  create new commands that perform the same action. Keep Commands in their logical module. If functionality is needed that
  incorporates logic from multiple commands then create a new command that orchestrates the logic of calling other commands.
- Avoid hard-coding strings into logic. Use descriptive global fields to hold the string value 
- Use appropriate log levels for log messages. The 3 levels that can be used are ERROR, DEBUG, and INFO
- NEVER assume that a 3rd party library exists and is available. If code being written uses code from a 3rd party library, verify
  that the library exists first.
- When creating or modifying code in a project, follow existing coding patterns/style for that project.


### Things Claude should NOT do
- Don't skip error handling unless EXPLICITLY told to do so
- Don't make breaking API changes unless EXPLICITLY told to do so
