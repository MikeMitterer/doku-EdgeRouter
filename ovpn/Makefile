# Self-Documented Makefile see https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
#
# Basics zum Makefile:
#	https://makefiletutorial.com/#getting-started
#
# You can set these variables from the command line.
# BUILDDIR      = _build

# https://www.chiark.greenend.org.uk/doc/make-doc/make.html/Recipes.html#One-Shell
# .ONESHELL:

# Explizit setzt, denn unter Debian ist /bin/sh nicht gleich /bin/bash
# (per default ist /bin/sh ein symlink auf /bin/dash)
SHELL       := /bin/bash
# .SHELLFLAGS := -eu -o pipefail -c

.PHONY: help init init312 client build deploy

.DEFAULT_GOAL := help

# Python
# init init312: ENVIRONMENT := ".vMLUtils"
# ENVIRONMENT   	:= ".vPetShopSample"

# Definiert in tests/unit/_resources/settings.toml
# UPLOAD_PATH     := "tests/unit/_resources/uploads"

include ${DEV_MAKE}/colours.mk

help:
	@echo
	@echo "Please use \`make <${YELLOW}target${RESET}>' where <target> is one of"
	@echo
	@echo "    ${YELLOW}help                   ${GREEN}This help message${RESET}"
	@echo
	@echo "    ${YELLOW}client CLIENT=clientX  ${GREEN}Creates new client certificate${RESET}"
#	@echo "    ${YELLOW}build                  ${GREEN}Build your setup${RESET}"
#	@echo "    ${YELLOW}deploy                 ${GREEN}Package wird gebaut. ${WHITE}'git push' muss per Hand ausgeführt werden!${RESET}"
#	@echo
#	@echo "${BLUE}Hints:${NC}"
	@echo

colors: 	$(call print_colours)

init init312:
	# [[ -d ${ENVIRONMENT} ]] || python3 -m venv ${ENVIRONMENT}
	@echo Init called

client:
	@[ "${CLIENT}" ] || (echo "${RED}CLIENT is not set!"; exit 1)
	@echo
	@echo "Generating Certificates for ${YELLOW}${CLIENT}${RESET}, continue with [${GREEN}ENTER${RESET}]"
	@echo "    ${BLUE}Hint:${RESET}"
	@echo "    - Use '${YELLOW}1234${RESET}' as passphrase"
	@echo "    - Common Name (e.g. server FQDN or YOUR name) []: ${YELLOW}${CLIENT}${RESET}"
	@echo && read
	./bin/CA.pl -newreq
	@echo
	@echo "Sign the certificate, continue with [${GREEN}ENTER${RESET}]" && read
	@echo
	./bin/CA.pl -sign
	@echo
	@echo "Create new folder ${YELLOW}clients/${CLIENT}${RESET}, continue with [${GREEN}ENTER${RESET}]" && read
	mkdir -p "clients/${CLIENT}"
	@echo
	@echo "Remove passphrase from key, continue with [${GREEN}ENTER${RESET}]"
	@echo "    ${BLUE}Hint:${RESET}"
	@echo "    - Use '${YELLOW}1234${RESET}' as passphrase"
	@echo && read
	mv newkey.pem ".backup/${CLIENT}-with-pass-phrase.key" && \
          openssl rsa -in ".backup/${CLIENT}-with-pass-phrase.key" -out "clients/${CLIENT}/${CLIENT}".key
	@echo
	@echo "Setup OpenVPN-Import-Folder, continue with [${GREEN}ENTER${RESET}]" && read
	mv newcert.pem "clients/${CLIENT}/${CLIENT}.pem" && \
        mv newreq.pem "clients/${CLIENT}/${CLIENT}.req" && \
        cp demoCA/cacert.pem "clients/${CLIENT}" && \
        cp ta.key "clients/${CLIENT}" && \
        zip -rj clients/${CLIENT}.ovpn.zip clients/${CLIENT}

#build:
#	@echo Build called
#
#deploy: build
#
#all: deploy

