#
# Dockerfile Java client for Windows docker container on Windows Server
#
# https://github.com/dockerfile/java

# Pull base image.
FROM mcr.microsoft.com/windows/servercore:ltsc2019

LABEL description="java-8-x64"

# Define working directory.
WORKDIR "C:\\"

# Install Java.
RUN powershell.exe -Command \
	$ErrorActionPreference = 'Stop'; \
	wget https://javadl.oracle.com/webapps/download/AutoDL?BundleId=245807_df5ad55fdd604472a86a45a217032c7d -OutFile jre-8-x64.exe; \
	Start-Process -FilePath jre-8-x64.exe -Wait -PassThru -ArgumentList "/s","INSTALLDIR=C:\Java\x64"; \
	Remove-Item jre-8-x64.exe -Force; \
	[Environment]::SetEnvironmentVariable('Path', $env:Path + ';C:\Java\x64\bin', [EnvironmentVariableTarget]::Machine); \
	[Environment]::SetEnvironmentVariable('JAVA_HOME', 'C:\Java\x64\bin', [EnvironmentVariableTarget]::Machine); \
	[Environment]::SetEnvironmentVariable('EXE4J_JAVA_HOME', 'C:\Java\x64', [EnvironmentVariableTarget]::Machine)

# Define default command.
CMD ["java.exe", "-version"]
