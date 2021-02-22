# Earned Income Tax Credit

#### File Author:
*Grant M. Seiter · American Enterprise Institute* (February 2021)

#### Notes: 
The Family Security Act (FSA) was proposed by U.S. Senator Mitt Romney (R-UT) (Feb. 2021) and is not relfected in current law. 
The FSA parameters required a modification to the Tax-Calc logic to include a maximum credit bonus for MarriedJ filers.

#### Description: 
The bill reforms the Earned Income Tax Credit (EITC) with a larger family benefit that does not vary depending on the 
number of dependents, eliminates marriage penalties, and slowsbenefit cliffs.

| Parameters          | For Childless Single Filers | For Single Filers w/ Dependents | For Childless Joint Filers | For Joint Filers w/ Dependents |
|---------------------|-----------------------------|---------------------------------|----------------------------|--------------------------------|
| Credit Phase In (%) | 12.5                        | 16.67                           | 12.5                       | 16.67                          |
| Maximum Credit      | 1000                        | 2000                            | 2000                       | 3000                           |
| Phasout Start       | 10000                       | 23000                           | 20000                      | 33000                          |
| Phaseout Rate (%)   | 14.29                       | 14.29                           | 14.29                      | 14.29                          |

Source: https://cdn.vox-cdn.com/uploads/chorus_asset/file/22279576/family_security_act_one_pager_appendix.pdf

### policy_current_law.json
```json
"EITC_c_MarriedJ": {
    "title": "Extra earned income credit for married filling jointly",
    "description": "This is the additional amount added on the regular credit for taxpayers with filling status of married filing jointly.",
    "notes": "This parameter is not reflected in current law.",
    "section_1": "Refundable Credits",
    "section_2": "Earned Income Tax Credit",
    "indexable": true,
    "indexed": true,
    "type": "float",
    "value": [
        {
            "year": 2013,
            "EIC": "0kids",
            "value": 0
        },
        {
            "year": 2013,
            "EIC": "1kid",
            "value": 0
        },
        {
            "year": 2013,
            "EIC": "2kids",
            "value": 0
        },
        {
            "year": 2013,
            "EIC": "3+kids",
            "value": 0
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
def EITC(MARS, DSI, EIC, c00100, e00300, e00400, e00600, c01000,
         e02000, e26270, age_head, age_spouse, earned, earned_p, earned_s,
         EITC_ps, EITC_MinEligAge, EITC_MaxEligAge, EITC_ps_MarriedJ,
         EITC_rt, EITC_c, EITC_prt, EITC_basic_frac, EITC_c_MarriedJ,
         EITC_InvestIncome_c, EITC_excess_InvestIncome_rt,
         EITC_indiv, EITC_sep_filers_elig,
         c59660):
    """
    Computes EITC amount, c59660.
    """
    # pylint: disable=too-many-branches
    if MARS != 2:
        eitc = EITCamount(EITC_basic_frac,
                          EITC_rt[EIC], earned, EITC_c[EIC],
                          EITC_ps[EIC], c00100, EITC_prt[EIC])
        if EIC == 0:
            # enforce age eligibility rule for those with no EITC-eligible
            # kids assuming that an unknown age_* value implies EITC age
            # eligibility
            h_age_elig = EITC_MinEligAge <= age_head <= EITC_MaxEligAge
            if (age_head == 0 or h_age_elig):
                c59660 = eitc
            else:
                c59660 = 0.
        else:  # if EIC != 0
            c59660 = eitc

    if MARS == 2:
        po_start = EITC_ps[EIC] + EITC_ps_MarriedJ[EIC]
        max_amt = EITC_c[EIC] + EITC_c_MarriedJ[EIC]
        if not EITC_indiv:
            # filing unit EITC rather than individual EITC
            eitc = EITCamount(EITC_basic_frac,
                              EITC_rt[EIC], earned, max_amt,
                              po_start, c00100, EITC_prt[EIC])
        if EITC_indiv:
            # individual EITC rather than a filing-unit EITC
            eitc_p = EITCamount(EITC_basic_frac,
                                EITC_rt[EIC], earned_p, max_amt,
                                po_start, earned_p, EITC_prt[EIC])
            eitc_s = EITCamount(EITC_basic_frac,
                                EITC_rt[EIC], earned_s, max_amt,
                                po_start, earned_s, EITC_prt[EIC])
            eitc = eitc_p + eitc_s

        if EIC == 0:
            h_age_elig = EITC_MinEligAge <= age_head <= EITC_MaxEligAge
            s_age_elig = EITC_MinEligAge <= age_spouse <= EITC_MaxEligAge
            if (age_head == 0 or age_spouse == 0 or h_age_elig or s_age_elig):
                c59660 = eitc
            else:
                c59660 = 0.
        else:
            c59660 = eitc

    if (MARS == 3 and not EITC_sep_filers_elig) or DSI == 1:
        c59660 = 0.

    # reduce positive EITC if investment income exceeds ceiling
    if c59660 > 0.:
        invinc = (e00400 + e00300 + e00600 +
                  max(0., c01000) + max(0., (e02000 - e26270)))
        if invinc > EITC_InvestIncome_c:
            eitc = (c59660 - EITC_excess_InvestIncome_rt *
                    (invinc - EITC_InvestIncome_c))
            c59660 = max(0., eitc)
    return c59660

```

### calculator.py
```python
	NONE
```
