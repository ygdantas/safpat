-----------------------------------------------------------------
SafPat
-----------------------------------------------------------------
SafPat is a domain-specific language for enabling automated safety reasoning with safety patterns.

SafPat has been introduced by the following article submitted to ICLP 2020:

Yuri Gil Dantas, Antoaneta Kondeva, and Vivek Nigam: 
Less Manual Work for Safety Engineers: Towards an Automated Safety Reasoning with Safety Patterns

-----------------------------------------------------------------
Purpose of this README file
-----------------------------------------------------------------
This README file provides instructions on how to reproduce the results presented in the article. 

-----------------------------------------------------------------
Setup
-----------------------------------------------------------------
The instructions have been tested in a Linux operating system (Ubuntu 18.04 desktop 64-bit).

-----------------------------------------------------------------
Files
-----------------------------------------------------------------
- safety.dlv: contains our safety reasoning with safety patterns 
- dlv: Linux version of dlv  
- acc/ : Adaptive Cruise Control case study
     acc/architecture-acc-not-ctl.dlv : contains architecture of ACC for exploration mode 
     acc/architecture-acc-ctl-safmon.dlv : contains architecture of ACC with safMon, TMR and watchDog
     acc/architecture-acc-ctl-nprog.dlv : contains architecture of ACC with nProg, TMR and watchDog
- battery/ : Battery Management System case study
     battery/architecture-battery-not-ctl.dlv : contains architecture of BMS for exploration mode
     battery/architecture-battery-ctl-solution1.dlv : contains architecture of BMS with safMon and HDR(bms,ci)
     battery/architecture-battery-ctl-solution2.dlv: contains architecture of BMS with safMon and HDR(bms,fw)

-----------------------------------------------------------------
Instruction for running the ACC case study
-----------------------------------------------------------------
(0) Run the following command to explore what safety patterns are recommended to control the identified hazards

./dlv acc/architecture-acc-not-ctl.dlv safety.dlv

The command (0) will provide 5 complete solutions that controls all of the hazards. We choose 2 solutions to illustrate
here. Solution (a) is with safMon, TMR and watchDog, and solution (b) is with nProg, TMR and watchdog. To enable solution 
(a), make sure the following facts are in acc/architecture-acc-not-ctl.dlv, and the fact "explore(1,nProg)" is commented out.

 explore(1,safMon) .
 explore(2,tmr) .
 explore(1,watchDog) .

To enable solution (b), make sure the following facts are in acc/architecture-acc-not-ctl.dlv, and the fact "explore(1,safMon)" 
is commented out.

 explore(1,nProg) . 
 explore(2,tmr) .
 explore(1,watchDog) .

Run the following command to only show the predicates for the recommended safety patterns and controllability for solution (a)

./dlv acc/architecture-acc-not-ctl.dlv safety.dlv -filter=ctl -filter=nctl -filter=safMon -filter=tmr -filter=watchDog

Run the following command to only show the predicates for the recommended safety patterns and controllability for solution (b)

./dlv acc/architecture-acc-not-ctl.dlv safety.dlv -filter=ctl -filter=nctl -filter=nProg -filter=tmr -filter=watchDog

The above commands will only show solutions (i.e., architectures) that all hazards are controlled by the suggested safety
patterns. To also show solutions where where not all hazards are controlled, make sure the following line is commented
out in acc/architecture-acc-not-ctl.dlv (needs manual change)

:- nctl(H,Z,V,P).

(1) Run the following command to see the solution (a) manually added to ACC

./dlv acc/architecture-acc-ctl-safmon.dlv safety.dlv 


(2) Run the following command to see the solution (b) manually added to ACC

./dlv acc/architecture-acc-ctl-nprog.dlv safety.dlv


-----------------------------------------------------------------
Instruction for running the BMS case study
-----------------------------------------------------------------

(0) Run the following command to explore what safety patterns are recommended to control the identified hazards

./dlv battery/architecture-battery-not-ctl.dlv safety.dlv

The command (0) will provide 4 complete solutions that controls all of the hazards. These solutions are with
safMon and HDR. 

Run the following command to only show the predicates for the recommended safety patterns and controllability.

./dlv battery/architecture-battery-not-ctl.dlv safety.dlv -filter=ctl -filter=nctl -filter=safMon -filter=hdr

The above commands will only show solutions (i.e., architectures) that all hazards are controlled by the suggested safety
patterns. To also show solutions where where not all hazards are controlled, make sure the following line is commented
out in battery/architecture-battery-not-ctl.dlv (needs manual change)

:- nctl(H,Z,V,P).

(1) Run the following command to see the solution with safMon and HDR (bms,ci) manually added to BMS

./dlv battery/architecture-battery-ctl-solution1.dlv safety.dlv -filter=ctl -filter=nctl -filter=safMon -filter=hdr


(2) Run the following command to see the solution with safMon and HDR (bms,fw) manually added to BMS

./dlv battery/architecture-battery-ctl-solution2.dlv safety.dlv -filter=ctl -filter=nctl -filter=safMon -filter=hdr
