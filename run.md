# Running the simulation

## Creating the output folders and compiling

Once we nesessary setups are spcified in the configuration file(`COSEnu/lig/config.yaml`) and specifying the initialization of the grid points, we need to compile the code and make the necessary foldes to store the output files. In order to reduce the effort to manually doing all these, `COSEnu` provide python script(`COSEnu/manage.py`), which read the configuration file, make the folders according to the specifucations on the configuration file and compile the code according to the options provided. Python 3.0+ is preffered to run the script (anaconda version can be installed from the terminal using `wget url`). The general form of using the script is as follows. 

`$python manage.py [mode] [scheme]`

Available options for `[mode]` are:

-`--score` : Compile the code for single core usage.

-`--mcore` : Compile the code for multi-core usage.

-`--acc` : Compile the code with `pgc++` to be used on GPU.

Avilable options for `[scheme]` are: 

`--fd` : Compile the code with finite differencing scheme(3rd order Kreiss-Oliger dissipation with $$\var\epsilon_\mathrm{KO}=0.1$$ is supported by default).

`--fv` : Compiles the code with finite volume scheme(flux reconstruction is done using 7th order WENO by default).

Finally the sript eill automatically copy the executable (named `main` by default) and configuration file(named `job.config` by default) to the folders corresponding folders(created automatically according to the specifications given in the `config.yaml` file). 
[<previous](usage.md)  &#124;  [home](index.md)  &#124;  [next>]() 