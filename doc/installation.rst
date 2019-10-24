.. _installation:

##########################################
Installation
##########################################

The subsequently described installation steps are demonstrated as shell commands, where the path before the ``%`` sign denotes the
directory in which the commands following the ``%`` should be entered.

Clone the Repository
====================

First of all, clone the `PyPSA-Eur repository <https://github.com/PyPSA/pypsa-eur>`_ using the version control system ``git``.
The path to the directory into which the ``git repository`` is cloned, must **not** have any spaces!

.. code:: bash

    /some/other/path % cd /some/path/without/spaces

    /some/path/without/spaces % git clone https://github.com/PyPSA/pypsa-eur.git

.. note::
    If you do not have ``git`` installed, follow installation instructions `here <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git>`_.

.. _deps:

Install Python Dependencies
===============================

PyPSA-Eur relies on a set of other Python packages to function.
We recommend using the package manager and environment management system ``conda`` to install them.
Install `miniconda <https://docs.conda.io/en/latest/miniconda.html>`_, which is a mini version of `Anaconda <https://www.anaconda.com/>`_ that includes only ``conda`` and its dependencies or make sure ``conda`` is already installed on your system.
For instructions for your operating system follow the ``conda`` `installation guide <https://docs.conda.io/projects/conda/en/latest/user-guide/install/>`_.

The python package requirements are curated in the `environment.yaml <https://github.com/PyPSA/pypsa-eur/blob/master/environment.yaml>`_ file.
The environment can be installed and activated using

.. code:: bash

    .../pypsa-eur % conda env create -f environment.yaml

    .../pypsa-eur % conda activate pypsa-eur

.. note::
    Note that activation is local to the currently open shell!
    After opening a new terminal window, one needs to reissue the second command! 

.. _data:

Download Data Dependencies
==============================

Not all data dependencies are shipped with the git repository,
since git is not suited for handling large changing files.
Instead we provide separate data bundles which can be obtained
using the described shell commands or by downloading and
extracting them manually in the locations outlined below.

.. note::

    The :ref:`tutorial` uses smaller data bundles than required for the full model.
    To start with the tutorial, substitute with the links below using the following alternatives:

    .. image:: https://zenodo.org/badge/DOI/10.5281/zenodo.3517921.svg
        :target: https://doi.org/10.5281/zenodo.3517921
    
    .. image:: https://zenodo.org/badge/DOI/10.5281/zenodo.3518020.svg
        :target: https://doi.org/10.5281/zenodo.3518020
    
    - **Data Bundle:** ``https://zenodo.org/record/3517921/files/pypsa-eur-tutorial-data-bundle.tar.xz`` (197 MB)
    - **Cutouts:** ``https://zenodo.org/record/3518020/files/pypsa-eur-tutorial-cutouts.tar.xz`` (19 MB)

.. image:: https://zenodo.org/badge/DOI/10.5281/zenodo.3517935.svg
   :target: https://doi.org/10.5281/zenodo.3517935

1. **Data Bundle:** `pypsa-eur-data-bundle.tar.xz <https://zenodo.org/record/3517935/files/pypsa-eur-data-bundle.tar.xz>`_ (1.4 GB) contains common GIS datasets like NUTS3 shapes, EEZ shapes, CORINE Landcover, Natura 2000 and also electricity specific summary statistics like historic per country yearly totals of hydro generation, GDP and POP on NUTS3 levels and per-country load time-series. It should be extracted in the ``data`` sub-directory, such that all files of the bundle are stored in the ``data/bundle`` subdirectory)

.. code:: bash

    .../pypsa-eur/data % curl -OL "https://zenodo.org/record/3517935/files/pypsa-eur-data-bundle.tar.xz"

    .../pypsa-eur/data % tar xJf pypsa-eur-data-bundle.tar.xz

.. image:: https://zenodo.org/badge/DOI/10.5281/zenodo.3517949.svg
   :target: https://doi.org/10.5281/zenodo.3517949

