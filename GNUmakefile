##
# GNUmakefile for curl
##

## Configuration ##

Project        = texinfo
Name           = $(Project)
Version        = 4.7
Name_Vers      = $(Name)-$(Version)
Compress_Type  = bz2
Tarball        = $(Name_Vers).tar.$(Compress_Type)
Extract_Dir    = $(Name_Vers)
Patch_List     = texindex.patch

## Don't modify below here ##

# Determine correct extract option (default = gzip).
ifeq ($(Compress_Type),bz2)
	Extract_Option = j
else
	Extract_Option = z
endif

no_target:
	@$(MAKE) -f Makefile

# Hijack the install stage to extract/patch the source.
install:
	@echo "-- Extracting distfiles --"
	rmdir $(OBJROOT)
	cp -r $(SRCROOT) $(OBJROOT)
	rm -rf $(OBJROOT)/$(Project)
	cd $(OBJROOT) && tar $(Extract_Option)xf $(OBJROOT)/$(Tarball)
	mv $(OBJROOT)/$(Extract_Dir) $(OBJROOT)/$(Project)
	@echo "-- Applying patches --"
	$(_v) for patchfile in $(Patch_List); do \
		cd $(OBJROOT)/$(Project) && patch -p0 < $(OBJROOT)/patches/$$patchfile; \
	done
	@echo "-- Done extracting/patching, continuing --"
	$(MAKE) -C $(OBJROOT) -f Makefile install \
		SRCROOT=$(OBJROOT) \
		OBJROOT=$(OBJROOT)/$(Project) \
		VERSION=$(VERSION)

.DEFAULT:
	@$(MAKE) -f Makefile $@
