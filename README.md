# modkitworkflow
workflow for running modkit on OSCER. Uses singularity/apptainer to update up to glibc2.18 so that modkit can run.


Heres the path to the modkit installation, 
  
"/ourdisk/hpc/rnafold/gjandebeur/dont_archive/software/modkit_v0.5/dist_modkit_v0.5.0_5120ef7/modkit"

**In order to run on OSCER you'll need to create a container for glibc2.18 by copying the following.**
*No clue how to explain what this does but you should be able to copy directly to create the image needed to run modkit*  

    mkdir -p /scratch/$USER/apptainer_images
    apptainer pull /scratch/$USER/apptainer_images/ubuntu20.sif docker://ubuntu:20.04 
    export APPTAINER_CACHEDIR=/scratch/$USER/apptainer_cache
    export APPTAINER_TMPDIR=/scratch/$USER/apptainer_tmp 
    mkdir -p $APPTAINER_CACHEDIR $APPTAINER_TMPDIR

After this, you will bind the apptainer image that was just created in your scratch directory to the modkit download path above by doing the following, which will allow modkit to run without the glibc version mismatch. You can change the paths but /mnt/ must be first to make sure it's pulling through the apptainer.


        apptainer exec --bind /ourdisk:/mnt /scratch/gjandebeur/apptainer_images/ubuntu20.sif \ 
/mnt/hpc/rnafold/gjandebeur/dont_archive/software/modkit_v0.5/dist_modkit_v0.5.0_5120ef7/modkit \
pileup \ 
--mod-thresholds a:0.99 --mod-thresholds m:0.99 --mod-thresholds 17802:0.99 --mod-thresholds 17596:0.99 \
/mnt/hpc/rnafold/path/to/rawdata/aligned.bam \
/mnt/hpc/rnafold/path/to/rawdata/pileup.bed


*for reference, a = m6A. m = m5C. 17802 = pseU. & 17596 = inosine_m6A*
