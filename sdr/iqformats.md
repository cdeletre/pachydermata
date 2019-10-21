# IQ record conversion

### IQ record
An IQ record is a file that containts intervaled I and Q samples (In-Phase and Quadrature).

### IQ record file

The IQ record file can be RAW or WAV formatted depending on the sofware.

### I/Q sample

An I/Q sample is a numerical value that can be store in various format. Here is a non-exhaustive list:

|Format|Application|Typical file extension|
|:-:|:-:|:-:|
|Float 32 bits|GQRX|\*.cfile|
|Signed-Integer 16 bits|SDR#|\*.wav|
|Signed-Integer 8 bits|HackRF|\*.cs8|
|Unsigned-Integer 8 bits|RTL-SDR|\*.cu8|

### IQ format conversion

SoX (Sound eXchange) tool can be used to convert IQ record files.

All the examples given here use an input IQ recorded file that has a **2M** sampling rate (2 Msps).

#### cu8 to cfile conversion

	sox -t raw -r2000k -b8 -eunsigned-integer -c2 iqrecord_2M.cu8 -t raw -b32 -efloat iqrecord_2M.cfile

*Input options*:

 - `-t raw` indicates RAW format
 - `-b8` indicates that the input samples are coded on 8 bits
 - `-eunsigned-integer` indicates that the input samples must be read as unsigned integer
 - `-r2000k` indicates 2 Msps input rate (2.000k = 2.000.000 = 2M).
 - `-c2` option tells that there are 2 channels are intevalled in the input: I and Q.
 - `iqrecord_2M.cu8` the input file

*Output options:*

 - `-t raw` set RAW format
 -  `-b32` set the output samples on 32 bits
 - `-efloat` indicates that the input samples must be written as float
 - `iqrecord_2M.cfile` the output file


#### cu8 to cs8 conversion

	sox -t raw -r2000k -b8 -eunsigned-integer -c2 iqrecord_2M.cu8 -t raw -b8 -esigned-integer iqrecord_2M.cs8

*SoX input options*:

 - same as previous

*SoX output options:*

 - `-t raw` set RAW format
 -  `-b8` set the output samples on 8 bits
 - `-esigned-integer` indicates that the input samples must be written as signed integer
 - `iqrecord_2M.cs8` the output file



#### cu8 to iq wav conversion

	sox -t raw -r2000k -b8 -eunsigned-integer -c2 iqrecord_2M.cu8 -t wav -b16 -esigned-integer iqrecord_2M.wav

*SoX input options*:

 - same as previous

*SoX output options:*

 - `-t wav` set ouput as WAV format
 -  `-b16` indicates that the output samples are coded on 16 bits
 - `-esigned-integer` indicates that the input samples must be written as signed integer
 - `iqrecord_2M.wav` the output file
	
#### on-the-fly conversion

It is also possible to perform on-the-fly conversion by using *FIFO*. It can be useful to open unsupported IQ file format in GQRX without storing the converted file. The following example shows how to open a cs8 file in GQRX:

	mkfifo /tmp/iqrecord_2M.cfile
	sox -t raw -r2000k -b8 -eunsigned-integer -c2 iqrecord_2M.cu8 -t raw -b32 -efloat - | pv -r > /tmp/iqrecord_2M.cfile

*SoX input options*:

 - same as *cu8 to cfile conversion*

*SoX output options:*

 - `-t`,`-r`,`-b`,`-e` same as *cu8 to cfile conversion*
 - `-` redirects the output to stdout

*PV pipe:*

 - The `pv` command write the output from SoX to the FIFO `/tmp/iqcapture_2M.cfile`
 - `-r` option is set to display a raw data bitrate
 
`/tmp/iqrecord_2M.cfile` can then be open in GQRX as *Complex Sampled (IQ) File* with the following device string `file=/tmp/iqcapture_2M.cfile,freq=100e6,rate=2e6,repeat=false,throttle=true`. When the job is done the `/tmp/iqcapture_2M.cfile` fifo can be remove by `rm /tmp/iqcapture_2M.cfile`