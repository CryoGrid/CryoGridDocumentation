# Adding new modules and functionality to CryoGrid

## Style guide
- Most important **(THIS IS A MUST!): Use clear and understandable variable names and not abbreviations**
- Physical properties should be named by their SI symbols (if it exists and makes sense).
- In functions, used variables from containers should be saved by their name, i.e. `theta = ground.CONST.theta`
- Variables and functions should be in lower case. For variable names consisting of several words, camel case or underscores can be used.
- If you use equations and constants, cite their source in comments.
- The CryoGrid code contains many cases in which the style guide was not followed, partly due to implementation of legacy code, partly due to negligence. To ensure readability of the code and error messages, rule No.1 is by far the most important and must be followed!!

## Class documentation
Every author should indicate her/his name and the date in the header of each class (same for files that are not a class). If major changes or updates are done, the author(s) of the changes should again include name(s) and date, but not remove theprevious entries in the header. In the code, comments should be inserted to make it understandable. In addition, each CryoGrid class should be described in this manual. Every author is responsible for the documentation of new CryoGrid classes.

## Not so Quick Start 
*How to create a new `SUBSURFACE` class*

There is no definite scheme for compiling new SUBSURFACE classes, so this is only a rough guide. Discuss your plans with an experienced developer prior to setting off!
Most important: Design and create new classes without changing anything (!) in existing functions or classes! Even if only a minor change in e.g. a function in a TIER1 class would be necessary, add a new function to the TIER1 class instead of changing the existing function. This ensures that existing functionality is not affected by the new class. If changes to existing code become necessary, this must be discussed with all developers.

1. Always develop and test a new `SUBSURFACE` class myClasswithout taking the snow cover into account. Normally (there are exceptions), the variable snowfall provided by the FORCING class should not be used at all. This means that development should start at the `TIER2` level.
2. Choose the `TIER2` class *oldClass* which is closest in functionality to the new class and copy + rename this class to newClass.
3. Change the class name in the initialize function. Add additional parameters (`PARA`), state variables (`STATVAR`) and constants (`CONST`) that must be initialized form the parameter and constant files in the appropriate private methods of newClass(i.e. `provide_PARA`, `provide_STATVAR` and `provide_CONST`). The variables in these functions must be declared empty ([]) and will be automatically populated during the initialization process. Dependent variables (i.e. variables computed from the variables populated during initialization) should not be declared here, but the appropriate code computing dependent variables should be added to the finalize_init() function.
4. Add statements to the mandatory functions in the new `SUBSURFACE` class. In general, additional functionality should be coded as functions in `TIER1` classes, and then called as a single line statement in the `TIER2` class. If adequate, make a new `TIER1` class and add it to the list of classes after the class_defstatement.
5. Test the new class without interactions with other classes, i.e. by making a parameter file in which the stratigraphy consists only of newClass (i.e. no other classes).
6. Compatibility with other classes: check the interaction classes used for oldClassand, if changes arenecessary, copy, rename and modify them in the same way as for the `SUBSURFACE` class, paying attention to the `TIER1` and `TIER2` levels. Add *newClass* and potential new `INTERACTION` classes to the 
function get_IA_class()in `TIER2/INTERACTION`. Carefully update the compatibility matrix in `get_IA_class.m.` Make sure you don’t change anything related to existing `SUBSURFACE` class combinations!
7. Test all combinations with other classes in dedicated test examples.
8. Compatibility with snow cover: To add snow cover representation to newClass, copy one of the `TIER3` classes (oldClass_snow.m) and rename it newClass_snow.m. Search and replace all occurrences of “oldClass” by “newClass”, as well as oldClass_snow with newClass_snowin thclass_def statement and the constructor. In general, all `TIER3` classes will work as template, the only rule is not to mix Xice classes and non-Xiceclasses (if myClass features Xice, use an oldClass which has Xice). Redo the previous two steps fornewClass_snow, but also ensure compatibility with the SNOW classes through dedicated `INTERACTION` classes.
9. Lateral interactions: **IMPORTANT!** All new code related to lateral fluxes needs to be added to `TIER2` *newClass.m* and related `TIER1` classes, not in the `LATERAL` classes. Also here, check the dedicated functions(in generalpush_... and pull_...)in oldClass and modify them. To ensure compatibility with a lateral class in `LATERAL/LAT1D` or `LAT3D`, check which `SUBSURFACE` class functions are called. In general, this will be in the functionspush and pull. Add these functions to `TIER2` newClass.m, placing the actual code in a suitable `TIER1` class.
10. Comment the new code and add a description to “Section 5: Detailed Documentation” in this document