# New Child Tax Credit
#### File Author:
*Grant M. Seiter · American Enterprise Institute* (February 2021)

#### Notes: 
Edits the New Child Tax Credit to Include Children Age 17.

#### Description: 
As in Biden Proposal. 

### policy_current_law.json
```json

"CTC_new_Age": {
        "title": "Whether or not maximum amount of the new refundable child tax credit is available to 17 year olds.",
        "description": "If True, this substitutes n24 with n24 + max(0,XTOT-n24-n1820 -n21 -num).",
        "notes": "This parameter is not reflected in current law.",
        "section_1": "Refundable Credits",
        "section_2": "New Refundable Child Tax Credit",
        "indexable": false,
        "indexed": false,
        "type": "bool",
        "value": [
            {
                "year": 2013,
                "value": false
            }
        ],
        "validators": {
            "range": {
                "min": false,
                "max": true
            }
        },
        "compatible_data": {
            "puf": true,
            "cps": true
        }
    },
```

### records_variables.json
```json
NONE
```

### records.py
```python
NONE
```

### calcfunctions.py
```python
@iterate_jit(nopython=True)
def CTC_new(CTC_new_c, CTC_new_rt, CTC_new_c_under6_bonus,
            CTC_new_ps, CTC_new_prt, CTC_new_for_all,
            CTC_new_refund_limited, CTC_new_refund_limit_payroll_rt,
            CTC_new_refund_limited_all_payroll, payrolltax,
            n24, nu06, c00100, MARS, ptax_oasdi, c09200,
            CTC_new_Age, XTOT, n1820, n21, num,
            ctc_new):
    """
    Computes new refundable child tax credit using specified parameters.
    """
    if not CTC_new_Age:
	    if n24 > 0:
	        posagi = max(c00100, 0.)
	        ctc_new = CTC_new_c * n24 + CTC_new_c_under6_bonus * nu06
	        if not CTC_new_for_all:
	            ctc_new = min(CTC_new_rt * posagi, ctc_new)
	        ymax = CTC_new_ps[MARS - 1]
	        if posagi > ymax:
	            ctc_new_reduced = max(0.,
	                                  ctc_new - CTC_new_prt * (posagi - ymax))
	            ctc_new = min(ctc_new, ctc_new_reduced)
	        if ctc_new > 0. and CTC_new_refund_limited:
	            refund_new = max(0., ctc_new - c09200)
	            if not CTC_new_refund_limited_all_payroll:
	                limit_new = CTC_new_refund_limit_payroll_rt * ptax_oasdi
	            if CTC_new_refund_limited_all_payroll:
	                limit_new = CTC_new_refund_limit_payroll_rt * payrolltax
	            limited_new = max(0., refund_new - limit_new)
	            ctc_new = max(0., ctc_new - limited_new)
	    else:
	        ctc_new = 0.
	else:
        if (n24 + max(0,XTOT - n24 -n1820 -n21 - num)) > 0:
	        posagi = max(c00100, 0.)
	        ctc_new = CTC_new_c * (n24 + max(0,XTOT - n24 -n1820 -n21 - num)) + CTC_new_c_under6_bonus * nu06
	        if not CTC_new_for_all:
	            ctc_new = min(CTC_new_rt * posagi, ctc_new)
	        ymax = CTC_new_ps[MARS - 1]
	        if posagi > ymax:
	            ctc_new_reduced = max(0.,
	                                  ctc_new - CTC_new_prt * (posagi - ymax))
	            ctc_new = min(ctc_new, ctc_new_reduced)
	        if ctc_new > 0. and CTC_new_refund_limited:
	            refund_new = max(0., ctc_new - c09200)
	            if not CTC_new_refund_limited_all_payroll:
	                limit_new = CTC_new_refund_limit_payroll_rt * ptax_oasdi
	            if CTC_new_refund_limited_all_payroll:
	                limit_new = CTC_new_refund_limit_payroll_rt * payrolltax
	            limited_new = max(0., refund_new - limit_new)
	            ctc_new = max(0., ctc_new - limited_new)
	    else:
	        ctc_new = 0.
    return ctc_new
```

### calculator.py
```python
NONE
```
