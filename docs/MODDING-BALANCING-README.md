# Balancing Summary

## Core Concepts

Most calculations in the game are set up via JSON. They can include:

### 1/. Rulesets (Baseline and modifier)

 - Box Office, Critic Reviews (avocadometer) and Audience Reviews (Sodameter) are calculated using a concept of baseline and modifiers.
 - The baseline and modifier are each calculated by applying a PMO to the sum of rules within their respective ruleset
 - We then have final value = baseline * (1 + modifier)

### 2/. PMOs

PMO contains:

 - “p” = power
 - “m” = multiplier
 - “o” = offset
 - “Shape” (optional): “linear” by default, can also be “exponent” or “logarithm”
 - “max” (optional): Max value post calculation
 - “min” (optional): Min value post calculation

Example: "pmo": { "p": 3, "m": 4, "o": 100, "shape": "EXPONENT" } applied to 2: 
4 * EXP (2)^3 + 100 =  1714

### 3/. Polynomials
Example { "m3": -2, "m2": 4, "m1": 10, "offset": -20 } applied to 5: 
-2 * 5^3 + 4 * 5^2 + 10 * 5 - 20 = - 120

## Difficulty settings

Available difficulty options displayed when starting a new game are loaded from the core data, meaning before the mods are loaded.
As a result, mods cannot add a level of difficulty.

However, any existing base difficulty settings can be modded. The only limitation is that the description of the difficulty cannot be changed as the localized string is loaded from the core data (non moddable).

For example, you can change the “seed-money” to $50M in “easy” but the description when selecting easy will still be: 
“You have inherited a chunk of money from your parents to fund your production company.
Starting Amount: $30M
Starting Theme Pool Size: 14”

## Box Office

 - Each market, “domestic” and “international”, have a baseline (“bo-baseline”), and a list of modifiers (“bo-modifier”) that will be used to calculate the domestic and international Box Office of your films.
	 - Base location: Data > Hollywood OR Chinawood > Markets > Domestic-BoxOffice.json AND International-BoxOffice.json
 - The calculation is as follows:
	 - Baseline = Exp ( sum of rulesets ) 
	 - Modifiers = sum of rulesets
	 - Total BO = Baseline * ( 1 + Modifiers)

## Reviews

 - Each review type, “critics” and “audience”, have a “baseline”, and a list of “modifier” that will be used to calculate the critics and audience reviews of your films.
	 - Base location: Data > Hollywood OR Chinawood > Metrics > Critics.json AND Audience.json
 - The calculation is as follows:
	 - Baseline = sum of rulesets 
	 - Modifiers = sum of rulesets
	 - Total BO = Baseline * ( 1 + Modifiers)

## Revenue Streams

###  1/. Theatrical, TV and Airlines
This covers the following type of revenue stream:
 - Theatrical
 - Pay TV
 - Free TV
 - Airlines

For each revenue stream, the revenue is calculated separately for domestic and international using the following structure:
 - Step 1 - “base”: Apply a PMO to the Box Office amount
 - Step 2 - “year”: Multiply that value by a polynomial applied to (current year - 1970)
 - Step 3: spread that total by defining the month by month % amount following the release 

Example:
 - Domestic Box Office = $10M
 - In game year = 1990 => amount used for the polynomial is 1990-1970=20
 - We have the following base data for “rev-airlines”
	 - "base": { "p": 1.0, "m": 0.02, "o": 0.0, "max": 10000000 }
	 - "year": { "m3": -0.00005914, "m2": 0.00387167, "m1": -0.03226876, "offset": 0.0, "min": 0 }
	 - "Spread":[0.10,0.10,0.10,0.10,0.10,0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.05,0.01,0.01,0.01,0.01,0.01]
 - Step 1 - “base” = 0.02 * 10,000,000^1 + 0 = 200,000
 - Step 2 - “year” = 200,000 * (-0.00005914 * 20^3 +0.00387167 * 20^2 -0.03226876 * 20^1 + 0) = 86,035
 - Step 3 = Month 1 = 10 % * 86,035 = 8,603 | Month 2 = 10%* 86,035 = 8,603 …

Base location: Data > Shared > Markets > Domestic-Distribution.json AND International-Distribution.json

 ### 2/. Home Entertainment
 - This covers Home Entertainment revenue
 - This covers the following type of revenue stream (independently for domestic and International):
	 - Physical Home Entertainment
	 - Digital Home Entertainment
 - The base calculation is similar to theatrical TV and airlines
 - The spread follows a slope during from "spread-slope-start" month post release to  "spread-slope-end" month post release with a first month revenue of "spread-slope-start-val" * the base revenue calculated above
 - That monthly revenue can then be impacted by:
	 - The distribution deal attributes 
	 - The Special edition unlocked if any
	 - The special status if any (cult or legendary)

