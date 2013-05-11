###############################################################################
## Makefile for fra-eng
###############################################################################

VERSION=0.1.0
LANG1=fra
LANG2=eng
PREFIX1=$(LANG1)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)

BASENAME=apertium-$(PREFIX1)

TARGETS_COMMON = $(PREFIX1).autobil.bin $(PREFIX2).autobil.bin \
	$(PREFIX1).automorf.hfst $(PREFIX1).autogen.hfst $(PREFIX1).t1x.bin  \
	$(PREFIX2).automorf.hfst $(PREFIX2).autogen.hfst $(PREFIX2).t1x.bin  \
        $(PREFIX1).t2x.bin $(PREFIX1).t3x.bin \
        $(PREFIX2).t2x.bin $(PREFIX2).t3x.bin \
        $(PREFIX1).rlx.bin $(PREFIX2).rlx.bin \
        $(PREFIX1).autolex.bin $(PREFIX2).autolex.bin \
	$(PREFIX1).mode $(PREFIX2).mode modes 

###############################################################################
## fra transducer
###############################################################################

.deps/$(LANG1).twol.hfst: $(BASENAME).$(LANG1).twol
	if [ ! -d .deps ]; then mkdir .deps; fi
	hfst-twolc $< -o $@
 
.deps/$(LANG1).RL.lexc: $(BASENAME).$(LANG1).lexc
	if [ ! -d .deps ]; then mkdir .deps; fi
	cat $< | grep -v 'Dir/LR' > $@
 
.deps/$(LANG1).LR.lexc: $(BASENAME).$(LANG1).lexc
	if [ ! -d .deps ]; then mkdir .deps; fi
	cat $< | grep -v 'Dir/RL' > $@

.deps/$(LANG1).RL.lexc.hfst: .deps/$(LANG1).RL.lexc
	hfst-lexc --format foma $< -o $@

.deps/$(LANG1).LR.lexc.hfst: .deps/$(LANG1).LR.lexc
	hfst-lexc --format foma $< -o $@

.deps/$(LANG1).RL.hfst: .deps/$(LANG1).RL.lexc.hfst .deps/$(LANG1).twol.hfst
	hfst-compose-intersect -1 .deps/$(LANG1).RL.lexc.hfst -2 .deps/$(LANG1).twol.hfst -o $@

.deps/$(LANG1).LR.hfst: .deps/$(LANG1).LR.lexc.hfst .deps/$(LANG1).twol.hfst
	hfst-compose-intersect -1 .deps/$(LANG1).LR.lexc.hfst -2 .deps/$(LANG1).twol.hfst -o $@

$(PREFIX2).autogen.hfst: .deps/$(LANG1).RL.hfst
	hfst-fst2fst -O $< -o $@

$(PREFIX1).automorf.hfst: .deps/$(LANG1).LR.hfst
	hfst-invert $< | hfst-fst2fst -O -o $@

###############################################################################
## eng transducer
###############################################################################

.deps/$(LANG2).twol.hfst: $(BASENAME).$(LANG2).twol
	if [ ! -d .deps ]; then mkdir .deps; fi
	hfst-twolc $< -o $@
 
.deps/$(LANG2).RL.lexc: $(BASENAME).$(LANG2).lexc
	if [ ! -d .deps ]; then mkdir .deps; fi
	cat $< | grep -v 'Dir/LR' > $@
 
.deps/$(LANG2).LR.lexc: $(BASENAME).$(LANG2).lexc
	if [ ! -d .deps ]; then mkdir .deps; fi
	cat $< | grep -v 'Dir/RL' > $@

.deps/$(LANG2).RL.lexc.hfst: .deps/$(LANG2).RL.lexc
	hfst-lexc --format foma $< -o $@

.deps/$(LANG2).LR.lexc.hfst: .deps/$(LANG2).LR.lexc
	hfst-lexc --format foma $< -o $@

.deps/$(LANG2).RL.hfst: .deps/$(LANG2).RL.lexc.hfst .deps/$(LANG2).twol.hfst
	hfst-compose-intersect -1 .deps/$(LANG2).RL.lexc.hfst -2 .deps/$(LANG2).twol.hfst -o $@

