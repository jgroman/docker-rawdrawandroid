# docker-rawdrawandroid

Docker image for building [rawdrawandroid framework](https://github.com/cnlohr/rawdrawandroid) projects. Host system `adb` is used as a server and `adb` inside container is connected to this server in client mode using `adb -H host.docker.internal` parameter.

## Usage in Windows

* You need to have `adb` installed on your host Windows. `adb` **versions on host system and inside Docker container MUST be the same**. Usually rebuilding Docker image and updating platform-tools on host system will ensure equal `adb` versions.
* Edit `rda.ps1` helper script and update `$CmdAdb` so that it points to `adb` executable on your system.
* Connect Android device to host system and check that host `adb` can detect it:

```powershell
> adb devices -l
List of devices attached
bcc4c4e1               device product:lineage_TBX704 model:Lenovo_TB_X704F device:X704F transport_id:3
```

* Clone project repository and build Docker image:

```powershell
git clone https://github.com/jgroman/docker-rawdrawandroid.git
cd docker-rawdrawandroid
.\rda.ps1 -Build
```

* Check that `adb` inside container can connect to your Android device:

```powershell
> .\rda.ps1 -Run adb devices -l
List of devices attached
bcc4c4e1               device product:lineage_TBX704 model:Lenovo_TB_X704F device:X704F transport_id:3
```

### Running commands

You can either run container separately for every command:

```powershell
> .\rda.ps1 -Run make run
```

Or you can preload container and leave it running in background. This way you will avoid delays in creating and tearing down of the container.

```powershell
> .\rda.ps1 -Start
```

Execute as many commands as you need:

```powershell
> .\rda.ps1 -Exec make run
```

And finally stop container:

```powershell
> .\rda.ps1 -Stop
```

Note: Commands executed using both `-Run` and `-Exec` are run in the current working directory.
