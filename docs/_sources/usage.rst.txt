Getting Started
===============

.. _installation:

Installation
------------

To use pyemaps, first install it using pip:

.. code-block:: console

   (.venv) $ pip install pyemaps

where .venv is the python virtual environment.

.. _Environment Variables:

Enviroment Variables
--------------------

*PYEMAPS_DATA* environment variable is set to represent pyemaps' data home directory.
It not only provides central location for organizing your own crystal data, but also 
for storing your simulation and calculation results.   

**pyemaps** searches order for crystal files when only file name is provided:

1. Current working directory. Or
2. Data home directory pointed by *PYEMAPS_DATA*.

For output from **pyemaps** simulations and calculations, the placement order is reversed:

1. Data home directory pointed by *PYEMAPS_DATA* if it is set. Or
2. Current working directory

The layout of the pyemaps data home directory is as follows:

.. code-block:: console

    $(PYEMAPS_DATA)=<local directory>  # pyemaps data home, must have read and write permisions
    $(PYEMAPS_DATA)/crystals           # hosts all custom crystal data files (.xtl, .cif)
    $(PYEMAPS_DATA)/bloch              # location for all bloch images output files
    $(PYEMAPS_DATA)/mxtal              # place for all crystal constructor output files such as *.xyz

.. note::
   
   The legacy environment variable $(PYEMAPS_CRYSTALS) is still supported if it is set.

Kick-start pyemaps
------------------

After *pyemaps* installation, run the following to check if the package is installed
and setup correctly on your system by verifying the version and other information about
the package: 

.. code-block:: console

   python -m pyemaps --version (-v)
   python -m pyemaps --copyright (-c)

A liveness test for *pyemaps* basic kinematic function is also available:  

.. code-block:: console

   python -m pyemaps --sample (-s)

All pyemaps simulations and calculations start from its 
`Crystal class <pyemaps.crystals.html#pyemaps.crystals.Crystal>`_. 
To import the class, run the follow:

.. code-block:: python

   from pyemaps import Crystal


Before starting pyemaps diffraction simulation, the crystal
object must be created and its data loaded. The following example
creates a *si* crystal object by loading its database from pyemaps 
`from_builtin <pyemaps.crystals.html#pyemaps.crystals.Crystal.from_builtin>`_ 
built-in database for Silicon crystal. 

.. code-block:: python
 
    from pyemaps import Crystal as cr  

    si = cr.from_builtin('Silicon')

.. note::
   
   Pyemaps provides methods for creating crystal objects from other 
   data sources including those from CIF, JSON formatted files. Go to 
   `Crystal class <pyemaps.crystals.html#pyemaps.crystals.Crystal>`_
   for more details.


Once the crystal object is created, it is ready for running simulations
and calculations.   

Kinematic Diffraction Simulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    from pyemaps import Crystal as cr        # Crystal class
    from pyemaps import DPList               # Kinematic diffraction patterns class
    from pyemaps import showDif              # Builtin visualization helper class
    
    # create a crystal class object and load it with builtin silicon data
    c_name = 'Silicon'
    si = cr.from_builtin(c_name)

    # Simulate kinematic diffraction with the crystal instance 
    # All controls input are default values
    
    dpl = DPList(c_name)

    emc, si_dp = si.generateDP()
    dpl.add(emc, si_dp)    

    # Plot and show the diffraction pattern using 
    # pyemaps built-in plot function
    showDif(dpl)

    # Show Diffraction patterns by hiding Kikuchi lines
    showDif(dpl, kshow=False) 

    # Show Diffraction patterns by hiding both Kukuchi line and Miller Indexes
    showDif(dpl, kshow=False, ishow=False) 

    # Show Diffraction patterns by hiding Miller Indices
    showDif(dpl, ishow=False)

Here crystal class method *generateDP* call with all default control parameters results in 
`kinmatic diffraction pattern <pyemaps.kdiffs.html#pyemaps.kdiffs.diffPattern>`_ (si_dp) . 

