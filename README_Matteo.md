# Test done by Matteo in portal1 machine

scope of the test: test entire reweighting module making in EFT2Obs with the VBS ssWW fully leptonic chain
Approximations tested so far:
- `MG2.6.7` version here vs. `MG2.6.5` used in UL SM samples generation
    - this should not be a problem since 2.6.7 is essentially identical, physics-wise
    - please note that for ReReco campaign the version should be MG2.4.X, so please check the `env.sh` and the other steps in EFT2Obs where it downloads madgraph (more instructions will follow at some point on this)
- modeL with no restrict cards (SMEFT) (for Eboli basis et Al. there should be no issue in just leading the correct model)


## series of commands that tested for VBS ssWW with hadronic taus:

### Set up the process folder in MG:
- cards folder `cards/WWjjTolnulnu_SS_ewk_dim6`
- Updated the `run_card.dat` with the pdf set for UL production, as written here: https://cms-pdmv.gitbook.io/project/mccontact/info-for-mc-production-for-ultra-legacy-campaigns-2016-2017-2018#recommendations-on-the-usage-of-pdfs-and-cpx-tunes
Here some care is needed to adapt the cards from CMS genproduction, e.g.:
    - replace ...



```sh
./scripts/setup_process.sh WWjjTolnulnu_SS_ewk_dim6
```

### Prepare MG cards
Prepare MG cards: at the setup step, the code clones the existing cards in the `cads/process` folder, but it is still possible to modify reweighting card and param card.
- reweight card:
Prepare congif `.json` file coding the reweighting options used:
```sh
python scripts/make_config.py -p WWjjTolnulnu_SS_ewk_dim6 -o config_WWjjTolnulnu_SS_ewk_dim6.json   \
--pars SMEFT:2,7,9,5,4,21,22,24,25,29,30,31,32,33,34 --def-val 0.01 --def-sm 0.0 --def-gen 0.0
```
Note the following SMEFT operators numbering scheme in the ouput:
    - [2] cw
    - [7] chw
    - [9] chwb
    - [5] chdd
    - [4] chbox
    - [21] chl1
    - [22] chl3
    - [24] chq1
    - [25] chq3
    - [29] cll
    - [30] cll1
    - [31] cqq1
    - [32] cqq11
    - [33] cqq3
    - [34] cqq31
Then we set up the reweighting card with the following command:
```sh
python scripts/make_reweight_card.py config_WWjjTolnulnu_SS_ewk_dim6.json cards/WWjjTolnulnu_SS_ewk_dim6/reweight_card.dat
```

- param card:    
Since the param card command in the following line does not work
```sh
python scripts/make_param_card.py -p zh-WWjjTolnulnu_SS_ewk_dim6 -c config_WWjjTolnulnu_SS_ewk_dim6.json \
-o cards/WWjjTolnulnu_SS_ewk_dim6/
```
we simply copy the existing default param_card.dat from `MG5_aMC_v2_6_7/WWjjTolnulnu_SS_ewk_dim6/Cards/` to `cards/WWjjTolnulnu_SS_ewk_dim6/`


### Make the gridpack (along with the reweighting module)
```sh 
./scripts/make_gridpack.sh WWjjTolnulnu_SS_ewk_dim6 1
```



______________________________________________________________
## series of commands that I run for VBS fully hadronic:
### Set up the process folder in MG:
```sh
./scripts/setup_process.sh WWjjTolnulnu_SS_ewk_dim6
```
