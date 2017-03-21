### 01. Clone repository and enter folder

 ```bash
git clone https://github.com/sauloal/JBrowseAutomaticInstanceCreator.git <INSTANCE NAME>

cd <INSTANCE NAME>
 ```

### 02. copy ```config.example``` to ```config```
 ```bash
NAME=jbrowse_automatic_instance_creator
VER=1.12.1
IMG=jbrowse/gmod-jbrowse:${VER}
INDEX_FILES=0
PORT=8006
```

**```NAME```**        = Docker instance name - change if more than one instance to be run

**```VER```**         = JBrowse version (especifically, tag name of docker image)

**```IMG```**         = JBrowse Docker image name

**```INDEX_FILES```** = Run ```generate-names.pl``` to index gene names, allowing for search

**```PORT```**        = Port to run

### 03. create and edit ```data/<PROJECT_NAME>.cfg```

 ```bash
REF=<REFERENCE FASTA NAME>
DB_NAME=<PROJECT_NAME>
```

### 04. create ```data/users.conf``` (an example can be found at ```data/users.conf.example```) to enable access protection with usernames and passwords 

 ``` bash
USERS=(user1 user2 user3)
UPASS=(pass1 pass2 pass3)
```

### 05. create a project folder as defined in the ```.cfg``` file

 ```mkdir data/<PROJECT_NAME>```

### 06. inside the ```data/<PROJECT_NAME>``` folder create folders:

#### ref
 containing the fasta file as defined in ```data/<PROJECT_NAME>.cfg```

  e.g.: ```data/<PROJECT_NAME>/ref/<REFERENCE FASTA NAME>```

#### bam
 containing bam and their index (.bam.bai) files (valid extensions: *.bam)

  e.g.: 
 ```bash
data/\<PROJECT_NAME\>/bam/data1.bam
data/\<PROJECT_NAME\>/bam/data1.bam.bai
 ```

#### gff
 containing GFF files (valid extensions: *.gff *.gff3 *.gff.gz *.gff3.gz)

  e.g.: 
 ```
data/\<PROJECT_NAME\>/gff/data1.gff
```

#### vcf
 VCF files and their index (.vcf.tbi or .vcf.gz.tbi) files (valid extensions: *.vcf  *.vcf.gz)

  e.g.:
 ```
data/\<PROJECT_NAME\>/vcf/data1.vcf
data/\<PROJECT_NAME\>/vcf/data1.vcf.tbi
data/\<PROJECT_NAME\>/vcf/data2.vcf.gz
data/\<PROJECT_NAME\>/vcf/data1.vcf.gz.tbi
```

  **NOTE: you can also create subfolders. they will appear as sub classes**
  
  e.g.: 
  
  ``` gff/data1 ``` - creating the category ``` Annotation ``` and track ``` data1 ```


  ``` gff/new/data1 ``` - creating the category ``` Annotation - new``` and track ``` data1 ```


  ``` gff/assembly/data1 ``` - creating the category ``` Annotation - assembly``` and track ``` data1 ```


