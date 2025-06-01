# AppNet Element Specification Documentation

An **AppNet element** defines the logic for an application network function (ANF) in a modular, programmable way. Elements process RPC requests and responses using match-action rules and can maintain typed local or shared state.

---

## Element Structure

Each element specification includes the following sections:

### 1. `state`
Defines local or shared, typed state variables. Maps and other types are supported.

```plaintext
state:
  firewall_table: Map<string, string>
````

### 2. `init()`

Optional initialization logic for state variables.

```appnet
init():
  set(firewall_table, 'test', 'DENY')
```

### 3. `req(rpc)`

Match-action rules for processing **RPC requests**. Match expressions can be nested, with pattern matching on `Some(...)` and `None` for optional types.

```plaintext
req(rpc):
  match get(acl, get(rpc, 'username')):
    Some(permission) =>
      match permission:
        'ALLOWED' =>
          send(rpc, Down)
        'DENY' =>
          send(err('firewall'), Up)
    None =>
      send(rpc, Down)
```

### 4. `resp(rpc)`

Logic for processing **RPC responses**.

```plaintext
resp(rpc):
  send(rpc, Up)
```

---

## Syntax Reference

### Typed State Declaration

```appnet
state:
  variable_name: Type
```

Examples:

* `Map<string, string>`
* `int`, `float`, `bool`

### Pattern Matching with Option Types

AppNet supports optional value semantics:

```appnet
match get(map, key):
  Some(value) => ...
  None => ...
```

---

## Common Actions

* `send(rpc, Down)`: Forward the RPC to the next element.
* `send(rpc, Up)`: Return the RPC to the previous element.
* `send(err("msg"), Up)`: Send an error upstream.
* `set(map, key, value)`: Write to a map or state variable.
* `get(map, key)`: Read a value from state; returns `Some(v)` or `None`.

---

## Full Example: Application Firewall

```appnet
state:
  firewall_table: Map<string, string>

init():
  set(firewall_table, 'test', 'No')

req(rpc):
  match get(acl, get(rpc, 'username')):
    Some(permission) =>
      match permission:
        'ALLOWED' =>
          send(rpc, Down)
        'DENY' =>
          send(err('firewall'), Up)
    None =>
      send(rpc, Down)

resp(rpc):
  send(rpc, Up)
```

This firewall element checks a body field of the RPC against an access control list (`firewall_table`). If the value is `'ALLOWED'`, it allows the request. If `'DENY'`, it blocks the request. If there's no entry (`None`), the request is allowed by default.

---

## Runtime Behavior

* **Down**: sends the request to the next element or the microservice.
* **Up**: returns a message to the previous element.

---

