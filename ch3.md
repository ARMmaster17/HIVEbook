# Installing the HIVE environment
At this point, you should have at least two eliglbe nodes on a wired network of some kind. For each node, you will need to install the HIVE engine service, for this you have a few options.

## Download files
You will need to aquire the files for HIVE. The free version comes in either a .zip or .tar.gz compressed file. Purchasing the enterprise version also gives the option of a .iso file that can be burned to a CD, DVD, or USB storage device. From there you will need to copy all files to a folder on your computer where an administrator can run the program. Copying the files to a RAID-enabled SAN may be desirable for large-scale deployment.

## Compile from source
This option is only available for trial purposes or organizations with an enterprise license. You will need Visual Studio 2013. This will be covered later in this part of the book. Keep in mind that directly modifying the HIVE engine is a violation of the license agreement.

## Deploy from MSI
At this time, there is no official MSI package available. If you have the tools to create MSI packages, you can deploy HIVE on all computers on the network using pre-built setting files. It is also possible to deploy batch scripts alongside the setup files to create settings files specific to each node.