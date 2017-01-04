# Code Generation Demonstration

This example is used to demonstrate the code generation
for DeOS, VxWorks and POK. You can generate the code
using Ocarina and also use the Ocarina/OSATE bridge
for that.

The system includes three functions
 * **sensor** : that send an altitude data to the filters. There are two sensors.
 * **filtering** : that filters data from the sensors and remove inconsistent data
 * **actuator**: receives data from the filter and take action

The example shows how to deploy the same system on the following targets:
 * DeOS
 * VxWorks653
 
# Installing Ocarina
Before proceeding, install Ocarina. You can download it on http://www.openaadl.org/download.html
Make sure you have the latest version.
Once you download it, configure OSATE to use Ocarina.
Click on Windows -> Preferences. Select OSATE preferences and then, Ocarina.
Enter the path where you installed Ocarina (the bin/ directory).

# Generate the code
In order to generate the code, you can do the following:

 * Select the main system implementation in the file integration.aadl
   The name of the root system depends on what tool you are
   using and what is the target platform. For example, for pok, 
   the name of the root system is integration.single_partition_pok.
 * With the system selected, choose in the main menu *OSATE* -> *Ocarina Bridge* ->
   then the appropriate code generator such as *Generate POK/VxWorks Code*.

 
# Integrate generated code with VxWorks
* Start VxWorks workbench 3.2
* Select File->New->Examples and select VxWorks 653 System. Give the name mydemo for example
* Select the board support package simpc and finish.
* Copy the generated code from OSATE in *codegen-demo/generated-code/module/pr* to VxWorks at *mydemo/mydemo_Partition1*
* Copy the functional code from OSATE in *codegen-demo/functional/vxworks* to VxWorks at *mydemo/mydemo_Partition1*
* In VxWorks, edit the file *mydemo/mydemo_Partition1/vxMain.c* and find the following line
```c
#define USER_APPL_INIT usrAppInit()
```
Change the define to look like this:
```c
#define USER_APPL_INIT main()
```
* Edit the file mydemo_PartitionOS/demo_PartitionOS.xml and make sure it looks like this
```xml
<Shared_Library_API
    xmlns="http://www.windriver.com/vxWorks653/SharedLibraryAPI"
    xmlns:xi="http://www.w3.org/2001/XInclude"
    Name="vThreads"
    >

    <Interface>
	<Version Name="template"/>
	<xi:include href="$(WIND_BASE)/target/vThreads/config/comps/xml/vthreads.xml"/>
	<xi:include href="$(WIND_BASE)/target/vThreads/config/comps/xml/apex.xml"/>
    </Interface>

</Shared_Library_API>
```
* Edit the file mydemo_PartitionOS/Makefile.vars and add *apexComponent.o* to *SSL_OBJS*:
```
SSL_OBJS += vThreadsComponent.o apexComponent.o
```
* Right click on *mydemo* and select *Build Project*.

# Run with VxWorks
1. Switch to the *Advanced Device Development* perspective.
2. In the main menu, Click on Target -> New Connection
3. Select *VxWorks 653* -> *Wind River VxWorks 653 Simulator Connection*.
4. Select Customer Simulator.
5. Choose the file boot.txt at the root of your project. If the file is not available, try
   to rebuild the project.
6. Continue the wizard until the end. After finishing the wizard, the simulator will automatically run.
7. To run the simulation again, in the remote system view, select the simulator and click on the *Connect* button.

# Integrate generated code with DeOS

1. Start DeOS
2. Click on File -> New -> DeOS Examples
3. Select deos653, blackboard_event_workspace
4. Select Finish
5. In blackboard_event/code remove the file
6. In blackboard_event/code, copy all the generated code
7. In blackboard_event/code, copy all the functional code.
8. In blackboard_event_configuration/xml remove the file config.653.xml
9. In blackboard_event_configuration/xml copy the generated configuration
   file for the kernel and rename it config.653.xml
10. In blackboard_event_configuration/xml/config.653.xml (the new file that you
   copied), make sure to replace Duration with the value 6000000
   
   You should have the following lines. Check the duration value
   as well as the ExecutableImageName
   
```xml   
   <Partitions>
      <Partition
           Name = "partition1"
           Identifier = "1"
           Period = "1250000000"
           Duration = "6000000"
           ExecutableImageName = "blackboard_event.exe"
           MainProcessStackSizeInPages = "1"
   
   <.. more XML code ..>
   
   
      <Schedule
        MajorFrameLength = "Automatic"
      >
      <PartitionTimeWindow
           PartitionNameRef = "partition1"
           Duration = "6000000"
           Offset = "0"
           PeriodicProcessingStart = "true"
           RepeatWindowAtNanosecondInterval = "PartitionPeriod"
           InhibitEarlyCompletion = "false"
      ></PartitionTimeWindow>
   </Schedule>
```   


# Run the code for DeOS

1. Create a virtual machine. Click on New -> New Platform Project
2. Select qemu-x86 and finish. A new project is visible in Eclipse
3. Right click on the eclipse project open the properties. 
4. In DDC-I options, in the dependencies, add blackboard_event for all registries
5. Open the Target Manager in the Project View.
6. Select Create New Target, leave the default values
7. Select the target available
8. Click on the run button
9. The simulator starts and you can see the result.






