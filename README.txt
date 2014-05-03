
---

Steps to start:

(1) Clone the repo
- git clone https://github.com/lmart999/FAST_CLIP

(2) Obtain and put a repeat masker file in ~/docs/
- This can be obtained as explained here: http://fantom.gsc.riken.jp/zenbu/wiki/index.php/Uploading_UCSC_repetitive_elements_track
- This is not included in the repo because the file is large.
- The code will, by default, look for a file with name: ~/docs/repeat_masker.bed
- Simply re-name your file to repeat_masker.bed or modify the code to target your file name.

(3) Obtain and put the hg19 bowtie2 index in ~/docs/hg19/*
- Again, this is not included in the repo because the files are large.
- Files can be obtained here: wget ftp://ftp.ccb.jhu.edu/pub/data/bowtie2_indexes/hg19.zip
- Format will be hg19.1.bt2, hg19.3.bt2, etc.

(4) Create a /rawdata directory within the parent (~/rawdata/)
- Move paired raw iCLIP .fastq files to /rawdata
- Un-zip and use name convention: <name>_R1.fastq, <name>_R2.fastq.

(5) Create result file directory (~/results/<name>/).
- Output files for <name>_R1/2.fastq will be sent to ~/results/<name>/

---

Dependencies:

(1) Python 2.7 (for CLIPper algorithm)
- https://www.python.org/download/releases/2.7/
(2) iPython 
- http://ipython.org/install.html
(3) iPython notebook 
- http://ipython.org/notebook
(4) Matplotlib (plotting)
- http://matplotlib.org/
(5) Pandas (data)
- http://pandas.pydata.org/
(6) Bowtie and bedTools
- http://bowtie-bio.sourceforge.net/index.shtml
- http://bedtools.readthedocs.org/en/latest/
(7) CLIPPER
- https://github.com/YeoLab/clipper/wiki/CLIPper-Home

---

Usage:

(1) Run the main pipeline:
$ ipython fast_iCLIP.py <name>
(2) The plots will, by defualt, be automatically generated.
- This can be modified in the code by commenting out runPlots()
(3) Plots and various -meta analysis are also included in the fast_iCLIP.ipynb ipython notebook.
- iPython web-based environment can be directed it to a specific port and used locally (link below).
- http://wisdomthroughknowledge.blogspot.com/2012/07/accessing-ipython-notebook-remotely.html
- For example (from the directory with my notebooks on the cluster)
(a) [lmartin@changrila CLIP]$ ipython notebook --no-browser --port=7000
(b) ssh -N -f -L localhost:8889:localhost:7000 lmartin@changrila

--- 

Debugging:
(1) Error cloning the repo
- Error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE
- Avoid checking the SSL certificate: export GIT_SSL_NO_VERIFY=true
- Source: http://stackoverflow.com/questions/3777075/ssl-certificate-rejected-trying-to-access-github-over-https-behind-firewall
- Also consider adding .git to path: https://github.com/lmart999/CLIP.git
(2) Mapping
- Ensure that bowtie2 is in the $PATH and executable.
(3) CLIPPER
- Uses Python 2.7. 
(4) Any scrip in /bin 
- The provided BedGraphToBigWig is built for Linux. 
- This, and related scripts, may be downloaded for other platforms: http://hgdownload.cse.ucsc.edu/admin/exe/macOSX.i386/
(5) Addition of a header to *mergedRT_CLIP_clusters file appears to cause a bug with parsing. Catch this error.
track name="/arrayCp1/lmartin/CLIP/results/ddx3mutv2/ddx3mutv2_allreads.mergedRT_CLIP_clusters" visibility=2 colorByStrand="0,0,0 0,0,0"
(6) If there is a problem parsing clusters from CLIPper, consider the version used.
- The older version of CLIPper results <name>_<clusterNum>_<readPerCluster>
- The newer version has name.val__<clusterNum>_<readPerCluster>