.deps/$(LANG2).LR.hfst: .deps/$(LANG2).LR.lexc.hfst .deps/$(LANG2).twol.hfst
	hfst-compose-intersect -1 .deps/$(LANG2).LR.lexc.hfst -2 .deps/$(LANG2).twol.hfst -o $@

$(PREFIX1).autogen.hfst: .deps/$(LANG2).RL.hfst
	hfst-fst2fst -O $< -o $@

$(PREFIX2).automorf.hfst: .deps/$(LANG2).LR.hfst
	hfst-invert $< | hfst-fst2fst -O -o $@

###############################################################################
## Debugging transducers (for testvoc)
###############################################################################

.deps/$(LANG1).LR-debug.hfst: $(BASENAME).$(LANG1).lexc
	if [ ! -d .deps ]; then mkdir .deps; fi
	cat $< | grep -v 'Dir/RL' | grep -v 'Use/Circ' > .deps/$(LANG1).LR-debug.lexc
	hfst-lexc --format foma .deps/$(LANG1).LR-debug.lexc -o .deps/$(LANG1).LR-debug.lexc.hfst
	hfst-compose-intersect -1 .deps/$(LANG1).LR-debug.lexc.hfst -2 .deps/$(LANG1).twol.hfst -o $@

.deps/$(LANG2).LR-debug.hfst: $(BASENAME).$(LANG2).lexc
	if [ ! -d .deps ]; then mkdir .deps; fi
	cat $< | grep -v 'Dir/RL' | grep -v 'Use/Circ' > .deps/$(LANG2).LR-debug.lexc
	hfst-lexc --format foma .deps/$(LANG2).LR-debug.lexc -o .deps/$(LANG2).LR-debug.lexc.hfst
	hfst-compose-intersect -1 .deps/$(LANG2).LR-debug.lexc.hfst -2 .deps/$(LANG2).twol.hfst -o $@

###############################################################################
## Bilingual dictionaries
###############################################################################

$(PREFIX1).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@

$(PREFIX2).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $< 
	lt-comp rl $< $@

###############################################################################
## Disambiguation rules 
###############################################################################

$(PREFIX1).rlx.bin: $(BASENAME).$(PREFIX1).rlx
	cg-comp $< $@

$(PREFIX2).rlx.bin: $(BASENAME).$(PREFIX2).rlx
	cg-comp $< $@


###############################################################################
## Lexical selection rules
###############################################################################

$(PREFIX1).autolex.bin: $(BASENAME).$(PREFIX1).lrx
	lrx-comp $< $@

$(PREFIX2).autolex.bin: $(BASENAME).$(PREFIX2).lrx
	lrx-comp $< $@

###############################################################################
## Structural transfer rules
###############################################################################

$(PREFIX1).t1x.bin: $(BASENAME).$(PREFIX1).t1x
	apertium-validate-transfer $(BASENAME).$(PREFIX1).t1x
	apertium-preprocess-transfer $(BASENAME).$(PREFIX1).t1x $@

$(PREFIX1).t2x.bin: $(BASENAME).$(PREFIX1).t2x
	apertium-validate-interchunk $(BASENAME).$(PREFIX1).t2x
	apertium-preprocess-transfer $(BASENAME).$(PREFIX1).t2x $@

$(PREFIX1).t3x.bin: $(BASENAME).$(PREFIX1).t3x
	apertium-validate-postchunk $(BASENAME).$(PREFIX1).t3x
	apertium-preprocess-transfer $(BASENAME).$(PREFIX1).t3x $@

$(PREFIX2).t1x.bin: $(BASENAME).$(PREFIX2).t1x
	apertium-validate-transfer $(BASENAME).$(PREFIX2).t1x
	apertium-preprocess-transfer $(BASENAME).$(PREFIX2).t1x $@

$(PREFIX2).t2x.bin: $(BASENAME).$(PREFIX2).t2x
	apertium-validate-interchunk $(BASENAME).$(PREFIX2).t2x
	apertium-preprocess-transfer $(BASENAME).$(PREFIX2).t2x $@