Go to `generateDP <pyemaps.crystals.html#pyemaps.crystals.Crystal.generateDP>`_ for a complete
and detailed set of control parameters for the simulation method. 

Finally, *showDif* method in pyemaps `display module <pyemaps.display.html#module-pyemaps.display>`_  
to visualize the Kikuchi lines, diffracted beanms in the *si_dp* diffraction pattern object. 

Notice that *showDif* provides options for users to control whether to show Kikuchi lines or
Miller Indexes.

Dynamic Diffraction Simulation - Bloch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    from pyemaps import Crystal as cr        # Crystal class
    from pyemaps import BImgList             # Dynamic diffraction image list class
    from pyemaps import showBloch            # Builtin visualization helper function

    # create a crystal class object and load it with builtin silicon data
    c_name = 'Silicon'
    si = cr.from_builtin(c_name)

    # Generate dynamic diffraction patterns using pyemaps' bloch module
    # with all other default parameters except sampling

    bloch_imgs_list = BImgList(c_name)
    emc, img = si.generateBloch(sampling = 20) 
    
    # Create a dynamic diffraction image list

    bloch_imgs_list.add(emc, img) 
    
    # Display the image with grey scale color map and #with predefined color map
    showBloch(bloch_imgs_list) 
    showBloch(bloch_imgs_list, bColor=True) 

The crystal method *generateBloch* starts the dynamic diffraction simulation with 
sampling resolution of 20. For a complete set of controls and input parameters for the simulation, 
go to `generateBloch <pyemaps.crystals.html#pyemaps.crystals.Crystal.generateBloch>`_  

Pyemaps also provides a helps class `BImgList <pyemaps.ddiffs.html#pyemaps.ddiffs.BlochImgs>`_
and a images rendering method *showBloch* in `display module <pyemaps.display.html#module-pyemaps.display>`_ 
to visualize the simulation resutls.

.. note::

   To generate multiple images with specified range of sample thickness and save 
   them in raw image data file that can be imported into other visulization 
   tools, see complete description of `generateBloch method 
   <pyemaps.crystals.html#pyemaps.crystals.Crystal.generateBloch>`_.

.. note::

   To start dynamic simultion session and retrieve scattering matrix and other
   dynamic diffraction session data, see `getSCMatrix method 
   <pyemaps.crystals.html#pyemaps.crystals.Crystal.getSCMatrix>`_  
   between `beginBloch
   <pyemaps.crystals.html#pyemaps.crystals.Crystal.beginBloch>`_ and 
   `endBloch calls
   <pyemaps.crystals.html#pyemaps.crystals.Crystal.endBloch>`_.

To see all crystal names in *pyemaps* built-in database, call 
`list_all_builtin_crystals: <pyemaps.crystals.html#pyemaps.crystals.Crystal.list_all_builtin_crystals>`_:

.. code-block:: python

   from pyemaps import Crystal as cr
   cr.list_all_builtin_crystals()



Samples code
------------

Sample scripts designed for you to explore pyemaps features are available 
in pyemaps' *samples* directory.

To copy all of the samples from **pyemaps** package installation directory
to the current working directory, run:

.. code-block:: console

   python -m pyemaps -cp

all of the samples will be copied from **pyemaps** install directory to 
a folder named *pyemaps_samples* in your current working directory:

* *si_dif.py*: 
   shows how kinematic diffraction patterns is generated and rendered with 
   *matplotlib pyplot* module. The code also shows how a list of diffraction 
   patterns are generated and changed with one of electron microscope and 
   sample controls - tilt in x direction.

* *si_bloch.py*: 
   demonstrates dynamic diffraction simulations by *bloch* pyemaps module

* *si_csf.py*: 
   generates and outputs structure factors by *CSF* pyemaps module. 

* *powder.py*: 
   calculates and displays electron powder diffraction with intensity plot by 
   *Powder* pyemaps module. 

* *si_stereo.py*: 
   prodcues and shows stereodiagram by **pyemaps** *Stereo* module. 

More samples code will be added as more features and releases are available. 
