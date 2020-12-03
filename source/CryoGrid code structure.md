# CryoGrid code structure

## Folder structure
The CryoGrid model code is contained in the folder “modules”, in which it is organized as follows:
- `modules/TIER0`: base level: contains the basic class definitions for CryoGrid SUBSURFACE and INTERACTION classes. TIER0 does not contain functional CryoGrid classes.
- `modules/TIER1`: library level: inherits from TIER0 base classes, contains classes comprising all functions related to a certain physical process. TIER1 does not contain functional CryoGrid classes.
- `modules/TIER2`: first SUBSURFACE class level: inherits from TIER1 library classes, contains fully functional CryoGrid classes
- `modules/TIER3`: second SUBSURFACE class level: inherits from TIER2 SUBSURFACE classes, contains fully functional CryoGrid classes. TIER3 in particular contains the SUBSURFACE classes (GROUND, LAKE, etc. classes) that can interact with a (SUBSURFACE) SNOW class.
- `modulesTIERXX/INTERACTION`: INTERACTION (IA) classes defining interactions and fluxes between pairs of SUBSURFACE classes, same TIER structure as for SUBSURFACE classes

and so on ....