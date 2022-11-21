---
title: "Installing CutLang"
teaching: 0
exercises: 20
questions:
- "How do I access information about CutLang in general?"
- "How do I install CutLang via Docker?"
- "How do I test my installation?"
objectives:
- "Setup CutLang via Docker"
- "Understand the most basic concepts about running CutLang"
- "Perform a test run with CutLang on an open data POET ntuple and check the output"
keypoints:
- "For up-to-date details for installing CutLang, the official documentation is the best bet."
- "Make sure you were able to setup CutLang via Docker and run its hello-world example."
- "Running CLA is the only thing that a user must know in order to work with CutLang"
---

Please make sure that you prepare the CutLang setup before the exercise.

## Installing CutLang: introduction

CutLang is the runtime interpreter we will use for processing ADL files on events.
CutLang is compatible multiple platforms.  It can be installed directly on Linux and MacOS, on all platforms via Docker or Conda, or can be accessed via a Jupyter/Binder interface.  
The generic and most up-to-date instructions for setting up CutLang can be found in the [CutLang github readme](https://github.com/unelg/CutLang#readme)

In this exercise, we will use run CutLang via a docker container as described in the next section.

## CutLang setup via Docker

We have prepared a CutLang docker container which functions similarly to [other Docker containers that work with CMS Open Data](https://cms-opendata-workshop.github.io/workshop2022-lesson-docker/).  The setup contains:
  * CutLang
  * ROOT
  * xrootd access to open data ntuples
  * VNC
  * Jupyter

1. As a first step, make sure that you have [Docker desktop](https://www.docker.com/products/docker-desktop) installed.

You can find detailed instructions on how to install Docker and on simple Docker concepts and commands [in this tutorial](https://cms-opendata-workshop.github.io/workshop2022-lesson-docker/02-installing-docker/index.html).

2. Next, download the CutLang image and run container in current directory from downloaded image

For linux/MacOS:
~~~
 docker run -p 8888:8888 -p 5901:5901 -p 6080:6080 -d -v $PWD/:/src --name CutLang-root-vnc cutlang/cutlang-root-vnc:latest 
~~~
{: .language-bash}

If you would like to re-run by mounting another directory, you should stop the container using
~~~
docker stop CutLang-root-vnc && docker container rm CutLang-root-vnc
~~~
{: .language-bash}

and rerun with a different path as ```docker run -p 8888:8888 -p 5901:5901 -p 6080:6080 -d -v /path/you/want/:/src ...``` . 
For example:
~~~
docker run -p 8888:8888 -p 5901:5901 -p 6080:6080 -d -v ~/example_work_dir/:/src --name CutLang-root-vnc cutlang/cutlang-root-vnc:latest
~~~
{: .language-bash}

For Windows:
~~~
 docker run -p 8888:8888 -p 5901:5901 -p 6080:6080 -d -v %cd%/:/src --name CutLang-root-vnc cutlang/cutlang-root-vnc:latest
 ~~~
{: .language-bash}

If you would like to re-run by mounting another directory, you should stop the container using
~~~
docker stop CutLang-root-vnc && docker container rm CutLang-root-vnc
~~~
{: .language-bash}

and rerun with a different path as ```docker run -p 8888:8888 -p 5901:5901 -p 6080:6080 -d -v /path/you/want/:/src ...``` . 
For example:
~~~
docker run -p 8888:8888 -p 5901:5901 -p 6080:6080 -d -v ~/example_work_dir/:/src --name CutLang-root-vnc cutlang/cutlang-root-vnc:latest
~~~
{: .language-bash}

3. Execute the container using

~~~
docker exec -it CutLang-root-vnc bash
~~~
{: .language-bash}

If you have installed the container successfully, you will see
~~~
For examples see /CutLang/runs/
and for LHC analysis implementations, see
https://github.com/ADL4HEP/ADLLHCanalyses
~~~
{: .output}

Now, the container is ready to run CutLang.

You can leave the container by typing
~~~
exit
~~~
{: .language-bash}


### Update (if relevant)
In case an update is necessary, you can perform the update as follows:
~~~
docker pull cutlang/cutlang-root-vnc:latest
docker stop CutLang-root-vnc && docker container rm CutLang-root-vnc
docker run -p 8888:8888 -p 5901:5901 -p 6080:6080 -d -v $PWD/:/src --name CutLang-root-vnc cutlang/cutlang-root-vnc
~~~
{: .language-bash}

### Remove

The CutLang docker container and image can be removed using
~~~
docker stop CutLang-root-vnc
docker ps -a | grep "CutLang-root-vnc" | awk '{print $1}' | xargs docker rm
docker images -a | grep "cutlang-root-vnc" | awk '{print $3}' | xargs docker rmi
~~~
{: .language-bash}

## Starting VNC and Jupyter
### Starting VNC

* VNC works similarly as in the other containers for CMS Open Data (described [in this tutorial](https://cms-opendata-workshop.github.io/workshop2022-lesson-docker/03-docker-for-cms-opendata/index.html)).  Once in the CutLang container, type
~~~
start_vnc
~~~
{: .language-bash}
You will see
~~~
VNC connection points:
	VNC viewer address: 127.0.0.1::5901
	HTTP access: http://127.0.0.1:6080/vnc.html
~~~
{: .output}
Copy the http address to your web browser and click connect.  Note that the vnc password is different for this exercise.

VNC password
~~~
cutlang-adl
~~~
Before leaving the container, please stop vnc using the command
~~~
stop_vnc
~~~
{: .language-bash}

## Starting Jupyter
### Starting Jupyter

We will do some plotting using pyROOT.  The plotting scripts can also be directly edited and run at commandline, but we will use Jupyter for this exercise.
Once in the container, type the command

~~~
CLA_Jupyter lab
~~~
{: .language-bash}

If all went well, you will see an output whose last few lines look as follows:
~~~
    To access the server, open this file in a browser:
        file:///home/cmsusr/.local/share/jupyter/runtime/jpserver-158-open.html
    Or copy and paste one of these URLs:
        http://da22da048c0d:8888/lab?token=c56366692a641a939320264a79dcb23a7a962e202896f5c0
     or http://127.0.0.1:8888/lab?token=c56366692a641a939320264a79dcb23a7a962e202896f5c0
~~~
{: .output}
Now open a browser window and copy the http address at the last line, e.g. starting with ```http://127.....``` into the browser.  You will see that a jupyter notebook opens.

In order to exit the Jupyter setup and go back to the container commandline, press ```ctrl-c```, and when prompted ```hutdown this Jupyter server (y/[n])?```, type ```y```.

## Run CutLang

In the CutLang Docker container, type
~~~
CLA
~~~
{: .language-bash}

If all went well, you will see the following output:
~~~
/CutLang/runs/CLA.sh
/CutLang/runs
/CutLang
ERROR: not enough arguments
/CutLang/runs/CLA.sh ROOTfile_name ROOTfile_type [-h]
    -i|--inifile
    -e|--events
    -s|--start
    -h|--help
    -d|--deps
    -v|--verbose
    -j|--parallel
ROOT file type can be:
 "LHCO"
 "FCC" 
 "POET" 
 "DELPHES2"
 "LVL0"
 "DELPHES"
 "ATLASVLL"
 "ATLMIN"
 "ATLTRT"
 "ATLASOD"
 "ATLASODR2"
 "CMSOD"
 "CMSODR2"
 "CMSNANO"
 "VLLBG3"
 "VLLG"
 "VLLF"
 "VLLSIGNAL"
~~~
{: .output}

This output explains us how to run CutLang.  We need the provide following inputs:
  * input ROOT file containing events.  ```ROOTfile_name``` specifies the address of this file.  
    * We also need to specify the type of this ROOT file, i.e. the format of the events, via ```ROOTfile_type```.  You can see above that CutLang automatically recognizes ROOT files with many different formats.  For this exercise, our input are of the ```POET``` type.
  * ADL file with the analysis description.  The ADL file is input via the ```-i``` flag.

There are also optional flags to specify run properties.  Here are two practical examples:
  * ```-e``` : Number of events to run.  For example, ```-e 10000``` means we run only over 10000 events.
  * ```-d``` : Enables a more efficient handling of dependent regions.  Reduces runtime by 20-30 percent.
  
Now let's try running CutLang!

For this test run, we will use a simple ADL file called ```/CutLang/runs/tutorials/ex00_helloworld.adl```.  First, open this ADL file using ```nano``` or ```vi``` to see how it has described a very simple object and event selection.  It should be quite self-descriptive!

For input events, we will use a POET open data simulation sample: ```root://eospublic.cern.ch//eos/opendata/cms/derived-data/POET/23-Jul-22/RunIIFall15MiniAODv2_TprimeTprime_M-800_TuneCUETP8M1_13TeV-madgraph-pythia8_flat.root```

Now, run CutLang with the folloing command:

~~~
CLA root://eospublic.cern.ch//eos/opendata/cms/derived-data/POET/23-Jul-22/RunIIFall15MiniAODv2_TprimeTprime_M-800_TuneCUETP8M1_13TeV-madgraph-pythia8_flat.root POET -i /CutLang/runs/toturials/ex00_helloworld.adl -e 10000
~~~
{: .language-bash}

WARNING: Please ignore the file does not exist message saying ```root://eospublic.cern.ch//eos/opendata/cms/derived-data/POET/23-Jul-22/RunIIFall15MiniAODv2_TprimeTprime_M-800_TuneCUETP8M1_13TeV-madgraph-pythia8_flat.root does not exist.```

If CutLang runs successfully, you will see an output that ends as follows:
~~~
number of entries 10000
starting entry 0
last entry 10000
Processing event 0
Processing event 5000
Efficiencies for analysis : BP_1
						preselection	Based on 10000 events:
                                                               ALL :      1 +-         0 evt:    10000
                                                size(goodjets) > 3 :  0.971 +-   0.00168 evt:     9710
                                             pt(goodjets[0]) > 300 : 0.7748 +-   0.00424 evt:     7523
                                             pt(goodjets[1]) > 200 : 0.8551 +-   0.00406 evt:     6433
                                             pt(goodjets[2]) > 100 : 0.9431 +-   0.00289 evt:     6067
                                                    [Histo] hnjets :      1 +-         0 evt:     6067
                                                   [Histo] hjet1pt :      1 +-         0 evt:     6067
                                                   [Histo] hjet2pt :      1 +-         0 evt:     6067
					 --> Overall efficiency	 =   60.7 % +-  0.488 %
Bins for analysis : BP_1
saving...	saved.
finished.
CutLang finished successfully, now adding histograms
hadd Target file: /src/histoOut-CMSOD-hello-world.root
hadd compression setting for all output: 1
hadd Source file 1: /src/histoOut-BP_1.root
hadd Target path: /src/histoOut-ex00-helloworld.root:/
hadd Target path: /src/histoOut-ex00-helloworld.root:/preselection
hadd finished successfully, now removing auxiliary files
end CLA single
~~~
{: .output}

> **OPTIONAL:** Copying events file to local
> 
> If you are connecting from outside CERN, it may take a while, e.g. several minutes for CutLang to access the file and start processing events.  To make things faster, > you can alternatively copy the events file to your local directory and run on this local file.
> 
> You can copy the file using the ```xrtdcp``` command
> ~~~
> xrdcp root://eospublic.cern.ch//eos/opendata/cms/derived-data/POET/23-Jul-22/RunIIFall15MiniAODv2_TprimeTprime_M-800_TuneCUETP8M1_13TeV-madgraph-pythia8_flat.root .
> ~~~
> {: .language-bash}
> then run CutLang as
> ~~~
> CLA RunIIFall15MiniAODv2_TprimeTprime_M-800_TuneCUETP8M1_13TeV-madgraph-pythia8_flat.root POET -i /CutLang/runs/tutorials/ex00_helloworld.adl -e 10000
> ~~~
> {: .language-bash}
> {: .testimonial}

Look at this output and try to understand what happened during the event selection.

You should also get an output ROOT file ```histoOut-ex00-helloworld.root``` that contains the histograms you made and some further information.

Start VNC, if you have not already done so.  Then open the root file and run a ```TBrowser```:
~~~
root -l histoOut-ex00-helloworld.root
new TBrowser
~~~
{: .language-bash}
Go to your web browser, where you should see the TBrowser open.  Click on the ```histoOut-ex00-helloworld.root``` file, then on the ```preselection``` directory.  

You will see the histograms you made in the preselection directory.  Click on each histogram and look at the content.

You will also see another (automatically created) histogram called ```cutflow```, which shows the event selection steps and events remaining after each step.

When you are done, you can quit the ROOT interpreter by ```.q```.

Congratulations! Now you are ready to run ADL analyses via CutLang!

## Run the tutorials for ADL/CutLang syntax

ADL syntax is self-descriptive.  One can study and run several tutorial examples to learn the main syntax rules.
These examples can be seen by ```ls /CutLang/runs/tutorials/*.adl```

Let's first download some simple event samples to run on the examples:
~~~
wget https://www.dropbox.com/s/ggi78bi4b6fv3r7/ttjets_NANOAOD.root
wget http://opendata.atlas.cern/release/samples/MC/mc_105986.ZZ.root
wget https://www.dropbox.com/s/zza28peyjy8qgg6/T2tt_700_50.root
~~~
{: .language-bash}
The samples contain ttjets events in CMSNANO format, ZZto4lepton events in ATLASOD format and SUSY events in DELPHES format, respectively,

Please first look into each file and understand the algorithm and syntax.  Then run the ADL files with the commands given below.  If there are histograms made, check out the resulting ROOT file and inspect the histograms.

~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex01_selection.adl
~~~
{: .language-bash}

~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex02_histograms.adl
~~~
{: .language-bash}

~~~
CLA mc_105986.ZZ.root ATLASOD -i /CutLang/runs/tutorials/ex03_objreco.adl
~~~
{: .language-bash}
~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex04_syntaxes.adl
~~~
{: .language-bash}
~~~
CLA T2tt_700_50.root DELPHES -i /CutLang/runs/tutorials/ex05_functions.adl
~~~
{: .language-bash}
~~~
CLA T2tt_700_50.root DELPHES -i /CutLang/runs/tutorials/ex06_bins.adl
~~~
{: .language-bash}
~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex07_chi2optimize.adl
~~~
{: .language-bash}
~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex08_objloopsreducers.adl
~~~
{: .language-bash}
~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex09_sort.adl
~~~
{: .language-bash}
~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex10_tableweight.adl
~~~
{: .language-bash}
~~~
CLA ttjets_NANOAOD.root CMSNANO -i /CutLang/runs/tutorials/ex11_printsave.adl 
~~~
{: .language-bash}
~~~
CLA T2tt_700_50.root DELPHES -i /CutLang/runs/tutorials/ex12_counts.adl
~~~
{: .language-bash}

More ADL files for various full LHC analyses (focusing on signal region selections) can be found [in this git repository](https://github.com/ADL4HEP/ADLLHCanalyses).


{% include links.md %}
