# Example INTERMAGNET seedlink process

The following uses IRIS slarchive and ringserver with no modification.

The slarchive is the suggested application at the receiver end (BGS) which will connect to all GINs hosting their own ringserver or an equivalent seedlink compatible server.  A single slarchive instance connects to one ringserver therefore multiple slarchive instance for each GIN will be required.

The docker-compose.yml briefly illustrates that connection between a single slarchive to ringserver.

## RINGSERVER

The Dockerfile shows the installation instruction for setting up the ringserver.  The GIN must open port 18000 for the slarchive instance to connect to it.  That property can be changed from within the ring.conf file "ListenPort".

The sample ring.conf has one key property "MSEEDSCAN".  This allows the ringserver to listen for new files in the specified directory and send its miniseed content to the local ringserver.

## SLARCHIVE

The Dockerfile shows the installation instruction for setting up a single slarchive connection.  The key parameter in the ENTRYPOINT is "-SDS" which is the location where the received miniseed data will be stored (in the SDS archive format).  The other component is the selector "-s" which defines to only grab magnetometer data from the remote ringserver.  "F" is the reservered miniseed code for magnetometer instruments.  See [https://github.com/INTERMAGNET/miniseed-sncl/blob/master/SNCL.md] for more information.

## Installation

The docker-compose is mostly used for an example.  It requires a base directory under /tmp for testing sending and receiving.

```bash
# Create the folder that will be used by the ringserver MSEEDSCAN
mkdir -p /tmp/intermagnet/ringserver
# Create the folder for the archive
mkdir -p /tmp/intermagnet/slarchive
```

The Dockerfiles uses a default UID (1000) for the process.  Change it as you wish.  The folders created must have read/write access to the owner.  Alternatively, you can make the folder world read/write.

Once setup, simply execute the docker containers.

```bash
docker-compose up
```

For testing, you can copy the samples/ file(s) under the ringserver which is the MSEEDSCAN folder.  The slarchive folder created should have that data in the SDS archive format.
