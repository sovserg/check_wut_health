libexec_SCRIPTS=check_wut_health
GL_MODULES=\
  ../GLPlugin/lib/Monitoring/GLPlugin/Commandline/Extraopts.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/Commandline/Getopt.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/Commandline.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/Item.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/TableItem.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/SysDescPrettify.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/CSF.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/Item.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/TableItem.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/MIB2MIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/EMDMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/ENVIROMUX5D.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/HWGWLDMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/SNMPFRAMEWORKMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/STULZWIB8000MIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/WEBGRAPH8XTHERMOMETERMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/WEBGRAPHTHERMOHYGROBAROMETERMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/WEBGRAPHTHERMOHYGROBAROMETERUSMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/LIEBERTGPCONDITIONSMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/LIEBERTGPENVIRONMENTALMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/LIEBERTGPFLEXIBLEMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/LIEBERTGPPOWERMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/LIEBERTGPREGISTRATIONMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/LIEBERTGPSYSTEMMIB.pm \
  ../GLPlugin/lib/Monitoring/GLPlugin/SNMP/MibsAndOids/THEV01MIB.pm
EXTRA_MODULES=\
  Classes/WebGraphThermoBaro.pm \
  Classes/WebGraphThermoBaroUS.pm \
  Classes/WebioAn8Graph.pm \
  Classes/HWG.pm \
  Classes/Raritan.pm \
  Classes/Raritan/EMD.pm \
  Classes/Stulz.pm \
  Classes/Stulz/WIB8000.pm \
  Classes/Liebert/Component/EnvironmentalSubsystem.pm \
  Classes/Liebert/Component/SensorSubsystem.pm \
  Classes/Liebert.pm \
  Classes/Papouch.pm \
  Classes/NTI/ENVIROMUX5D/Component/EnvironmentalSubsystem.pm \
  Classes/NTI/ENVIROMUX5D/Component/SensorSubsystem.pm \
  Classes/NTI/ENVIROMUX5D.pm \
  Classes/NTI.pm \
  Classes/Device.pm

SED=/bin/sed
GREP=/bin/grep
CAT=/bin/cat
ECHO=/bin/echo
if DISABLE_STANDALONE
STANDALONE = no
else
STANDALONE = yes
endif

SUFFIXES = .pl .pm .sh

VPATH=$(top_srcdir) $(top_srcdir)/plugins-scripts $(top_srcdir)/plugins-scripts/t

EXTRA_DIST=$(libexec_SCRIPTS).pl $(EXTRA_MODULES) $(GL_MODULES)

CLEANFILES=$(libexec_SCRIPTS)

AM_INSTALL_PROGRAM_FLAGS=@INSTALL_OPTS@

.pm :
	$(AWK) -f ./subst $< > $@
	chmod +x $@

.pl :
	$(AWK) -f ./subst $< > $@
	chmod +x $@

.sh :
	$(AWK) -f ./subst $< > $@
	chmod +x $@

$(libexec_SCRIPTS) : $(EXTRA_DIST)
	$(ECHO) "#! #PERL# -w" | $(AWK) -f ./subst > $@
	$(ECHO) "# nagios: -epn" >> $@
	$(ECHO) >> $@
	if [ "${STANDALONE}" == "yes" ]; then \
		$(ECHO) i am standalone; \
		for m in ${GL_MODULES}; do \
			$(SED) -e 's/^1;//g' < $$m | $(SED) -e '/^__END__/,$$d' | $(AWK) -f ./subst >> $@; \
		done \
	fi
	for m in ${EXTRA_MODULES}; do \
		$(SED) -e 's/^1;//g' < $$m | $(SED) -e '/^__END__/,$$d' | $(AWK) -f ./subst >> $@; \
	done
	$(ECHO) "package main;" >> $@
	$(CAT) $(libexec_SCRIPTS).pl | $(AWK) -f ./subst >> $@
	chmod +x $@

