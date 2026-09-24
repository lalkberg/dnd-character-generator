# D&D Character Creator — MVP Tasks

This document expands the roadmap into implementation tasks. Treat these as a backlog rather than a strict requirement to complete every task exactly as written.

The tasks intentionally avoid implementation code. Use the linked documentation and your own research to decide how to implement each item.

---

# Milestone 1 — Set up the Java development environment

## 1.1 Install a JDK

- Install a current LTS JDK suitable for the project.
- Prefer Java 25 LTS for this project unless you have a specific reason to target another version.
- Verify that the Java compiler and runtime are available from your terminal.
- Learn the difference between the JDK, the JVM, and the Java runtime.
- Make sure your IDE is configured to use the same JDK.

## 1.2 Install Maven

- Install Apache Maven.
- Verify that Maven is available from the terminal.
- Learn what Maven does and why a Java project benefits from a standard build tool.
- Read the Maven Getting Started Guide before creating the project.

## 1.3 Create the Maven project

- Create a new directory for the project.
- Initialize a Git repository.
- Create a Maven Java project.
- Choose a sensible group/package name that you expect to keep for the lifetime of the project.
- Choose the application artifact name.
- Inspect the generated project instead of immediately replacing it with your own structure.
- Identify the purpose of the source, test, and resource directories.
- Understand what the `pom.xml` file represents.
- Build the empty project from the terminal.

## 1.4 Create the first application

- Add the application's entry point.
- Make the application start successfully.
- Make it print a simple message.
- Run it from your IDE.
- Run it through Maven.
- Confirm that both approaches use the same project configuration.

## 1.5 Add JUnit

- Add JUnit 5 as a test dependency.
- Create one deliberately simple test.
- Run the test from the IDE.
- Run the same test suite through Maven.
- Learn what a unit test is and what the Arrange/Act/Assert pattern means.

## 1.6 Establish the repository basics

- Add a `.gitignore` appropriate for Java and your IDE.
- Create a README.
- Document the Java and Maven versions you are targeting.
- Make the first Git commit.
- Verify that the repository can be cloned and built from scratch.

---

# Milestone 2 — Establish the project structure and domain boundaries

## 2.1 Define the initial domain vocabulary

Write down the nouns your application needs before creating classes.

At minimum consider:

- Character
- Ability
- Ability score
- Species
- Class
- Background
- Level
- Feature
- Proficiency
- Spell
- Equipment

For each concept, decide whether it represents:

- an individual character value
- a reusable game definition
- an external data record
- a calculated value
- a user choice

## 2.2 Decide what should be an enum

Identify concepts that are genuinely closed and stable.

Good candidates may include the six abilities and some internal categories.

Do not automatically make every D&D entity an enum. Anything expected to come from external data should generally remain data-driven.

## 2.3 Decide what should be a record or class

Research Java records before deciding how to model small immutable values.

Consider records for simple values where identity and behavior are minimal.

Consider normal classes where the object has meaningful behavior, invariants, lifecycle, or internal state.

Do not worry about getting this perfect. The purpose is to make an initial decision you can revise.

## 2.4 Establish package boundaries

Create a package structure that separates:

- domain concepts
- application/use-case logic
- CLI concerns
- data/infrastructure concerns

Keep the initial number of packages small.

Do not create dozens of empty packages merely because a future architecture diagram contains them.

## 2.5 Model Character

Define what information a Character must contain.

Decide:

- which fields are mandatory
- which can be absent
- which values are derived
- which values belong to other objects
- whether a Character should be mutable

Write down these decisions in a short design note if useful.

## 2.6 Model ability scores

Define a representation for the six abilities.

Decide how the application will:

- store scores
- retrieve a score for a specific ability
- calculate an ability modifier
- represent an invalid score

Write tests for the modifier rules as soon as the behavior exists.

## 2.7 Review the model

Before moving on, inspect the domain model for:

- duplicated concepts
- classes that know too much
- dependencies on CLI code
- dependencies on JSON
- premature abstractions
- hardcoded D&D data

Commit the milestone once you are satisfied with the basic boundaries.

---

# Milestone 3 — Build the first end-to-end character creation flow

## 3.1 Design the user flow

Write the CLI flow on paper before implementing it.

For example:

- start
- name
- species
- class
- background
- level
- ability scores
- confirmation
- summary

Decide what should happen when the user enters invalid input.

## 3.2 Build reusable CLI input handling

Create a small CLI input component responsible for:

