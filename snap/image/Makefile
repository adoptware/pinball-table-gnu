#!/usr/bin/make -f
# -*- makefile -*-
# ex: set tabstop=4 noexpandtab:
# SPDX-License-Identifier: GPL-2.0-or-later
# Copyright: Philippe Coval <https://purl.org/rzr/> - 2021+

default: archive
	-sync

project?= pinball-table-gnu

-include ~/bin/mk-private.mk

gadget?=pc
base?=core
base_version?=20
architecture?=amd64
channel?=${base_version}/edge

export architecture
name?=${project}-${base}${base_version}-${architecture}-${gadget}
developer_id?=$(shell snapcraft whoami \
	| grep 'developer-id:' | cut -d' ' -f2 \
	|| echo "TODO: https://ubuntu.com/core/docs/custom-images")
authority_id?=${developer_id}
brand_id?=${authority_id}
ubuntu_image_snap_options?=
# image_size?=3G
# ubuntu_image_snap_options+=--image-size ${image_size}
key?=default

all?=version
all+=pc
all+=pi

sudo?=sudo
fd_file?=/usr/share/OVMF/OVMF_CODE.fd

help:
	snap keys

all: ${all}
	-sync

setup/snap:
	snap list
	${sudo} snap install snapcraft --edge
	${sudo} snap install ubuntu-image --edge
	${sudo} snap refresh
	snap list
	snap info ${project}

${fd_file}:
	${sudo} apt-get install ovmf
	ls "$@"

snap/snapcraft.yaml:
	${@D} init

archive: ${name}.img.gz
	du -hs $<

${name}.json: ubuntu-image-${gadget}.json Makefile
	jq -M . $<
	sed -e "s|{architecture}|${architecture}|g" < $< \
	| jq -r '.["authority-id"] = "${authority_id}"' \
	| jq -r '.architecture = "${architecture}"' \
	| jq -r '.["brand-id"] = "${brand_id}"' \
	| sponge $@
	jq -M . $@

image: ${name}.img.gz
	du -hs $<

%.img: %.model
	-mv $@.manifest $@.manifest.prev.tmp
	ubuntu-image snap ${ubuntu_image_snap_options} -o $@ $<
	sort seed.manifest | tee $@.manifest
	rm seed.manifest
	-diff -u $@.manifest.prev.tmp $@.manifest
	du -hsc $@

%.model: %.json timestamp Makefile
	jq -M . $< | snap sign -k ${key} > $@.tmp
	mv $@.tmp $@

timestamp: ${name}.json
	jq -M . "$<" | sed \
-e "s|\"timestamp\": .*|\"timestamp\": \"$$(date -Iseconds --utc)\",|g" \
	| jq -M . > "$<.tmp"
	mv "$<.tmp" "$<"

clean:
	rm -fv *~ *.img *.img.gz *.model

%.img.gz: %.img
	rm -f "$@"
	du -hsc $<
	gzip -9 $<
	du -hsc $@

pc:
	${MAKE} gadget=$@ architecture=amd64

pi:
	${MAKE} gadget=$@ architecture=arm64

pi3:
	${MAKE} gadget=pi architecture=armhf

version:
	${MAKE} --version
	ubuntu-image --version
	snapcraft version
	snap --version
	jq --version

%.key.tmp:
	snap keys | grep "^${key}" \
	|| snap create-key ${key}
	snap keys | grep "^${key}" | awk '{ print $$2 }' | tee $@


register:
	snapcraft list | grep '^${project} ' && exit 1
	snapcraft whoami || snapcraft login
	snapcraft whoami | grep '^developer-id: ${developer_id}'
	snapcraft list | grep '^${project} ' || snapcraft register "${project}"
	snapcraft list | grep '^${project} '

${project}.reg.tmp:
	snapcraft list | grep '^${project} ' || ${MAKE} register
	snapcraft list | grep '^${project} ' | tee "$@"

key: ${project}.key.tmp
	ls $<

run/%: %.img ${fd_file}
	sudo qemu-system-x86_64 \
	-enable-kvm -smp 2 -m 1500 -cpu host \
	-device virtio-vga,virgl=on \
	-drive file=${fd_file},if=pflash,format=raw,unit=0,readonly=on \
	-device virtio-net-pci,netdev=net0 \
	-netdev user,id=net0,hostfwd=tcp::50022-:22,hostfwd=tcp::50080-:80 \
	-drive file=$<,format=raw

run: run/${name}


.PRECIOUS: \
	%.img \
	%.model \
#EOL
