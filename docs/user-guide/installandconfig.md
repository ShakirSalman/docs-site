# Introduction

The installation of Zowe&trade; consists of two independent processes: installing the Zowe server components either entirely on z/OS or a combination of z/OS and Docker, and then installing Zowe CLI on a desktop computer.  

The Zowe server components provide a web desktop that runs in a web browser providing a number of applications for z/OS users, together with an API mediation layer provides single-sign on (SSO), organization of the multiple zowe servers under a single website, and other capabilities useful for z/OS developers. 

Zowe CLI can connect to z/OS servers and allows tasks to be performed through a command line interface.

Because Zowe is a set of components, before installing Zowe first determine which components you want to install, and where you want to install them. This guide covers all of the components, but is split into sections so you can install as much as you need.

Here are some scenarios to consider:

- If you will only be accessing the Zowe server components through a web browser or REST API client, then you do not need to install the Zowe CLI.
- If you will only be using the Zowe CLI, depending on the plugins used you may not need to install the Zowe server components.
- If you intend to use Docker for the server components, less components need to be installed on z/OS. If you are not using the Desktop or ZSS, then it's possible run the other Zowe components without installing any of Zowe onto z/OS.

Before you start the installation, review the information on system requirements and other considerations.

## Planning the installation of Zowe server components

All Zowe server components can be installed on z/OS, but some have the alternative option of being run inside of a Docker image on a Linux host.
Which option you choose effects the prerequisites, where they are installed, and the installation steps needed.

### Planning z/OS installation
If you are installing one or more server components onto z/OS, the following information is required during the installation process. Software and hardware prerequisites are covered in the next section.

- The zFS directory where you will install the Zowe runtime files and folders.  For more details of setting up and configuring the UNIX Systems Services (USS) environment, see [UNIX System Services considerations for Zowe](configure-uss.md).

- A HLQ that the installation can create a load library and samplib containing load modules and JCL samples required to run Zowe.

- Multiple instances of Zowe can be started from the same Zowe z/OS runtime.  Each launch of Zowe has its own zFS directory that is known as an instance directory.  

- (If not using Docker) Zowe uses a zFS directory to contain its northbound certificate keys as well as a truststore for its southbound keys.  Northbound keys are one presented to clients of the Zowe desktop or Zowe API Gateway, and southbound keys are for servers that the Zowe API gateway connects to.  The certificate directory is not part of the Zowe runtime so that it can be shared between multiple Zowe runtimes and have its permissions secured independently. 

- Zowe has two started tasks.
   - `ZWESISTC` is a cross memory server that the Zowe desktop uses to perform APF-authorized code. More details on the cross memory server are described in [Configuring the Zowe cross memory server](configure-xmem-server.md). 
   - `ZWESVSTC` brings up the every other other part of the Zowe runtime on z/OS that was requested. This may include Desktop, API mediation layer, ZSS, and more, but when using Docker likely only ZSS will be used here.
   
     In order for the two started tasks to run correctly, security manager configuration needs to be performed.  This is documented in [Configuring the z/OS system for Zowe](configure-zos-system.md) and a sample JCL member `ZWESECUR` is shipped with Zowe that contains commands for RACF, TopSecret, and ACF2 security managers.  

