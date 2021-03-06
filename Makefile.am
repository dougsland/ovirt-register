# -*- coding: utf-8 -*-
#
# Copyright (C) 2015 Red Hat, Inc.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

AUTOMAKE_OPTIONS = foreign 1.9
ACLOCAL_AMFLAGS = -I m4

PYTHONSOURCES = \
	$(shell find . -name \*.py -o -name ovirt-register)

SHELL := /bin/bash

SUBDIRS = \
	src \
        src/ovirt_register \
        src/scripts \
	man \
	docs \
	$(NULL)	

EXTRA_DIST = \
	python-ovirt-register.spec \
	python-ovirt-register.spec.in \
	$(NULL)

DISTCLEANFILES = \
  aclocal.m4 \
  configure \
  install-sh \
  missing \
  py-compile

DISTCLEANDIRS = autom4te.cache

TMPREPOS = tmp.repos
RPM_FLAGS := --define="_topdir `pwd`/$(TMPREPOS)"

rpm: check-local srpm
	rpmbuild $(RPM_FLAGS) --rebuild "$(TMPREPOS)"/SRPMS/*.src.rpm
	cp $(distdir).tar.gz $(TMPREPOS)/SOURCES/
	@echo
	@echo "rpm available at '$(TMPREPOS)'"
	@echo


srpm: dist
	rm -rf "$(TMPREPOS)"
	mkdir -vp $(TMPREPOS)/{SPECS,RPMS,SRPMS,SOURCES}
	rpmbuild $(RPM_FLAGS) -ts $(distdir).tar.gz
	@echo
	@echo "srpm available at '$(TMPREPOS)'"

.PHONY: rpm srpm

check-local: 2to3 pep8 pyflakes
	@echo -e "---\n Passed.\n---"

pep8:
	@for M in $(PYTHONSOURCES); \
	do \
		echo pep8 on "$$M"; \
		PYTHONPATH=. pep8 -r $$M || exit 1; \
	done

pyflakes:
	@for M in $(PYTHONSOURCES); \
	do \
		echo pyflakes on "$$M"; \
		PYTHONPATH=. pyflakes $$M || exit 1; \
	done

2to3:
	@for M in $(PYTHONSOURCES); \
	do \
		echo 2to3 on "$$M"; \
		PYTHONPATH=. 2to3 $$M || exit 1; \
	done
