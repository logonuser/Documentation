Certainly — here's a **formal, emoji-free** version of your Select2 SOP, written in a style suitable for official internal documentation.

---

# **Standard Operating Procedure (SOP)**

**Title:** Implementing Select2 with Manually Supplied Data
**Department:** Frontend Development
**Version:** 1.0
**Date:** \[Insert Date]
**Prepared by:** \[Your Name]

---

## **Purpose**

This document outlines the correct procedure for implementing the Select2 dropdown when using manually supplied data via JavaScript. The goal is to ensure consistent behavior for placeholder functionality and to avoid common integration mistakes.

---

## **Scope**

This SOP applies to all developers integrating the Select2 plugin into web applications where dropdown options are dynamically supplied through JavaScript using the `data` configuration option.

---

## **Objective**

* To ensure placeholder text displays correctly when using Select2 with custom data.
* To eliminate the use of unnecessary HTML `<option>` elements when working with JavaScript-supplied data.
* To provide clarity on handling non-standard data field names (e.g., using `name` instead of `text`).

---

## **Procedure**

### 1. Do Not Use HTML `<option>` Elements When Using `data`

When the `data` option is used in Select2, any `<option>` elements inside the `<select>` tag are ignored and replaced. Therefore, avoid placing `<option>` tags inside the HTML.

**Example:**

```html
<!-- Correct: leave the select element empty -->
<select id="mySelect" style="width: 300px;"></select>
```

---

### 2. Provide Dropdown Data Using the `data` Option

When data contains the default `text` field, Select2 will handle the placeholder and display logic automatically.

**Example:**

```javascript
let data = [
  { id: "1", text: "Apple" },
  { id: "2", text: "Banana" }
];

$('#mySelect').select2({
  placeholder: "Select an item",
  data: data
});

$('#mySelect').val('').trigger('change');
```

---

### 3. Handling Custom Field Names (e.g., `name` Instead of `text`)

If the data uses a field other than `text` (e.g., `name`), the placeholder will not appear automatically. In this case:

* Manually add a placeholder entry in the data array.
* Use `templateResult` and `templateSelection` to render the custom field.
* Set the placeholder item as the default selected value.

**Example:**

```javascript
let dataWithCustomField = [
  { id: "-1", name: "Select item", text: "" },
  { id: "1", name: "Apple" },
  { id: "2", name: "Banana" }
];

$('#mySelect').select2({
  data: dataWithCustomField,
  templateResult: function (data) {
    return data.name;
  },
  templateSelection: function (data) {
    return data.name;
  }
});

$('#mySelect').val("-1").trigger('change');
```

Note: Including a `text` field with an empty string in the placeholder item is required to suppress warnings and enable proper behavior.

---

### 4. Do Not Rely on HTML `<option>` for Placeholder

Even if an `<option value="">Select item</option>` is added in HTML, it will not be used when `data` is provided via JavaScript. All existing `<option>` tags will be ignored.

---

### 5. Prevent Form Submission with Placeholder Value

To prevent users from submitting a form with the placeholder selected, validate the selection before submission.

**Example:**

```javascript
$('#form').on('submit', function (e) {
  if ($('#mySelect').val() === "-1") {
    alert("Please select a valid item.");
    e.preventDefault();
  }
});
```

---

## **Summary**

| Rule                                                           | Description                                            |
| -------------------------------------------------------------- | ------------------------------------------------------ |
| Do not use `<option>` tags with `data:`                        | They will be ignored                                   |
| Always include `text` in data if possible                      | Required for placeholder and internal rendering        |
| Add a placeholder entry manually if using custom fields        | Example: `{ id: "-1", name: "Select item", text: "" }` |
| Use `.val('-1').trigger('change')` to set placeholder          | Required to show the placeholder value initially       |
| `templateResult` and `templateSelection` do not replace `text` | These are for rendering only, not for internal logic   |

---

## **Reusable Template**

```javascript
let dropdownData = [
  { id: "-1", name: "Select item", text: "" },
  { id: "0", name: "hello" },
  { id: "1", name: "apple" },
  { id: "2", name: "mango" },
  { id: "3", name: "grapes" },
  { id: "4", name: "banana" }
];

$('#mySelect').select2({
  placeholder: "Select item",
  data: dropdownData,
  templateResult: function(data) {
    return data.name;
  },
  templateSelection: function(data) {
    return data.name;
  }
});

$('#mySelect').val("-1").trigger('change');
```

---

## **Additional Notes**

* The `allowClear` option can be used in conjunction with a placeholder if needed, but is not required.
* Data transformation functions can be written to convert back-end data into the required format with `id` and `text` fields for standardization.

---

## **References**

* Select2 Official Documentation: [https://select2.org/data-sources/formats](https://select2.org/data-sources/formats)

---

Let me know if you'd like this exported to PDF, Markdown, or formatted for your internal documentation portal.
