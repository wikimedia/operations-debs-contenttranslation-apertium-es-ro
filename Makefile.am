VERSION=0.7.3
LANG1=es
LANG2=ro
PREFIX1=$(LANG1)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)

BASENAME=apertium-$(PREFIX1)

TARGETS_COMMON = $(PREFIX1).automorf.bin $(PREFIX1).autobil.bin $(PREFIX1).autogen.bin \
     $(PREFIX1).autopgen.bin $(PREFIX2).automorf.bin $(PREFIX2).autobil.bin \
     $(PREFIX2).autogen.bin $(PREFIX2).autopgen.bin \
     $(PREFIX1).t1x.bin $(PREFIX1).t2x.bin $(PREFIX1).t3x.bin $(PREFIX2).t1x.bin modes

TARGETS = $(TARGETS_COMMON) 

$(PREFIX1).automorf.bin: $(BASENAME).$(LANG1).dix
	apertium-validate-dictionary $(BASENAME).$(LANG1).dix
	lt-comp lr $(BASENAME).$(LANG1).dix $@ $(BASENAME).$(LANG1).acx

$(PREFIX1).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $(BASENAME).$(PREFIX1).dix 
	lt-comp lr  $(BASENAME).$(PREFIX1).dix $@

$(PREFIX1).autogen.bin: $(BASENAME).$(LANG2).dix
	apertium-validate-dictionary $(BASENAME).$(LANG2).dix
	lt-comp rl $(BASENAME).$(LANG2).dix $@

$(PREFIX1).autopgen.bin: $(BASENAME).post-$(LANG2).dix
	apertium-validate-dictionary $(BASENAME).post-$(LANG2).dix
	lt-comp lr $(BASENAME).post-$(LANG2).dix $@

$(PREFIX2).automorf.bin: $(BASENAME).$(LANG2).dix
	apertium-validate-dictionary $(BASENAME).$(LANG2).dix
	lt-comp lr $(BASENAME).$(LANG2).dix $@ $(BASENAME).$(LANG2).acx

$(PREFIX2).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $(BASENAME).$(PREFIX1).dix
	lt-comp rl $(BASENAME).$(PREFIX1).dix $@

$(PREFIX2).autogen.bin: $(BASENAME).$(LANG1).dix
	apertium-validate-dictionary $(BASENAME).$(LANG1).dix
	lt-comp rl $(BASENAME).$(LANG1).dix $@

$(PREFIX2).autopgen.bin: $(BASENAME).post-$(LANG1).dix
	apertium-validate-dictionary $(BASENAME).post-$(LANG1).dix
	lt-comp lr $(BASENAME).post-$(LANG1).dix $@

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
	apertium-preprocess-transfer $(BASENAME).$(PREFIX2).t1x \
	                             $(PREFIX2).t1x.bin

EXTRA_DIST=$(BASENAME).$(LANG1).dix $(BASENAME).$(PREFIX1).dix \
           $(BASENAME).post-$(LANG1).dix $(BASENAME).post-$(LANG2).dix \
           $(BASENAME).$(LANG2).dix $(BASENAME).$(LANG1).tsx \
           $(BASENAME).$(LANG2).tsx $(BASENAME).$(PREFIX2).t1x \
           $(PREFIX1).prob $(PREFIX2).prob  \
           $(BASENAME).$(PREFIX1).t1x $(BASENAME).$(PREFIX1).t2x \
           $(BASENAME).$(PREFIX1).t3x modes.xml $(BASENAME).$(LANG1).acx \
	   $(BASENAME).$(LANG2).acx

apertium_es_rodir=$(prefix)/share/apertium/apertium-$(PREFIX1)/
apertium_es_modesdir=$(prefix)/share/apertium/modes/

modes/$(PREFIX2).mode: modes.xml 
	apertium-validate-modes modes.xml
	apertium-gen-modes modes.xml
	cp *.mode modes/

noinst_DATA= modes/$(PREFIX2).mode

apertium_es_ro_DATA=$(PREFIX1).automorf.bin $(PREFIX2).automorf.bin \
	$(PREFIX1).autobil.bin $(PREFIX2).autobil.bin \
	$(PREFIX1).autogen.bin $(PREFIX2).autogen.bin \
	$(PREFIX1).autopgen.bin $(PREFIX2).autopgen.bin \
	$(PREFIX1).prob $(PREFIX2).prob  \
	$(PREFIX1).t1x.bin $(PREFIX1).t2x.bin $(PREFIX1).t3x.bin \
	$(PREFIX2).t1x.bin \
	modes.xml

install-data-local:
	mv modes modes.bak
	apertium-gen-modes modes.xml apertium-$(PREFIX1)
	rm -rf modes
	mv modes.bak modes
	test -d $(DESTDIR)$(apertium_es_modesdir) || mkdir $(DESTDIR)$(apertium_es_modesdir)
	$(INSTALL_DATA) $(PREFIX2).mode $(DESTDIR)$(apertium_es_modesdir)
	rm $(PREFIX2).mode
	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t1x $(BASENAME).$(PREFIX1).t2x $(DESTDIR)$(apertium_es_rodir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX1).t3x $(DESTDIR)$(apertium_es_rodir)
	$(INSTALL_DATA) $(BASENAME).$(PREFIX2).t1x $(DESTDIR)$(apertium_es_rodir)

CLEANFILES = -rf $(TARGETS) modes *.mode
