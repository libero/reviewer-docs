---
description: Approach to building Bounded Contexts
---

# Design Principles

## Background

The design of all Bounded Contexts will follow a Domain Driven Design \(DDD\) pattern. This will follow a hexagonal type architecture following a similar pattern as in [this tutorial](https://vaadin.com/learn/tutorials/ddd/ddd_and_hexagonal).

The rest of this document details the principles we have agreed to follow, in order to have a common approach when in comes to coding.

## Layering

Traditionally there are the following layers:

* **UI Layer** - this is handled by the [Reviewer Client](../reviewer-client.md)
* **Application Layer** - Any _Bounded Context_ \(for example [Submission](submission.md)\) will expose a number of resolvers that will provide the Port/Adapter necessary for _Reviewer Client_ to connect.  This layer provides the functionality to perform the Business operations in terms of using the Domain Services within the Domain Layer. 
* **Domain Layer** - Provides functionality in domain-parlance, this aids development in terms of the ubiquitous language. 
* **Infrastructure Layer** - Provides the raw CRUD operations for persisting the state of the Domain.

Taking the 3 layers above that are implemented in terms of the bounded context on the server, these will be expressed in-terms of the following classifications.

## Software Concepts

### Resolvers

{% hint style="info" %}
Responsible for exposing the functionality over GraphQL
{% endhint %}

#### Principles

* Network Security 
* Delgation to Service\(s\)

#### Example

```text
const resolvers = (submissionService: SubmissionService): IResolvers => ({
    Query: {
        async getSubmissions(): Promise<DtoViewSubmission[] | null> {
            return await submissionService.findAll();
        },
    ...
```

### Application Services

{% hint style="info" %}
Responsible for providing the application's functionality.
{% endhint %}

The Application Services should contain functions to provide a one-to-one mapping of the functionality exposed by the _Resolvers._ 

* **Services** - These

#### Principles

* Application Security / Permissions
* Needs to be stateless - Transactional Management? 
* Services are hierarchical, i.e. services may contain / use other services.

#### Example

```text
class SubmissionService {
    ...
    submitManuscript(userId, submissionId) {
        if (!userService.hasPermission(userId, SUBMIT) ||
            !userService.isAuthor(userId, submissionId)) {
            throw 
        }
        return exportService.export(userId, submissionId)
    }
    ...    
}
```

### Domain Model / Services

Currently we are not discriminating between "Application Services" and "Domain Services"

### Repositories

{% hint style="info" %}
Responsible for storing the state of the Domain
{% endhint %}

#### Principles

* Data Mappers are internal to the repository.
* Any internal peculiarities of storing data are encasulated here \(don't leak\)  

#### Example

```text
export class KnexSubmissionRepository implements SubmissionRepository {
    private readonly TABLE_NAME = 'manuscript';

    public constructor(private readonly knex: Knex<{}, unknown[]>) {}
    ...
    public async update(sub: Submission): Promise<Submission | null> {
        const submission = await this.findById(sub.id);
        if (submission === null) {
            return null;
        }
        const dtoSubmission: DtoSubmission = SubmissionMapper.toDto(sub);
        const dtoToSave = { ...SubmissionMapper.toDto(submission), ...dtoSubmission, updated: new Date() };
        await this.knex
            .withSchema('public')
            .insert(dtoToSave)
            .into(this.TABLE_NAME);

        return SubmissionMapper.fromDto(dtoToSave);
    }
...
```





