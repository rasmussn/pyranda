# Profiling Runs
Can use Intel's loopprofileviewer.sh to view `.xlm` files or look at `.dump` for
plain-text version.

## Hardware and Software Setup
Runs done with a single node with two Intel E5-2620 v4 (Broadwell) CPUs.
Giving total 16 cores and 128GB of shared memory.
Played around with array sizes and 112x112x112 was as large as I could get on
on serial version.
Used ifort 19.0.1, Python 3.5.2, MPICH 3.2.1

## Notable Results
----------------------serial runtime------------------------
./test_pent 5000 112 1 112 1 112 1
 Ellapsed time =    49.16910
------------------------mpi runtime-------------------------
mpirun -machinefile /var/spool/pbs/aux/104466.mgmt01 -n 16 ./test_pent 5000 112 2 112 4 112 2
 Ellapsed time =    4.517300


----------------------serial functions----------------------
time(abs)    time(%)  self(abs)    self(%)  call_count  exit_count  loop_ticks(%)	function                   file:line
67309498774  64.62    52918695673  50.80    5000        5000 	     0.00  les_compact_mp_eval_compact_op1x_d1_	   compact.f90:1908
103213783854 99.09    35904285080  34.47    5000        5000         0.00  les_compact_operators_mp_d1x_   compact_operators.f90:20
14390803101  13.82    14390803101  13.82    560000      560000       0.00  les_pentadiagonal_mp_bpentlus2y_   pentadiagonal.f90:690
-----------------------mpi functions-----------------------
7076484818   65.88     5152241150   47.97    5000         5000       0.00  les_compact_mp_eval_compact_op1x_d1_	compact.f90:1908
9467365692   88.14     2390880874   22.26    5000         5000       0.00  les_compact_operators_mp_d1x_   compact_operators.f90:20
1617627931   15.06     1617627931   15.06    280000       280000     0.00  les_pentadiagonal_mp_bpentlus2y_   pentadiagonal.f90:690