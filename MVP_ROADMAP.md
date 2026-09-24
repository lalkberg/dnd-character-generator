# D&D Character Creator — MVP Roadmap

## Project goal

Build a portable, offline-first Java CLI application that can quickly create a playable D&D character using the 2024/5.5-era rules represented by SRD 5.2.1.

The MVP should let a user:

- create a character from the command line
- choose a name, species, class, background, and level
- determine ability scores by rolling, using a predefined method, or entering them manually
- make the choices required by the supported character options
- calculate the important derived character values
- select or automatically determine supported spells and equipment
- validate the resulting character
- display a useful character sheet
- save and load characters locally
- work without an internet connection once the application and bundled data have been installed

The project is primarily a Java learning project. Prefer simple, explicit designs over sophisticated abstractions. Do not optimize for supporting every possible D&D rule until the MVP works.

## Guiding principles

1. **Build vertically.** Get a tiny end-to-end slice working before implementing large portions of the rules.
2. **Keep the CLI thin.** User interaction belongs in the CLI/application layer, not in the domain model.
3. **Keep rules separate from presentation.** The character model and rules should not know how the terminal output is formatted.
4. **Treat rules data as data.** Avoid hardcoding large lists of spells, classes, species, equipment, etc. in Java source.
5. **Offline first.** Runtime internet access should not be required for character creation.
6. **Test the rules.** The most important logic should be covered by automated tests.
7. **Prefer boring Java.** Since the goal is to learn Java, avoid frameworks and patterns that do not solve an actual problem.
8. **Keep the MVP intentionally incomplete.** Unsupported rules should be explicit rather than silently approximated.

---

# Milestone 1 — Set up the Java development environment

## Goal

Create a minimal Maven-based Java application, understand the project layout, and be able to compile, run, and test it.

## Deliverables

- A Git repository
- A Maven project
- A supported JDK installed locally
- A minimal runnable application
- JUnit-based tests
- A README explaining how to build and run the project

## Learning focus

- JDK vs JRE
- Java source layout
- packages
- Maven
- `pom.xml`
- compilation
- test execution
- Java entry points
- basic Git workflow

## Definition of done

A fresh checkout can be built and tested using Maven, and the application can be launched locally.

## Suggested reading

