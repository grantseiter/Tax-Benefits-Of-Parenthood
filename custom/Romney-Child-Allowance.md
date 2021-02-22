# Romney Child Allowance -- A Proposed Replacement of the Child Tax Credit

#### File Author:
*Grant M. Seiter · American Enterprise Institute* (February 2021)

#### Notes: 
This replacement of the existing Child Tax Credit (CTC), The Family Security Act (FSA) was proposed by U.S. Senator Mitt Romney (R-UT) (Feb. 2021)
and is not relfected in current law.

#### Description: 
Provides a flat child allowance equal to $350 per month ($4,200 per year) for children ages 0-5 and $250 per month ($3,000 per yrear) for children ages
6 to 17. The maximum monthly payment is called at $1,250 per family ($15,000 per year) and the allowance phases-out at a rate of $50 for every
$1,000 (5%) above the current CTC income thresholds (S: 200,000 MFJ: 400,000). I model as a refundable tax credit as any over- / under-payments would be
reconciled through the IRS atfter filing year-end taxes. 

 Source: https://www.romney.senate.gov/sites/default/files/2021-02/family%20security%20act_one%20pager.pdf  
// ===========================================================#

### policy_current_law.json
```json
"RomneyCA_c": {
    "title": "Maximum annual allowance per child",
    "description": "The maximum allowance for each child.",
    "notes": "This parameter is not reflected in current law.",
    "section_1": "Refundable Credits",
    "section_2": "Romney Child Allowance",
    "indexable": true,
    "indexed": false,
    "type": "float",
    "value": [
        {
            "year": 2013,
            "value": 0.0
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
"RomneyCA_under6_bonus": {
    "title": "Bonus allowance for qualifying children under six",
    "description": "The maximum annual allowance for each child is increased by this amount for qualifying children under 6 years old.",
    "notes": "This parameter is not reflected in current law.",
    "section_1": "Refundable Credits",
    "section_2": "Romney Child Allowance",
    "indexable": true,
    "indexed": false,
    "type": "float",
    "value": [
        {
            "year": 2013,
            "value": 0.0
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
"RomneyCA_limit": {
    "title": "Maximum annual allowance per filing unit",
    "description": "The maximum annual allowance for each fiing unit.",
    "notes": "This parameter is not reflected in current law.",
    "section_1": "Refundable Credits",
    "section_2": "Romney Child Allowance",
    "indexable": true,
    "indexed": false,
    "type": "float",
    "value": [
        {
            "year": 2013,
            "value": 0.0
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
"RomneyCA_ps": {
    "title": "Romney child allowance phaseout starting AGI",
    "description": "The total amount of child allowance is reduced for taxpayers with AGI higher than this level.",
    "notes": "This parameter is not reflected in current law.",
    "section_1": "Refundable Credits",
    "section_2": "Romney Child Allowance",
    "indexable": true,
    "indexed": true,
    "type": "float",
    "value": [
        {
            "year": 2013,
            "MARS": "single",
            "value": 0.0
        },
        {
            "year": 2013,
            "MARS": "mjoint",
            "value": 0.0
        },
        {
            "year": 2013,
            "MARS": "mseparate",
            "value": 0.0
        },
        {
            "year": 2013,
            "MARS": "headhh",
            "value": 0.0
        },
        {
            "year": 2013,
            "MARS": "widow",
            "value": 0.0
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
"RomneyCA_prt": {
    "title": "Romney child allowance phaseout rate",
    "description": "The total amount of the child allowance is reduced at this rate per dollar exceeding the phaseout starting AGI, RomneyCA_ps.",
    "notes": "This parameter is not reflected in current law.",
    "section_1": "Refundable Credits",
    "section_2": "Romney Child Allowance",
    "indexable": false,
    "indexed": false,
    "type": "float",
    "value": [
        {
            "year": 2013,
            "value": 0.0
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
"RomneyCA_Age": {
        "title": "Whether or not maximum amount of the child allowance is available to 17 year olds.",
        "description": "If True, this substitutes n24 with n24 + max(0,XTOT-n24-n1820 -n21 -num).",
        "notes": "This parameter is not reflected in current law.",
        "section_1": "Refundable Credits",
        "section_2": "Romney Child Allowance",
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

"romneyca": {
	"type": "float",
	"desc": "search taxcalc/calcfunctions.py for how calculated and used",
	"form": {"2013-20??": "calculated variable"}
},
```

### records.py
```python
NONE
```

