###############################################################################
## Makefile for Finnish--Karelian
###############################################################################

LANG1=fin
LANG2=krl
PREFIX1=$(LANG1)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)

BASENAME=apertium-$(PREFIX1)

TARGETS_COMMON =		\
	$(PREFIX1).automorf.hfst	\
	$(PREFIX1).rlx.bin	\
	$(PREFIX1).autobil.bin	\
	$(PREFIX1).autolex.bin	\
	$(PREFIX1).autogen.hfst	\
	$(PREFIX1).t1x.bin	\
	$(PREFIX1).t2x.bin	\
	$(PREFIX1).t3x.bin	\
	$(PREFIX2).automorf.hfst	\
	$(PREFIX2).rlx.bin	\
	$(PREFIX2).autobil.bin	\
	$(PREFIX2).autolex.bin	\
	$(PREFIX2).autogen.hfst	\
	$(PREFIX2).t1x.bin	\
	$(PREFIX2).t2x.bin	\
	$(PREFIX2).t3x.bin


# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

###############################################################################
## Monolingual transducers
###############################################################################

# -------------------
# Finnish analysis:
# -------------------

$(PREFIX1).automorf.hfst: $(AP_SRC1)/$(LANG1).automorf.hfst
	cp -v $< $@

# -------------------
# Finnish generation:
# ---------------------

$(PREFIX2).autogen.hfst: $(AP_SRC1)/$(LANG1).autogen.hfst
	cp -v $< $@

# -------------------
# Karelian analysis:
# -------------------

$(PREFIX2).automorf.hfst: $(AP_SRC2)/$(LANG2).automorf.hfst
	cp -v $< $@

# -------------------
# Karelian generation:
# ---------------------

$(PREFIX1).autogen.hfst: $(AP_SRC2)/$(LANG2).autogen.hfst
	cp -v $< $@

###############################################################################
## Disambiguation rules
###############################################################################

$(PREFIX1).rlx.bin: $(AP_SRC1)/apertium-$(LANG1).$(LANG1).rlx
	cg-comp $< $@

$(PREFIX2).rlx.bin: $(AP_SRC2)/apertium-$(LANG2).$(LANG2).rlx
	cg-comp $< $@

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
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

$(PREFIX1).t2x.bin: $(BASENAME).$(PREFIX1).t2x
	apertium-validate-interchunk $<
	apertium-preprocess-transfer $< $@

$(PREFIX1).t3x.bin: $(BASENAME).$(PREFIX1).t3x
	apertium-validate-postchunk $<
	apertium-preprocess-transfer $< $@


$(PREFIX2).t1x.bin: $(BASENAME).$(PREFIX2).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t2x.bin: $(BASENAME).$(PREFIX2).t2x
	apertium-validate-interchunk $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t3x.bin: $(BASENAME).$(PREFIX2).t3x
	apertium-validate-postchunk $<
	apertium-preprocess-transfer $< $@


###############################################################################
## Distribution
###############################################################################

EXTRA_DIST=					\
		$(BASENAME).$(PREFIX1).lrx	\
		$(BASENAME).$(PREFIX1).dix	\
		$(BASENAME).$(PREFIX2).lrx	\
		$(BASENAME).$(PREFIX1).t1x	\
		$(BASENAME).$(PREFIX1).t2x	\
		$(BASENAME).$(PREFIX1).t3x	\
		$(BASENAME).$(PREFIX2).t1x	\
		$(BASENAME).$(PREFIX2).t2x	\
		$(BASENAME).$(PREFIX2).t3x	\
		modes.xml

###############################################################################
## Installation
###############################################################################

apertium_fin_krldir=$(prefix)/share/apertium/$(BASENAME)/

# Files which should be installed to $(prefix)/share/apertium/$(BASENAME)/:
apertium_fin_krl_DATA=	\
	$(TARGETS_COMMON)				\
	$(BASENAME).$(PREFIX1).t1x			\
	$(BASENAME).$(PREFIX1).t2x			\
	$(BASENAME).$(PREFIX1).t3x			\
	$(BASENAME).$(PREFIX2).t1x			\
	$(BASENAME).$(PREFIX2).t2x			\
	$(BASENAME).$(PREFIX2).t3x

# All modes are created by the same goal, listing several will just give problems with make -j2
noinst_DATA=modes/$(PREFIX1).mode

install-data-local: install-modes

###############################################################################
## Cleanup
###############################################################################

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	echo "talo" | apertium -d . fin-krl | tee .test-mt
	@echo "talo" > .test-ref
	@diff .test-ref .test-mt
	echo "talo" | apertium -d . krl-fin | tee .test-mt
	@echo "talo" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt

## apevis gh-pages
if CAN_SAXON
LANG1HTML=Finnish
LANG2HTML=Karelian

bidix2html-split.xslt:
	wget https://raw.githubusercontent.com/flammie/apevis-xslt/master/$@

bidix2html.xslt:
	wget https://raw.githubusercontent.com/flammie/apevis-xslt/master/$@

tix2html.xslt:
	wget https://raw.githubusercontent.com/flammie/apevis-xslt/master/$@

dix2stats.xslt:
	wget https://raw.githubusercontent.com/flammie/apevis-xslt/master/$@

