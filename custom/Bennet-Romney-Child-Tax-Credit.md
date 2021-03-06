
# Bennet-Romney Child Tax Credit

#### File Author:
*Grant M. Seiter · American Enterprise Institute* (December 2020)

#### Notes: 
This proposed reform from U.S. Senators Michael Bennet (D-CO) and Mitt Romney (R-UT) (Dec. 2019) is not reflected in current law.

#### Description: 
Provides a $2,000 credit per child for children from age six up to age 17. The first $1,000 per child fully refundable up to the current law
phase-out threshold (and phases out at a rate of 5%). The next $1,000 per child phases in at a 15 percent rate starting at the first dollar
of income (and begins phasing down at current law thresholds at a rate of 5%).

### policy_current_law.json
```json
"BRCTC_c": {
		"title": "Bennet-Romney maximum refundable child tax credit per child",
		"description": "The maximum refundable credit allowed for each child.",
		"notes": "This parameter is not reflected in current law.",
		"section_1": "Refundable Credits",
		"section_2": "Bennet-Romney Child Tax Credit",
		"indexable": true,
		"indexed": false,
		"type": "float",
		"value": [
				{
						"year": 2021,
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
"BRCTC_ref": {
		"title": "The fully refundable portion of the child tax credit per child",
		"description": "The portion of the maximum credit allowed that is fully refundable.",
		"notes": "This parameter is not reflected in current law.",
		"section_1": "Refundable Credits",
		"section_2": "Bennet-Romney Child Tax Credit",
		"indexable": false,
		"indexed": false,
		"type": "float",
		"value": [
				{
						"year": 2021,
						"value": 0.0
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
"BRCTC_ps": {
		"title": "Bennet-Romney child tax credit phaseout MAGI start",
		"description": "The tax credit begins to decrease when MAGI is above this level.",
		"notes": "This parameter is not reflected in current law.",
		"section_1": "Refundable Credits",
		"section_2": "Bennet-Romney Child Tax Credit",
		"indexable": true,
		"indexed": false,
		"type": "float",
		"value": [
				{
						"year": 2021,
						"MARS": "single",
						"value": 0.0
				},
				{
						"year": 2021,
						"MARS": "mjoint",
						"value": 0.0
				},
				{
						"year": 2021,
						"MARS": "mseparate",
						"value": 0.0
				},
				{
						"year": 2021,
						"MARS": "headhh",
						"value": 0.0
				},
				{
						"year": 2021,
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
"BRCTC_prt": {
		"title": "Bennet-Romney child tax credit phaseout rate",
		"description": "The amount of the credit starts to decrease at this rate if MAGI is higher than child tax credit phaseout start.",
		"notes": "This parameter is not reflected in current law.",
		"section_1": "Refundable Credits",
		"section_2": "Bennet-Romney Child Tax Credit",
		"indexable": false,
		"indexed": false,
		"type": "float",
		"value": [
				{
						"year": 2021,
						"value": 0.0
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
"BRCTC_rt": {
		"title": "Bennet-Romney child tax credit phase-in rate",
		"description": "The amount of the credit starts to increase beginning at the first dollar of MAGI.",
		"notes": "This parameter is not reflected in current law.",
		"section_1": "Refundable Credits",
		"section_2": "Bennet-Romney Child Tax Credit",
		"indexable": false,
		"indexed": false,
		"type": "float",
		"value": [
				{
						"year": 2021,
						"value": 0.0
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
```

### records_variables.json
```json
"brctc": {
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
def BennetRomneyChildTaxCredit(n24, nu06, MARS, c00100,
							   BRCTC_c, BRCTC_ref, BRCTC_ps,
							   BRCTC_prt, BRCTC_rt, brctc):
	"""
	Computes the 2019 proposed refundable child tax credit from senators Bennet and Romney
	using specified parameters.
	"""
	line1 = BRCTC_c * (n24 - nu06)
	line1a = line1 * BRCTC_ref
	line1b = line1 - line1a
	line4 = c00100 # no foreign earned income exclusion to add to AGI (lines 2,3, and 4)
	line10 = min(line1b, line4 * BRCTC_rt)
	if line1 > 0. and line4 > BRCTC_ps[MARS - 1]:
		line6 = line4 - BRCTC_ps[MARS - 1]
		line8 = max(0., line1a - BRCTC_prt * line6)
		line12 = max(0., line10 - BRCTC_prt * line6)
	else:
		line8 = line1a
		line12 = line10
	brctc = max(0., line8 + line12)
	return brctc

@iterate_jit(nopython=True)
def IITAX(c59660, c11070, c10960, personal_refundable_credit, ctc_new, rptc, brctc,
          c09200, payrolltax,
          eitc, refund, iitax, combined):
    """
    Computes final taxes.
    """
    eitc = c59660
    refund = (eitc + c11070 + c10960 +
              personal_refundable_credit + ctc_new + rptc + brctc)
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
								   BennetRomneyChildTaxCredit,
                                   PersonalTaxCredit, SchR,
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
 IITAX(self.__policy, self.__records)
 ```
