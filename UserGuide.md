# 🧾 User Guide for this Framework

This guide explains how to write Cucumber steps to interact with iMAL Java Applications using the defined Gherkin step definitions.

## ⚠️ **Important Note about `LABEL` Parameter**

> ### 🏷️ **`LABEL` is *only* used for debugging, logging, and readability.**
>
> * It does **not affect test execution logic** or DOM element selection.
> * Always use meaningful `LABEL`s for better **traceability** in test reports and console outputs.

---
## 1. Login to Application

```gherkin
Given User login to application "APP_NAME" and module "MODULE_NAME" with username "USERNAME" and password "PASSWORD"
```

* **APP\_NAME**: Name of the application (used to form config key).
* **MODULE\_NAME**: Module under the application.
* **USERNAME / PASSWORD**: If empty, default credentials from config are used.

---

## 2. Input Text Field

```gherkin
And User enters value "VALUE" into input with id "ELEMENT_ID" where label is "LABEL"
```

* **VALUE**: Text to input.
* **ELEMENT\_ID**: ID mapped to the HTML input.

---

## 3. Set Date

```gherkin
And User sets date "DD-MM-YYYY" into input with id "ELEMENT_ID" where label is "LABEL"
```

Used for date picker input fields.

---

## 4. Click Button

```gherkin
And User clicks button with id key "BUTTON_ID" where label is "LABEL"
```

* For clicking buttons by ID.

---

## 5. Click Anchor (Link)

```gherkin
And User clicks anchor with label key "LABEL_KEY" where label "LABEL"
```

* **LABEL\_KEY**: Internal label key for anchor.

---

## 6. Double Click

```gherkin
And User double-clicks id key "ELEMENT_ID" where label is "LABEL"
```

Used when an element requires double-clicking (e.g., rows in a grid).

---

## 7. Select Dropdown

```gherkin
And User selects "OPTION_TEXT" from dropdown with id "DROPDOWN_ID" where label is "LABEL"
```

Selects an option in a dropdown.

---

## 8. Set Checkbox

```gherkin
And User sets checkbox with id "CHECKBOX_ID" to "true/false" where label is "LABEL"
```

---

## 9. Delay

```gherkin
And User delay
```

Waits for a predefined time (likely a few seconds).

---

## 10. Open Screen from App Menu

```gherkin
When User opens screen "SCREEN_NAME" in app menu path:
  | Menu 1 |
  | Menu 2 |
  | Submenu |
```

Navigates through app menu to open a screen.

---

## 11. Handle Welcome Pop-up

```gherkin
And User clicks welcome popup
```

Automatically handles welcome or logout confirmation popups.

---

## 12. Validate and Save Deal Number

```gherkin
And User validate "POPUP_ID" and save result to "SAVE_KEY"
```

* Saves the extracted deal number into `ScenarioContext` for reuse.

---

# 🛠 Tips for Writing Feature Files

* Always match the ID keys and label keys with those defined in your application or mapping config.
* Use meaningful labels to help in logging and debugging.
* `ScenarioContext` allows transferring values across steps using keys.

---

## Example Feature Scenario - 1

```gherkin
Feature: Create Reason

  @CreateReason
  Scenario: Create Investment Deal without trade deal
    Given User login to application "IIS" and module "Params" with username "MODEL.B" and password "321"
    And User enters value "1" into input with id "lookuptxt_COMP_CODE" where label is "Company code"
    And User enters value "1" into input with id "lookuptxt_BRANCH_CODE" where label is "Branch code"
    And User clicks button with id key "ajaxlink" where label is "Continue"
    And User clicks welcome popup
    
    When User opens screen "Maintenance" in app menu path:
      | Parameters |
      | Charges    |

    
    And User enters value "@IIS.chargesCode" into input with id "trsChargesCode_M08MT" where label is "Reason code"
    And User enters value "Sarathi Automation Test" into input with id "!trsCHARGESVO_brief_name_eng_M08MT" where label is "Brief Description"
    And User enters value "Sarathi Automation Test Large Description" into input with id "!trsCHARGESVO_long_name_eng_M08MT" where label is "Long Description"

    And User clicks button with id key "trsChargesMainTab2_M08MT" where label is "Additional Details"
    And User enters value "100" into input with id "flat_amount_M08MT" where label is "Flat Amount"
    And User selects "Account Ccy" from dropdown with id "trscharges_flatamountcurrencytype_M08MT" where label is "Flat amount"

    And User clicks button with id key "trsChargesMaintFormId_M08MT_Save_key" where label is "Save charges"
    And User clicks button with id key "_popup_path_sol_confirm_ok" where label is "Confirmation Approve Popup"
    And User clicks button with id key "_popup_path_sol_ok" where label is "Confirmation Approve Popup"

    And User clicks button with id key "infoBarSearchButton_M08MT" where label is "Search"
    And User enters value "$IIS.chargesCode" into input with id "!*trsChargesListGridTbl_Id_M08MT_gs_trsCHARGESVO.CODE" where label is "Charge code"
    And User double-clicks id key "trsChargesListGridTbl_Id_M08MT_gs_trsCHARGESVO.CODE" where label is "Charge code"


    And User delay
```

