# Submission Server

## Summary

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Description / Link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Responsibility</b>
      </td>
      <td style="text-align:left">To audit, by listening to <code>audit</code>events from the event bus and
        then recording them to the database. The responsibility of the other components
        is to generate these <code>audit</code>messages.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Repository</b>
      </td>
      <td style="text-align:left"><a href="https://github.com/libero/audit">https://github.com/libero/audit</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Interfaces</b>
      </td>
      <td style="text-align:left"><code>GraphQL</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Dependents</b>
      </td>
      <td style="text-align:left">
        <p><a href="../services/event-bus.md">Event Bus</a>
        </p>
        <p>Postgres</p>
      </td>
    </tr>
  </tbody>
</table>## Design

how what and where 

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