- reading text
- reading numbers
- presenting choices
- validating simple input
- repeating a question when necessary

Keep D&D rules out of this component.

## 3.3 Implement the first character creation use case

Create an application-level flow that:

- asks for the required values
- constructs the character
- returns the resulting character

Do not implement every D&D rule yet.

Use a very small amount of placeholder data if necessary.

## 3.4 Add input validation

Handle:

- empty names
- unsupported selections
- invalid numbers
- levels outside the supported range
- unexpected input

Make sure invalid input does not terminate the entire application unexpectedly.

## 3.5 Add a basic summary

Print a simple character summary after creation.

Do not spend time making it beautiful yet.

The objective is proving that information can travel cleanly from CLI input into the domain and back out again.

---

# Milestone 4 — Implement ability scores

## 4.1 Research the rules

Read the relevant SRD section for ability scores and character creation.

Do not implement from memory.

Record the exact rules you intend to support.

## 4.2 Define the generation abstraction

Decide how the application will represent different ways of obtaining ability scores.

The goal is that the character creation flow can choose a strategy without containing the actual dice/score-generation implementation.

## 4.3 Implement the predefined generation method

Implement the chosen standard/predefined method from the selected ruleset.

Add validation and tests.

## 4.4 Implement dice rolling

Create a small dice abstraction.

Decide how randomness will be represented so it can be replaced or controlled during tests.

Implement the required ability-score rolling procedure from the SRD.

Test edge cases.

## 4.5 Implement manual entry

Allow the user to enter all six ability scores.

Validate:

- number of values
- allowed score range
- any other restrictions imposed by the chosen generation method

## 4.6 Test the rules thoroughly

Add tests for:

- ability modifiers
- minimum and maximum supported values
- dice generation
- predefined generation
- invalid manual input
- deterministic behavior where possible

Avoid tests that depend on the actual random output of production randomness.

---

# Milestone 5 — Import and bundle SRD data

## 5.1 Choose the exact data source

Decide which source will be used to populate your bundled data.

Prefer data that corresponds clearly to SRD 5.2.1 and the 2024/5.5 rules.

Document:

- source repository/API
- source version
- SRD version
- license
- date of import

## 5.2 Review licensing before redistributing data

Read the official SRD 5.2.1 legal text.

Confirm what attribution is required.

Do not assume that a third-party API's software license automatically applies to all game data hosted by that API.

Create a `NOTICE` or equivalent attribution document before bundling the data.

## 5.3 Inspect the source data manually

Before writing an importer, explore the data.

Identify:

- identifiers
- names
- references between entities
- nested structures
- optional properties
- arrays
- text fields
- version-specific differences

Pay particular attention to how the source represents relationships.

## 5.4 Decide your internal data boundary

Define what your domain model needs from the external data.

Do not copy the external API's entire schema into your domain model simply because it is available.

Treat the external schema as an input format.

## 5.5 Choose a JSON library

Evaluate Jackson or another established Java JSON library.

Read enough documentation to understand:

- deserialization
- serialization
- mapping JSON to Java objects
- handling optional/missing properties
- error handling

## 5.6 Implement local data loading

Create a data provider/repository abstraction that can read the bundled resources.

The rest of the application should ask for game data without knowing whether it came from:

- a JSON resource
- an API
- another storage mechanism

## 5.7 Bundle resources with Maven

Place the selected data under the project's resources.

Build the application artifact.

Verify that the resources are actually included in the built artifact.

## 5.8 Prove offline operation

Disable network access.

Run the application.

Confirm that basic data lookup still works.

This is an important acceptance test for the project's offline-first goal.

---

# Milestone 6 — Implement character creation rules

## 6.1 Read the SRD character creation sections

Read the relevant SRD sections for:

- character creation
- character origins
- species
- backgrounds
- classes
- levels
- feats
- ability scores

Keep notes on rules that affect character creation.

## 6.2 Model reusable definitions

Create data-backed representations for the supported:

- species
- classes
- backgrounds
- features
- proficiencies
- other required definitions

Avoid putting every individual game option into Java source.

## 6.3 Introduce a character creation session

Design a representation for an in-progress character.

It should be possible for the user to make choices incrementally without pretending the character is already complete.

## 6.4 Implement level selection

Decide what level range the MVP supports.

If the MVP supports multiple levels, ensure that the selected level drives progression rather than merely being stored as a number.

## 6.5 Implement species/background/class effects

Apply the relevant effects of the selected options.

Do this in small increments.

After each category works, add tests.

## 6.6 Implement progression

