FILE_NAME := report

TEMP_FILES := \
	$(FILE_NAME).aux \
	$(FILE_NAME).bbl \
	$(FILE_NAME).log \
	$(FILE_NAME).blg \
	$(FILE_NAME).brf \
	$(FILE_NAME).out

SECTION_FILES = \
	conclusions.tex \
	design.tex \
	implications.tex \
	intro.tex \
	preamble.tex \
	related.tex \
	report.tex \
	results.tex \
	threats.tex

BIBTEX_FILES = \
	report.bib

qt_figs = \
	figures/filtering/qt_4.pdf \
	figures/importance/qt_lrm_2.pdf \
	figures/importance/qt_lrm_4.pdf \
	figures/perf/qt_lrm_2_AUC.pdf \
	figures/perf/qt_lrm_2_Brier.pdf \
	figures/perf/qt_lrm_4_AUC.pdf \
	figures/perf/qt_lrm_4_Brier.pdf \
	figures/perf/qt_lrm_2_AUC_delta.pdf \
	figures/perf/qt_lrm_2_Brier_delta.pdf \
	figures/perf/qt_lrm_4_AUC_delta.pdf \
	figures/perf/qt_lrm_4_Brier_delta.pdf \
	figures/stability/qt_lrm_2.pdf \
	figures/stability/qt_lrm_4.pdf

openstack_figs = \
	figures/filtering/openstack_4.pdf \
	figures/importance/openstack_lrm_2.pdf \
	figures/importance/openstack_lrm_4.pdf \
	figures/perf/openstack_lrm_2_AUC.pdf \
	figures/perf/openstack_lrm_2_Brier.pdf \
	figures/perf/openstack_lrm_4_AUC.pdf \
	figures/perf/openstack_lrm_4_Brier.pdf \
	figures/perf/openstack_lrm_2_AUC_delta.pdf \
	figures/perf/openstack_lrm_2_Brier_delta.pdf \
	figures/perf/openstack_lrm_4_AUC_delta.pdf \
	figures/perf/openstack_lrm_4_Brier_delta.pdf \
	figures/stability/openstack_lrm_2.pdf \
	figures/stability/openstack_lrm_4.pdf

GEN_FIGURES = \
	$(qt_figs) \
	$(openstack_figs)

STATIC_FIGURES = \
	figures/contrib_mgmt.pdf \
	figures/study_design.pdf \
	figures/period_examples.pdf

TABLES = \
	tables/cases.tex \
	tables/metrics.tex \
	tables/model.tex \
	tables/filterings.tex \
	tables/overtime.tex

DEBUG=no
ifneq '$(DEBUG)' 'yes'
LATEX_FLAGS="-interaction=batchmode"
endif

.DELETE_ON_ERROR:

################################################################################
# Main paper building
################################################################################

$(FILE_NAME).pdf: $(SECTION_FILES) $(GEN_FIGURES) $(STATIC_FIGURES) $(TABLES) \
		$(BIBTEX_FILES)
	pdflatex $(LATEX_FLAGS) $(FILE_NAME)
	bibtex $(FILE_NAME)
	pdflatex $(LATEX_FLAGS) $(FILE_NAME)
	pdflatex $(LATEX_FLAGS) $(FILE_NAME)
	rm -f $(TEMP_FILES)

bibtex/report_bibdesk.bib:
	if [ ! -d bibtex ]; then svn co --depth empty https://sailsvn.cs.queensu.ca/shane/ bibtex; fi
	cd bibtex && svn up report_bibdesk.bib

################################################################################
# Figures
################################################################################

fig_basics = \
	scripts/figs/preamble.r \
	scripts/process_data/load_data.r \
	scripts/process_data/performance.r \
	scripts/process_data/fit_models.r

figures/filtering/%_4.pdf: scripts/figs/filtering.r $(fig_basics)
	mkdir -p $(dir $@)
	Rscript $< $(patsubst %_lrm,%,$*)

figures/importance/%_2.pdf figures/perf/%_2_AUC.pdf figures/perf/%_2_Brier.pdf figures/stability/%_2.pdf \
		figures/importance/%_4.pdf figures/perf/%_4_AUC.pdf figures/perf/%_4_Brier.pdf figures/stability/%_4.pdf \
		figures/perf/%_4_AUC_delta.pdf figures/perf/%_4_Brier_delta.pdf figures/perf/%_2_AUC_delta.pdf \
		figures/perf/%_2_Brier_delta.pdf: \
			scripts/figs/figs.r $(fig_basics)
	mkdir -p figures/perf figures/importance figures/stability
	Rscript $< $(patsubst %_lrm,%,$(patsubst %_AUC,%,$(patsubst %_Brier,%,$(patsubst %_delta,%,$*))))

################################################################################
# Clean
################################################################################
.PHONY: clean
clean:
	rm -f $(TEMP_FILES) $(GEN_FIGURES) $(FILE_NAME).pdf
