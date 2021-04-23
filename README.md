# Surface-Simulation
Surface interaction simulation (v1, v2, and Summary)

## Installation of Jupyter Notebook (Windows)

1. Install Jupyter Notebook
    * Open command prompt
      * Right click Windows button (bottom right of screen)
      * Click "run"
      * Type "cmd"
      * Enter the following:
```bash
pip install jupyterlab
```
If you do not have pip installed, make sure you download Python 2 or Python 3 from the official website: https://www.python.org/downloads/

2. Open Jupyter Notebook
      * Right click Windows button (bottom right of screen)
      * Click "run"
      * Type "jupyter notebook"

Jupyter Notebook should boot up on your default browser. You'll be able to see the directories of your computer.

3. Download the code packages ("Cartesian Coordinate" and "Matrix Simulation").
4. Reload jupyter notebook in your browser, and go to the folder where you've downloaded the code packages.
5. The .ipynb file can be found and opened.

## Setup (v1a - Cartesian Coordinate)
Note: v1 is much harder to use and requires the user to know how to read code.
### Prereqs:
* Open CartesianCoordinateSimulation.ipynb
* Make sure the Results.xlsx file is in the same directory
### Description:
* Gradient Surface (0% positive <-> 100% positive)
* Cylindrical Bacteria
* **Main purpose** -> run lots of simulations with outputs in Excel sheet: surface charge, (histogram) where the bacteria lands on the gradient, number of interactions
### Parameters (code to look for):
* Most variables that are allowed to be change are located near the **END** of the code

## Setup (v1b - Cartesian Visualization)
### Prereqs:
* Open CartesianVisualization.ipynb
### Description:
* Gradient Surface (0% positive <-> 100% positive)
* Cylindrical Bacteria
* **Main purpose** -> produce a visualization of what your bacteria and surface look like (before and after interaction).

## Changing domain size requires changing the code a lot...
For CartesianCoordinateSimulation.ipynb, Lines 217-251 and 452-471 need to be modified to change domain size. Both of these, require a deep understanding of how the cylinder is made from a 1D array into a 3D array.

If domain size wants to be changed / Questions / Help: please reach out to **stanley.lo@mail.utoronto.ca** with email title "Surface Simulation Troubleshooting"

## Setup (v2a - Matrix Simulation)
Note: v2 is more user friendly, and has better infrastructure to modify the simulation settings. Also, an Excel sheet is automatically created for this version.
### Prereqs:
* Open main_matrix.ipynb
* Change the _dirname_ variable to the directory of your choice. (format: dirname = r"your_directory")
### Description:
* Net Zero Surface (50% positive / 50% negative), Domains are negative
      * Concentration can be changed by modifying the 0.5 in: 
```bash
num_domains = int(((10500*10500)*0.5)/144)
```
* Flat Bacteria
* **Main purpose** -> run lots of simulations with outputs in Excel sheet: Characteristics of bacteria/surface and minimum energy of each simulation
* Domain Size Table:
<br />

| Domain Name | Size (pts/domain) | Shape |
| :---------: | :--: | :---: |
| single | 1  | circle/dot |
| cross | 5  | cross/plus sign |
| small | 12  | octagon |
| medium | 25  | diamond |
| large | 40  | diamond |
| xlarge | 60  | diamond |
| xxlarge | 84  | diamond |
| xxxlarge | 112  | diamond |
| x4large | 144  | diamond |
| x5large | 180  | diamond |
| x6large | 220  | diamond |
| x7large | 264  | diamond |
| x8large | 312  | diamond |
| x9large | 364  | diamond |
| x10large | 420  | diamond |
| x11large | 480  | diamond |
| x12large | 544  | diamond |
| x13large | 612  | diamond |
| x14large | 684  | diamond |
| x15large | 760  | diamond |
| x16large | 840  | diamond |

### Parameters:
The parameters will be available to change when the code is run. Prompts will guide you to enter the desired parameters.
<br />
Num_sim - indicates which type of simulation you can run.
   * Num_sim = 1 (runs 1 simulation (not very useful).)
   * Num_sim = 2 (creates 1 surface with many different bacteria interacting with the same surface.)
   * Num_sim = 3 (creates 1 bacteria with many different surfaces interacting with the same bacteria.)

## Setup (v2b - Matrix Visualization)
Visualizing the **bacteria** uses the matrix_bacteria_visualization.ipynb file.

### Steps:
* Open matrix_bacteria_visualization.ipynb
* The bacteria can be modified by changing the parameters in the following line (found near the end of the code):
```bash
bact = Bacteria(2, 0.2, 'x14large', 100, 100) #parameters to change (seed, concentration, domain_size, size_x, size_y)
```

<br />

Visualizing the **surface** is done by adding the following snippet of code to any of the surface files (at the end).

```bash
main_surface = Net_Zero_Surface("medium", 1) #surface is modified here (domain_size, seed)

#used to plot main_surface
# %matplotlib qt (used to make interactive jupyter-only matplotlib BUT not usable with millions of pts)
import matplotlib.pyplot as plt

#setup figure
fig = plt.figure(figsize = (300,300))
ax = fig.add_subplot(111)

#plot surface
print(main_surface.surf_2d)
xyz = main_surface.surf_2d
pos = np.where(xyz == 1)
neg = np.where(xyz == -1)
pos_x = pos[0]
pos_y = pos[1]
neg_x = neg[0]
neg_y = neg[1]

ax.scatter(pos_x, pos_y,  c = 'blue', label = 'pos')
ax.scatter(neg_x, neg_y,  c = 'red', label = 'neg', s = 1)

ax.legend(loc = "upper right")
ax.set_xlabel("X")
ax.set_ylabel("Y")

plt.show()
```

### Steps:
* Open the main_surfacev4_XXXXXXXXX.ipynb file that corresponds to the desired domain size. (The domain size will be found in the title of the file, and refer to the table above)
* Insert the snippet of code (found above) to the end of the opened file.
* Run all of the code to produce the surface visualization

### Extra Info:
* There are modifications that are not included in this repository:
     * Gradient matrix surface
     * Export pictures
     * Capture surface underneath bacteria
* For more info, don't hesitate to contact me at **stanley.lo@mail.utoronto.ca** with email title "Surface Simulation Troubleshooting"
