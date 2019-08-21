# SumUpTools
#### Copyright © 2019 Max Stevens <maxstev@uw.edu>

Tools to work with the .nc-formatted SumUp database.

This is a work in development. I created it to make working with the sumup database easy for my own needs. If there is a feature you want me to add, or you want to add yourself, let me know. Currently, these tools are only for working with the density dataset.

If you want to use these tools you need at minimum a basic knowledge of the python package pandas, and ideally familiarity with jupyter notebooks.

There are two files: sumup.py and SumUpTools.ipynb, which is a jupyter notebook. 

## Requirements
You will need python 3.7 (should work on python 3.6, but untested) with the numpy, scipy, matplotlib, and pandas packages. I make use of pandas dataframes, so if you are unfamiliar with those you might need to learn how they work. I also make use of jupyter notebooks as a means of working with the data.

## Workflow

The workflow should be:
- Download the sumup database (sumup_density_2019.csv)
- Run sumup.py. It takes a bit of time because it needs to parse out all the unique cores, which it does by comparing citation, date, and location.
- Monkey with the data as you need. See below.

## Files
### sumup.py
sumup.py creates 4 files: sumup_antarctica.pkl, sumup_antarctica.csv, sumup_greenland.pkl, and sumup_greenland.csv. The antarctica files are technically any data from the southern hemisphere, and greenland anything from the northern hemisphere.

The script processes the sumup netCDF-formatted database of firn cores and puts them into pandas dataframes, which are then saved in pickles (.pkl files). The dataframes are generated by assigning each unique core a number (which is arbitrary. It is just the order of the core in the .nc database). The data is split into two pandas dataframes (one for Antarctica and one for Greenland). The data is saved in a pickle for easy reloading (the processing is slow because the script needs to find unique values.) 

The changes that I make from the original sumup database are:
- There is a core from the 1950's (likely a Benson core from Greeland) that has the lat/lon listed as -9999. I set the coordinates of the core to 75N, 60W.
- For cores that do not have a specific date, Lynn put 00 for the month and day. I use January 1 for all of those cores so that I can create a datetime object.
- Let me know if you find other anamalous cores that need special treatment.

sumup.py also saves metadata (lat/lon, citation, coreid number, bottom depth, date) for each core to a csv file that can be imported into google earth to see the locations of each core.

I have not done this, but it should be easy to do manipulte sumup.py to write a csv with only cores meeting some criteria, e.g. deeper than some depth.

### SumUpTools.ipynb
SumUpTools.ipynb is a jupyter notebook that I threw together for easily working the the pandas dataframes. It loads the dataframes (from the pickles) and you can have all the fun you want from there. I have added a few cells as examples of how to query the dataframes.

The dataframes use a multiindex, so you can query the dataframe on any of those levels.

## Loading the metadata into Google Earth
Once you have run sumup.py, you should have files sumup_antarctica.csv and sumup_greenland.csv. In Google Earth Pro, do:
#### file --> open --> select sumup_antarctica.csv or sumup_greenland.csv, click 'open'
This opens the Data Import Wizard. It should just work if you click Finish. You can click No for a style, which will give you white and black dots as markers, but if you want to use different colors you can add a style. 

Once the data are in Google Earth Pro, you can click on any of the markers and get the metadata for that core (Lat,Lon, Citation #, core id #, bottom depth, and date). You can then use that information to find that particular core in the dataframes. 


## License
Distributed under terms of the MIT license.
