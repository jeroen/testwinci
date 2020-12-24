FROM mcr.microsoft.com/windows/servercore:ltsc2019

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# "latest" version redirect would be available via https://cloud.r-project.org/bin/windows/base/release.htm
ENV R_VERSION 3.6.3
ENV RTOOLS Rtools35

WORKDIR /

# flexibly allow security protocols, see https://stackoverflow.com/questions/51208896/
RUN Write-Debug [Net.ServicePointManager]::SecurityProtocol ;\
    [Net.ServicePointManager]::SecurityProtocol ;\
    [Net.ServicePointManager]::SecurityProtocol = 'Ssl3', 'Tls12'; \
    Invoke-WebRequest https://cloud.r-project.org/bin/windows/base/old/$env:R_VERSION/R-$env:R_VERSION-win.exe -OutFile .\R-win.exe; \
    Start-Process .\R-win.exe -ArgumentList '/VERYSILENT /DIR="c:\R"' -Wait ; \
    Remove-Item .\R-win.exe -Force ; \
    Invoke-WebRequest https://cran.r-project.org/bin/windows/Rtools/$env:RTOOLS.exe -OutFile .\Rtools-installer.exe; \
    Start-Process .\Rtools-installer.exe -ArgumentList '/VERYSILENT' -Wait ; \
    Remove-Item .\Rtools-installer.exe -Force ; \
    $rPath = 'C:\R\bin;C:\Rtools\bin' ; \
    $newPath = ('{0};{1}' -f $rPath, $Env:PATH); \
    setx /M PATH $newPath; \
    Add-Content c:\R\etc\Rprofile.site 'options(repos = c(CRAN = \"https://cloud.r-project.org/\"))'



CMD ["R.exe"]
