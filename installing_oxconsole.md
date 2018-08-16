## Installing Ox Console

OxConsole is an open source statistical modelling application available from Jurgen Doornik's [Website](http://www.doornik.com/download.html).
The free version, which we will download and configure here, is a serial application.

As the application is only available as an RPM package (Redhat Package Manaagement system package), 
there are a number of steps needed to download and install. It can be installed and configured on the ARC HPC clusters by individual users.

It is a relatively small application so can safely be installed in your HOME directory.

[Download page](http://www.doornik.com/download/oxmetrics7/Ox_Console/)

At the time of writing, the current version is 7.20. These instructions are for that version.

1. **Download the 64-bit RPM**

Navigate to your home directory and download:

```
cd $HOME
wget http://www.doornik.com/download/oxmetrics7/Ox_Console/oxcons-7.20-0.x86_64.rpm
```

2. **Extract the RPM**
```
rpm2cpio oxcons-7.20-0.x86_64.rpm | cpio -idmv
```
This will extract the files into a `usr` subdirectory within your HOME directory.

4. **Move files to more suitable path** 

Move the directory containing all the code and libraries to the 'root' of your HOME directory:

```
mv $HOME/usr/share/OxMetrics7 $HOME
```
5. **Delete unwanted files**

```
rm -rf usr/
rm oxcons-7.20-0.x86_64.rpm
```

6. **Set PATHs**

OxConsole needs a set of PATHs to be set so that the shell can find the executable and libraries.  These should be put in your `~/.bashrc` file
so that they are set every time you log in. This code will do that for you.

```
cat >> ~/.bashrc << EOF
export OX_DIR=$HOME
export LD_LIBRARY_PATH=$OX_DIR/OxMetrics7/ox/bin64:$LD_LIBRARY_PATH
export OX7PATH=$OX_DIR/OxMetrics7/ox/include:$OX_DIR/OxMetrics7/ox
export PATH=$OX_DIR/OxMetrics7/ox/bin64:$PATH
EOF
```

8. **Check it all works**
* Log out and back in again to make sure the PATHs have been set correctly.
* Run the Ox command `oxl.bin`. This should report back the help summary confirming it has been installed correctly:

```
Ox Console version 7.20 (Linux_64) (C) J.A. Doornik, 1994-2017
This version may be used for academic research and teaching only
Ox Object-oriented matriX language
Usage:	Ox [options] filename[.ox] [arguments for Ox program]
Options:
  -d          debugging mode
  -i          interactive mode (not all versions)
  -c          create object (.oxo) file
  -Dtoken     define a token, e.g. -DOPTION1+OPTION2
  -g          source is gauss file
  -lfilelist  link object file, e.g. -lfile1+file2+file3
  -ipath      add include/link path
  -x          clear include/link path
  -od         switch code optimizations off
  -on         switch line numbering off
  -r-         do not run code
  -rc#,#      set matrix cache: entries,max_size (default: -rc0,0)
  -rr         print cache report
  -s#,#       set symbol table and stack size (default: -s3000,1000)
  -v#         set verbose (-v is same as -v1)
  -w0         switch parse warnings off
```

7. **Sample script** 

This is a sample submission script for OxConsole. Save it as `ox.sh` and submit using `qsub ox.sh`.

```
#Submission script for ox jobs
#Request time for job- here 30 minutes. Format is hh:mm:ss with maximum of 48:00:00
#$ -l h_rt=00:30:00

#Run from current working directory and with current environment
#$ -cwd -V

#Request memory per core- default is 1GB, 2GB here
#$ -l h_vmem=2G

#Give the job a suitable name
#$ -N oxjob1

#Receive email at beginning and end of job
#$ -m be

#Now run the job
oxl.bin inputfile.ox > outputfile.txt
```
