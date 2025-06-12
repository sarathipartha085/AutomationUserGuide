
## Input Field IDs and Values (`!`, `*`, `@`, `$`)

This system supports **special character prefixes** to modify behavior dynamically. These prefixes apply either to the **input field ID** or the **input value** and are interpreted in this strict order:

> **Order of Evaluation:** `!` → `*` → `@` → `$`

---

### Prefixes on Input Field ID (`idKey`)

| Prefix | Description                                                                                                                        |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| `!`    | **Skip post-input validation**. Prevents validation of the value after entering it.                                                |
| `*`    | **Press Enter** after entering the value instead of the default Tab key. Useful for triggering autocomplete or instant validation. |

You can **combine** these prefixes:

* `!*elementId`
* `*elementId`
* `!elementId`

---

### Prefixes on Input Value (`value`)

| Prefix | Description                                                                                                      |
| ------ | ---------------------------------------------------------------------------------------------------------------- |
| `@`    | **Fetch value from the database** using a configured SQL query key. Value is also stored into `ScenarioContext`. |
| `$`    | **Retrieve value from ScenarioContext**. Useful for using previously saved values.                               |

---

## ✅ Evaluation Sequence

Here's how the step processes both ID and value in exact order:

1. **`!`** – If present on `idKey`, disable validation after entering value.
2. **`*`** – If present on `idKey`, press `Enter` after input instead of `Tab`.
3. **`@`** – If present on `value`, run DB query (from config) and store the result.
4. **`$`** – If present on `value`, retrieve value from `ScenarioContext`.

---

## 💡 Examples

### ✔️ Full Combination (Skip validation, press Enter, DB lookup)

```gherkin
    And User enters value "$IIS.chargesCode" into input with id "!*trsChargesListGridTbl_Id_M08MT_gs_trsCHARGESVO.CODE" where label is "Charge code"
```

* `!` → skip validation
* `*` → press Enter
* `@latestCustomerId` → fetch value from DB
* Final ID: `customer_id_input`

---

### ✔️ Use Context Variable

```gherkin
And User enters value "$dealNumber" into input with id "deal_input" where label is "Deal No"
```

* Pulls value from ScenarioContext with key `dealNumber`

---

### ✔️ Trigger Enter Only

```gherkin
And User enters value "12345" into input with id "*searchBox" where label is "Search"
```

* Inputs value and presses Enter
* Then moves forward without validation skipping

---

### ✔️ Skip Validation Only

```gherkin
And User enters value "Optional" into input with id "!optionalField" where label is "Optional Field"
```

* No validation occurs after input
