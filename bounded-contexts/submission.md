# Submission Server

## Summary

|   | Description / Link |
| :--- | :--- |
| **Responsibility** | The submission server has the responsibility of serving data to the [Reviewer Client](../reviewer-client.md) within the bounded context of "Submission". |
| **Repository** | [https://github.com/libero/reviewer-submission](https://github.com/libero/reviewer-submission) |
| **Interfaces** | `GraphQL` |
| **Dependents** | Postgres |

## Design

At the moment there is only one server \(Nest.js\) with one bounded context \(submission\). In the future there needs to be a decision as to whether all the bounded contexts for the server will live in one repository or seperate ones - either as packages or seperate servers in themselves.

TBD - how what and where 

## Configuration

* database connection / config

## Deploy & Test

* Explain  docker 
* Explain tests
* 
## Examples

* Storage format
* Example Code see continuum-auth

```text
FROM xxxx
```