noinst_DATA+=docs/$(BASENAME).$(PREFIX1).dix.html \
			 docs/$(BASENAME).$(PREFIX2).dix.html \
			 docs/$(BASENAME).$(PREFIX1).t1x.html \
			 docs/$(BASENAME).$(PREFIX1).t2x.html \
			 docs/$(BASENAME).$(PREFIX1).t3x.html \
			 docs/$(BASENAME).$(PREFIX2).t1x.html \
			 docs/$(BASENAME).$(PREFIX2).t2x.html \
			 docs/$(BASENAME).$(PREFIX2).t3x.html \
			 docs/statistics.html

docs/$(BASENAME).$(PREFIX1).dix.html: $(BASENAME).$(PREFIX1).dix bidix2html.xslt
	$(SAXON) -s:$< bidix2html.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@
docs/$(BASENAME).$(PREFIX2).dix.html: $(BASENAME).$(PREFIX1).dix bidix2html.xslt
	$(SAXON) -s:$< bidix2html.xslt from=$(LANG2HTML) to=$(LANG1HTML) pair=$(PREFIX1) antipair=$(PREFIX2)  invert=true > $@

docs/$(BASENAME).$(PREFIX1).t1x.html: $(BASENAME).$(PREFIX1).t1x tix2html.xslt
	$(SAXON) -s:$< tix2html.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2)  > $@

docs/$(BASENAME).$(PREFIX2).t1x.html: $(BASENAME).$(PREFIX2).t1x tix2html.xslt
	$(SAXON) -s:$< tix2html.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2)  > $@



docs/$(BASENAME).$(PREFIX1).t2x.html: $(BASENAME).$(PREFIX1).t2x tix2html.xslt
	$(SAXON) -s:$< tix2html.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2)  > $@

docs/$(BASENAME).$(PREFIX2).t2x.html: $(BASENAME).$(PREFIX2).t2x tix2html.xslt
	$(SAXON) -s:$< tix2html.xslt  from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2)  > $@

docs/$(BASENAME).$(PREFIX1).t3x.html: $(BASENAME).$(PREFIX1).t3x tix2html.xslt
	$(SAXON) -s:$< tix2html.xslt  from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2)  > $@

docs/$(BASENAME).$(PREFIX2).t3x.html: $(BASENAME).$(PREFIX2).t3x tix2html.xslt
	$(SAXON) -s:$< tix2html.xslt  from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/stats-$(BASENAME).$(PREFIX1).dix.html: $(BASENAME).$(PREFIX1).dix dix2stats.xslt
	$(SAXON) -s:$< dix2stats.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/stats-$(BASENAME).$(PREFIX1).t1x.html: $(BASENAME).$(PREFIX1).t1x dix2stats.xslt
	$(SAXON) -s:$< dix2stats.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/stats-$(BASENAME).$(PREFIX2).t1x.html: $(BASENAME).$(PREFIX2).t1x dix2stats.xslt
	$(SAXON) -s:$< dix2stats.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/stats-$(BASENAME).$(PREFIX1).t2x.html: $(BASENAME).$(PREFIX1).t2x dix2stats.xslt
	$(SAXON) -s:$< dix2stats.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/stats-$(BASENAME).$(PREFIX2).t2x.html: $(BASENAME).$(PREFIX2).t2x dix2stats.xslt
	$(SAXON) -s:$< dix2stats.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/stats-$(BASENAME).$(PREFIX1).t3x.html: $(BASENAME).$(PREFIX1).t3x dix2stats.xslt
	$(SAXON) -s:$< dix2stats.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/stats-$(BASENAME).$(PREFIX2).t3x.html: $(BASENAME).$(PREFIX2).t3x dix2stats.xslt
	$(SAXON) -s:$< dix2stats.xslt from=$(LANG1HTML) to=$(LANG2HTML) pair=$(PREFIX1) antipair=$(PREFIX2) > $@

docs/statistics.html: docs/stats-$(BASENAME).$(PREFIX1).dix.html \
				 docs/stats-$(BASENAME).$(PREFIX1).t1x.html \
				 docs/stats-$(BASENAME).$(PREFIX2).t1x.html \
				 docs/stats-$(BASENAME).$(PREFIX1).t2x.html \
				 docs/stats-$(BASENAME).$(PREFIX2).t2x.html \
				 docs/stats-$(BASENAME).$(PREFIX1).t3x.html \
				 docs/stats-$(BASENAME).$(PREFIX2).t3x.html
	$(AWK) '/DOCTYPE/,/<\/h1>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX1).dix.html > $@
	$(AWK) '/<h2>/,/<\/div>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX1).dix.html | fgrep -v '</div>' >> $@
	$(AWK) '/<h2>/,/<\/div>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX1).t1x.html | fgrep -v '</div>' >> $@
	$(AWK) '/<h2>/,/<\/div>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX1).t2x.html | fgrep -v '</div>' >> $@
	$(AWK) '/<h2>/,/<\/div>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX1).t3x.html | fgrep -v '</div>' >> $@
	$(AWK) '/<h2>/,/<\/div>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX2).t1x.html | fgrep -v '</div>' >> $@
	$(AWK) '/<h2>/,/<\/div>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX2).t2x.html | fgrep -v '</div>' >> $@
	$(AWK) '/<h2>/,/<\/div>/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX2).t3x.html | fgrep -v '</div>' >> $@
	echo '</div><div class="row">' >> $@
	$(AWK) '/<div id="footer"/,/<\/html/ {print;}' < \
		docs/stats-$(BASENAME).$(PREFIX1).dix.html | fgrep -v '</div>' >> $@



endif

