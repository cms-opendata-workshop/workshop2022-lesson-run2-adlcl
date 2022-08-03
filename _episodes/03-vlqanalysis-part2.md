---
title: "Vector-like quark analysis with ADL/CutLang: Part 1: Analysis algorithm, local runs, shape comparisons"
teaching: 20
exercises: 60
questions:
- "How do I produce plots comparing distribution shapes for signal(s) and background(s)?"
- "How do I produce plots showing data, along with simulated samples of signals and SM backgrounds normalized to analysis integrated luminosity?"

objectives:
- Run the full analysis selection locally on CutLang on limited number of signal and background events.
- Produce plots comparing distribution shapes for signal(s) and background(s) using PyROOT scripting and Jupyter.
- Produce plots showing data, along with simulated samples of signals and SM backgrounds normalized to analysis integrated luminosity" 
---

## Datasets
So far, we have been running only on signal events.  We also have data events and simulated standard model background events to be used with this analysis.
All events are listed in ................ file.

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
wget .....
tar -xzvf ......
~~~
{: .language-bash}

In the VLQrunresults directory, you will find a set of files with name
~~~
histoOut-CMS-B2G-16-024_<samplename>.root
~~~
(we took out ```_step5``` from the naming.)

We will use these files in the remaining exercises.

## Produce plots comparing distribution shapes 







