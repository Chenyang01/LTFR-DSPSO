# LTFR-DSPSO
Continuous Dynamic Constrained Optimization with an Ensemble of Locating and Tracking Feasible Regions Strategies (LTFR-DSPSO)

[Notice]
Developed by Chenyang Bu. Email address: bucy1991@mail.ustc.edu.cn or chenyangbu@hfut.edu.cn

The codes were not developed professionally. 
The purpose of making the codes available is to allow other researchers to reproduce our reported results.

If you make use of these codes, please cite our publications below. 
1. Chenyang Bu, Wenjian Luo and Lihua Yue. Continuous Dynamic Constrained Optimization with an Ensemble of Locating and Tracking Feasible Regions Strategies. IEEE Transactions on Evolutionary Computation. vol. 21, no. 1, pp. 14-33, 2017.
2. Chenyang Bu, Wenjian Luo and Tao Zhu. Differential evolution with a species-based repair strategy for constrained optimization. Proceedings of the 2014 IEEE Congress on Evolutionary Computation (CEC 2014). IEEE, 2014, pp. 967–974.

Permission is hereby granted to use this source code for any purpose without fee. However, for non-scientific purpose, please inform us the objective for using
   "LTFR-DSPSO" before using the software:
       E-mail address: chenyangbu@hfut.edu.cn
   The reason is that we'd like to know the real applications.

[Compilation]
 We compiled this software on Windows.

System: Windows Server 2003 R2 Enterprise x64 Edition
CPU: Inter Xeon X5670 2.93GHz
Language: C++ and MATLAB mixed programming (Visual Studio 2010 + Matlab R2014b)
Algorithm: LTFR-DSPSO

Note that the GNU Scientific Library (GSL) should be installed to conduct the gradient-based repair,
and the C++ environment needs to be configured to call the dynamic link library "libMyAdd.dll", 
which was created by the MATLAB written functions. The file "libMyAdd.dll" is required because we 
employ the MATLAB function "fmincon" to conduct the local search (SQP). Thus, the hybrid programming of
MATLAB and C++ is adopted.

We created the dll "libMyAdd.dll" via Matlab2014b on a 64-bit windows, 
if an aother version of Matlab is used, 
you should recreate the dll file using your own matlab.
The following commands might be required.
"mex – setup"
"mex -setup C++"
"mbuild – setup"
"mex -setup C++ -client MBUILD"
"mex evaluateF.cpp"
"mcc -W cpplib:libMyAdd -T link:lib localSearch"

Once the dll "libMyAdd.dll" has been successfully re-created, the existing files "libMyAdd.dll", "libMyAdd.h", and 
"libMyAdd.lib" in the current directory should be overwritten by the newly created files.

The source files that created the "libMyAdd.dll" are given in the folder "dll".
Note that to reproduce the results of Tables IX - X, you should define the term "TestEffectofDynamicSeverity" in 
the file "stdafx.h", and use the codes in the directory "dll\Tables IX-X (TestEffectOfDynamicSeverity)".
It is because for Table IX and Table X, the size of each region was not changed in each run, i.e., the height and 
the width of the constraint function remains unchanged during a run.

CONFIGURATION OF C++ ENVIRONMENT is the following, 
where "$MATLAB" is the installation directory of the software matlab, and $gsl is the directory of "gsl-64" (also provided in the source files).
Project menu - > Properties - > Configuration Properties - > Linker - > General category,  add "$MATLAB\extern\lib\win64\microsoft" 
Project menu - > Properties - > Configuration Properties - > Linker - > Input - >  Additional Dependencies, add "libmx.lib;libmat.lib;libeng.lib;libmex.lib;libMyAdd.lib;mclmcrrt.lib;libgsl-0.lib;libgslcblas-0.lib;"
Configuration Properties - >  C/C++ - >  General - >  Additional Include Directories, add "$MATLAB\extern\include" and "$gsl\gsl-64\include"

Note: 
1)we created the dll via Matlab2014b on the 64-bit windows, 
if an aother version of Matlab is used, 
you should recreate the dll file (i.e., "libMyAdd.dll") using your own matlab.
The following commands might be required.
"mex – setup"
"mex -setup C++"
"mbuild – setup"
"mex -setup C++ -client MBUILD"
"mex evaluateF.cpp"
"mcc -W cpplib:libMyAdd -T link:lib localSearch"
 
2)we adopted the 64-bit windows and the 64-bit matlab; 
if you would like to run the program on a 32-bit windows, 
then the dll "libMyAdd.dll" should be re-created, 
and the 32-bit GNU Scientific Library (GSL) should be installed.

