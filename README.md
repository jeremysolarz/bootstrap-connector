# Introduction

This is a short How-To for quickly solving the regularly occurring error "no office executable found!". 
You can read a huge number of threads in various OpenOffice.org forums dealing with this problem. 
This error is caused by using the [Bootstrap Connection Mechanism](https://forum.openoffice.org/en/forum/viewtopic.php?f=44&t=1013) (introduced with OOo 2.x) in 
conjunction with 
moving 
or copying "juh.jar" (OOo's "Java UNO Helper" JAR file containing the class file "Bootstrap.class") from the subfolder 
"program\classes" of the OOo installation folder (which is for example on Windows "c:\program files\OpenOffice.org 2.3") 
to another folder (mostly a subfolder, which is directly accessible from a web container or web server like for 
example Tomcat).

# How-to
So, if you're encountering **"no office executable found!"** follow these steps:

1. Download the file "bootstrapconnector.jar" attached to this post and save it.
1. Put the JAR file "bootstrapconnector.jar" for example in the same folder, where you put "juh.jar", and add it to 
your CLASSPATH or add it in your IDE to the libraries for source code compiling (for example open in NetBeans the 
project properties and select there "Libraries" > tab "Compile" > press "Add JAR/Folder" > locate the "bootstrapconnector.jar" and press "Open").
1. Determine the folder of your OpenOffice.org executable "soffice.exe" (on Windows systems) or "soffice" (on *nix 
systems). On Windows it might be something like "c:\program files\OpenOffice.org 2.3\program\", and on *nix for example something like "/opt/openoffice.org2.3/program" or "/usr/lib/openoffice.org/program".
1. Edit your Java source code file, that tries to get the connection by calling "Bootstrap.bootstrap()". This is mostly
   done with a Java source code line looking like this:<br>
   ```XComponentContext xContext = Bootstrap.bootstrap();```
   Perform the following steps in this Java source file:
   1. Add the following line to your import statements:<br/>
   ```import ooo.connector.BootstrapSocketConnector;```
   1. Add a line above "Bootstrap.bootstrap()" to assign the name of the folder of your OpenOffice.org executable to a String variable:<br/>
      ```String oooExeFolder = "C:/Program Files/OpenOffice.org 2.3/program/";```
   1. Change the call of "Bootstrap.bootstrap()" to "BootstrapSocketConnector.bootstrap(oooExeFolder)":
      ```XComponentContext xContext = BootstrapSocketConnector.bootstrap(oooExeFolder);```
   1. Save and compile the edited file and see if "no office executable found!" has really vanished.
1. If you want to learn more about what you can do with "bootstrapconnector.jar", stay tuned. (To be continued...)   
 
# Credits

Most of the source code in the attached JAR file has been taken from the Java class "Bootstrap.java" (Revision: 1.15) 
from the UDK project (Uno Software Development Kit) from OpenOffice.org (http://udk.openoffice.org/). 
The source code is available for example through a browser based online version control access at http://udk
.openoffice.org/source/browse/udk/. The Java class "Bootstrap.java" is there available at 
http://www.openoffice.org/udk/source/browse/udk/javaunohelper/com/sun/star/comp/helper/Bootstrap.java?view=markup
 
The idea to develop the BootstrapConnector, BootstrapSocketConnector and BootstrapPipeConnector comes from the blog 
"Getting started with the OpenOffice.org API part III : starting OpenOffice.org with jars not in the OOo install dir by 
Wouter van Reeven" (http://technology.amis.nl/blog/?p=1284) and from various posts in the "(Unofficial) OpenOffice.org 
Forum" at http://www.oooforum.org/ and this forum complaining about "no office executable found!".
 
## Source
Taken from 
[https://forum.openoffice.org/en/forum/viewtopic.php?f=44&t=2520](https://forum.openoffice.org/en/forum/viewtopic.php?f=44&t=2520#).