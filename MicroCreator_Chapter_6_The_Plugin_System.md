### Index Navigation ###

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 5: Error Messages](MicroCreator_Chapter_5_Error_Messages.md).

The next chapter is [Chapter 7: MicroCreator's Internals](MicroCreator_Chapter_7_Internals.md).

# Plugin System #

MicroCreator's plugin system allows users to modify the default behavior of the tool.  It allows the addition of custom passes to the list of existing ones without having to modify the code structure or makefiles.  The plugin also enables users the replacement of any existing pass.  Finally, it permits the modification of the pass gates behavior.  Before each pass execution, MicroCreator calls the associated gate.  The gate is a function returns true or false depending whether or not the system wants to execute the pass or not.  A pass is simply a piece of code with a kernel as input and provides the same or multiple new kernels as its output.

# Requirements #

The user must provide a C, a C++, or a dynamic library version of the plugin.  If the user provides a source code version, MicroCreator compiles, at execution, into a dynamic library.  The tool only supports one file as the plugin.  In cases of a plugin using multiple files, the use of a dynamic library is the only solution.

The plugin must contain the definition of one single function:

```
 void pluginInit (Driver *driver, const Description *desc);
```

The function takes two parameters, the tool's driver class and the description instance.  The description instance contains all the execution's information such as the output directory, maximum number of benchmark programs to be generated, etc.

There are currently two ways to link the plugin to the core program: adding a new pass, changing an existing pass, or adding a new gate system.  Using the plugin system, a user may add a new pass or modify any of MicroCreator's internal passes.

To add or modify a pass, use the following function:

```
 void pluginAddPass (Driver *driver, const SPluginPassInfo *info);
```

The driver provided should be the driver passed in argument to the pluginInit function.  SPluginPassInfo contains the information required for MicroCreator to handle a new pass.

To set a new plugin gate, use the following function:

```
 bool setPluginGate (const SPluginGateInfo *gateInfo);
```

The SPluginGateInfo page explains the data structure and MicroCreator takes it into account.

## List of Passes ##

MicroCreator, via the command-line option -l, prints the pass used internally.  Currently, MicroCreator outputs:

```
 The list of passes currently used by the driver: 
 -> Statement scheduling
 -> Induction selection
 -> Immediate selection before unroll
 -> Operand swap before unroll
 -> Operation choose before unroll
 -> Stride selection
 -> Pass unroll
 -> Operation choose after unroll
 -> Operand swap after unroll
 -> Immediate selection after unroll
 -> Induction insertion
 -> Register allocation
 -> Code generation
```

## Loading Plugins ##

Plugins are loaded with:

```
$.  /microcreator input.xml --fplugin='/path/to/name.so'
```

There is no limit on the number of the plugins loaded.

## Building MicroCreator Plugins ##

The following GNU Makefile shows how to build a simple plugin:

```
EXE=MyPlugin.so
OPT_INCLUDE = -I.  ./Core/Include
OPT = -fPIC -shared $(OPT_INCLUDE)  
all: $(EXE)
$(EXE):
    g++ MyPlugin.cpp -o $(EXE) $(OPT)
clean:
    rm *.so
```

# Plugin Pass #

A plugin pass is a pass defined by a user, replacing an existing pass or creating a new pass.  To define a plugin pass, the user must write a certain number of functions:

  * Plugin name: provides the name of the plugin pass

  * Plugin gate: whether or not, depending on the content of the description and the kernel, does MicroCreator perform the pass or not?

  * Plugin entry: the entry point to the plugin, if the gate returns true, MicroCreator the entry function

In the structure SPluginPassInfo, the user must also define:

  * PassRef: the name of the pass to position, or replace, the plugin pass with

  * PluginPos: tells the system whether to replace or how to place the new pass

## Plugin Pass Example ##

The first plugin example performs a pass to reduce the number of microbenchmark programs generated.  The input file is first explained before presenting the plugin code.

### XML Input File ###

The directory ''examples/PluginExamples/Pass'' contains the following input example file.  The input file is in XML format two kernels, both defining a single instruction to be unrolled from one to eight times.  Thus, MicroCreator should generate sixty-four microbenchmarks.  However, the users wishes to keep the microbenchmarks with the same unrolling factor for both kernels.  As a result, only eight microbenchmarks with the same unrolling factor, one to eight, are kept.  The [MicroCreator\_Chapter\_4: Input Specification input specification page presents more information on the input format.

The input file starts with the definition of a [MicroCreator\_Chapter\_4: Input Specification#Description node description node].

```
 <?xml version="1.0"?>
 <description>	
```

The first kernel is then defined.  It contains a single addpd instruction with the source registers and destination registers defined as Xmm registers.  The minimal and maximal values allow MicroCreator to use different registers during the unrolling process.  The [unrolling node page](MicroCreator_Chapter_4_Input_Specification_Unrolling.md) provides more information.

```
    <kernel>
        <instruction>     
                <operation>addpd</operation>
                <register>
                        <phyName>%xmm</phyName>
                        <min>0</min>
                        <max>8</max>
                </register>
                
                <register>
                        <phyName>%xmm</phyName>
                        <min>0</min>
                        <max>8</max>
                        </register>	
                </instruction>	
 	
                <unrolling>
                        <min>1</min>
                        <max>8</max>
                        <progress>1</progress>
                </unrolling>
    </kernel>
```

The second kernel is identical except for the use of a mulpd instruction.
```
    <kernel>
        <instruction>     
                <operation>mulpd</operation>
                <register>
                        <phyName>%xmm</phyName>
                        <min>0</min>
                        <max>8</max>
                </register>
                
                <register>
                        <phyName>%xmm</phyName>
                        <min>0</min>
                        <max>8</max>
                        </register>	
                </instruction>	
 
                <unrolling>
                        <min>1</min>
                        <max>8</max>
                        <progress>1</progress>
                </unrolling>
    </kernel>    
```
Finally, the description node is closed:
```
 </description>
```
### Plugin Source Code ###

The plugin filters the kernels having different unrolling factors, removing any description containing two different unrolling factors.  The example is in the examples/PluginExamples/Pass folder.

#### Includes ####

The first part of the source code contains the file includes.  The following section depends on what the user is using.  For the current example, a certain number of elements of the main Application Program Interface (API) are required:

```
#include <cassert>
#include <sstream>

#include "Comment.h"
#include "Driver.h"
#include "Kernel.h"
#include "Logging.h"
#include "PassElement.h"
```

#### Declarations ####

After the include lies the plugin's local variable and function declarations.

```
static bool chooseKernel (Kernel *kernel, Description *desc, int &unrollFactor);
static bool handleKernel (Kernel *kernel, Description *desc, int &unrollFactor);
static int extractInt (const std::string &str);

static const std::string& ourName = "Filtering Plugin";		/** < @brief The plugin name */
static bool ourGate = true; 					/** < @brief The plugin gate */
```

  * HandleKernel: walks through the kernels and calls chooseKernel

  * ChooseKernel: determines whether or not to keep the kernel

  * ExtractInt: helper function transforming a string into an integer

The two local variables are used by the plugin functions to determine the name of the plugin and the gate value.

#### Mandatory Functions ####

The file contains, after the local functions, mandatory functions.  For example, myPluginGate returns the gate of plugin pass and myPluginName returns the plugin name.

```
extern "C" bool myPluginGate (const Description *desc, const Kernel *kernel)
{
	return ourGate;
}
```

```
extern "C" const std::string& myPluginName (void)
{
	return ourName;
}
```

The function which defines what the plugin should do is called myPluginEntry.  It returns a vector of passElement handled by the passEngine.  Each new passElement in the vector is added to the PassEngine and provokes the generation of a new microbenchmark program.

The function starts by logging its pass, used for debugging purposes.  If ever the pass should fail, the log contains the initial log entry and not the final log entry at the end of the function.  Logging helps the user to determine which plugin has failed.

```
std::vector <PassElement *> *myPluginEntry (PassElement *pe, Description *desc)  
{
    Logging::log (0, "Starting Filtering", NULL);
```

The first job of any entry function is generally to retrieve the kernel from the PassElement.  If ever it is NULL, there is nothing to be done.  Sometimes, the user might want to create a default kernel.

```
    //First get Kernel
    Kernel *kernel = pe->getKernel ();

    if (kernel == NULL)
    {
	return NULL;
    }
```

To go through each kernel, a coloring system is used.  Therefore, before doing anything, the plugin should clear the color and create a new result vector.
NULL as a value for the result vector tells MicroCreator the kernel contained in pe is to be dropped.  Otherwise, MicroLauncher inserts the new kernels into the workflow.  The new kernels may or not contain the original one.

```
    //Clear color
    kernel->clearColor ();    

    //Prepare result
    std::vector <PassElement *> *res = NULL;

```

The next question is whether or not to keep the kernel.  Remember the purpose of the plugin is to keep the kernels containing two identical unrolling factors.  If handleKernel returns false, it is not the case and the kernel should be dropped.  Otherwise, the kernel should be added to a result vector and returned.  Finally, the end log entry is performed.
```
	//Now we start filtering
	int unrollFactor = -1;
	bool keepIt = handleKernel (kernel, desc, unrollFactor);

	if (keepIt == true)
	{
                //Create new vector
                res = new std::vector <PassElement *> ();
                //Ok we want this one, push it in
                res->push_back (pe);
	}
    
	Logging::log (0, "Stopping Filtering", NULL);
	    
	return res;
}
```

#### Particular Functions ####

The myPluginEntry function calls handleKernel.  The latter function also calls other functions presented here.  handleKernel is a typical tree walker.  It starts by determining whether or not the algorithm already colored the kernel.  If it has, there is no longer anything to do.

```
static bool handleKernel (Kernel *kernel, Description *desc, int &unrollFactor)
{
    //Handle whole kernel and find all the non colored kernels and pass them on

    //Check if we are finished
    if (kernel->getColor () == 0)
    {
```

However, if the algorithm has not considered the kernel yet, it colors the kernel and determines whether or not the kernel should be kept by the system.  Keeping the kernel or not is determined by the chooseKernel function, explained later.

```
        //Color this Kernel
        kernel->setColor (1);
 
       	//Now handle this Kernel: if over, no need to continue
        if (chooseKernel (kernel, desc, unrollFactor) == false)
		return false;
    }
```

In every case, the walker must go through each statement of the kernel and, in the case of an internal kernel, walk through it.

```
    //Now go into the kernel
    unsigned int nbr = kernel->getNbrStatements ();
 
    //Now go into this Kernel and find the inner kernels
  	for (unsigned int i = 0; i < nbr; i++)
    {
        //Get the instruction
        Statement *stmt = kernel->getModifiableStatement (i);
 
        //If it's a Kernel, go into it
        Kernel *inner = dynamic_cast<Kernel *> (stmt);
 
        if (inner!= NULL)
        {
	        bool tmp = handleKernel (inner, desc, unrollFactor);

		//Only stop if tmp is false
		if (tmp == false)
		{
			return false;
		}
        }
    }
 
    return true;
}
```

The chooseKernel function determines whether the system should keep the overall kernel or not.  The decision is performed depending on the unroll factors of the code.  If they are not all identical, the plugin requests the kernel be dropped.  The function starts by checking for a NULL pointer and defining a couple of local variables.

```
static bool chooseKernel (Kernel *kernel, Description *desc, int &unrollFactor) 
{
        if (kernel == NULL)
	{
		return true;	
	}

	std::string str = "";
	
	unsigned int nbr = kernel->getNbrStatements ();
```

The variable nbr contains the number of statements of the kernel.  The next step is to check each statement and find the comments.  A statement can be multiple child classes, as shown in its doxygen page.  A dynamic cast determines whether a statement is a comment.

```
	for (unsigned int i = 0; i < nbr; i++)
	{
		//Get statement
        	Statement *stmt = kernel->getModifiableStatement (i);

		//Paranoid
	        assert (stmt!= NULL);

		//We only care about comment
		Comment *comment = dynamic_cast<Comment *> (stmt);	 
  
       		if (comment!= NULL)   
		{
```

When unrolling, the unrolling pass adds a decipherable comment here.  One such comment is for example:

```
	#Unrolled factor 8
```

The rest of the code simply decodes it and uses the parameter unrollFactor as a holder of the last unrolling factor.  If they are not the same, the function returns false, signifying the kernel should be dropped.  Otherwise, it should be kept.

```
			stmt->getString (str, desc, 0);

			//We care about '#Unroll factor'
			if (str.  find ("#Unrolled factor")!= str.  npos)
       			{
				//The first unroll comment update the factor of unrolling
				if (unrollFactor == -1)  
				{
					//Update the unroll factor
					unrollFactor = extractInt (str);
				}
				else
				{
					if (extractInt (str)!= unrollFactor)
					{	
						return false;
					}
				}			
			}
		}
   	}

	return true;
}
```

#### Init the Plugin ####

After loading a plugin into memory, MicroCreator calls the pluginInit to initialize it.  It is the function requesting the addition of the plugin and It must be defined in each plugin.  pluginInit can add, remove, or replace multiple passes at the same time.  The code itself contains a local variable plugInfo1 of type SPluginPassInfo.

```
extern "C" void pluginInit (Driver *driver, const Description *desc, const Kernel *kernel)
{
	//PluginPass
	SPluginPassInfo plugInfo1 = {myPluginName, myPluginGate, myPluginEntry, "Pass Unroll", PASS_POS_INSERT_AFTER};
	PluginPass *pass1 = new PluginPass (plugInfo1);
	pass1->pluginAddPass (driver, plugInfo1);
}
```

### Output ###

After executing the MicroCreator by calling the plugin, the tool generates eight microbenchmarks.  The unrolled eight times contains the following code:

```
	#Unroll beginning
	#Unrolled factor 8
	#Unrolling, iteration 1 out of 8
	addpd %xmm0, %xmm0
	#Unrolling, iteration 2 out of 8
	addpd %xmm1, %xmm1
	#Unrolling, iteration 3 out of 8
	addpd %xmm2, %xmm2
	#Unrolling, iteration 4 out of 8
	addpd %xmm3, %xmm3
	#Unrolling, iteration 5 out of 8
	addpd %xmm4, %xmm4
	#Unrolling, iteration 6 out of 8
	addpd %xmm5, %xmm5
	#Unrolling, iteration 7 out of 8
	addpd %xmm6, %xmm6
	#Unrolling, iteration 8 out of 8
	addpd %xmm7, %xmm7
	#Unroll ending
	#Unroll beginning
	#Unrolled factor 8
	#Unrolling, iteration 1 out of 8
	mulpd %xmm0, %xmm0
	#Unrolling, iteration 2 out of 8
	mulpd %xmm1, %xmm1
	#Unrolling, iteration 3 out of 8
	mulpd %xmm2, %xmm2
	#Unrolling, iteration 4 out of 8
	mulpd %xmm3, %xmm3
	#Unrolling, iteration 5 out of 8
	mulpd %xmm4, %xmm4
	#Unrolling, iteration 6 out of 8
	mulpd %xmm5, %xmm5
	#Unrolling, iteration 7 out of 8
	mulpd %xmm6, %xmm6
	#Unrolling, iteration 8 out of 8
	mulpd %xmm7, %xmm7
	#Unroll ending
```

## Conclusion ##

As a conclusion, the chapter showed what a user must provide to create a plugin pass for the microcreator tool.  A plugin can replace or be added before or after a given pass.  Additionally, the chapter explained, through an example, how to walk the intermediate representation and how to manipulate the basic classes the user might encounter.

# New Gate System #

A gate plugin redefines any gate in the MicroCreator tool.  It is used, for example, to turn off a pass depending on the kernel.

The gate plugin must define, via the pluginInit function, a SPluginGateInfo structure.

The data structure contains two elements:

  * Name: provides the name of the gate system

  * Gate: defines the entry point to the gate plugin system

Once the SPluginGateInfo is defined in the pluginInit function, the user must call the setPluginGate from the parameter driver's instance.

## Example ##

The example source code is defined in the examples/PluginExamples/Gate directory.

### Includes ###

The plugin starts with a series of includes.
```
#include <cassert>
#include <iostream>
#include <string.h>

#include "Driver.h"
#include "PassElement.h"
#include "PluginGateInfo.h"
```

### Declaration ###

In the example, there is only the need for one variable containing the name of the plugin.

```
static const std::string ourName = "Gate plugin";		/** < @brief The plugin name */
```

### Mandatory Functions ###

myPluginGate return the gate of the plugin pass and myPluginName return the plugin name.

```
extern "C" const std::string &myPluginName (void)
{
	return ourName;
}
```

### Gate Changer ###

The function changes the gate of the pass contained in the PassElement.  The user can determine depending on the kernel contained in elem and the pass name whether or not to execute the pass.  To request an execution of the pass, the function must return true.  For the example, the ''Immediate selection before unroll'' pass is disabled.

```
bool myGate (const Description *desc, const PassElement *elem)
{
	//If nothing, why not?
	if (elem == NULL)
		return true;

	bool res = elem->gate (desc, elem->getKernel ());
	const Pass *pass = elem->getPass ();

	//If original gate says no, so do we
	if (res == false)
		return false;

	return (pass->getName ()!= "Immediate selection before unroll");
}
```

### Initializing the Plugin ###

The plugin initialization is performed by the pluginInit function.  Contrary to the previous example, the user must define a SPluginGateInfo variable.  The structure contains the name of the plugin and the gate function to be called.

```
extern "C" void pluginInit (Driver *driver, const Description *desc)
{
	//PluginGate
	SPluginGateInfo plugInfo1 = {myPluginName, myGate};
	driver->setPluginGate (plugInfo1);
}
```

# Index Navigation #

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 5: Error Messages](MicroCreator_Chapter_5_Error_Messages.md).

The next chapter is [Chapter 7: MicroCreator's Internals](MicroCreator_Chapter_7_Internals.md).