default: example.html

%.html: %.md
	pandoc --standalone -t revealjs -o $@ $<
