PRJ = FFT
FIXED = true
MEM = --inlineMem
VERILOGTB = false

fpga:
	mkdir -p ../Verilog${PRJ}/fpga ; rm -f ../Verilog${PRJ}/fpga/* ;\
	make vlsi MEM=--inlineMem ;\
	mv vlsi/generated-src/*.v ../Verilog${PRJ}/fpga/. ;\
	mv sbt/generator_out.json ../Verilog${PRJ}/fpga/.

asic:
	mkdir -p ../Verilog${PRJ}/asic ; rm -f ../Verilog${PRJ}/asic/* ;\
	make vlsi MEM=--noInlineMem ;\
	mv vlsi/generated-src/*.v ../Verilog${PRJ}/asic/. ;\
	mv sbt/generator_out.json ../Verilog${PRJ}/asic/. ;\
	if [ -f vlsi/generated-src/${PRJ}.conf ]; then \
		mv vlsi/generated-src/${PRJ}.conf ../Verilog${PRJ}/asic/. ;\
	fi

vlsi:
	mkdir -p vlsi/generated-src ; make rm_gen ; cd sbt ; \
	sbt "run -params_true_false ${MEM} --genHarness --backend v --targetDir ../vlsi/generated-src "

test:
	mkdir -p test/generated-src ; make rm_gen ; cd sbt ; \
	sbt "run -params_${FIXED}_${VERILOGTB} --test --debugMem --genHarness --compile --targetDir ../test/generated-src "

debug:
	mkdir -p test/generated-src ; make rm_gen ; cd sbt ; \
	sbt "run -params_${FIXED}_${VERILOGTB} --test --debugMem --genHarness --compile --debug --vcd --targetDir ../test/generated-src " | tee console_out

rm_gen:
	rm -f vlsi/generated-src/*.v ; rm -f vlsi/generated-src/*.conf ;\
	cd sbt; rm -f constraints.xdc ; rm -f tb.v ;  rm -f *.json ;  rm -f MakeFrag

clean:
	rm -rf sbt/target sbt/project vlsi/generated-src/* test/generated-src/* sbt/console_out sbt/*.h sbt/*.cpp 
	rm -rf target sbt/ChiselCompatibility/target sbt/ChiselDSP_Modules/target sbt/ChiselDSP_Overlay/target
	rm -rf sbt/sim.start
	make rm_gen

.PHONY: vlsi test