Ensure that selecting a level results in the correct supported features and other level-dependent values.

Avoid scattering level checks throughout the application.

## 6.7 Implement validation

Create a dedicated validation mechanism.

It should be able to report multiple problems when practical rather than stopping after the first invalid choice.

Examples:

- invalid ability score allocation
- missing required selection
- invalid class option
- invalid level
- invalid number of selected items

## 6.8 Handle unsupported rules explicitly

If a rule is outside the MVP:

- detect it
- report it clearly
- do not silently create an incorrect character

This is preferable to pretending the application supports a rule it does not.

---

# Milestone 7 — Add derived character statistics

## 7.1 Identify all required derived values

Using the SRD, make a list of the values needed for a useful character sheet.

Separate:

- directly selected/stored values
- calculated values
- values derived from multiple rules

## 7.2 Create calculation responsibilities

Decide which calculations belong together.

Avoid creating one giant calculator containing every rule.

Keep calculations independently testable.

## 7.3 Implement proficiency bonus

Implement and test the proficiency progression relevant to the supported levels.

## 7.4 Implement saving throws

Calculate saving throw modifiers from:

- ability scores
- class/species/background effects where applicable
- proficiency
- other supported modifiers

## 7.5 Implement skills

Calculate skill modifiers from the relevant ability and proficiencies.

Ensure the source data identifies the correct ability association.

## 7.6 Implement combat statistics

Implement the supported calculations for:

- hit points
- armor class
- initiative
- speed
- other essential combat values

Keep equipment effects in mind when designing the calculation boundary.

## 7.7 Implement tests

For every important calculation:

- test normal cases
- test boundary cases
- test proficiency cases
- test combinations of modifiers

Use SRD examples or hand-calculated scenarios to verify your implementation.

---

# Milestone 8 — Add spells and spellcasting

## 8.1 Inspect spell data

Study how spells are represented in the chosen source.

Identify:

- level
- school
- casting information
- classes
- components
- duration
- range
- concentration
- ritual status
- description
- other fields needed for the sheet

Decide which fields are actually required for MVP.

## 8.2 Model spell definitions

Create a representation suitable for displaying and selecting spells.

Do not implement spell effects.

## 8.3 Model spellcasting state

Separate:

- spell definitions
- the spells available to a class
- the spells selected by a character
- spell slots/uses

## 8.4 Implement spellcasting calculations

Implement the supported calculations for:

- spellcasting ability
- spell save DC
- spell attack modifier
- spell slots

Test them independently.

## 8.5 Implement spell selection

Support the selection behavior required by the classes included in the MVP.

Include validation for:

- too many selections
- invalid spell level
- unavailable spells
- missing required selections

## 8.6 Add automatic spell selection

Provide an option such as "Choose automatically" if it is practical.

Keep the algorithm simple and deterministic.

The MVP does not need to optimize a character's spell choices.

---

# Milestone 9 — Add equipment

## 9.1 Inspect equipment data

Identify the equipment information required for a character sheet.

Prioritize:

- armor
- weapons
- starting equipment
- relevant properties
- damage
- AC
- quantity

## 9.2 Model equipment

Separate:

- equipment definitions
- character-owned equipment
- equipped items

Do not build a complete inventory system.

## 9.3 Implement starting equipment

Implement the equipment choices required by the supported classes/backgrounds.

## 9.4 Implement automatic equipment selection

If practical, allow the user to choose between:

- making selections manually
- using a sensible default/automatic selection

## 9.5 Integrate equipment with calculations

Ensure equipment can affect the relevant derived values.

Add tests for common cases.

---

# Milestone 10 — Build the character sheet output

## 10.1 Design the terminal layout

Sketch the character sheet on paper first.

Group information into sections.

Possible sections:

- identity
- abilities
- combat
- saves and skills
- proficiencies
- features
- equipment
- spells

## 10.2 Create a sheet/output model

Decide what data the renderer needs.

Do not let the renderer query the rule engine or recalculate values.

## 10.3 Implement a terminal renderer

Create a dedicated presentation component.

Keep formatting logic out of domain objects.

## 10.4 Handle long content

Decide how to display:

- long feature descriptions
- long spell descriptions
- lists of equipment
- multiple spells

Keep the initial output readable rather than trying to perfectly reproduce an official character sheet.

## 10.5 Test representative output

Create at least a few representative generated characters.

Verify that the output contains the important sections and values.

---

# Milestone 11 — Save and load characters

## 11.1 Define the character file format

