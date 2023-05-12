PKG = pmbb

$(PKG).gfn: $(PKG).inp $(PKG).spec $(PKG)_sample.inp
	# fold -s -w 80 $(PKG)_help.md | tee $(PKG)_help.md
	gretlcli --makepkg $(PKG).inp

install: $(PKG).gfn
	echo pkg install $(PKG).gfn --local | gretlcli -b -

clean:
	rm -f $(PKG).gfn
