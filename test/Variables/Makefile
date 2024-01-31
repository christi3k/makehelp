UNEXPORTED_CONDITIONAL_DEFINED=unexported conditional defined
export EXPORTED_CONDITIONAL_DEFINED=exported conditional defined

# Unexported recursively expanded variable.
UNEXPORTED_EXPANDED=unexported expanded

# Unexported simple variable.
UNEXPORTED_SIMPLE:=unexported simple

# Exported recursively expanded variable.
export EXPORTED_EXPANDED=exported expanded

# Exported simple variable.
export EXPORTED_SIMPLE=exported simple

# Unexported conditional defined variable.
UNEXPORTED_CONDITIONAL_DEFINED?=unexported conditional redefined

# Exported conditional defined variable.
export EXPORTED_CONDITIONAL_DEFINED?=exported conditional redefined

# Unexported recursive variable defined with spaces
UNEXPORTED_RECURSIVE_WITH_SPACES := unexported recursive with spaces

# Exported recursive variable defined with spaces
export EXPORTED_RECURSIVE_WITH_SPACES := exported recursive with spaces

# Unexported simple variable defined with spaces
UNEXPORTED_SIMPLE_WITH_SPACES := unexported simple with spaces

# Exported simple variable defined with spaces
export EXPORTED_SIMPLE_WITH_SPACES := exported simple with spaces

.PHONY: test
# Runs this Makefile as a test.
# This Makefile tests that all flavors of variables are properly expanded in the help text.
# This requires that unexported variables are exported for the help: make goal.
test: help.txt
	<$^ grep -q 'Current value: unexported expanded'
	<$^ grep -q 'Current value: unexported simple'
	<$^ grep -q 'Current value: exported expanded'
	<$^ grep -q 'Current value: exported simple'
	<$^ grep -q 'Current value: unexported conditional defined'
	<$^ grep -q 'Current value: exported conditional defined'
	<$^ grep -qv 'original unexported conditional redefined'
	<$^ grep -qv 'original exported conditional redefined'
	<$^ grep -q 'Current value: unexported simple with spaces'
	<$^ grep -q 'Current value: exported simple with spaces'
	<$^ grep -q 'Current value: unexported recursive with spaces'
	<$^ grep -q 'Current value: exported recursive with spaces'

-include ../../include/makehelp/Help.mk

help.txt: $(MAKEFILE_LIST) ../../bin/makehelp
	$(MAKE) help >$@
