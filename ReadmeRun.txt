STEADY	 
 - Case folder structure to run MRF steady state solution for P4119 simulation. No turbulence model used - i.e. laminar turbulence model is used

	0
	    - Contains U and p files with all all patches assigned their respective boundary conditions

	constant
	    - Contains MRFProperties requred for MRF application to rotor domain.
	    - Contains transportProperties for Reynolds number matching to experiemtal setup
	    - Contains turbulenceProperties for laminar solution

	system
	    - Contains bounded fvSchemes and steady state fvSolution files
	    - Contains decomposeParDict for parallel simulation
	    - Contains force function object for parameter inspection
	    - Contains controlDict for steady state solution

---------------------------------------------------------------------------------------------------------------------------------------------------

LES
 - Case folder structure to run sliding mesh transient state solution for P4119 simulation.

	
	SGS Additions
	    - Contains k and nut fields to add to created 0 folder for ELES simluation

	constant
	    - Contains dynamicMeshDict requred for sliding mesh application to rotor domain.
	    - Contains transportProperties for Reynolds number matching to experiemtal setup
	    - Contains turbulenceProperties for ILES and ELES solution.

	system
	    - Contains 2nd Order fvSchemes and transient fvSolution files
	    - Contains decomposeParDict for parallel simulation
	    - Contains FO (Function Objects) directory for post processing
	    - Contains controlDict for transient solution solution

-------------------------------------------------------------------------------------------------------------------------------------------------
To Use:
- Move polyMesh from the constant folder of the desired mesh into the constant folder within both STEADY and LES

- Run steady state solution either in 1) serial or 2) parallel:
	1) simpleFoam
	2) decomposePar -> mpirun -np # simpleFoam -parallel -> reconstructPar

- Copy the latest steady state solution time (likely 500 if no changes have been made) into LES, and rename it as 0. Additionally, delete the uniform directory located within

- Rename turbulenceProperties_ILES or turbulenceProperties_ELES. For ELES, rename turbulenceProperties_ELES to turbulenceProperties. For ILES, rename turbulenceProperties_ILES to turbulenceProperties_ILES

- If running ELES, add SGS additions to renamed 0 folder

- Run transient solution either in 1) serial or 2) parallel:
	1) pimpleFoam
	2) decomposePar -> mpirun -np # pimpleFoam -parallel -> reconstructPar