- [dev.java — Learn Java](https://dev.java/learn/)
- [dev.java — Getting Started](https://dev.java/learn/getting-started/)
- [Apache Maven — Getting Started Guide](https://maven.apache.org/guides/getting-started/)
- [Apache Maven — Installation](https://maven.apache.org/install)
- [JUnit 5 User Guide](https://docs.junit.org/5.13.1/user-guide/)

---

# Milestone 2 — Establish the project structure and domain boundaries

## Goal

Create the initial domain model and decide which responsibilities belong to the domain, application, CLI, and infrastructure/data layers.

## Deliverables

A small, coherent model containing at least:

- Character
- Ability
- Ability scores
- Species
- Class
- Background
- Level

Also establish package boundaries for:

- domain
- application
- CLI
- data/infrastructure
- tests

Do not implement the complete D&D rules yet.

## Learning focus

- classes
- records
- enums
- constructors
- access modifiers
- collections
- composition
- interfaces
- package organization

## Design questions to answer

- Which concepts are stable application concepts and which are external data?
- Which objects should be immutable?
- Which concepts should be represented as enums?
- Which concepts should be represented as data-driven definitions?
- What belongs on Character and what belongs in separate services/calculators?

## Definition of done

The project has a small domain model with clear responsibilities and no dependency from the domain model on CLI input/output.

## Suggested reading

- [dev.java — Classes and Objects](https://dev.java/learn/classes-objects/)
- [dev.java — Interfaces](https://dev.java/learn/interfaces/)
- [dev.java — Records](https://dev.java/learn/classes-objects/records/)
- [dev.java — Collections](https://dev.java/learn/api/collections-framework/)
- [dev.java — Enums](https://dev.java/learn/classes-objects/enum-types/)

---

# Milestone 3 — Build the first end-to-end character creation flow

## Goal

Create a deliberately small vertical slice of the application.

The user should be able to start the application, enter basic character information, and receive a basic character representation.

At this stage, support only a very small subset of species/classes/backgrounds if that keeps the implementation manageable.

## Deliverables

The CLI should support:

- character name
- species selection
- class selection
- background selection
- level selection
- a simple character summary

The application should reject invalid input without crashing.

## Learning focus

- reading terminal input
- parsing user input
- loops
- branching
- validation
- exceptions
- separation between CLI and application logic

## Definition of done

A user can create a basic character from start to finish through the CLI.

---

# Milestone 4 — Implement ability scores

## Goal

Implement the ability-score system independently from the CLI.

## Supported methods

The MVP should support:

1. A predefined score-generation method appropriate to the selected ruleset
2. Dice rolling
3. Manual entry

The exact rules should be sourced from the chosen SRD version rather than from memory.

## Deliverables

- Ability score representation
- Ability modifiers
- Score generation abstraction
- Dice rolling abstraction
- CLI selection between generation methods
- Validation for manually entered scores
- Automated tests for modifier calculations and generation rules

## Learning focus

- interfaces
- polymorphism
- dependency injection without a framework
- `Random`
- collections
- testability
- parameterized tests

## Definition of done

All three supported ability-score workflows produce valid ability scores and the resulting character uses them correctly.

## Suggested reading

- [dev.java — Interfaces](https://dev.java/learn/interfaces/)
- [dev.java — Collections](https://dev.java/learn/api/collections-framework/)
- [JUnit 5 User Guide](https://docs.junit.org/5.13.1/user-guide/)

---

# Milestone 5 — Import and bundle SRD data

## Goal

Make the application data-driven and offline-first.

Use the chosen SRD 5.2.1-compatible data source as the basis for local application data. The application should not require the upstream API to be available at runtime.

The data-import/update mechanism is a development or maintenance concern; it should not be part of the core character-creation flow.

## Deliverables

- A clearly documented SRD data source
- A data import process
- Bundled local data
- A clear attribution/NOTICE file
- A documented data version
- Local repositories/providers for reading the data
- Tests proving that bundled data can be loaded

## Important licensing rule

Only redistribute data you have verified is covered by the applicable license.

SRD 5.2.1 is published by Wizards of the Coast under CC-BY-4.0. The official SRD page says that SRD 5.2.1 is the latest SRD 5.2 release and that future SRD versions remain separately available under Creative Commons. The official legal text requires attribution. See the official sources below.

Do not assume that every dataset in a third-party API is covered by the same license as the API software itself.

## Learning focus

- JSON
- serialization/deserialization
- resources inside a JAR
- Maven resources
- repository abstractions
- mapping external data into domain objects

## Definition of done

A clean machine with the application artifact can create characters without network access.

## Suggested reading

- [D&D Beyond — SRD 5.2.1](https://www.dndbeyond.com/srd)
- [D&D Beyond — Creator FAQ](https://www.dndbeyond.com/creator-faq)
- [D&D Beyond — SRD 5.2.1 legal text](https://media.dndbeyond.com/compendium-images/srd/5.2/SRD_CC_v5.2.pdf)
- [D&D Beyond — Converting to SRD 5.2.1](https://media.dndbeyond.com/compendium-images/srd/guide/converting-to-srd-5.2.1.pdf)
- [Jackson Databind](https://github.com/FasterXML/jackson-databind)

---

# Milestone 6 — Implement character creation rules

## Goal

Turn the basic character creator into a rules-aware character creator.

Implement the supported SRD rules for:

- species
- class
- background
- level
- ability score effects
- proficiencies
- class features
- origin-related choices
- other character-creation choices required by the supported content

Avoid implementing the entire ruleset at once. Implement only what is necessary to generate a valid character.

## Deliverables

- Data-driven definitions
- Character creation session/state
- Rules application logic
- Validation
- Unit tests for important rules
- Clear handling of unsupported choices

## Important design constraint

Do not make the Character object responsible for every rule.

Prefer focused components such as:

- character validator
- character calculator
- progression calculator
- choice/selection logic

The exact names are up to you.

## Definition of done

The creator can generate characters from the supported SRD data and can explain/reject invalid choices instead of producing silently invalid characters.

## Suggested reading

- [dev.java — Classes and Objects](https://dev.java/learn/classes-objects/)
- [dev.java — Interfaces](https://dev.java/learn/interfaces/)
- [dev.java — Exceptions](https://dev.java/learn/exceptions/)
- [JUnit 5 User Guide](https://docs.junit.org/5.13.1/user-guide/)

---

# Milestone 7 — Add derived character statistics

## Goal

Calculate the values a player expects to see on a usable character sheet.

Examples include, where applicable:

- hit points
- armor class
- initiative
- proficiency bonus
- saving throws
- skill modifiers
- passive values
- spellcasting statistics

Use the SRD as the source of truth for the actual rules.

## Deliverables

- Derived-stat calculation layer
- Automated tests for each calculation category
- Character sheet model containing calculated values
- No duplicated calculation logic in the CLI

## Definition of done

A generated character contains enough derived information to be useful during play.

---

# Milestone 8 — Add spells and spellcasting

## Goal

Support the spellcasting information required to make supported spellcasting characters playable.

## Deliverables

- Spell definitions loaded from data
- Spell selection
- Cantrips where applicable
- Prepared/known spell representation where applicable
- Spell slots where applicable
- Spellcasting ability
- Spell save DC
- Spell attack modifier
- Validation of spell choices

Do not attempt to implement spell effects or a combat engine.

The MVP only needs to answer questions such as:

> What spells does this character have?

and:

> What are this character's spellcasting values?

## Definition of done

A supported spellcasting character can be generated with a valid spell selection and useful spellcasting information on the character sheet.

---

# Milestone 9 — Add equipment

## Goal

Generate the equipment necessary for a usable starting character.

## Deliverables

- Equipment data
- Starting equipment options
- Equipment selection
- Basic weapon information
- Basic armor information
- Relevant derived combat values
- Validation for equipment choices

Avoid implementing a full inventory-management system.

The MVP needs enough equipment support to produce a playable starting character, not a complete virtual tabletop inventory.

---

# Milestone 10 — Build the character sheet output

## Goal

Produce a readable, stable terminal representation of the character.

## Deliverables

The sheet should contain, as appropriate:

- identity
- species
- class
- background
- level
- ability scores and modifiers
- combat statistics
- saving throws
- skills
- proficiencies
- features
- equipment
- spells

The output should be presentation-only. It should not contain D&D calculation logic.

## Definition of done

A user can generate a character and immediately understand the resulting character sheet from the terminal.

---

# Milestone 11 — Save and load characters

## Goal

Allow generated characters to be persisted locally.

## Deliverables

- Save command
- Load command
- Character serialization format
- Format version
- Validation when loading
- Useful error messages for corrupt or incompatible files

Prefer storing the user's choices and durable character state rather than duplicating every derived value.

## Definition of done

A character can be saved, exited, and loaded later without losing its important information.

## Suggested reading

- [Jackson Databind](https://github.com/FasterXML/jackson-databind)
- [Jackson Documentation](https://github.com/FasterXML/jackson-docs/wiki)

---

# Milestone 12 — Package the MVP for offline use

## Goal

Make the project feel like an actual CLI application rather than a development exercise.

## Deliverables

- Reproducible Maven build
- Runnable application artifact
- Bundled data
- License/attribution information
- README with installation and usage instructions
- Example commands
- Clear supported-ruleset documentation
- Basic release checklist

Consider a single executable/fat JAR first. Native executables can be a post-MVP enhancement.

## Definition of done

Someone who has never seen the source code can follow the README and run the application locally.

## Suggested reading

- [Apache Maven — Getting Started](https://maven.apache.org/guides/getting-started/)
- [Apache Maven — JAR Plugin](https://maven.apache.org/plugins/maven-jar-plugin/)
- [Apache Maven — Shade Plugin](https://maven.apache.org/plugins/maven-shade-plugin/)

---

# Milestone 13 — MVP acceptance pass

## Goal

Stop adding features and verify that the existing scope works reliably.

## Acceptance checklist

A user must be able to:

- start the application
- create a character
- select name, species, class, background, and level
- choose an ability-score generation method
- make all required supported choices
- receive a valid character
- see a useful character sheet
- save the character
- load the character
- perform all of this without internet access

The project should also:

- have automated tests for important rules
- fail gracefully on invalid CLI input
- have no dependency on the upstream API during normal character creation
- document the exact ruleset and data source
- include the required SRD attribution
- have a clean README
- be buildable from a fresh checkout

## Explicitly out of scope for MVP

Do not add these unless they become necessary:

- combat simulation
- monster management
- campaign management
- DM tools
- networking
- multiplayer
- web UI
- database server
- account system
- cloud storage
- full rules coverage outside the selected SRD
- full VTT functionality
- multiclassing unless it is required by the defined MVP scope
- native executables

---

# Recommended learning strategy

For each milestone:

1. Read the relevant Java documentation first.
2. Spend some time designing the solution yourself.
3. Implement the smallest version that satisfies the milestone.
4. Write tests.
5. Refactor only after it works.
6. Commit the result.
7. Only then move to the next milestone.

When you get stuck, prefer asking questions such as:

- "What Java concept should I research to solve this?"
- "What are the trade-offs between these two designs?"
- "Can you explain this compiler error?"
- "Can you review my approach without writing the implementation?"

This keeps the project useful as a Java-learning exercise instead of turning it into an AI-generated codebase.

---

# Reference material

### Java

- [dev.java](https://dev.java/)
- [Java API Documentation](https://docs.oracle.com/en/java/javase/25/docs/api/)
- [Java Language Specification](https://docs.oracle.com/javase/specs/)
- [dev.java — Learn Java](https://dev.java/learn/)

### Build and testing

- [Apache Maven](https://maven.apache.org/)
- [Maven Getting Started Guide](https://maven.apache.org/guides/getting-started/)
- [JUnit 5](https://junit.org/junit5/)
- [JUnit 5 User Guide](https://docs.junit.org/5.13.1/user-guide/)

### D&D rules and licensing

- [D&D Beyond — SRD 5.2.1](https://www.dndbeyond.com/srd)
- [D&D Beyond — Creator FAQ](https://www.dndbeyond.com/creator-faq)
- [SRD 5.2.1 legal text](https://media.dndbeyond.com/compendium-images/srd/5.2/SRD_CC_v5.2.pdf)
- [SRD 5.2.1 conversion guide](https://media.dndbeyond.com/compendium-images/srd/guide/converting-to-srd-5.2.1.pdf)

### JSON

- [Jackson Databind](https://github.com/FasterXML/jackson-databind)
- [Jackson Documentation](https://github.com/FasterXML/jackson-docs/wiki)