### 07. optionally, create a ```.nfo``` file for each data file containing extra parameters for the track.

  e.g.: for ```data1.vcf.gz``` create ```data1.vcf.gz.nfo```

 **NOTE: parameters in the NFO get hardcoded in the JSON created by flatfile-to-json.pl, therefore, after changes in NFO files linked to GFF files, the respective _data folder should be deleted**
 
 e.g.: for ```data1.gff``` create a ```data1.gff.nfo```, then delete the existing ```data1.gff_data``` folder so that changes take effect

  Available variables to be set are:
 ```
    LBL           = Pretty name for track
    EXTRA         = Extra parameters to be added to the CONF configuration file
                    GFF and Ref uses JSON
                    BAM and VCF uses CONF file
                    e.g.: EXTRA="style.color = red"
    EXTRAJ        = Extra parameters to be added to the JSON configuration file
                    this will be added to a JSON string so it should start with a ","
                    e.g.: EXTRAJ=", \"key_i_want\"= [0,1,2,3]"
    GFF_TYPE      = GFF type for filtering
                    added as the parameter --type to flatfile-to-json.pl 
                    e.g.: GFF_TYPE="gene"
```

  Available variables to be used are:
 ```
    FILENAME      = current track file name
    BN            = basename of the current file
    DN            = subfolder name of the current file
    CAT           = category to be put under
    TYPE_DIR      = file type folder (bam, vcf, gff, ref)
    CANVAS_TYPE   = type of canvas to be plotted
    AS_JSON       = whether the config is .json or .conf
    INDEX_EXT     = extension of index file
    CLS           = class
    KEY_EXT       = key extension
    STORE_CLASS   = store class
    CATEGORY      = category name

    JBROWSE       = JBrowse path
    R_FOLDER      = project name
    R_FOLDER_REF  = reference folder
    R_FOLDER_BAM  = bam folder
    R_FOLDER_GFF  = gff folder
    R_FOLDER_VCF  = vcf folder
    R_FOLDER_JSON = output json folder
    R_JSON        = output json file
    R_FASTA       = reference fasta file
    TRACKS        = tracks.conf path
    TRACKLIST     = trackList.json path
    DATA_DIR      = path containing data
    J_CONF        = global conf file path

    GFFS          = GFF files found
    VCFS          = VCF files found
    BAMS          = BAM files found
 ```

  e.g.:
 ```bash
LBL="Miraculous sample 1"
EXTRA="style.color = red"
```


### 08. The folder structure should look like this:
 ```bash
# base system files
<INSTANCE NAME>/config
<INSTANCE NAME>/edit.sh
<INSTANCE NAME>/fix_perm.sh
<INSTANCE NAME>/run.sh
<INSTANCE NAME>/LICENSE.md
<INSTANCE NAME>/README.md

# track scripts
<INSTANCE NAME>/data/config.sh
<INSTANCE NAME>/data/crypt.pl
<INSTANCE NAME>/data/gen_reg.scr
<INSTANCE NAME>/data/run.scr
<INSTANCE NAME>/data/secret.conf
<INSTANCE NAME>/data/users.conf

#project files
<INSTANCE NAME>/data/<PROJECT_NAME>.cfg
<INSTANCE NAME>/data/<PROJECT_NAME>/
<INSTANCE NAME>/data/<PROJECT_NAME>/ref/<REFERENCE>.fa
<INSTANCE NAME>/data/<PROJECT_NAME>/bam/
<INSTANCE NAME>/data/<PROJECT_NAME>/bam/data1.bam
<INSTANCE NAME>/data/<PROJECT_NAME>/bam/data1.bam.bai
<INSTANCE NAME>/data/<PROJECT_NAME>/bam/data1.bam.nfo
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/annotation/data1.gff
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/annotation/data1.gff.nfo
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/annotation/data1.gff_data/
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/annotation/data1.gff3
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/annotation/data1.gff3.nfo
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/annotation/data1.gff3_data/
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/assembly/data1.gff
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/assembly/data1.gff.nfo
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/assembly/data1.gff_data/
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/assembly/data1.gff3
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/assembly/data1.gff3.nfo
<INSTANCE NAME>/data/<PROJECT_NAME>/gff/assembly/data1.gff3_data/
<INSTANCE NAME>/data/<PROJECT_NAME>/vcf/
<INSTANCE NAME>/data/<PROJECT_NAME>/vcf/data1.vcf
<INSTANCE NAME>/data/<PROJECT_NAME>/vcf/data1.vcf.tbi
<INSTANCE NAME>/data/<PROJECT_NAME>/vcf/data1.vcf.nfo
```

**NOTE: the ```_data``` folders accompaining the GFF files are automatically generated by ```flatfile-to-json.pl``` after the first run*** 

### 09. run
 ``` ./run.sh ```

This will:
* first time
  * index and convert the reference fasta
  * index and convert the gff files (creating _data folders)
* every time
  * symlink the indexed fasta data
  * symlink the indexed gff data
  * index tracks and features (if ```INDEX_FILES``` is set to 1 in the config file)
  * create .conf file for JBrowse with all the data found

### 10. open your browser at
 ``` http://<your ip address>:<port defined in config>```

 e.g.: ```http://127.0.0.1:8006```
