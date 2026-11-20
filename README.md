# Fitness Club (Java)

A console Java application for managing a fitness club — clients, subscriptions and trainings — built as a showcase of classic design patterns and a layered architecture.

**University project — BSU, Faculty of Mechanics and Mathematics**

## Features

- Clients, subscriptions (`MONTHLY`, `YEARLY`, `VISIT_BASED`; states such as active / frozen) and trainings (`GROUP`, `PERSONAL`).
- Subscription service: create, freeze, activate and check expiration.
- Training registration that validates the training, applies a group/personal strategy and notifies subscribed clients.
- Loading trainings from a CSV-like file (`resources/data/trainings.txt`); invalid lines are validated and reported instead of crashing the run.
- Logging to console and `logs/app.log` through Log4j2.

## Design patterns (where they are used)

| Pattern | Implementation |
|---|---|
| Factory | `factory/SubscriptionFactory` — returns `Optional<Subscription>`, also offers `Supplier`-based variants |
| Builder | `factory/TrainingBuilder` — step builder (client id → coach → type are required) plus a classic fluent builder |
| Strategy | `strategy/TrainingStrategy` with `GroupTrainingStrategy` and `PersonalTrainingStrategy` |
| Template Method | `template/RegistrationTemplate.register()` (validate → reserve slot → apply strategy → notify) with group and personal subclasses |
| Observer | `observer/` — `TrainingScheduleObservable` notifies `ClientObserver`s |
| Specification | `specification/` — filter trainings by client, coach, date, duration or type via `TrainingRepository.query(...)` |
| Repository | `repository/` — in-memory repositories for clients, trainings, subscriptions |

Also: `Validator<T>` implementations, service interfaces with `impl` classes, and a custom exception hierarchy (`FitnessException`, `ServiceException`, `RepositoryException`, `ValidationException`).

## Project structure

```
src/main/java/by/bsu/fitness/
  entity/  repository/  service(/impl)/  specification/  strategy(/impl)/
  observer/  template/  factory/  validator/  exception/  util/  main/FitnessMain.java
src/test/java/by/bsu/fitness/   TestNG tests mirroring the main packages
resources/                      log4j2.xml, data/trainings.txt
lib/                            TestNG, Log4j2, SLF4J and other jars
```

## Tech stack

Java (the IntelliJ project is configured for JDK 24), TestNG 7, Log4j2.

## How to run

1. Open the folder in IntelliJ IDEA (it is a plain IntelliJ module, `fitness-club.iml`, not Maven or Gradle); add the jars from `lib/` as libraries if they are not picked up.
2. Mark `src/main/java` as sources, `src/test/java` as tests and `resources` as resources.
3. Run `by.bsu.fitness.main.FitnessMain`. The sample data is read from `resources/data/trainings.txt`.

## Tests

There are 22 TestNG test classes (196 test methods in the committed TestNG report, all passing) covering entities, repositories, services, validators, specifications, strategies, the template, factory/builder, observer and utilities. Run them from IntelliJ or with TestNG on the classpath.

## Author

Ksenia Plehova — [GitHub](https://github.com/kseniaplehova)
