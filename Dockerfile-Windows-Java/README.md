# Dockerfile-Windows-Java
This is the Java client version for creating Docker container on Windows servercore:ltsc2019

## Installation
- Install Docker
- Download the required Dockerfile

### For Windows Server
- Execute `Get-Content .\Dockerfile | docker build -t java-8-x64 -`

## Notes
Added environment variables to run applications packaged with exe4j.
If you try to use the JAVA_TOOL_OPTIONS environment variable , the packaged EXE4J application will not be able to start and will report an error:

    No JVM could be found on your system.
    Please define EXE4J_JAVA_HOME
    to point to an installed 64-bit JDK or JRE or download a JRE from www.java.com.
  
## Run Example

    docker run -dit --name "javaProg" --env JAVA_TOOL_OPTIONS="-Dfile.encoding:UTF8" --network "nat" -v "C:\Temp:C:\Temp" "java-8-x64" "cmd.exe" "/C chcp 65001 && cd C:\Temp && java.exe -jar myProg.jar"