### calcfunctions.py
```python
@iterate_jit(nopython=True)
def RomneyChildAllowance(RomneyCA_c, RomneyCA_under6_bonus, RomneyCA_limit, XTOT, n1820, n21, num,
            RomneyCA_ps, RomneyCA_prt, RomneyCA_Age, n24, nu06, c00100, MARS, romneyca):
    """
    Computes the 2021 proposed child allowance from Senator Romney using specified parameters.
    """
    if not RomneyCA_Age:
        if n24 > 0:
            romneyca = RomneyCA_c * n24 + RomneyCA_under6_bonus * nu06
            posagi = max(c00100, 0.)
            ymax = RomneyCA_ps[MARS - 1]
            if posagi > ymax:
                romneyca_reduced = max(0., romneyca - RomneyCA_prt * (posagi - ymax))
                romneyca = min(romneyca, romneyca_reduced)
            if romneyca > RomneyCA_limit:
            	romneyca = min(romneyca, RomneyCA_limit)
        else:
            romneyca = 0.
    else:
        if (n24 + max(0,XTOT - n24 -n1820 -n21 - num)) > 0:
            romneyca = RomneyCA_c * (n24 + max(0,XTOT - n24 -n1820 -n21 - num)) + RomneyCA_under6_bonus * nu06
            posagi = max(c00100, 0.)
            ymax = RomneyCA_ps[MARS - 1]
            if posagi > ymax:
                romneyca_reduced = max(0., romneyca - RomneyCA_prt * (posagi - ymax))
                romneyca = min(romneyca, romneyca_reduced)
            if romneyca > RomneyCA_limit:
                romneyca = min(romneyca, RomneyCA_limit)
        else:
            romneyca = 0.
    return romneyca

@iterate_jit(nopython=True)
def IITAX(c59660, c11070, c10960, personal_refundable_credit, ctc_new, rptc, brctc, nyctc, romneyca,
          c09200, payrolltax,
          eitc, refund, iitax, combined):
    """
    Computes final taxes.
    """
    eitc = c59660
    refund = (eitc + c11070 + c10960 +
              personal_refundable_credit + ctc_new + rptc + brctc + nyctc + romneyca)
    iitax = c09200 - refund
    combined = iitax + payrolltax
    return (eitc, refund, iitax, combined)
```

### calculator.py
```python
from taxcalc.calcfunctions import (TaxInc, SchXYZTax, GainsTax, AGIsurtax,
                                   NetInvIncTax, AMT, EI_PayrollTax, Adj,
                                   DependentCare, ALD_InvInc_ec_base, CapGains,
                                   SSBenefits, UBI, AGI, ItemDedCap, ItemDed,
                                   StdDed, AdditionalMedicareTax, F2441, EITC,
                                   RefundablePayrollTaxCredit,
                                   ChildDepTaxCredit, AdditionalCTC, CTC_new,
                                   BennetRomneyChildTaxCredit, NewYoungChildTaxCredit,
                                   RomneyChildAllowance, PersonalTaxCredit, SchR,               #ADDED TO THIS LINE
                                   AmOppCreditParts, EducationTaxCredit,
                                   CharityCredit,
                                   NonrefundableCredits, C1040, IITAX,
                                   BenefitSurtax, BenefitLimitation,
                                   FairShareTax, LumpSumTax, BenefitPrograms,
                                   ExpandIncome, AfterTaxIncome)

# Calculate taxes with optimal itemized deduction
self._taxinc_to_amt()
F2441(self.__policy, self.__records)
EITC(self.__policy, self.__records)
RefundablePayrollTaxCredit(self.__policy, self.__records)
PersonalTaxCredit(self.__policy, self.__records)
AmOppCreditParts(self.__policy, self.__records)
SchR(self.__policy, self.__records)
EducationTaxCredit(self.__policy, self.__records)
CharityCredit(self.__policy, self.__records)
ChildDepTaxCredit(self.__policy, self.__records)
NonrefundableCredits(self.__policy, self.__records)
AdditionalCTC(self.__policy, self.__records)
C1040(self.__policy, self.__records)
CTC_new(self.__policy, self.__records)
BennetRomneyChildTaxCredit(self.__policy, self.__records)
NewYoungChildTaxCredit(self.__policy, self.__records)
RomneyChildAllowance(self.__policy, self.__records)                                             #ADDED TO THIS LINE  
IITAX(self.__policy, self.__records)
```