# CS_JVMHeapThreshold.bat

*** USE THIS TOOL ONLY WHEN VMWARE GSS/ENGINEERING RECOMMENDS MODIFYING DEFAULT VALUES OF JVM HEAP MEMORY TO CUSTOM VALUE. ***

CAUTION: It is highly recommended to test this registry change script in test environment to ensure it works for your environment! 

It is NOT recommended to set more than 8 GB to tomcat services in connection server. So far only the MessageBusServices and TomcatServices were modified to higher values in some of the customer environments. This script is written to avoid manual modification of registry.

Maximum or safe limits of Heap memory for horizon services. It is important that system has enough RAM to allocate below heap memory.

TomcatService : 8 GB

MessageBusService: 4 GB

TunnelService: 4 GB


Utility to set custom JVM threshold values on connection server.

Usage:
    
    CS_JVMHeapThreshold.bat
        
        * Updates only the JVM heap settings in registry.
        
        * Requires manual restart of Connection Server services for heap settings to take effect.
    
    CS_JVMHeapThreshold.bat -restart
        
        * Updates only the JVM heap settings in registry.
        
        * Restarts VMware Horizon Connection Server services automatically after updating the registry
    
    Use CS_JVMHeapThreshold.bat -h or CS_JVMHeapThreshold.bat -help for help
    
How to update the allocated JVM Heap Memory to different value in the bat file ?


REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\VMware, Inc.\VMware VDM\plugins\wsnm\TomcatService\Params" /t REG_MULTI_SZ /f /v "JvmHeapThresholds" /d "0MB:-Xmx1024m -Dcom.vmware.vdi.SmallPhysMemory=1\09728MB:-Xmx4096m\015872MB:-Xmx6144m\024064MB:-Xmx8192m"

REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\VMware, Inc.\VMware VDM\plugins\wsnm\TunnelService\Params" /t REG_MULTI_SZ /f /v "JvmHeapThresholds" /d "0MB:-Xmx1024m\09728MB:-Xmx2048m\015872MB:-Xmx4096m\024064MB:-Xmx4096m"

REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\VMware, Inc.\VMware VDM\plugins\wsnm\MessageBusService\Params" /t REG_MULTI_SZ /f /v "JvmHeapThresholds" /d "0MB:-Xmx1024m\09728MB:-Xmx2048m\015872MB:-Xmx4096m\024064MB:-Xmx4096m"

For example, if the tunneling is not used in the connection server, it can be left at default values of 2 GB heap.

9728MB:-Xmx2048m --> This line means if the system has 10 GB RAM, then allocate 2 GB of heap memory to tunneling Java process. As 2 GB is enough we will remove the other options below

REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\VMware, Inc.\VMware VDM\plugins\wsnm\TunnelService\Params" /t REG_MULTI_SZ /f /v "JvmHeapThresholds" /d "0MB:-Xmx1024m\09728MB:-Xmx2048m\015872MB:-Xmx4096m\024064MB:-Xmx4096m"

Now, the below lines configures the JVM heap threshold value as 2 GB. 

REG ADD "HKEY_LOCAL_MACHINE\SOFTWARE\VMware, Inc.\VMware VDM\plugins\wsnm\TunnelService\Params" /t REG_MULTI_SZ /f /v "JvmHeapThresholds" /d "0MB:-Xmx1024m\09728MB:-Xmx2048m"

If the custom JVM heap values set in JVMThresholds, then after each connection server installation it is required to modify the JVMThresholds as it will overwrite the values back to its previous values during installation. 
