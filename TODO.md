# Things to do to finish the package/paper/project

## Package 

### Overarching Package Issues

- [ ] Puzzle over why the BH approach gave such different results on the Alewife SNPs than the
vanilla MCMC-and-add-em-up approach.
- [ ] Unless there is a good reason to, I think we should remove the BH stuff from our three or
four high level functions.  (see next item)
- [ ] The _main functions_ that users should have to deal with are `infer_mixture()`,
`simulate_and_assess_reference()` (maybe we should shorten that to `assess_reference()`), `assess_bp_bias_correction()`, and we need one that is `self_assign()`.  These all
should spit out tidy data, to the extent possible.  We should try to expose very few other 
functions.  
- [ ] Clean up code so that it is CRAN compliant. Run Check to
see all the problems.  We get a lot of NOTEs and
a number of WARNINGs.  **ERIC**
- [ ] Use `if(getRversion() >= "2.15.1") utils::globalVariables(c("my_var"))` to
keep CRAN checks from creating notes for variable `my_var` used in a dplyr context.
Do this for all variables that create NOTEs  **ERIC**
- [ ] Deprecate the old "pipeline functions" that have been superseded by the ones in `eca_funcs.R`.
Use the .Deprecated
function [here](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Deprecated.html).
We want to keep them around for a few iterations for getting results
for the paper, but a lot of the paper
stuff needed to come out of them. **BEN**
- [ ] Update documentation to reflect the deprecation, and update the documentation
for the _main functions_ 
- [ ] Discuss transferring "ownership" of the repo to Eric's GitHub account.
- [ ] Set Travis-CI up to automatically run CRAN checks.  **ERIC** 
- [ ] Write a README.Rmd that shows all the different uses of the package in an easy, step-by-step,
beginner's mind way.  This will eventually turn into a vignette.  **ERIC** and **BEN**. 
- [ ] Minimize the number of functions that are exported and, hence, exposes to the user. In
the roxygen block for these functions add a line: `#' @keywords internal` to keep the function
documentation out of the help files (if users aren't going to use them directly, there is
no reason to have them.) **BEN**.




### Specific Functions and Issues

- [ ] We need a simple function that will take a reference data set in and return a tidy-formatted
output that includes self-assignment log-likelihoods and posterior probabilities in tidy format. Let's
call this `self_assign()`.
- [ ] It would be nice to modify `infer_mixture` so that multiple different mixture samples can
be specified in a single data frame input.  With really large baselines, the vast majority of the
time in the function is spent processing the data, counting alleles, etc., and it is a shame to have
to do this each time you want to analyze a different mixture sample.  I'm not sure how to go about this, but Ben might!
- [ ] Try to make almost all user-exposed functions return tidy data.  For example `infer_mixture`
returns a list at the moment.  Can that be cleaned up.
- [ ] We could really use a way for users to have more control over the simulation parameters---just
setting alpha is pretty limited.  It would be nice for users to explicitly give proportions (or maybe 
even the actual counts).  For this we need to spend some time thinking about how to do it elegantly.
- [ ] `assess_bp_bias_correction` spits out some nice tidy data at this point.  It looks like:
```
# A tibble: 700 × 6
    iter     repunit   true_rho   rho_mcmc     rho_bh     rho_pb
   <int>      <fctr>      <dbl>      <dbl>      <dbl>      <dbl>
1      1         CAN 0.04573827 0.06489907 0.04550360 0.06226238
2      1         NNE 0.06962820 0.07385272 0.05886538 0.06360804
3      1          MB 0.14588315 0.21344097 0.21242781 0.22291767
4      1         NUN 0.46891013 0.14646259 0.35338886 0.24710874
5      1         BIS 0.06644381 0.09336929 0.15767998 0.08005169
6      1         LIS 0.15719031 0.35012560 0.11664560 0.27002139
7      1 MidAtlantic 0.04620614 0.05784975 0.05548877 0.05403009
8      2         CAN 0.02751659 0.02866411 0.02224521 0.02645824
9      2         NNE 0.23190134 0.27633524 0.20446014 0.26219858
10     2          MB 0.16320168 0.07701908 0.17914736 0.11657221
```
But, we need to 
    - [ ] remove the `rho_bh` calculation and return column.
    - [ ] include a `true_n` column in the output, which gives the
    actual number of individuals sampled into that population on that iteration.


## Paper

- [ ]  Eric needs to talk to Eric P. about the possibility of using the 
alewife SNP data as an example.
- [ ] Do the parametric bootstrapping on a few other data sets to show that
it doesn't break anything, and it usually improves them.