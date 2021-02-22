# Other-Dependent-Tax-Credit

#### File Author:
*Grant M. Seiter · American Enterprise Institute* (January 2021)

#### Notes: 
This nonrefundable credit is applied to non-child dependents and phases out along with the CTC amount.

#### Description: 
Add in phase-out parameters to separate CTC and ODC within calcfuncs. 


### policy_current_law.json
```json
"ODC_ps": {
        "title": "Other dependent tax credit phaseout MAGI start",
        "description": "Other dependent tax credit begins to decrease when MAGI is above this level; read descriptions of the dependent credit amounts for how they phase out when MAGI is above this level.",
        "notes": "",
        "section_1": "Child/Dependent Credits",
        "section_2": "Other Dependent Tax Credit",
        "indexable": true,
        "indexed": false,
        "type": "float",
        "value": [
            {
                "year": 2013,
                "MARS": "single",
                "value": 75000.0
            },
            {
                "year": 2013,
                "MARS": "mjoint",
                "value": 110000.0
            },
            {
                "year": 2013,
                "MARS": "mseparate",
                "value": 55000.0
            },
            {
                "year": 2013,
                "MARS": "headhh",
                "value": 75000.0
            },
            {
                "year": 2013,
                "MARS": "widow",
                "value": 75000.0
            },
            {
                "year": 2014,
                "MARS": "single",
                "value": 75000.0
            },
            {
                "year": 2014,
                "MARS": "mjoint",
                "value": 110000.0
            },
            {
                "year": 2014,
                "MARS": "mseparate",
                "value": 55000.0
            },
            {
                "year": 2014,
                "MARS": "headhh",
                "value": 75000.0
            },
            {
                "year": 2014,
                "MARS": "widow",
                "value": 75000.0
            },
            {
                "year": 2015,
                "MARS": "single",
                "value": 75000.0
            },
            {
                "year": 2015,
                "MARS": "mjoint",
                "value": 110000.0
            },
            {
                "year": 2015,
                "MARS": "mseparate",
                "value": 55000.0
            },
            {
                "year": 2015,
                "MARS": "headhh",
                "value": 75000.0
            },
            {
                "year": 2015,
                "MARS": "widow",
                "value": 75000.0
            },
            {
                "year": 2016,
                "MARS": "single",
                "value": 75000.0
            },
            {
                "year": 2016,
                "MARS": "mjoint",
                "value": 110000.0
            },
            {
                "year": 2016,
                "MARS": "mseparate",
                "value": 55000.0
            },
            {
                "year": 2016,
                "MARS": "headhh",
                "value": 75000.0
            },
            {
                "year": 2016,
                "MARS": "widow",
                "value": 75000.0
            },
            {
                "year": 2017,
                "MARS": "single",
                "value": 75000.0
            },
            {
                "year": 2017,
                "MARS": "mjoint",
                "value": 110000.0
            },
            {
                "year": 2017,
                "MARS": "mseparate",
                "value": 55000.0
            },
            {
                "year": 2017,
                "MARS": "headhh",
                "value": 75000.0
            },
            {
                "year": 2017,
                "MARS": "widow",
                "value": 75000.0
            },
            {
                "year": 2018,
                "MARS": "single",
                "value": 200000.0
            },
            {
                "year": 2018,
                "MARS": "mjoint",
                "value": 400000.0
            },
            {
                "year": 2018,
                "MARS": "mseparate",
                "value": 200000.0
            },
            {
                "year": 2018,
                "MARS": "headhh",
                "value": 200000.0
            },
            {
                "year": 2018,
                "MARS": "widow",
                "value": 400000.0
            },
            {
                "year": 2019,
                "MARS": "single",
                "value": 200000.0
            },
            {
                "year": 2019,
                "MARS": "mjoint",
                "value": 400000.0
            },
            {
                "year": 2019,
                "MARS": "mseparate",
                "value": 200000.0
            },
            {
                "year": 2019,
                "MARS": "headhh",
                "value": 200000.0
            },
            {
                "year": 2019,
                "MARS": "widow",
                "value": 400000.0
            },
            {
                "year": 2026,
                "MARS": "single",
                "value": 75000.0
            },
            {
                "year": 2026,
                "MARS": "mjoint",
                "value": 110000.0
            },
            {
                "year": 2026,
                "MARS": "mseparate",
                "value": 55000.0
            },
            {
                "year": 2026,
                "MARS": "headhh",
                "value": 75000.0
            },
            {
                "year": 2026,
                "MARS": "widow",
                "value": 75000.0
            }
        ],
        "validators": {
            "range": {
                "min": 0,
                "max": 9e+99
            }
        },
        "compatible_data": {
            "puf": true,
            "cps": true
        }
    },
    "ODC_prt": {
        "title": "Other dependent tax credit phaseout rate",
        "description": "The amount of the credit starts to decrease at this rate if MAGI is higher than child tax credit phaseout start.",
        "notes": "",
        "section_1": "Child/Dependent Credits",
        "section_2": "Other Dependent Tax Credit",
        "indexable": false,
        "indexed": false,
        "type": "float",
        "value": [
            {
                "year": 2013,
                "value": 0.05
            }
        ],
        "validators": {
            "range": {
                "min": 0,
                "max": 1
            }
        },
        "compatible_data": {
            "puf": true,
            "cps": true
        }
    },
    "ODC_Age": {
        "title": "Whether or not maximum amount of the credit is available to 17 year olds.",
        "description": "If True, this substitutes n24 with n24 + max(0,XTOT-n24-n1820 -n21 -num).",
        "notes": "This parameter is not reflected in current law.",
        "section_1": "Child/Dependent Credits",
        "section_2": "Other Dependent Tax Credit",
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
def ChildDepTaxCredit(n24, MARS, c00100, XTOT, num, c05800,
                      e07260, CR_ResidentialEnergy_hc,
                      e07300, CR_ForeignTax_hc,
                      c07180,
                      c07230,
                      e07240, CR_RetirementSavings_hc,
                      c07200,
                      CTC_c, CTC_ps, CTC_prt, exact, ODC_c,
                      ODC_ps, ODC_prt, ODC_Age,
                      CTC_c_under6_bonus, nu06, nu18,
                      c07220, odc, codtc_limited):
    """
    Computes amounts on "Child Tax Credit and Credit for Other Dependents
    Worksheet" in 2018 Publication 972, which pertain to these two
    nonrefundable tax credits.
    """
    # Worksheet Part 1
    line1 = CTC_c * n24 + CTC_c_under6_bonus * nu06
    if not ODC_Age:
        line2 = ODC_c * max(0, XTOT - n24 - num)
    else:
        line2 = ODC_c * max(0, XTOT - (n24 + max(0, XTOT - n24 -n1820 -n21 -num)
    line3 = line1 + line2
    modAGI = c00100  # no foreign earned income exclusion to add to AGI (line6)
    if line1 > 0. and modAGI > CTC_ps[MARS - 1]:
        excess = modAGI - CTC_ps[MARS - 1]
        if exact == 1:  # exact calculation as on tax forms
            excess = 1000. * math.ceil(excess / 1000.)
        line101 = max(0., line3 - CTC_prt * excess)
    else:
        line101 = line1

    if line2 > 0. and modAGI > ODC_ps[MARS - 1]:
        excess = modAGI - ODC_ps[MARS - 1]
        if exact == 1:  # exact calculation as on tax forms
            excess = 1000. * math.ceil(excess / 1000.)
        line102 = max(0., line2 - ODC_prt * excess)
    else:
        line102 = line2
    line10 = line101 + line102
    if line10 > 0.:
        # Worksheet Part 2
        line11 = c05800
        line12 = (e07260 * (1. - CR_ResidentialEnergy_hc) +
                  e07300 * (1. - CR_ForeignTax_hc) +
                  c07180 +  # child & dependent care expense credit
                  c07230 +  # education credit
                  e07240 * (1. - CR_RetirementSavings_hc) +
                  c07200)  # Schedule R credit
        line13 = line11 - line12
        line14 = 0.
        line15 = max(0., line13 - line14)
        line16 = min(line10, line15)  # credit is capped by tax liability
    else:
        line16 = 0.
    # separate the CTC and ODTC amounts
    c07220 = 0.  # nonrefundable CTC amount
    odc = 0.  # nonrefundable ODTC amount
    if line16 > 0.:
        if line1 > 0.:
            c07220 = line16 * line1 / line3
        odc = max(0., line16 - c07220)
    # compute codtc_limited for use in AdditionalCTC function
    codtc_limited = max(0., line10 - line16)
    return (c07220, odc, codtc_limited)
```

### calculator.py
```python
NONE
```
