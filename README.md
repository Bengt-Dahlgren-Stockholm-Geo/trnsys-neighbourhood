# Geothermal borehole in a neighbourhood
This repository contains a TRNSYS component for simulating a geothermal borehole within a neighbourhood of hydraulically independent boreholes. The surrounding boreholes may differ in length, be located at arbitrary positions, and have different installation years. The borehole of interest must be the most recently installed borehole in the area.

The component is implemented in Python as a [Type 3157](https://trnsys.de/static/77828438acd0697c30be234f0f248eff/Calling-Python-from-TRNSYS-with-CFFI.pdf). It relies on the [GSHPsDesigner](https://gitlab.com/mlfasci/GSHPsDesigner) library to model the thermal interaction between the neighbours and the [pygfunction](https://github.com/MassimoCimmino/pygfunction) library to model the borehole self-response. 

To use this component, you must have the following installed:

- TRNSYS 18
- Python 3.10
- Julia 1.11.

**Input interface**

An Excel interface (`GeoInput.xlsx`) allows users to easily modify borehole properties, neighbourhood properties, ground parameters, and circulating fluid characteristics.

This makes it straightforward to set up and customize your geothermal simulation without modifying the Python code directly.

The simulation time step and total simulation duration are configured as usual in TRNSYS by adjusting the settings in the `.tpf` file. **The simulation time step must be set in hours.**

## Table of Contents

- [Detailed installation](#detailed-installation)
- [Usage](#usage)

## Detailed installation
Details about how to configure **Python 3.10** and the **TRNSYS Add-On** are available in the user guide: https://trnsys.de/static/77828438acd0697c30be234f0f248eff/Calling-Python-from-TRNSYS-with-CFFI.pdf

Below is a simplified step-by-step guide including all the steps needed for this specific component.

### 1. Add the TRNSYS Add-On to TRNSYS 18 
1. Go to the [TRNSYS Add-Ons page](https://trnsys.de/en/addons-en) and search for **"Calling Python CFFI Type 3157"**


>These instructions and component are tested on version **v0.6.0 (2022-05-06)** of the add-on.

2. Download the Zip archive containing all the necessary files:
3. Extract the contents of the ZIP archive into your **TRNSYS installation directory**. Default location:

        C:\TRNSYS18

   You should now have a folder similar to:

        C:\TRNSYS18\TRNLib\CallingPython-Cffi\Examples

   This is the folder where you will install the new component in step 5.

### 2. Install python 3.10
> ⚠️ To follow these instructions you need Python 3.10 exactly. It is ok to have other Python versions installed at the same time.
1. Download the **latest 64-bit Python 3.10** release from [python.org](https://www.python.org)  
2. During the installation, make sure to check the box: 

        Add Python to PATH

If you succesfully installed Python 3.10 and could check the box "Add Python to Path" continue to the section "Install the main Python dependencies". Otherwise, if for some reason the **Add Python to PATH** box did not appear during the installation, add Python to Path manually by doing the following:

I. Find where Python is installed. Possible common locations: 

        C:\Users\<YourUser>\AppData\Local\Programs\Python\Python310\
        C:\Program Files\Python310\
II. Open **Edit Environment Variables** from the Windows Start menu.

III. Add two new Paths (make sure that you use the correct paths, the two paths below are just examples):
        C:\Users\<YourUser>\AppData\Local\Programs\Python\Python310\ and
        C:\Users\<YourUser>\AppData\Local\Programs\Python\Python310\Scripts

### 3. Install the main Python dependencies
1. Open the **Command Prompt** (search "Command Prompt" in Windows)
2. Type the following commands **one at a time**, pressing Enter after each:

        py -3.10 -m pip install numpy 
        py -3.10 -m pip install cffi

If you succesfull installed numpy and cffi continue to section "Install Julia". Otherwise, **if the two previous commands did not work**, install the numpy and cffi libraries from Python:

I. Open Python 3.10

II. Run the following commands inside Python, pressing enter after each line:

        import sys
        import subprocess
        subprocess.check_call([sys.executable, "-m", "pip", "install", "numpy"])
        subprocess.check_call([sys.executable, "-m", "pip", "install", "cffi"])

### 3. Install Julia

1. Download the **Julia 1.11.5** release from [julialang.org](https://julialang.org/downloads/oldreleases/). Version 1.11.5 is among the old releases.
 
>   ⚠️  OBS: if you donwload a different version this module may not work! 
3. During the installation, make sure to check the box: 

        Add Julia to PATH

### 4. Install the Julia package GSHPsDesigner

   Start Julia and type the following commands **one at a time**, pressing Enter after each:

       using Pkg;
       pkg"registry add "https://gitlab.com/mlfasci/gshpregistry";
       pkg"registry add General";
       Pkg.add("GSHPsDesigner")

### 5. Download the model from GitHub
Through your **Command Prompt**, navigate to the folder containing the examples installed together with the CallingPython-Cffi Add-On (keep reading to see how to navigate to a folder through the command prompt). Default location:

        C:\TRNSYS18\TRNLib\CallingPython-Cffi\Examples

1. To navigate to the folder through the Command Prompt write:

        cd <your actual location>

   For example:
   
       cd C:\TRNSYS18\TRNLib\CallingPython-Cffi\Examples
   
        
3. Clone this GitHub repository into that folder, still in the Command Prompt run:

        git clone https://github.com/Letizia-BD/trnsys-neighbourhood.git

If you have succesfully cloned the repository a new folder has been created inside "C:\TRNSYS18\TRNLib\CallingPython-Cffi\Examples" (or your equivalent location). You can proceed to section "Install the specific Python dependencies for this model".

If you instead got the error: 

        'git' is not recognized as an internal or external command operable program or batch 

you have to install Git:

I. Download and install the latest Git version from [git-scm.com](https://git-scm.com/install/windows)

II. Make sure **Add Git to PATH** is checked during installation.

III. Restart your computer and try the clone command again.

### 6. Install the specific Python dependencies for this model
In the **Command Prompt**, run these commands **one at a time**, pressing Enter after each:

            py -3.10 -m pip install openpyxl==3.1.5
            py -3.10 -m pip install scipy==1.15.3
            py -3.10 -m pip install pandas==2.2.3
            py -3.10 -m pip install juliacall==0.9.25
            py -3.10 -m pip install pygfunction==2.3.0
            py -3.10 -m pip install pytest==8.3.5

### 7. First simulation

In the folder **trnsys-neighbourhood** (it should be in a location similar to C:\TRNSYS18\TRNLib\CallingPython-Cffi\Examples\trnsys-neighbourhood), you can find the file **test_trnsys_neighbourhood.py**. Run* this python script to check if everything works as expected. 
*to run the python script you can:

1. navigate to the trns-bns directory through the command prompt:

        cd <directory_name>

   for example:

        cd C:\TRNSYS18\TRNLib\CallingPython-Cffi\Examples\trnsys-neighbourhood

2. type:

           python test_trnsys_neighbourhood.py

   and press enter.

If everything works properly, the command prompt will state that the test passed. Moreover, a new file will appear in the **trnsys-bns** folder named **Tout_neighbourhood.txt**. 

## Usage

Use the **GeoInput.xlsx** file to define your borehole and he neighbouring boreholes, including:

- geometrical characteristics and year of instalaltion of all boreholes
- estimated yearly borehole extraction of each neighbouring borehole
- ground properties
- circulating fluid

See the example below:
<!--
![Set the geometrical boreholes properties](images/Excel_BHs.png)
![Set the ground properties](images/Excel_ground.png)
![Select the circulating fluid](images/Excel_fluid.png)
-->
<div style="display:flex; gap:15px; align-items:flex-start;">
<img src="images/Excel_BH.png" alt="Set the borehole properties" width="300
  <img src="images/Excel_BHs.png" alt="Set the properties of the neighbouring boreholes" width="300"/>
<img src="images/Excel_ground.png" alt="Set the ground properties" width="300"/>
<img src="images/Excel_fluid.png" alt="Select the circulating fluid" width="300"/>
</div>


>   ⚠️  OBS: the component allows any number of neighbouring boreholes, but only **ONE** borehole can be connected to the rest of the TRNSYS system. The borehole connected to the TRNSYS system must be the most recently installed borehole in the area.

The simulation time and time step are set as usual for a TRNSYS simulation. 

>   ⚠️  OBS: The unit for time and time steps **MUST BE** hours for the component to work correctly. 
            
