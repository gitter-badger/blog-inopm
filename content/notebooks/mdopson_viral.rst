==========================================
M Dopson Viral Metagenomes Project
==========================================
:date: 2014-05-31 00:00
:summary: Viral metagenomes assemblies
:start_date: 2014-03-21
:end_date: 2014-05-31

Performing assemblies, mappings and some analysis for viral metagenomes. It
uses a lot of the `metassemble`_ scripts.


Google docs
===========
`Assembly stats`_


Assemblies
============
Performed assemblies with Ray on `Lindgren`_ over kmers 31 to 81 with a stepsize of 10. First copied the files::
    
    rsync -va xxxx@xxxx.xxxx.xxx:/proj/b2013127/INBOX/M.Dopson_13_05/ .

Directory structure like that::
    
    $ ls
    assemblies
    P911_101
    P911_102
    P911_103
    P911_104
    P911_105
    P911_106

First did assemblies for P911_101, P911_102 and P911_106::
    
    cd assemlies
    for d in ../P911_10{1,2,6}; do
        mkdir -p ${d##.*/} && cd ${d##.*/}
        for k in `seq 31 10 81`; do
            qsub  -V -d `pwd` -o ray-$k-pbs.out -l procs=2048,walltime=24:00:00 \
                -v QSUB_ARGUMENTS="aprun -n 2048 Ray -k $k -o out_$k -p ../$d/*/4_*.fastq.gz -p ../$d/*/7_*.fastq.gz" \
                /cfs/klemming/nobackup/i/inodb/github/metassemble/bin/wrapper_jobscript.pbs
        done
        cd ..
    done

It uses two libraries, since those were resequenced. The wrapper_jobscript
executes whatever is in the ``QSUB_ARGUMENTS`` variable. Similarly for
P911_103, P911_104, P911_105, but with one library::

    for d in ../P911_10{3,4,5}; do
        reads=( $d/*/5_*.fastq.gz )
        mkdir -p ${d##.*/} && cd ${d##.*/}
        for k in `seq 31 10 81`; do
            qsub  -V -d `pwd` -o ray-$k-pbs.out -l procs=2048,walltime=24:00:00 \
            -v QSUB_ARGUMENTS="aprun -n 2048 Ray -k $k -o out_$k -p ../$d/*/5_*.fastq.gz" \
            /cfs/klemming/nobackup/i/inodb/github/metassemble/bin/wrapper_jobscript.pbs
        done
        cd ..
    done

Copied the files back to the original server::

    rsync -va assemblies xxxx@xxx:/proj/b2013127/nobackup/projects/M.Dopson_13_05/

Then combined the different kmer assemblies using Newbler::
    
    cd /proj/b2013127/nobackup/projects/M.Dopson_13_05/assemblies
    for dir in P911_{101,102,103,104,105,106}; do
        cd $dir
        sbatch --output=newbler-slurm.out -J merge-dopson -A b2013127 -t 2-00:00:00 -p core -n 8 \
            ~/bin/sbatch_job bash -x $METASSEMBLE_DIR/scripts/assembly/merge-asm-newbler.sh newbler \ 
            out_*/Contigs.fasta
        cd ..
    done

Which results in the following assemblies::

    $ ls */newbler/454AllContigs.fna
    P911_101/newbler/454AllContigs.fna  P911_103/newbler/454AllContigs.fna  P911_105/newbler/454AllContigs.fna
    P911_102/newbler/454AllContigs.fna  P911_104/newbler/454AllContigs.fna  P911_106/newbler/454AllContigs.fna

Mapping
======================

After the assemblies all reads were mapped back against every merged assembly::

    cd assemblies
    d=`pwd`
    for p in P911_10{1,2,3,4,5,6}; do
        mkdir -p $p/newbler/map
        cd $p/newbler/map
        for s in /proj/b2013127/INBOX/M.Dopson_13_05/P911_*/*/*_1.fastq.gz; do
            mkdir -p ${s##*/}
            cd ${s##*/}
            ls bowtie2/asm_pair-smds.coverage || \
                sbatch -A b2013127 -t 01-00:00:00 -J mdopson-map-$p -p core -n 4 ~/bin/sbatch_job \
                bash $METASSEMBLE_DIR/scripts/map/map-bowtie2-markduplicates.sh -ct 4 $s ${s/_1.fastq/_2.fastq} \
                pair ../454AllContigs.fna asm bowtie2
            cd ..
        done
        cd $d
    done

.. _Lindgren: https://www.pdc.kth.se/resources/computers/lindgren
.. _metassemble: https://github.com/inodb/metassemble
.. _Assembly stats: https://docs.google.com/spreadsheet/ccc?key=0Ammr7cdGTJzgdG4tb2tfMGpsX1UxeWlYX0pEaFQ5RGc&usp=drive_web#gid=0