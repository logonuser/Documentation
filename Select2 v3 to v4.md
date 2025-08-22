**Select2 v3 to v4:**

---

**Example 1 – formatResult → templateResult**

Prev (v3 style):

```javascript
formatResult: function(item) {
    return item.text;
}
```

Later (v4 style):

```javascript
templateResult: function(item) {
    return item.text;
}
```

---

**Example 2 – Setting a value programmatically**

Prev (v3 style):

```javascript
j("#gridTaxApplicableNameId").select2("val", someValue);
```

Later (v4 style):

```javascript
j("#gridTaxApplicableNameId").val(someValue).trigger("change");
```

---

**Example 3 – Appending a new option dynamically**

Prev (v3 style):

```javascript
j("#accountHeadNameId").select2('data', { "id": '-1', "text": '-- Select --' });
```

Later (v4 style):

```javascript
j("#accountHeadNameId").empty().append(new Option("-- Select --", "-1", true, true)).trigger("change");
```

---

**Example 4 – templateSelection**

Prev (v3 style):

```javascript
templateSelection: function(item) { return item.name; }
```

Later (v4 style):

```javascript
templateSelection: function(item) { return item.text; }
```

---

**Example 5 – focusing the search box**

Prev (v3 style):

```javascript
j('#s2id_itemNameId').find('.select2-focusser').focus();
```

Later (v4 style):

```javascript
j('#itemNameId').select2('open'); // open dropdown
j('.select2-container--open .select2-search__field').focus(); // focus search box
```

---

**Example 6 – select3 open (dropdown open)**

Prev (v3 style):

```javascript
j('#s2id_itemNameId').select2('open');
```

Later (v4 style):

```javascript
j('#itemNameId').select2('open');
```

---

**Example 7 – Getting non-editable field value**

Prev (v3 style):

```javascript
j('#gridTaxApplicableNameId_nonEdit').html(j("#gridTaxApplicableNameId").select2('data').name);
```

Later (v4 style):

```javascript
j('#gridTaxApplicableNameId_nonEdit').html(j("#gridTaxApplicableNameId").select2('data')[0]?.text || "");
```

---

**Example 8 – Getting value, Attribute change:** 'name' **to** 'text'

Prev (v3 style):

```javascript
vendorSelection = j("#vendorNameId").select2('data').name;
```

Later (v4 style):

```javascript
vendorSelection = j("#vendorNameId").select2('data')[0]?.text || "";
```

---

**Example 9 – AJAX results → processResults**

Prev (v3 style):

```javascript
results: function (data,page) {
    vendorPageNo = page;
    return { results: data.result, more: data.more };
}
```

Later (v4 style):

```javascript
processResults: function (data, params) {
    vendorPageNo = params.page || 1;
    return {
        results: data.result,
        pagination: { more: data.more }
    };
}
```

---

**Example 10 – quietMillis → delay**

Prev (v3 style):

```javascript
quietMillis: 50,
```

Later (v4 style):

```javascript
delay: 50,
```

---

**Example 11 – AJAX `data` function**

Prev (v3 style):

```javascript
data: function (term) {
    return { searchTerm: term, page: vendorPageNo };
}
```

Later (v4 style):

```javascript
data: function (params) {
    return { searchTerm: params.term, page: vendorPageNo || 1 };
}
```

---

**Example 12 – Removal of `initSelection`**

Prev (v3 style):

```javascript
initSelection: function(el, fn) { },
```

Later (v4 style):

```javascript
// initSelection no longer needed; v4 handles initial value automatically
```

---

**Example 13 – HTML hidden → select element**

Prev (v3 style):

```html
<input type="hidden" id="vendorNameId" style="width: 300px" onchange="clearRecords();" />
```

Later (v4 style):

```html
<select id="vendorNameId" style="width: 300px" onchange="clearRecords();"></select>
```

---

**Example 14 – Clearing a Select2 field programmatically**

Prev (v3 style):

```javascript
j("#vendorNameId").select2("val", "-1");
```

Later (v4 style):

```javascript
j("#vendorNameId").val(null).trigger("change");
```

---

**Example Snippet code**

Prev (v3 style):

```javascript
    j('#vendorNameId').select2({
    	placeholder: "Select a Vendor",
    	minimumInputLength: 2,
        initSelection: function(el, fn) {
        },
	    ajax: {
	        url: "/TnEV1_0AWeb/master/selectTwo.tne?command=getSelect2ListFromAjaxForVendor&isWithOrWithoutPO="+document.forms[0]["itemType"].value,
	        dataType: 'json',
	        type: "POST",
	        quietMillis: 50,
	        data: function (term) {
	   	        return {
	   	          searchTerm: term, // search term
	   	          rateCardType: document.forms[0]["rateCard"][1].value,
	   	          ouId: document.forms[0]["empOuId"].value,
	   	          invDate: document.forms[0]["invoiceDate"].value,
	   	          pageSize: vendorPageSize,
	   	          page: vendorPageNo
	   	        };
	        },
	        results: function (data,page) {
	        	vendorPageNo = page;
	            return {
	                results: data.result
	                , more: data.more
	            };
	        }
	    }
    });
```

Later (v4 style):

```javascript
    j('#vendorNameId').select2({
		placeholder: "Select a Vendor",
		minimumInputLength: 2,									// removed initSelection
		ajax: {
			url: "/TnEV1_0AWeb/master/selectTwo.tne?command=getSelect2ListFromAjaxForVendor&isWithOrWithoutPO="
				+ document.forms[0]["itemType"].value,
			dataType: 'json',
			type: "POST",
			delay: 50,   										// changed 'quietMillis' : to 'delay'
			data: function(params) {
				return {
					searchTerm: params.term || '',   			// params.term = text entered in searchbox
					rateCardType: document.forms[0]["rateCard"][1].value,
					ouId: document.forms[0]["empOuId"].value,
					invDate: document.forms[0]["invoiceDate"].value,
					pageSize: vendorPageSize,
					page: vendorPageNo || 1
				};
			},
			processResults: function(data, params) {			// changed 'results' : to 'processResults'
				// console.log("Backend response:", data);
				vendorPageNo = params.page || 1;				// page present in params.page
				return {
					results: data.result,       				// matches {id, text} format
					pagination: {
						more: data.more         				// more moved inside pagination object
					}
				};
			}
		}
	});
```

---
