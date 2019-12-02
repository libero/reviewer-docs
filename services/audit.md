# Audit Service

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
      <td style="text-align:left">
        <p>&lt;code&gt;&lt;/code&gt;<a href="https://github.com/libero/audit/blob/b51a6157c3b773dffdd3bef4650e1f70f278f302/src/domain/types.ts#L8"><code>AuditRepository</code></a>&lt;code&gt;&lt;/code&gt;</p>
        <p>&lt;code&gt;&lt;/code&gt;<a href="https://github.com/libero/audit/blob/b51a6157c3b773dffdd3bef4650e1f70f278f302/src/domain/types.ts#L1"><code>AuditLogItem</code></a>&lt;code&gt;&lt;/code&gt;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Dependents</b>
      </td>
      <td style="text-align:left"><a href="event-bus.md">Event Bus</a>
      </td>
    </tr>
  </tbody>
</table>

