# Virtual FTMW Spectrometer

## Table of Contents
* [Overview](#overview)
* [Front End](#front-end)
* [Back End](#back-end)
* [Project Updates](#project-management)
* [Student Team](#student-team)

## Overview
In response to the challenges faced in conducting experiments during the COVID-19 pandemic, the Raston Lab initiated the development of virtual scientific instruments to ensure accessible education for students worldwide. Building on the success of their Fourier Transform InfraRed (FTIR) Spectroscopy Simulator, their new project for Spring 2025 focuses on creating a Fourier Transform Microwave (FTMW) Spectroscopy Simulator. Unlike infraRed spectroscopy, FTMW operates in the microwave range and involves entirely different electronic components and principles. This simulator will provide students with a user-friendly platform to perform microwave spectroscopy experiments on any device. This eliminates the need for costly physical instruments and ensures that education can continue even in the event of another pandemic.

## Front-End:

### Setup Window
![setup-window.png](img%2Fsetup-window.png)

The purpose of this window is to configure the spectrometer parameters. It uses the [Redux Toolkit](https://github.com/reduxjs/redux-toolkit) to store the applied values into the redux store.

### Menu Items
![menu-items-1.png](img%2Fmenu-items-1.png)
![menu-items-2.png](img%2Fmenu-items-2.png)

Changed the Menu Items to be more accurate with the FTMW spectrometer.

### Instrument Window SVG Components
All of these components were made using [Inkscape](https://inkscape.org/release/inkscape-1.4/). The instrument window can be found [here](https://github.com/FTMW-Scientific-Simulator/Virtual-FTMW-Spectrometer/tree/main/src/assets/svg/components/instruments).

### Instrument Window
This window shows the components of the FTMW Spectrometer. The components are gets animated when the user clicked "Acquire Spectrum" button on the menu bar. It shows how the FTMW Spectrometer works.

![instrument-window.png](img%2Finstrument-window.png)

### Schedule Window SVG Components
All of these components were made using [Inkscape](https://inkscape.org/release/inkscape-1.4/). The schedule window can be found [here](https://github.com/FTMW-Scientific-Simulator/Virtual-FTMW-Spectrometer/tree/issue-15/src/assets/svg/components/schematic).

### Schedule Window
This window shows the schematic of the FTMW Spectrometer. The schematic is a diagram that shows the electrical components of the FTMW Spectrometer.

![schematic-window.png](img%2Fschematic-window.png)

## Back-End:

`acquire_spectra.py`

- **`acquire_spectra(params: dict, window=30, resolution=0.001, hwhm=0.007)`**  
  This function creates a simulated spectrum for a given molecule based on input parameters
  - **Parameter Extraction & File Retrieval:** It reads the molecule name and optional cropping limits from the `params` dictionary. Then it calls `get_datafile` (from the utils module) to obtain the correct data file.
  - **Data Loading:** The function reads spectral data from the file into a pandas DataFrame, converting the frequency and intensity columns into numeric types.
  - **Frequency Doubling & Cropping:** It doubles the frequency values and, if crop limits are provided, filters the data so only relevant spectral lines are kept.
  - **Local Spectrum:** For each spectral line, a local frequency grid is generated around the line (with a span defined by `window` and step size by `resolution`). Two Lorentzian profiles are computed (one at the line frequency and one at a slightly shifted “split” frequency) using the provided `lorentzian_profile` function. These two profiles are then summed to form the local spectrum.
  - **Global Spectrum:** It defines a common overall frequency grid (either based on the crop limits or the combined range of all local grids) and interpolates each local spectrum onto this grid. The contributions are then summed together.
  - **Noise Addition & Plotting:** If a noise level is specified in `params`, white noise is added to the final spectrum using `add_white_noise`. Finally, the spectrum is plotted
  - **Return:** A dictionary indicating success and containing the final frequency grid and spectrum data as lists.

`acquire_spectra_utils.py`

- **`get_datafile(molecule: str, directory: str = "linelists") -> str`**  
  This helper function maps a given molecule (using its chemical formula) to the corresponding data file name. 
  - It uses a predefined dictionary that links molecule formulas to specific file names.
  - If the molecule is found, it returns the full file path by combining the directory and file name.
  - If the molecule isn’t mapped, it raises a `ValueError`.

- **`lorentzian_profile(grid, center, hwhm)`**  
  This function calculates a Lorentzian profile over a given frequency grid.

- **`add_white_noise(spectrum: np.ndarray, noise_level: float) -> np.ndarray`**  
  This function adds random white noise to a given spectrum.

### Current Back-End Spectra
- This is the current spectra that the back-end generates for C7H5N with a crop of
16,412 MHz to 16,416 MHz and a noise level of 0.02.
- **Note**: The backend will not return a plotted graph, but it will return the frequency grid and the spectrum data as lists. These lists will be used to plot the graph in the front-end.
  ![C7H5N-spectra.png](img%2FC7H5N-spectra.png)

## Project Updates

### Week 1
- **Front-End**:
  - Created the setup window to configure the spectrometer parameters.
    - Used the Redux Toolkit to store the applied values into the redux store.
  - Changed the menu items to be more accurate with the FTMW spectrometer.

### Week 2
- **Front-End**:
  - Created the instrument window components using Inkscape.

### Week 3
- **Front-End**:
  - Created the instrument window to show the components of the FTMW Spectrometer.
  - Creaded the schematic components using Inkscape.

### Week 4
- **Front-End**:
  - Polished schematic window componented.
  - Started animation on the instrument window components.
    - Moving the movable mirror and necessary components on the x-axis.
    - Acquire Spectrum menu item starts the animation.
    - Stop Acquisition menu item stops the animation.
    - Cancel Acquisition menu item resets the animation.
- **Back-End**:
  - Slowly worked with sponsor to understand the backend code.
  - Started off creating a Gaussian spectrum for a given line list file

### Week 5
- **Front-End**:
  - Continued polishing the schematic window components based on sponsor feedback.
  - Worked on the schematic window component.
    - Created the schematic diagram shown on the window using inkscape.
- **Back-End**:
  - Created a function to broaden the spectral lines using Lorentzian profiles and splitting the spectral lines.
    - The output of this function is what the final spectrum will look like.

### Week 6
- **Front-End**:
    - Continued polishing the schematic window components based on sponsor feedback.
- **Back-End**:
  - Made the splititng and broadening of the spectral lines dynamic to use user params from the front end and returns x and y values for the spectrum.
    - The output of this function is what the final spectrum will look like.
    - Final spectrum will be plotted in the front end.
  - Created a function to add white noise to the spectrum.

## Student Team

Shedrick Ulibas and Weibin Wu
