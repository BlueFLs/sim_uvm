rtl_list = ../filelist/rtl_list
ip_list  = ../filelist/ip_list
sim_list = ../filelist/sim_list

#incdir  += 

TESTNAME = consistence_test 
SEED     = 2

vlogan_uvm += -full64 -sverilog -kdb 
vlogan_uvm += -ntb_opts uvm-1.2 -nc -q 
vlogan_uvm += -work worklib 

vlogan_opt += -full64 -sverilog +v2k -kdb 
vlogan_opt += -timescale=1ns/1ps
vlogan_opt += +define+SIM 
#vlogan_opt += +define+xxx=
vlogan_opt += -ntb_opts uvm-1.2 -nc -q
vlogan_opt += -f ${rtl_list} -f ${ip_list} -f ${sim_list}
vlogan_opt += -work worklib

vcs_opt    += -full64 -sverilog -lca -kdb 
vcs_opt    += -debug_access+all
vcs_opt    += -timescale=1ns/1ps
vcs_opt    += -ntb_opts uvm-1.2 -nc -q 
vcs_opt    += -cm line+cond+fsm+branch+tgl -cm_hier cm.cfg
vcs_opt    += worklib.tb

simv_opt   += -l run.log
simv_opt   += -sml -cm line+cond+fsm+branch+tgl
simv_opt   += -cm_name tb_$(SEED)
simv_opt   += +ntb_random_seed=$(SEED)
simv_opt   += UVM_TAESTNAME=$(TESTNAME)

make:config analysis0 analysis1 vcs 

config:
	makedir -p worklib 

analysis0:
	vlogan $(vlogan_uvm)

analysis1:
	vlogan $(vlogan_opt)

vcs:
	vcs $(vcs_opt)

sim:
	./simv $(sim_opt)

verdi:
	verdi -uvmDebug -ssf tb.fsdb -dbdir ./simv.daidir +UVM_TAESTNAME=$(TESTNAME) -nologo

editcov:
	urg -full64 -format both -dir simv.vdb 
	#dve -cov -dir simv.vdb
	verdi -cov -covdir simv.vdb

viewcov:
	urg -full64 -format both -dir simv.vdb
	firefox urgReport/dashboard.html 

clean:
	rm -rf *fsdb* csrc novas* verdiLog ucli.key simv* worklib *.h urgReport vdCovLog *.log* DVEfiles *vpd*