$(PREFIX2).t3x.bin: $(BASENAME).$(PREFIX2).t3x
	apertium-validate-postchunk $(BASENAME).$(PREFIX2).t3x
	apertium-preprocess-transfer $(BASENAME).$(PREFIX2).t3x $@


###############################################################################
## Postgeneration dictionaries
###############################################################################

#$(PREFIX1).autopgen.bin: $(BASENAME).post-$(LANG1).dix
#	apertium-validate-dictionary $<
#	lt-comp lr $< $@
#
#$(PREFIX2).autopgen.bin: $(BASENAME).post-$(LANG2).dix
#	apertium-validate-dictionary $< 
#	lt-comp lr $< $@

###############################################################################
## Translation modes
###############################################################################

modes/$(PREFIX1).mode: modes.xml
	apertium-validate-modes modes.xml
	apertium-gen-modes modes.xml
	cp *.mode modes/

modes/$(PREFIX2).mode: modes.xml
	apertium-validate-modes modes.xml
	apertium-gen-modes modes.xml
	cp *.mode modes/

###############################################################################
## Installation stuff
###############################################################################

debug: .deps/$(LANG1).LR-debug.hfst .deps/$(LANG2).LR-debug.hfst

EXTRA_DIST=$(PREFIX1).prob $(PREFIX2).prob \
           $(BASENAME).$(PREFIX1).t1x $(BASENAME).$(PREFIX1).t2x $(BASENAME).$(PREFIX1).t3x \
           $(BASENAME).$(PREFIX2).t1x $(BASENAME).$(PREFIX2).t2x $(BASENAME).$(PREFIX2).t3x \
           modes.xml

apertium_fra_engmodesdir=$(prefix)/share/apertium/modes/
apertium_fra_engdir=$(prefix)/share/apertium/$(BASENAME)/

apertium_fra_eng_DATA=$(PREFIX1).autobil.bin $(PREFIX2).autobil.bin \
                    $(PREFIX1).prob $(PREFIX2).prob \
		    $(PREFIX1).autogen.hfst $(PREFIX2).autogen.hfst \
		    $(PREFIX1).automorf.hfst $(PREFIX2).automorf.hfst \
		    $(PREFIX1).t1x.bin $(PREFIX2).t1x.bin \
		    $(PREFIX1).t2x.bin $(PREFIX2).t2x.bin \
		    $(PREFIX1).t3x.bin $(PREFIX2).t3x.bin \
		    $(PREFIX1).rlx.bin $(PREFIX2).rlx.bin \
		    $(PREFIX1).autolex.bin $(PREFIX2).autolex.bin \
		    $(BASENAME).$(PREFIX1).t1x $(BASENAME).$(PREFIX2).t1x \
		    modes.xml

#		    $(PREFIX1).autopgen.bin $(PREFIX2).autopgen.bin 

noinst_DATA=modes/$(PREFIX1).mode modes/$(PREFIX2).mode


install-data-local:
	mv modes modes~
	$(prefix)/bin/apertium-gen-modes modes.xml $(BASENAME)
	rm -rf modes
	mv modes~ modes
	test -d $(DESTDIR)$(apertium_fra_engmodesdir) || mkdir $(DESTDIR)$(apertium_fra_engmodesdir)
	$(INSTALL_DATA) $(PREFIX1).mode $(DESTDIR)$(apertium_fra_engmodesdir)
	rm $(PREFIX1).mode
	rm $(PREFIX2).mode
	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t1x $(DESTDIR)$(apertium_fra_engdir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX2).t1x $(DESTDIR)$(apertium_fra_engdir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t2x $(DESTDIR)$(apertium_fra_engdir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX2).t2x $(DESTDIR)$(apertium_fra_engdir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t3x $(DESTDIR)$(apertium_fra_engdir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX2).t3x $(DESTDIR)$(apertium_fra_engdir)

CLEANFILES = -r $(TARGETS_COMMON) .deps/