---

## Example Feature Scenario - 1

```gherkin
Feature: Investment Deal without trade deal
  
  @CSM
  Scenario: Create Investment Deal without trade deal
    Given User login to application "test" and module "Params" with username "MODEL.B" and password "321"
    And User enters value "1" into input with id "lookuptxt_COMP_CODE" where label is "Company code"
    And User enters value "1" into input with id "lookuptxt_BRANCH_CODE" where label is "Branch code"
    And User clicks button with id key "ajaxlink" where label is "Continue"
    And User clicks welcome popup

    When User opens screen "Maintenance" in app menu path:
      | Investment Deals -Combined without Trade Deal |
    And User enters value "1" into input with id "lookuptxt_investmentDeals_PARTY_T022MT" where label is "Party"
    And User clicks button with id key "_popup_path_sol_confirm" where label is "Active Deleted /ReverseDeals Exists"
    And User enters value "3" into input with id "lookuptxt_trsdealVO_DEAL_TYPE_T022MT" where label is "Category"
    And User enters value "290" into input with id "lookuptxt_investmentDeals_CLASS_T022MT" where label is "Product Class"
    And User enters value "840" into input with id "lookuptxt_trsdealVO_DD_DEAL_CY_T022MT" where label is "Deal CY"
    And User enters value "10000" into input with id "trsdealVO_DD_DEAL_AMOUNT_T022MT" where label is "Amount"

    And User enters value "1" into input with id "trsdealVO_DD_PERIODICITY_NBR_T022MT" where label is "Tenure"
    And User selects "Years" from dropdown with id "trsdealVO_DD_PERIODICITY_TYPE_T022MT" where label is "Tenure"

    And User clicks button with id key "investmentDealsMainTabs2_T022MT" where label is "Contribution Details"

    And User selects first element where label is "Contributor Details"

    And User enters value "435123" into input with id "nostro_gl_T022MT" where label is "Nostro GL"
    And User enters value "0" into input with id "nostro_cif_T022MT" where label is "Nostro CIF"
    And User enters value "37" into input with id "lookuptxt_nostro_sl_T022MT" where label is "Nostro Sl"
    And User clicks button with id key "btnContribDetSave_T022MT" where label is "Save Contributor Detail"
    And User clicks button with id key "investmentDeals_Save_btn_T022MT" where label is "Save Deal"
    And User clicks button with id key "_popup_path_sol_ok" where label is "Save Confirmation Popup"

    And User clicks button with id key "repayPlanBtn_T022MT" where label is "Click Repayment Plan"
    And User enters value "12" into input with id "No_Of_Payments_T022MT" where label is "No of Payments"
    And User enters value "1" into input with id "paym_periodn_nbr_T022MT" where label is "Payment period"
    And User selects "Months" from dropdown with id "paymPeriodicity_T022MT" where label is "Payment periodicity"
    And User sets checkbox with id "rep_grace_period_option_T022MT" to "false" where label is "Grace period"
    And User clicks button with id key "createSchedule_T022MT" where label is "Click create Repayment Plan"
    And User clicks button with id key "_popup_path_sol_confirm_ok" where label is "Create Confirmation Repayment plan Popup"

    And User clicks the repayment plan close button

    And User clicks button with id key "investmentDeals_Validate_btn_T022MT" where label is "Validate"
    And User validate "div__popup_path_sol_ok" and save result to "deal_number"

    And User clicks button with id key "_popup_path_sol_ok" where label is "Okay confirmation"

    And User enters value "11" into input with id "alertsGrid_Id_T022MT_gs_alertsParamCO.userId" where label is "Alert"
    And User double-clicks id key "alertsGrid_Id_T022MT_gs_alertsParamCO.userId" where label is "Alert"

  @CSM
  Scenario: Investment Deal without trade deal
    Given User login to application "test" and module "Params" with username "12" and password "111"
    And User enters value "1" into input with id "lookuptxt_COMP_CODE" where label is "Company code"
    And User enters value "1" into input with id "lookuptxt_BRANCH_CODE" where label is "Branch code"
    And User clicks button with id key "ajaxlink" where label is "Continue"
    And User clicks welcome popup

    When User opens screen "Approve" in app menu path:
      | Investment Deals -Combined without Trade Deal |
    And User enters value "$deal_number" into input with id "!investmentDealsGridTbl_Id_T022P_gs_trsdealVO.SERIAL_NO" where label is "Deal No"
    And User double-clicks id key "investmentDealsGridTbl_Id_T022P_gs_trsdealVO.SERIAL_NO" where label is "Search result"

    And User clicks button with id key "investmentDeals_Approve_btn_T022P" where label is "Approve Deal"
    And User clicks button with id key "_popup_path_sol_confirm_ok" where label is "Confirmation Approve Popup"
    And User clicks button with id key "_popup_path_sol_confirm_ok" where label is "Confirmation Approve Popup"

    And User delay
```

---
