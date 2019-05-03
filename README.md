# msvc-bundle: creating a resource bundle for Windows cross-compilation

## Create Windows build container image
`docker build -t vs_buildtools:latest -m 2GB`

## Run powershell inside the consider with a volume mount
`docker run -it vs_buildtools:latest -v C:\msvc-bundle:C:\msvc-bundle`

## Copy files from the container to the volume mount

```
$Env:MSVC_BUNDLE = "C:\msvc-bundle"
Copy-Item -Recurse -Path $Env:VCToolsInstallDir\include -Destination $Env:MSVC_BUNDLE\vctools\include
Copy-Item -Recurse -Path $Env:VCToolsInstallDir\lib -Destination $Env:MSVC_BUNDLE\vctools\lib
Copy-Item -Recurse -Path $Env:WindowsSdkDir\Include\$Env:WindowsSDKVersion\ $Env:MSVC_BUNDLE\winsdk\Include\$Env:WindowsSDKVersion
Copy-Item -Recurse -Path $Env:WindowsSdkDir\Lib\$Env:WindowsSDKVersion\ $Env:MSVC_BUNDLE\winsdk\Lib\$Env:WindowsSDKVersion
```

## Create tarball from extracted MSVC bundle

Install the 7-zip command-line tool if you don't have it:
`choco install 7zip`

Create a tarball from the extracted msvc-bundle resources (run from cmd.exe):
`7z a -ttar -so msvc-bundle.tar C:\msvc-bundle | 7z a -si msvc-bundle.tar.gz`
