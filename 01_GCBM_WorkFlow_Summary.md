# 1 - GCBM Workflow Summary

**Topics covered:**
- Introduction to GCBM
- Overall GCBM Project Structure
- Standalone GCBM project

## Introduction to the GCBM
The Generic carbon budget model (GCBM) has been developed by the natural resources canada, canadian forest services in collaboration with moja global.

The FLINT, moja global’s flagship software, is an open sourced tool developed by the moja global that for the first time gives countries the ability to build and run advanced MRV system quickly and efficiently. The FLINT provides infrastructure to read spatial and aspatial data, call event-driven modules, generate spatial and tabular output, and log errors and other information. The structure also allows the users of FLINT to focus development on FLINT modules of their specific scientific interest.  The GCBM is an open sourced model that operates on the FLINT platform and is a  spatially-explicit modelling framework
 
**Some of the key advantages of the GCBM** are the level of detail specified in the scientific model and ability to run these over large spatial scales to generate detailed maps of carbon changes and emission associated with land use change.
 
Here we detail the general workflow to conduct a GCBM analysis. The purpose of this document is to inform the design of the FLINT UI - by describing the workflow, we can develop an interface that facilitates this workflow and create a simple user journey that makes a complex model more accessible.
 
We are using the GCBM Demo Run as a template, representing a very simple example of a GCBM model. See the data section below for a complete description of all inputs, but note that we will initially focus on a reduced subset of the available options, then build more complexity over time.
 
## The overall GCBM project structure
 
 
The user starts off with the spatial data including the forest inventory and disturbances. This is combined with non spatial modelling parameters including their growth curves and disturbance matrices. Spatial Data and Modelling parameters are combined and  pre processed with the help of a few python pre processing tools to put everything into a GCBM readable format. The GCBM configuration files, in json format go through a GCBM simulation. The simulation produces a set of spatial outputs and SQL database output.

![GCBM Project stucture](\Images\GCBM_project_structure.png "GCBM Projecr structure")

**Data required to run thee GCBM:**
 
Spatial data - The user would require atleast a map of the initial forest age or the time since last disturbance and their classifiers. It is recommended to have a Mean annual temperature layer, and a layer identifying administrative  and ecological boundaries.
 
Tabular data - The GCBM requires a CBM-CFS3 ArchiveIndex Database (AIDB)-- which contains the non spatial ecological modelling parameters and a CBMCFS3 yield table is csv or xl format. For those applying the model outside Canada using a customised ArchiveIndex Database with the ecological parameters is recommended to improve the carbon results.
 
Disturbance Events formats -  The model can use any spatial layer format and a Raster file can be used for per year. The pixel value is equal to the disturbance type. The GCBM can also use any number of vector files containing polygon of completely disturbed areas. The vector files also require the disturbance year and disturbance type.
 
 
## Standalone GCBM project
 
The process of a standalone project is best suited for projects that have data available in correct format and comes with some extra tools for pre- and post-processing. The standalone project is the recommended way to get new projects up and running.
 
The Standalone Project is a compiled version of FLINT with the GCBM Science modules with some additional tools. You can see a copy of it at https://github.com/moja-global/GCBM.Belize/tree/master/Standalone_GCBM/tools
 

 
The Standalone GCBM is provided in an executable format for Windows by NRCan, which is one of the easiest ways to get started on this project. You need a login for NFIS but it is freely available. Alternatively, you can build the GCBM from scratch using the source code at https://github.com/moja-global/moja.canada/ or using our pre-built docker containers at https://github.com/moja-global/flint.cloud

![CFSCAT](\Images\CFSCAT.png")
 
Available at: https://carbon.nfis.org/cbm/index_e.jsp

 
**Workflow of a Standalone project workflow.**
 
The process starts off with a tiler that converts the data into a GCBM readable format. After this the Recliner2GCBM creates the SQL input database. Once the input database is in place the GCBM simulation can be run. Once the simulation is successfully runs the process moves into two post processing tools. First is the CompileGCBMSpatialOutput. It puts all the outputs together into GeoTiff files. Secondly is the CompileGCBMResults tool which generates ecosystem indicators in the form of a SQLite database.
 
![GCBM Standalone project workflow](\Images\GCBM_standalone_project.png "GCBM standalone project")

**The Output of the Standalone project.**
 
The tabular data can be in a SQLite format by default or postgres database. The raw spatial output is the ENVI or GeoTiff file. After processing it’s converted to the standard GeoTiff file. Additional output includes Flattened reporting tables that are more user friendly. They can either be SQLite or Postgres.