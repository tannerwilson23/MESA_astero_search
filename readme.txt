So here's how to run the asteroid search.

Move these files along the following directory path; $MESA_DIR/star/astero/work and replace these files in this directory with the files I have uploaded. (Msg me if you have issues but they should be fine)

./clean
./mk
./rn (to start from the initial model)
./re x(latest photo of the latest model in /photos) (to start from the latest model)

I have changed a few variables;

redistrb.c.pruned.in ==> few variables about redistributing variables after meshing is adapted. replaced 10 with 0 (adaptive meshing parameter for RGB)

inlist_astero ==> is our physics inlist, this has a few options changed for when to output fgong files (files about the pulsations). This has been updated according to Earls Github inlist. I think it has specific RGB oscillation physics stuff.

inlist_astero_search_controls ==> what it says on the tin, the options for the searching algorithm.

chi2_spectro variables; Teff, long etc. Set according to the observed values for the star we are looking at (SET for Star A in Deheuvals et al 2014.). Checks star values against these variables. If they are good enough moves to chi2_seismo

chi2_seismo; delta_nu and nu_max, set according to Star A again. Checks values against the models, when good enough moves onto chi2_radial

chi2_radial; Uses adipls to generate the radial modes of the model. Checks these agains the l=0 modes that are listed with uncertainties. MODEL FAILING HERE. GETTING TO THE POINT WHERE chi2_radial ~50 then jumps to 15,000 in one time step.

After l=0 modes, l=1 and 2 are given.


SEARCH CONTROLS

Search type; Currently have this set to 'simplex'. Haven't touched any of the simplex options because not really any use without calculating chi2.


Skip options from here, these are just options for from file etc. Until you get to first values. I have these set to the best fitting values from Ball 2016 for Star A using the combined surface term. Best fitting values change depending on how the surface effects are dealt with.

Min and max of these values give the range that the values can search over (I have a range of all values but I only have the flags for vary mass and alpha set to true, so just uses the first values of all other variables).


Can set minimum age before doing chi2 checks, (I set this to a fairly low number compared to the ages given in the papers I referenced above because different MESA versions can give wildly different ages and these don't always line up with the 'observed age' of the star.)

chi2_spectroscopic_limit and delta_nu_limit are checks that the model needs to pass before calculating chi2_radial (easier to calc chi2_spectro, delta_nu before radial before full chi_2). Have these set to 10 because I set the time step after these are calculated to be very small (needs to be for RGB because they change so rapidly otherwise could easily step over the best values). Have chi2_radial set to 50 in hopes that a model will pass this limit and calc chi2 full and simplex algorithm may take us to better point(?)


Max time steps, max time steps depending on how close the model is. smaller time steps as we approach the best model, 5d4 is just an informed guess at how small the time step needs to be (frees change by 1 sigma every 1d5 so 5d4 should work?)


"Variable"_limit options; When to kill the current model.
Main killing variable is delta_nu. This needs to be very close to the observed value so the limit can be quite small. Kill when delta_nu value decreases below the observed value by 2 sigma.


Surface correction. Can chose which one you want, 'combined' seems to work best from Ball 2016 (lowest chi2) basically any except kjeldsen will be good (solar calibrated so doesn't work well for RGB stars)


Output controls. Options for when to spit out data for each model. Because not calculating chi2 full not really of much help. Can be used to check the frequencies by hand in an python notebook, but really just outputs the last frame of each model it tries because that's usually the best. 


Played with the atmosphere here compared to what they have from Earls RGB inlist. Set to true and used T-tau as the atmosphere option. Seems to have an effect, only seems to get down to ~250 using it, but has a higher temp than when set to false.

Set do_redistribute_mesh to true as advised for RGB adipls models from forum posts about astero search of RGB models. Doesn't have a large effect that I can see.

The rest of the options are plotting and the range of the number/range of frequencies calculated but this may be fine from the input frees etc. May be worth having a look at to watch how the frequencies evolve.
