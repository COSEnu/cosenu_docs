<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Usage

Along with the main simulation engine, $$\text{COSE}\nu$$ we provide simple interface written in
`python` to reduce the difficulties of setting up and running the simulation. The enitre
process of the simulation can be thought of as consisting of following steps.

- Setting up the spatial grid and phase-space bins.
- Initializing the grid points.
- Running the simulation.
- Analysing the results.

In the following we will explain the details of carrying out each of these steps.

## Setting up the spatial grid

Open `COSEnu/lib/configs.yaml` file. The domain spacific part typically
look like shown below.

```yml
# Spatial domain specifics.
zrange: [-600, 600]
nzs: [1000]
CFLS: [0.4]
```

`zrange`: Start and end values for the domain required for the simulation. Both satrt and end values are necessary.

`nzs` : Values of resolutions for which we want to run the simulations. In principle we can put any number of value
inside the square bracket separated by comma. However the bracket should contain atleast one value.

`CFLS` : Values of the `CFL` parameter for which we want to run the simulation. Any nmber of values with
in the square brackets are allowed. The Bracket should not be empty. For numerical stability each of these values
should be chosen such that $$ 0< \text{CFL} \leq 1$$.

## Setting up the velocity bins

In the current implementation we only consder the z-componant of velocity $$\text{cos}(\theta)$$
which can take values from -1 to 1. To evolve each velocity modes numerically, we treat the beam of
neutrinos whose velocity lies between $$\mathrm{v}$$ and $$\mathrm{v} + d\mathrm{v}$$ as asingle beam
with velocity $$\mathrm{v} + d\mathrm{v}/2$$. The value of $$d\mathrm{v}$$can be estimated from the number of
velocity bins $$\text{N}_{\mathrm{v}_z}$$. The starting and ending values of $$\mathrm{v}$$ and, the
resolution of the phsase-space can be set as shown below.

```yml
# Phase-space specifics.
v0: -1.0
v1: 1.0
nvzs: [50]
```

`v0` : Starting value of the velocity range.

`v1` : Ending value of the velocity range.

`nvzs` : Number of angular bins we require for the simulation.

## Specifying the output folders

The name of the folders to store the outputs from the simulation can be specified as follows.

```yml
# Folder names
folder_fv: "outputs-from-fv"
folder_fd: "outputs-from-fd"
```

`folder_fv` : Create folder with specifid name to store the results from simulation with finite
            volume (FV) scheme.
              
`folder_fd` : Create folder with specifid name to store the results from simulation with finite
              difference (FD) scheme.

Inside each of these folders, sub-folders will be created to store the output files from each configuration. Each of these sub-folders will be named with their `ID`. By default, the `ID` takes the form `Nz_Nvz_CFL`.

## Number of iterations

The number of iterations for which the simulation should be carried out can be specified as follows.
```yml
# Number of iterations = int(end_time/dt)
# dt = CFL*dz
end_time: 100
```

`end_time` : End time for the simulation in physical units. Given the physical end time the number of iterations 
are estimated using $$\text{N_ITER} = $$ end_time$$/dt$$, where $$ dt = \text{CFL}\times dz$$.

## Switching the advection

One can also switch on and off the advection throught the configuration file.


```yml
# Switch on/off advection.
advection_off: False # Options -> True, False
```

`advection_off`: Accepts Boolean values. Setting `advection_off` to `False` ensure the advection is switched on while setting it to 
`True` will solve for the ordinary differential equation (ODE) in time (Eq. (1) without advection).

## Specifying the oscillation parameters

To set up goth vacuum and collective neutino oscillation related parameters, we can use the configuration file as follows.

```yml
# Switch on/off vacuum oscillation
vac_osc_on: False # options -> True, False
pmo: 1.0 # 1.0 -> normal mass ordering, -1.0 -> inverted mass ordering, 0.0 -> no vacuum term.
omega: 1.0e-4 # \Dedlta m^2/2E.
theta: 37 # Vacuum mixing angle in degrees.

# Switch on/off collective oscillations
collective_osc_on: True # Options -> True, False
mu: 1.0 # \sqrt{2} G_F n_{\nu_e}

```

`vac_osc_on`: Accepts Boolean values. `True` for switching on the vacuum oscillation and `False` for switching off.
By default vacuum oscillation is turned off.

`pmo`: To specify the neutrino mass ordering. Set it to 1 for normal mass ordering and -1 for inverted mass ordering.

`omega` : Vacuum oscillation frequency.

`theta` : Vacuum mixing angle.

`collective_osc_on` : Also accepts Boolean values. `True` for turning neutino collective oscillation on and `False` for turning it off.
By default, collective oscillation is turned on.

`mu` : Indicates the streangth of $$\nu-\nu$$ interaction ($$\mu=\sqrt{2}G_Fn_{\nu_e}$$). By default, $$\mu$$ is set to 1.


## Analysis

Along with the simulation, a few subroutines to check the deviation 
of conserved quantities and take snapshots are included. Following 
are the details default output files and their output layout.

- ID_conserved_quantities.dat
    - Stores the details of the violation of conserved quantities.
    - Output layout
        - Time  $$\delta P_\text{max}$$  $$<\delta P>$$  $$<\delta\bar{P}$$  $$|M_0|$$        

```yml
# ANALYSIS
#----------------------------------------------------------------#

# To analyze the behaviour of the conserved quantities.
n_analyze: 100 # Total number of analysis to be carried out per job.

# To capture the evolution of the field variables for all the velocities over entire domain.
n_fullsnap: 3

# To capture the evolution of the field variables for all th velocity modes at given locations.
n_vsnap: 5 # snapshot of phase-space at vsnap_zlocs
vsnap_z: [-300, 0, 300] # z-locations for phsse-space snapshots.

# To capture the evolution of the field variables for given modes over the entire domain.
n_zsnap:  5 # snapshot of entire domain for the v_modes at zsnap_vmodes
            # between time = 0 and time = end_time
zsnap_v: [-1, -0.5, 0.5, 1] # v-modes for full spatial domain snapshots.

#----------------------------------------------------------------#

#----------------------------------------------------------------#
```

[<previous](comp_setup.md)  &#124;  [home](index.md)  &#124;  [next>]() 
