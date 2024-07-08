# Running a simulation

## Creating the output folders and compiling

Once the nesessary setups and the initialization of the grid points are spcified in the configuration file (`COSEnu/lig/config.yaml`), we need to compile the code and make the necessary foldes to store the output files. In order to reduce the effort to manually doing all these, `COSEnu` provides a python script(`COSEnu/manage.py`), which reads the configuration file, makes the folders according to the specifucations on the configuration file and compiles the code according to the options provided. Python 3.0+ is preffered to run the script (anaconda version can be installed from the terminal using `wget <url>`). The general form of using the script is as follows. 

`$python manage.py [opt] [scheme]`

Available options for `[opt]` are:

`--score` : Compiles the code for single core usage.

`--mcore` : Compiles the code for multi-core usage.

`--acc` : Compiles the code with `pgc++` to be used on GPU.


Avilable options for `[scheme]` are: 

`fd` : Compiles the code with finite difference scheme(3rd order Kreiss-Oliger dissipation with $$\epsilon_{\rm{KO}}=0.1$$ is supported by default).

`fv` : Compiles the code with finite volume scheme.

The sript will automatically copy the executable (named `main` by default) and configuration file(named `job.config` by default) to the corresponding folders (created automatically according to the specifications given in the `config.yaml` file). 

Finally run the command

`./main [mode] --config job.config`

Available options for `mode` are:

`--ff` : Initialize the density matix componets and the angular distributions from previously stored binary file. This option is necessary only if we need to resume the simulation which has been stopped before finishing. Otherwise this option can be left unspecified.

from the job folder to run the executable. 

To allow `manage.py` submit automatically, run

`$python manage.py [opt] [scheme] --s [submit_opt]`

Run `$python manage.py --help` to get detailed instruction about executing the simulation. Make sure that required compilers are installed on the system. 

[<previous](usage.md)  &#124;  [home](index.md)  &#124;  [next>](example.md) 
