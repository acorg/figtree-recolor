## FigTree re-coloring

The `figtree-recolor.py` script in this repo can be used to re-color the
Nexus file that is saved by
[FigTree](http://tree.bio.ed.ac.uk/software/figtree/).

The process is slightly awkward, but it works.

Runs on (at least) Python 2.7.10 and 3.5

#### Caveats

This really is a hack. There are no tests for the code. I know almost
nothing about the Nexus format or FigTree. This code may cease to work
properly if the exact format of the Nexus output written by FigTree
changes.

Andrew Rambaut says the "Import Colour Scheme" menu item in FigTree 1.4.3
will be working in a future version
([Google group discussion](https://groups.google.com/forum/#!topic/figtree-discuss/TJpd174Q02E)).

### Usage

Load your tree into FigTree. I think you may need to make some change to it
(e.g., do a rotation at a node and then undo it) and then Save the file.
FigTree will write out your tree in
[Nexus](http://hydrodictyon.eeb.uconn.edu/eebedia/index.php/Phylogenetics:_NEXUS_Format)
format.

You can then produce a new Nexus file with colored nodes using this script.

#### Specify your taxon colors

Create a text file with lines like

    # This is a comment.
    taxon1 #00FF00
    taxon2 #FF00FF
    taxon3 antique fuchsia

Lines are treated as comments if `#` is the first character on the line.

Otherwise, lines must give a taxon name (no whitespace!) followed by a
color. Colors are either specified as 6 hexadecimal digit
[RGB](https://en.wikipedia.org/wiki/RGB_color_model#Numeric_representations)
with a leading `#` (e.g., `#00FF00` and `#FF00FF` above), or are given as a
name (`antique fuchsia` above). To list the available names, see below.

By default, case will not be considered when matching taxa names against
the first field of your color specification file. To change this, use the
`--matchCase` option when calling `figtree-recolor.py`.

It is also possible for the first field of each line of the color
specification file to be a
[regular expression](https://en.wikipedia.org/wiki/Regular_expression) to
match taxa names against. In this case, you will need to run
`figtree-recolor.py` with the `--regex` option.  Regular expressions
matches are not tied to the start of the taxon string, so remember to use
`^` and `$` if you want to tie your regular expression to one or both ends
of the taxon name.  Note that the first regular expression that matches a
taxon will be the one whose color is used.

#### Create a new Nexus file

`figtree-recolor.py` writes a Nexus file on standard output. So you run it via:

```sh
$ figtree-recolor.py --nexusFile figtree.nexus --colorFile colors.txt > new.nexus
```

It can also read the Nexus from standard input:

```sh
$ figtree-recolor.py --colorFile colors.txt < figtree.nexus > new.nexus
```

If your Nexus file already has colors and you want to keep them (in the
case where your color specification file doesn't indicate otherwise), you
can preserve the original colors:

```sh
$ figtree-recolor.py --nexusFile figtree.nexus --colorFile colors.txt --preserveOriginalColors > new.nexus
```

The default is that all original colors will be stripped.

You can also specify a default color for taxa that are not given an
explicit color in the color specification file. E.g., 

```sh
$ figtree-recolor.py --nexusFile figtree.nexus --colorFile colors.txt --defaultColor red > new.nexus
```

or

```sh
$ figtree-recolor.py --nexusFile figtree.nexus --colorFile colors.txt --defaultColor 0000FF > new.nexus
```

Note that if you use `--preserveOriginalColors` and a taxon has a
pre-existing color (and is not named in the colors specification file),
then the original color will be preserved (i.e., the default will not be
applied).

If you use `--colorFile /dev/null`, all pre-existing colors will be removed
(assuming you don't also use `--preserveOriginalColors`, of course).

#### View your colored tree

Once you've created a new Nexus file, go back into FigTree and open it.

### Named colors

A list of the known color names (and their hex values) can be printed via:

```sh
$ figtree-recolor.py --listColors
```