Choose JSON for the MVP.

Decide what belongs in the persisted representation.

Prefer storing durable user choices and character state over storing values that can always be recalculated.

## 11.2 Add a format version

Include a format version from the beginning.

This allows the file format to evolve later.

## 11.3 Implement save

Add a CLI command or workflow for saving a character.

Handle:

- missing directories
- invalid file names
- existing files
- IO failures

## 11.4 Implement load

Allow a previously saved character to be loaded.

Validate the loaded data.

Provide understandable error messages.

## 11.5 Test round trips

Test:

- create
- save
- load
- compare

Ensure that important character information survives the round trip.

## 11.6 Consider ruleset versioning

Store enough information to identify which rules/data version the character was created against.

This becomes important when the bundled rules data changes later.

---

# Milestone 12 — Package the MVP

## 12.1 Define the supported environment

Document:

- required Java version
- supported operating systems
- whether Java must be installed by the user
- how to build from source

## 12.2 Create a runnable artifact

Configure Maven so that the project can produce a runnable application artifact.

Prefer a simple JAR-based distribution for the first release.

## 12.3 Verify bundled resources

Build the artifact and inspect it.

Confirm that:

- SRD data is included
- application classes are included
- required dependencies are available
- the application works outside the IDE

## 12.4 Test from a clean environment

Use a machine/container/environment that does not contain the project's build artifacts.

Follow the README exactly.

Fix anything that requires undocumented knowledge.

## 12.5 Write the attribution documentation

Ensure the repository contains the required SRD attribution.

Clearly distinguish:

- your source code
- third-party libraries
- SRD content
- other external data

## 12.6 Document the data pipeline

Explain:

- where the data comes from
- which version was imported
- how it was transformed
- how someone can update it
- which license applies

---

# Milestone 13 — MVP acceptance pass

## 13.1 Freeze the feature list

Do not add new major features during this milestone.

Create a list of everything the MVP promises to support.

Anything else should be explicitly listed as future work.

## 13.2 Test complete character creation

Create representative characters covering:

- non-spellcaster
- spellcaster
- different species
- different backgrounds
- different levels
- different ability-score methods

## 13.3 Test invalid input

Try to break the CLI deliberately.

Test:

- empty values
- invalid numbers
- out-of-range levels
- invalid selections
- invalid spell choices
- invalid equipment choices
- invalid saved files

## 13.4 Test offline behavior

Disable networking completely.

Perform the full create/save/load flow.

## 13.5 Run the complete test suite

Run all tests through Maven.

Do not consider the MVP complete if tests are being skipped or ignored.

## 13.6 Review the architecture

Look for:

- domain classes that know about the CLI
- CLI classes that contain D&D rules
- duplicated calculations
- unnecessary abstractions
- hardcoded external data
- classes with too many responsibilities

Refactor only issues that materially improve the MVP.

## 13.7 Final documentation pass

Update the README with:

- project description
- requirements
- installation
- build instructions
- usage
- supported ruleset
- supported features
- known limitations
- data source
- licensing/attribution

## 13.8 Tag the MVP

Create a Git tag for the MVP release.

Record the exact rules/data version used by that release.

---

# Suggested research order

When you need to learn something, use this order:

1. Official Java documentation
2. Official library/build-tool documentation
3. Small examples from official repositories
4. Your IDE's documentation
5. Community discussions when official documentation does not answer the question

Try to avoid starting with a generated implementation.

For this project, useful searches are often:

- Java records
- Java interfaces
- Java collections
- Java exceptions
- Maven lifecycle
- Maven resources
- JUnit parameterized tests
- Jackson JSON deserialization
- Java `Random`
- Java file I/O
- Java command-line arguments

---

# MVP completion checklist

- [ ] Project builds from a fresh checkout
- [ ] Tests run through Maven
- [ ] CLI starts successfully
- [ ] Character name can be entered
- [ ] Species can be selected
- [ ] Class can be selected
- [ ] Background can be selected
- [ ] Level can be selected
- [ ] Ability scores can be generated using all supported methods
- [ ] Character choices are validated
- [ ] Important derived statistics are calculated
- [ ] Supported spellcasting works
- [ ] Starting equipment can be generated
- [ ] Character sheet is readable
- [ ] Characters can be saved
- [ ] Characters can be loaded
- [ ] Application works offline
- [ ] SRD data is versioned
- [ ] Required attribution is present
- [ ] README explains how to use the application
- [ ] MVP scope and limitations are documented
- [ ] MVP is tagged in Git
