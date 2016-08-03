# Code Generation Demonstration

This example is used to demonstrate the code generation
for DeOS, VxWorks and POK. You can generate the code
using Ocarina and also use the Ocarina/OSATE bridge
for that.

The system includes three functions
 * **sensor** : that send an altitude data to the filters. There are two sensors.
 * **filtering** : that filters data from the sensors and remove inconsistent data
 * **actuator**: receives data from the filter and take action

The example shows how to deploy the same system in
different configurations. Here, for each target,
we show two deployment strategies:
 * **single partition** : all functions in one partition
 * **four partitions** : all functions in different partitions.

This example works with the following targets:
 * DeOS
 * VxWorks653
 * POK

# Generate the code
In order to generate the code, you can do the following:

 * Select the main system implementation in the file integration.aadl
   The name of the root system depends on what tool you are
   using and what is the target platform. For example, for pok, 
   the name of the root system is integration.single_partition_pok
   is integration.four_partitions_pok
 * Select the system in the outline view. Right click, select Ocarina
   menu and then the appropriate code generator

# Compile & run the code for POK
 * Install POK on your system. Make sure the x86 toolchain is working
 * Copy the functional code in the partitions. For the single partition
   example, copy all the c files into the partition directory (generated-code/module/pr).
   For the four partitions example, copy each functional code into the appropriate
   directory (generated-code/module/filter/, generated-code/module/act/,
   generated-code/module/sensor0, generated-code/module/sensor1)
 * Compile and run: ```make clean all run```
 

# Compile & run the code for VxWorks



# Compile & run the code for DeOS

# Notes
 * The POK demo has been tested on Linux