3) a matlab must be installed on your machine,
 and the installation directory of matlab (i.e., "$MATLAB") must be added into the C++ environment according to the details given above.

4) you might need to install the latest version of Matlab, because early versions might not compatible with the latest version of Visual Studio. 

5) the current version of LTFR-DSPSO waste much time on calling the dll, because of the mixed programming.
In the future, we will adopt C++ to implement the procedure of SQP to save computing time.

6) the results might be sightly different if the codes are conducted on a different 
machine, because of the precision problem of floating point calculation in conducting the local search operator.


[Usage]
You could test different combinations of the proposed strategies via defining or undefining
a particular term in the file "stdafx.h". For example, if you would like to use the strategy of
tracking current feasible regions, the term "UseTrackingCurrentFeasibleRegionsStrategy"
should be defined.
The settings of the tested combinations in the paper are given below.
1) DSPSO:
#define DSPSO
#define originalDSPSO 
#define UseSortingRuleOfOriginalSPSO 
2) DSPSO-1: 
#define DSPSO
#define originalDSPSO
#undef UseSortingRuleOfOriginalSPSO
3) DSPSO-2:
#undef DSPSO
+ undefine all the strategies
4) TFR1-DSPSO:
#undef DSPSO
#define UseTrackingCurrentFeasibleRegionsStrategy
+ undefine the other strategies
5) TFR2-DSPSO:
#undef DSPSO
#define UseTrackingPreviouslyAppearedFeasibleRegionsStrategy
+ undefine the other strategies
6) TFR3-DSPSO:
#undef DSPSO
#define UsePredictingFutureFeasibleRegionsStrategy
+ undefine the other strategies
7) LFR-DSPSO:
#undef DSPSO
#define UseLocatingFeasibleRegionsStrategy
+ undefine the other strategies
8) LFR1-DSPSO:
#undef DSPSO
#define UseLocatingFeasibleRegionsStrategy
#define UseTrackingCurrentFeasibleRegionsStrategy
+ undefine the other strategies
9) LFR2-DSPSO:
#undef DSPSO
#define UseLocatingFeasibleRegionsStrategy
#define UseTrackingPreviouslyAppearedFeasibleRegionsStrategy
+ undefine the other strategies
10) LFR3-DSPSO:
#undef DSPSO
#define UseLocatingFeasibleRegionsStrategy
#define UsePredictingFutureFeasibleRegionsStrategy
+ undefine the other strategies
11) LTFR-DSPSO:
#undef DSPSO
#define UseTrackingCurrentFeasibleRegionsStrategy
#define UseLocatingFeasibleRegionsStrategy
#define UseTrackingPreviouslyAppearedFeasibleRegionsStrategy
#define UsePredictingFutureFeasibleRegionsStrategy 
#define UseLS 
#define UseRepair
#define UseChangeDetectionStrategy

If you would like to test the benchmark G24 (proposed by Nguyen and Yao), 
the term "G24" in the file "stdafx.h" should be defined;
if you would like to test MFRB-R, then undefine "G24" and undefine "MFRB_C";
if you would like to test MFRB-C, then undefine "G24" and define "MFRB_C".

Note:
For the Table IX and Table X of the paper "Continuous Dynamic Constrained Optimization with an Ensemble of Locating and Tracking Feasible Regions Strategies",
the size of each region was not changed in each run. Thus, the term "TestEffectofDynamicSeverity" should be defined in the file "stdafx.h".
Moreover, you should use the files in the director "dll\Tables IX-X (TestEffectOfDynamicSeverity)".
In other words, the problem settings in the matlab (e.g. in the file "evaluateF.cpp") should be consistent with the settings of the C++ files.

Problem ID
(format: "problem name" + "problem ID")
G24_u	1
G24_1	2
G24_f	3
G24_uf	4
G24_2	5
G24_2u	6
G24_3	7
G24_3b	8
G24_3f	9
G24_4	10
G24_5	11
G24_6a	12
G24_6b	13
G24_6c	14
G24_7	16
G24v_3	3001
G24w_3	3003
G24v_3b	8001
G24w_3b	8003
MFRB-C 10000
MFRB-R 10001


[ACKNOWLEDGMENT]
We take advantage of the source codes of SPSO (available: "http://goanna.cs.rmit.edu.au/~xiaodong/publications.php"),
eDEag (avaiable: "http://www.ints.info.hiroshima-cu.ac.jp/~takahama/eng/index.html"),
and EAlib (also called OFEC, avaiable:"http://cs.cug.edu.cn/teacherweb/lichanghe/pages/EAlib.html" or "http://www.ntu.edu.sg/home/EPNSugan/").
The authors would like to thank Dr. T. T. Nguyen and Dr. H. K. Singh for their generous help.
