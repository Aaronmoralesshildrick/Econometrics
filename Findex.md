# Findex data

drop if wsal_val == 0 
/* Drop values for people who have no income */ 
gen wage = wsal_val/wkswork/hrswk
drop if wage < 1 
/* Drop values for people who have income under 1 */
gen educ = a_hga
gen propval = hprop_val


/*education = a_hga.   39 = HS   43 = bachelor     46 = phd*/
drop if educ == 31
drop if educ == 32
drop if educ == 33
drop if educ == 34
drop if educ == 35
drop if educ == 36
drop if educ == 37
drop if educ == 38
drop if educ == 40
drop if educ == 41
drop if educ == 42
drop if educ == 44
drop if educ == 45


 /*Table 1 Descriptive stats*/
summ educ 
summ wage
summ propval
 
 
/*Figure 1, all wage densities compared from educ*/
twoway kdensity wage if a_hga == 43 & wage < 150 || kdensity wage if a_hga == 46 & wage < 150 || kdensity wage if a_hga == 39 & wage < 150

/*Table 2 Comparisson of means*/
mean wage if educ == 39
mean wage if educ == 43
mean wage if educ == 46

mean propval if educ == 39
mean propval if educ == 43
mean propval if educ == 46


/*Table 3 Regression showing the relationship between wage and estimated property value*/
regress propval wage
/*Figure 2 The residuals from the regression against wage*/
regress propval wage
predict yhat, xb
predict ehat, resid
graph twoway scatter ehat wage

/* Test of hypothesis */
regress wage PhD
regress wage bachelor
regress wage hs

mean wage
