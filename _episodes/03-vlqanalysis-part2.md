---
title: "Vector-like quark analysis with ADL/CutLang: Part 1: Analysis algorithm, local runs, shape comparisons"
teaching: 10
exercises: 15
questions:
- "How do I produce plots comparing distribution shapes for signal(s) and background(s)?"
- "How do I produce plots showing data, along with simulated samples of signals and SM backgrounds normalized to analysis integrated luminosity?"

objectives:
- Run the full analysis selection locally on CutLang on limited number of signal and background events.
- Produce plots comparing distribution shapes for signal(s) and background(s) using PyROOT scripting and Jupyter.
- Produce plots showing data, along with simulated samples of signals and SM backgrounds normalized to analysis integrated luminosity" 

keypoints:
- "In a HEP analysis, we usually see two types of plots: distribution shape comparisons between different processes (normalized to a constant, e.g. 1), and plots showing data, backgrounds and signals (normalized to integrated luminosity). "
- "Once an ADL analysis is run by CutLang, the output files can be analyzed with simple ROOT scripts to obtain both types of plots."
---

## Datasets
So far, we have been running only on signal events.  We also have data events and simulated standard model background events to be used with this analysis.
All events are listed in [this file](https://github.com/cms-opendata-workshop/workshop2022-lesson-run2-adlcl/blob/gh-pages/data/CMS-B2G-16-024_POETsamples.txt).

To access these files, write the filename as 
~~~
root://eospublic.cern.ch//eos/opendata/cms/upload/POET/23-Jul-22/<filename>
~~~
e.g., for TT+jets:
~~~
root://eospublic.cern.ch//eos/opendata/cms/upload/POET/23-Jul-22/RunIIFall15MiniAODv2_TT_TuneCUETP8M1_13TeV-powheg-pythia8_flat.root
~~~


## Locally running the complete analysis selection on a limited number of events

The complete analysis selection is included in the ADL file ```CMS-B2G-16-024_step5.adl```, which we studied in the previous episode.  If you do not still have it, please get it via
~~~
wget https://raw.githubusercontent.com/ADL4HEP/ADLAnalysisDrafts/main/CMS-B2G-16-024/CMS-B2G-16-024_step5.adl
~~~

Run this ADL file with CutLang using the VLQ TTm800 signal file on 100000 events.  If you want, you can pipe the output into a text file for comparisons with other datasets, by adding ```>& TT80.txt``` to the end of the run command.
~~~
CLA root://eospublic.cern.ch//eos/opendata/cms/upload/POET/23-Jul-22/RunIIFall15MiniAODv2_TprimeTprime_M-800_TuneCUETP8M1_13TeV-madgraph-pythia8_flat.root POET -i CMS-B2G-16-024_step1.adl -e 100000
~~~

You can also run on SingleMuon collision data:
~~~
CLA root://eospublic.cern.ch//eos/opendata/cms/upload/POET/23-Jul-22/Run2015D_SingleMuon_flat.root POET -i CMS-B2G-16-024_step1.adl -e 100000
~~~

How do selection efficiencies compare for VLQ TT800 signal, tt+jets and data?  
You can run with increased number of events to increase statistics.

### Complete set of analysis output files

The total number of events in the full list of datasets required for this analysis amounts to more than a hundred million.  Running through all samples locally would take hours.  We have the option to run via cloud, which is outside the scope of this exercise, given the limited time.

However, we already ran the analysis on the full set of samples and obtained the output root files.  You can download the full set of output files by
~~~
wget https://www.dropbox.com/s/7yrz7cz9dlrxylv/CMS-B2G-16-024_histoouts.tgz
tar -xzvf CMS-B2G-16-024_histoouts.tgz
~~~
{: .language-bash}

In the CMS-B2G-16-024_histoouts directory, you will find a set of files with name
~~~
histoOut-CMS-B2G-16-024_<samplename>.root
~~~
(we took out ```_step5``` from the naming.)

We will use these files in the remaining exercises.

## Produce plots comparing distribution shapes 

We will compare shapes of various distributions obtained in different regions.  Shape comparison means that all histograms that are compared are normalized to the same quantity, e.g. 1.  

We will use PyROOT commands through a Jupyter notebook.

In your CutLang docker container:
~~~
cd /CutLang/binder
CLA_Jupyter lab
~~~
{: .language-bash}

Then, as described earlier, copy the last html link to a browser window, which will open the Jupyter interface.

In the left panel, open the notebook ```ROOTshapecomparison.ipynb``` by double-clicking on it.
The notebook has instructions.  Follow them to draw shape comparison plots.  Use the various ```histouout...root``` files from the ```CMS-B2G-16-024_histoouts``` directory.

> Make sure that the paths of input ROOT files in the notebook match to the location of your own files!
{: .warning}

> ### Challenge: Playing with plots
> * Can you try comparing shapes between other processes?  Different backgrounds?  Different signals?
> * Can you compare shapes of ```cutflow``` histograms in various regions?
> 
{: .challenge}

## Produce plots showing data, simulated MC and signals

In a real analysis, we usually compare data with simulated samples (such as figs 3,4,5,6,7,8 in the paper).  Signal samples are also overlaid.  Once the background estimation is performed, final distributions in signal regions are plotted, showing data and estimated backgrounds (e.g. figs 9, 10).  In this last part, we will draw such plots.  Of course, since we did not perform data-driven background estimation, we will simply use the simulated MC histograms for the backgrounds.

Execute the following in your CutLang docker container
~~~
cd /CutLang/
wget https://www.dropbox.com/s/0xbkkx3g5kksizo/model_VLQ.tgz
tar -xzvf model_VLQ.tgz
cd model_VLQ
python plotAll.py
~~~
{: .language-bash}

That's it!  Now you will see plenty of ```.png``` figures appearing.  We can view these plots with the help of ```TBrowser```.  Simply execute ```root -l``` in the same directory and start a TNrowser by ```new TBrowser```.  You will see all figures in the TBrowser filesystem.  Just click on the plot you would like to view.

How do the plots compare to those in the paper?

## The end -- or is it?

Congratulations! You have finished the exercise.  We hope you enjoyed writing analyses with ADL and find it useful.  We encourage to test this approach in your own studies.  We are always happy to hear your suggestions and answer your questions! 

Your instructors: *Sezen Sekmen, Gokhan Unel, Burak Sen*








