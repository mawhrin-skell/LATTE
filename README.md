# LATTE

Written by Nora L. Eisner

email: *nora.eisner@new.ox.ac.uk*

![LATTE logo](https://github.com/noraeisner/LATTE/blob/master/LATTE/LATTE_imgs/LATTE_logo_small.png)


### THE CODE

--------

*The aim of this code is to provide a series of diagnostic tests which are used in order to determine the nature of events found in *TESS* lightcurves.*


--------
--------

### Installation

LATTE requires python3 to be installed on your computer, which can be download from https://www.python.org/downloads/. Once you have python3, you can download the code directly from github. Alternatively you can install LATTE using pip (https://pypi.org/project/tessLATTE/) via your command line with:

	pip3 install tessLATTE      

In order for LATTE to work you will need to have the right versions of certain modules installed, so downloading it in a virtual environemt. **Note: ensure that the matplotlib version that you are using is v3.2.0rc1 (pip install matplotlib==3.2.0rc1). You will also need a module called TKinter installed. If this is not already installed please use: sudo apt-get install python3-tk .**

The first time that the program is run you will be prompted to enter a path to a file on your computer where all the output data will be stored (this path can be changed later using --new-path). The first time that the code is run it will also have to download the text data files from MAST, which are around 325M. This download will only have to run in full once but may take a couple of minutes to complete.

If the code doesn't run when you first install it, this could be due to an SSL issue. To check, please try executing the following command from your command line: **python -m tess_stars2px -t 261136679**.

If this command returns the error: *"ssl.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed"*, you will need to change your SSL settings, which can be done following these [instructions](https://timonweb.com/tutorials/fixing-certificate_verify_failed-error-when-trying-requests_html-out-on-mac/). 


### How to run it?

LATTE is simply run through the command line with:

	python3 -m LATTE      or     python3 -m LATTE --new-data     (if run for the first time or when new data is released)

This will open up a box for you that prompts you to enter a TIC ID and indicate whether you would like to extract the information from the 2-minute cadence ('Standard mode') or 30-minute candence Full Frame Image (FFI) data ('FFI mode').

Once a TIC ID has been entered, the program will tell you in what sectors that target has been observed. If you want to look at all of the sectors, either enter them or simply press enter with nothing in the box. Alternatively, enter the sectors that you are interested in and enter them separated by commas. Remember that LATTE will have to download all the data for each sector so you might not always want to look at all of the sectors. 

**Normal Mode**

The '*normal mode*' looks at the short-cadence *TESS* data which has already been detrended and extracted by the SPOC pipeline. Optimal aperture lightcurve extraction aperture sizes have therefore already been identified and do not need to be selected manually.


**FFI Mode**

In *FFI mode* the data is downloaded using TESScut and the data detrended using PCA, a moving average filter and k-sigma clipping. Unlike the *TESS* 2-minute cadence targets, the FFIs do not come with a pre-chosen optimal aperture. By default, the user is given the option to manually select the pixels within each aperture for a 'large' and a 'small' aperture. This GUI opens automatically and the two apertures are selected by clicking on the desired pixels in the interface. The two (large and small aperture) lightcurves are simultaneously displayed. When the FFI mode is run with the command '--auto', the aperture size is chosen manually using a threshold flux value centred at the midpoint of the postage stamp.


## Input target list

In order to efficiently generate diagnostic plots for multiple targets without having to interactively enter the target TIC IDs, ``LATTE`` can be executed with an input file that lists the TIC IDs, the times of the transit-like events, and the observational sectors to consider (if known). See example. In the cases where the times of the transit events or the sectors have not been entered, the user is prompted to enter these manually via the GUI, as descibed above. For longer target lists the code can also be parallelized (see example). If you want to run it with this mode, enter: 

	python3 -m LATTE --targetlist*=path/to/the/csv/input/file*

The program will skip targets that have already been analysed by you, so if you want to overwrite data add '--o' to the command.

### Transit time selection

Once you have identified the TIC ID, the observational sectors and the aperture sizes (*FFI mode* only), you will see a screen that has the full lighcurve as well as a zoom in of the lightcurve. The solid red line across the full lightcurve lets you know where on the lighcurve you are zooming in on. Click on the top (full) or bottom (zoomed in) plots to change the location of the zoom in until the red vertical line is centred on the mid-point of a transit-event. When you are happy with the location press the 'Add Time' button below the plots in order to record this time (in TBJD) . You can delete wrongly entered times with the 'Delete time'. The saved times will be shown on the screen. The position of the red line can also be changed by dragging the teal coloured 'Transit' slider with your mouse. The y-scale of the plots can be changed with the grey coloured slider.

Additional options are displayed to the left of the plots.

Binning Factor: 
- change the binning of the top plot (only available in the normal and not FFI mode)

Settings:

- Simple: only run the most basica diagnostic tests (not suing TPF). This is for a very quick analysis. 
- Hide Plots: Don't display the plots after they are made (will still store them). This speeds up the code.
- North: Align all the images due North (this slows down the code).
- BLS: Run a Box-Least-Squares algorithm that searches for periodic signals in the lightcurve. 
- Save: Save all the images (default this is already checked)
- Report: Generate a pdf file at the end of the code to summaise the findings (default this is already checked).

Finally, there is an optional box to enter a 'Memorable name' for the candidate. This name is used to store the data at the end in order to make identifying certain targets easier.


Once all the options have been chosen and the transit times stored (you have to enter at least one transit time), press the orange 'Done' button to continue.

The code will then generate download and process all of the data. Note that all the data is downloaded but not stored. The more sectors you analyse in one go the longer the code will take to run.


### Output


**Figures:**

- Full lightcurve with the times of the momentum dumps marked. 

![Full LC](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_fullLC_md.png)

- Background flux around the times of the marked transit event(s).

![Background Flux](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_background.png)

- Centroid positions around the time of the marked transit event(s).

![Centroid](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_centroids.png)

- The lightcurve around the time of the marked event(s) extracted in two different aperture sizes. 

![Aperture Size](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_aperture_size.png)

- The apertures used for the extraction. Please note that for very bright and very faint stars the aperture selection for the smaller aperture may not work correctly so these should be checked in order to correctly interperet the results.

![Apertures](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_apertures_0.png)

- The average flux in and out of the marked event(s) and the differences between the two.

![In and out of transit flux comaprison](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_flux_comparison.png)

- The average flux of the target pixel file with the locations of nearby stars (magnitude < 15) indicated (GAIA DR2 queried).

![Nearby stars](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_star_field.png)

- The lightcurves of the 6 closest stars that were also observed by *TESS* (TP files).

![Nearest Neighbours](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_nearest_neighbours.png)

- A lightcurve around the time of the marked event(s) extracted for every pixel in the target pixel file.

![Nearest Neighbours](https://github.com/noraeisner/LATTE/blob/master/example_output/94986319_individual_pixel_LCs_0.png)

- (optional) Two simple BLS plots. The second with the highest detected signal-to-noise transits from the initial run removed.
- (in progress, will be available in next release of LATTE) Modelling of the signal using a Bayesian approach with an MCMC sampling. This makes use of the Pyaneti code (Barragan et al. 2017). 

**FFI Mode**

- The FFI mode currently does not plot the nearby stars lightcurves (will be implemented soon).
- Saves the extracted apertures used
- Saves the corrected and the uncorrected lightcurves to verify that the detrending is workign correctly - there's are nt stored in the DV reports. 


**Tables:**

- Stellar parameters summarized, as well as information to whether the target has been flagged as a TCE or a TOI. The table links to the relevant reports (if applicable) as well as to the exofop page on the star.
- (optional) Summary of the BLS results. 
- (optional) Fitting parameters with uncertainties from the modelling. 


### Arguments

NOTE: all of these arguments (except new-path, auto and targetlist, new-data) can be changed as option in the GUI. They are arguments in case the same options wish to be run multiple times and the user therefore wishes to identify them in the command line when the program is executed.

**--new-data**  The code requires multiple text files to be stored on your computer in order to run - these are downloaded automatically from the MAST server. The first time the proghram is run, and any time that there is new data available, add **--new-data** to the command line when running the program. The code checks what data has already been downloaded and doesn't re-download anything that already exists.

**--auto** When looking at the FFIs, the default option is that you choose both the large and small apertures interactivelty. In order for the system to choose them run the command with '--auto'. 

**--targetlist***=path/to/the/csv/input/file* instead of entering the information manually everytime, you can provide LATTE with an input file which lists all the TIC IDs and the times of the transit events. Look at the example file to see the required format of the information.

**--new-path** If you want to define a new path to store the data.

**--o** If the input target list option is chosen, the program will check whether each target has already been analysed, in which case it will skip this target. If you do not wish to skip targets that have already been assessed use this in order to specify the 'overwrite' option. When the program is run interactively (not with an input file) this option has no effect.

**--noshow** if you do not want the figures to pop up on your screen you can run the program with this command in the command line. The program will run significantly faster with this run. If this option is chosen the files are always saved (also option in the GUI)

**--nickname** In order to keep track of all of the candidates, it can be useful to asign them a nickname. This can be entered here which will simply change the name of the folder at the end (also option in the GUI)

**--FFI** If you want to look at an FFI write '--FFI' in the command line (also option in the GUI) 

**--north** If you want all the images to be oriented due north (this is not the default as it takes longer to run)

**--mpi** If the code is parallelized (see example), this needs to be entered in the command line. This is because the module that reprojects the TPFs, astroplan, cannot be parallelized due to problems with multiple processes accessing python's shelve storage at the same time.

**--tic** You can skip the box to enter the tic ID by entering it in the command line with e.g. --tic=55525572. 

**--sector** You can skip entering the sectors by entering them in the command line with e.g. --sector=2,5. You will need to know in what sectors this target was observed (option in the GUI)

### Perform automated tests

The code has been tested using the python unittest module. There are a number of tests including testing the connections to the servers where data is downloaded as well as tests that process pre-downloaded data in order to ensure that the outputs are as expected. In order to execute the tests please clone the GitHub LATTE repository. You must then be in the parent LATTE directory and from there you can run all the tests using: 

	python3 -m unittest

or individual tests using, for example: 
	
	python3 -m unittest LATTE/tests/test_DVreport.py

Before running the tests makes sure that you have configured an output path (*python3 -m LATTE --new-path*) and that the data files are downloaded (*python3 -m LATTE --new-data*). 

### Contributing

If you believe that you have found any bugs in the code or if you need help or have any questions or suggestions, please feel free to file a new Github issue. You can also raise an issue or a pull request which fixes the issue. Please see CONTRIBUTING.md for more details. 


### Future Work

The next release will allow for the option to model the transit-like events using the open source package *Pyaneti* [@pyaneti] which uses a Bayesian approach with an MCMC sampling to determine the parameters of the best-fit.

Future releases will also allow for modeling and removal of periodic trends in the light curve. This is useful for the detection and circumbinary planets and for the analysis of multi-star systems. 


### License 

LATTE can be used under GNU Lesser General Public License v3.0


### Acknowledgements

I thank all of the Planet Hunters *TESS* volunteers whose dedication to the project encouraged me to write this analysis tool. I am also extremely grateful to all of the support provided by the Zooniverse team and the Oxford exoplanet group.














