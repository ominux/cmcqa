
#
#	Example Makefile to run tests and check results.
#
#  Rel  Date            Who             Comments
# ====  ==========      =============   ========
#  1.5  06/10/08        Geoffery Coram  Common Target for all simulators
#  1.0  04/13/06        Colin McAndrew  Initial version
#
#	This is an example makefile for running QA tests on a
#	model and then checking the simulated results against
#	reference results. A separate target is defined for each
#	variant of the model. The program runQaTests.pl runs the
#	tests, and that program expects a perl module SIMULATOR.pm
#	to be provided for each simulator that is tested.
#	Examples of these are provided.
#

qaSpecFile		=	qaSpec
qaResultsDirectory	=	results
testProgramName		=	../../../../lib/runQaTests.pl
#testProgramName		=	runQaTests.pl
testProgramFlags        =       -nw
#testProgramFlags        =       -d

help:
	@echo "" ; \
	echo "Valid targets are:" ; \
	echo "" ; \
	echo "all                run tests and compare results for all simulators" ; \
	echo "" ; \
	echo "spice              run tests and compare results spice" ; \
	echo "hspice             run tests and compare results hspice" ; \
	echo "spectre            run tests and compare results spectre" ; \
	echo "ads                run tests and compare results ads" ; \
	echo "eldo               run tests and compare results eldo" ; \
	echo "nexxim             run tests and compare results nexxim" ; \
	echo "smartspice         run tests and compare results smartspice" ; \
	echo "" ; \
	echo "clean              remove all previously generated simulation results"; \
	echo "" ; \
	echo "NOTE: if test results exist they are not resimulated" ; \
	echo "NOTE: to force resimulation run \"make clean\" first" ; \
	echo ""

all:	spice hspice spectre ads eldo nexxim smartspice

#####
##### common target for all simulators
#####

spice hspice spectre ads eldo nexxim smartspice %:
	@-echo ""; \
	localPlatform=`$(testProgramName) -platform` ; \
        localVersion=`$(testProgramName) -sv -s $@ $(qaSpecFile) | awk '{print $$1}'` ; \
	localVersionAndPlatform=$$localVersion._.$$localPlatform ; \
	echo "******"; \
	echo "****** $(qaSpecFile) tests for $@"; \
	echo "****** (for version $$localVersion on platform $$localPlatform)"; \
	echo "******"; \
	for test in `$(testProgramName) -lt -s $@ $(qaSpecFile)` ; \
	do \
	    echo ""; \
	    echo "****** Checking test ($@): $$test" ; \
	    for variant in `$(testProgramName) -lv -s $@ $(qaSpecFile)` ; \
	    do \
	        $(testProgramName) $(testProgramFlags) -s $@ -r -t $$test -var $$variant $(qaSpecFile) ; \
	    done ; \
	done ; \
	for version in `ls -C1 $(qaResultsDirectory)/$@` ; \
	do \
	    for platform in `ls -C1 $(qaResultsDirectory)/$@/$$version` ; \
	    do \
	        versionAndPlatform=$$version._.$$platform ; \
	        if [ $$versionAndPlatform = $$localVersionAndPlatform ] ; \
	        then \
	            break ; \
	        fi ; \
	        echo "" ; \
	        echo "******"; \
	        echo "****** Comparing previously run $(qaSpecFile) tests for $@"; \
	        echo "****** (for version $$version on platform $$platform)"; \
	        echo "******"; \
	        for test in `$(testProgramName) -lt -s $@ $(qaSpecFile)` ; \
	        do \
	            echo ""; \
	            echo "****** Checking test ($@): $$test" ; \
	            for variant in `$(testProgramName) -lv -s $@ $(qaSpecFile)` ; \
	            do \
	                $(testProgramName) -c $$version $$platform -s $@ -t $$test -var $$variant $(qaSpecFile) ; \
	            done ; \
	        done ; \
	    done ; \
	done

clean:
	@/bin/rm -rf $(qaResultsDirectory)/spice spiceCkt* b3v3check.log
	@/bin/rm -rf $(qaResultsDirectory)/hspice hspiceCkt*
	@/bin/rm -rf $(qaResultsDirectory)/spectre spectreCkt* *ahdlcmi
	@/bin/rm -rf $(qaResultsDirectory)/ads adsCkt* spectra.raw
	@/bin/rm -rf $(qaResultsDirectory)/eldo eldoCkt*
	@/bin/rm -rf $(qaResultsDirectory)/nexxim *.sdf *.csv *.csv.*
	@/bin/rm -rf $(qaResultsDirectory)/smartspice smartspiceCkt*
