include ../Makefile.config
-include ../Makefile.local

FILES   := xmllexer.ml camllexer.ml simplexmlparser.mli pa_tyxml.mli
RFILES  := basic_types.ml \
	   simplexmlparser.ml \
	   xhtmlparser.ml xhtmlsyntax.ml

PREDEP  := xmllexer.ml camllexer.ml

LIBS    := -package camlp4
PP      := -pp "camlp4of"
PPR     := -pp "camlp4rf"

OCAMLC   := $(OCAMLFIND) ocamlc $(DBG) $(LIBS)
OCAMLOPT := $(OCAMLFIND) ocamlopt $(DBG) $(LIBS)
OCAMLDEP := $(OCAMLFIND) ocamldep $(LIBS)

byte: pa_${LIBNAME}.cmi pa_${LIBNAME}.cmo xmllexer.cmo simplexmlparser.cmo
opt:: xmllexer.cmx simplexmlparser.cmx
ifeq "${NATDYNLINK}" "yes"
opt:: xmllexer.cmxs simplexmlparser.cmxs
endif

OBJS := xmllexer.cmo basic_types.cmo camllexer.cmo xhtmlparser.cmo xhtmlsyntax.cmo

pa_${LIBNAME}.cmo: ${OBJS}
	$(OCAMLC) -pack -o $@ $^

# Common rules

ML   := $(filter %.ml,${FILES})
MLI  := $(filter %.mli,${FILES})
MLR  := $(filter %.ml,${RFILES})
MLIR := $(filter %.mli,${RFILES})

${MLI:.mli=.cmi}: %.cmi: %.mli
	$(OCAMLC) $(PP) -c $<
${ML:.ml=.cmo}: %.cmo: %.ml
	$(OCAMLC) $(PP) -c $<
${ML:.ml=.cmx}: %.cmx: %.ml
	$(OCAMLOPT) $(PP) -c $<

${MLIR:.mli=.cmi}: %.cmi: %.mli
	$(OCAMLC) $(PPR) -c $<
${MLR:.ml=.cmo}: %.cmo: %.ml
	$(OCAMLC) $(PPR) -c $<
${MLR:.ml=.cmx}: %.cmx: %.ml
	$(OCAMLOPT) $(PPR) -c $<

%.ml: %.mll
	$(OCAMLLEX) $<

%.cmxs: %.cmx
	$(OCAMLOPT) -shared -linkall -o $@ $<

# Clean up
clean:
	-rm -f ${PREDEP}
	-rm -f *.cm[ioax] *.cmxa *.cmxs *.o *.a *.annot
distclean: clean
	-rm -f *~ \#* .\#*

# Dependencies
depend: ${PREDEP}
	$(OCAMLDEP) $(PP) ${FILES} > .depend
	$(OCAMLDEP) $(PPR) ${RFILES} >> .depend

FORCE:

-include .depend
