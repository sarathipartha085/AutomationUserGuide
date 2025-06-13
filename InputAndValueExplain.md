## 🔧 Input Field IDs and Values (`!!`, `!`, `*`, `@`, `$`)

This system supports **special character prefixes** to dynamically control input behavior. These prefixes apply to either the **input field ID** (`idKey`) or the **value**, and they are processed in the following **strict order**:

> 🧠 **Order of Evaluation:** `!!` → `!` → `*` → `@` → `$`

---

### 🔹 Prefixes on Input Field ID (`idKey`)

| Prefix | Description                                                                                                                           |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| `!!`   | **Make the step optional**. If any error occurs, the step will be skipped and test will continue. *(Used mostly for fragile inputs.)* |
| `!`    | **Skip post-input validation**. Prevents validation of the value after entering it.                                                   |
| `*`    | **Press Enter** after entering the value instead of the default Tab key. Useful for triggering autocomplete or instant validation.    |

✅ These can be **combined** in this order:
`!!`, `!`, `*`

#### ➕ Examples of Combinations:

| ID Key Input   | Behavior                                  |
| -------------- |-------------------------------------------|
| `*searchBox`   | Press `Enter` after entering the value    |
| `!!*searchBox` | Optional step + press Enter               |
| `!*fieldId`    | Skip validation + press Enter             |
| `!!*fieldId`   | Optional + skip validation + press Enter  |
| `!fieldId`     | Skip validation only                      |

---

### 🔹 Prefixes on Input Value (`value`)

| Prefix | Description                                                                                                   |
| ------ | ------------------------------------------------------------------------------------------------------------- |
| `@`    | **Fetch value from the database** using a configured SQL query key. The result is saved to `ScenarioContext`. |
| `$`    | **Retrieve value from ScenarioContext** (usually from a previously saved step or DB value).                   |

---

## ✅ Evaluation Sequence (Full Order)

| Step | Prefix | Applies To | Action                                                                        |
| ---- | ------ | ---------- | ----------------------------------------------------------------------------- |
| 1️⃣  | `!!`   | `idKey`    | Make the step optional. Any errors will be caught and logged; test continues. |
| 2️⃣  | `!`    | `idKey`    | Skip validation after input                                                   |
| 3️⃣  | `*`    | `idKey`    | Press `Enter` after input instead of default `Tab`                            |
| 4️⃣  | `@`    | `value`    | Fetch from DB using config key and save to `ScenarioContext`                  |
| 5️⃣  | `$`    | `value`    | Pull from `ScenarioContext`                                                   |

---

## 💡 Examples

### ✔️ Full Combination (Optional + Skip Validation + Press Enter + Context)

```gherkin
And User enters value "$IIS.chargeCode" into input with id "!!*trsChargesListGridTbl_Id_M08MT_gs_trsCHARGESVO.CODE" where label is "Charge Code"
```

| Prefix | Meaning                                               |
| ------ | ----------------------------------------------------- |
| `!!`   | Step is optional                                      |
| `*`    | Press Enter after value is entered                    |
| `$`    | Fetch from `ScenarioContext` (key = `IIS.chargeCode`) |

---

### ✔️ Use Value from Context Only

```gherkin
And User enters value "$dealNumber" into input with id "deal_input" where label is "Deal No"
```

* Retrieves value stored as `dealNumber` earlier in the test

---

### ✔️ Use Database Result as Input

```gherkin
And User enters value "@latestCustomerId" into input with id "customer_id" where label is "Customer"
```

* Runs configured SQL and stores value as `latestCustomerId` in ScenarioContext

---

### ✔️ Optional Field Entry Only

```gherkin
And User enters value "Optional123" into input with id "!!optionalField" where label is "Optional Field"
```

* If the field is missing or input fails, the step is logged and skipped