2. **Cutouts:** `pypsa-eur-cutouts.tar.xz <https://zenodo.org/record/3517949/files/pypsa-eur-cutouts.tar.xz>`_ (3.9 GB) are spatiotemporal subsets of the European weather data from the `ECMWF ERA5 <https://software.ecmwf.int/wiki/display/CKB/ERA5+data+documentation>`_ reanalysis dataset and the `CMSAF SARAH-2 <https://wui.cmsaf.eu/safira/action/viewDoiDetails?acronym=SARAH_V002>`_ solar surface radiation dataset for the year 2013. They have been prepared by and are for use with the `atlite <https://github.com/PyPSA/atlite>`_ tool. You can either generate them yourself using the ``build_cutouts`` rule or extract them directly into the ``pypsa-eur`` directory. To download cutouts yourself you need to `set up the CDS API <https://cds.climate.copernicus.eu/api-how-to>`_. For more details read the `atlite documentation <https://atlite.readthedocs.io>`_. For beginners, extracting the bundle is recommended:

.. code:: bash

    .../pypsa-eur % curl -OL "https://zenodo.org/record/3517949/files/pypsa-eur-cutouts.tar.xz"

    .../pypsa-eur % tar xJf pypsa-eur-cutouts.tar.xz

.. image:: https://zenodo.org/badge/DOI/10.5281/zenodo.3518215.svg
   :target: https://doi.org/10.5281/zenodo.3518215

3. **Natura:** Optionally, you can download a rasterized version of the NATURA dataset `natura.tiff <https://zenodo.org/record/3518215/files/natura.tiff>`_ and put it into the ``resources`` sub-directory. If you don't, it will be generated automatically, which is a time-consuming process.

.. code:: bash

    .../pypsa-eur % curl -L "https://zenodo.org/record/3518215/files/natura.tiff" -o "resources/natura.tiff"


4. **Remove Archives:** Optionally, if you want to save disk space, you can delete ``data/pypsa-eur-data-bundle.tar.xz`` and ``pypsa-eur-cutouts.tar.xz`` once extracting the bundles is complete. E.g.

.. code:: bash

    .../pypsa-eur % rm -rf data/pypsa-eur-data-bundle.tar.xz pypsa-eur-cutouts.tar.xz

Install a Solver
================

PyPSA passes the PyPSA-Eur network model to an external solver for performing a total annual system cost minimization with optimal power flow.
PyPSA is known to work with the free software

- `Ipopt <https://coin-or.github.io/Ipopt/INSTALL.html>`_
- `Cbc <https://projects.coin-or.org/Cbc#DownloadandInstall>`_
- `GLPK <https://www.gnu.org/software/glpk/>`_ (`WinGLKP <http://winglpk.sourceforge.net/>`_)

and the non-free, commercial software (for which free academic licenses are available)

- `Gurobi <https://www.gurobi.com/documentation/8.1/remoteservices/installation.html>`_
- `CPLEX <https://www.ibm.com/products/ilog-cplex-optimization-studio>`_

and any other solver that works with the underlying modelling framework `Pyomo <http://www.pyomo.org/>`_. For installation instructions of these solvers for your operating system, follow the links above.

.. seealso::
    `Getting a solver in the PyPSA documentation <https://pypsa.readthedocs.io/en/latest/installation.html#getting-a-solver-for-linear-optimisation>`_

.. note::
    Commercial solvers such as Gurobi and CPLEX currently significantly outperform open-source solvers for large-scale problems.
    It might be the case that you can only retrieve solutions by using a commercial solver.

.. _defaultconfig:

Set Up the Default Configuration
================================

PyPSA-Eur has several configuration options that must be specified in a ``config.yaml`` file located in the root directory.
An example configuration ``config.default.yaml`` is maintained in the repository. 
More details on the configuration options are in :ref:`config`.

Before first use, create a ``config.yaml`` by copying the example.

.. code:: bash

    .../pypsa-eur % cp config.default.yaml config.yaml

Users are advised to regularly check their own ``config.yaml`` against changes in the ``config.default.yaml``
when pulling a new version from the remote repository.

.. Using PyPSA-Eur with Docker Images
.. ==================================

.. If docker. Optional.
.. To run on cloud computing.
.. Gurobi license - floating token server - license must not be tied to a particular machine
.. Provide ``Dockerfile``.