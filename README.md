## FigTree re-coloring

The `figtree-recolor.py` script in this repo can be used to re-color the
Nexus file that is saved by
[FigTree](http://tree.bio.ed.ac.uk/software/figtree/).

The process is slightly awkward, but it works.

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
    DA119 #00FF00
    DA195_6_5148_EU859952.1 #ff00ff
    DA344 antique fuchsia

Lines are either comments (`#` must be the first character on the line), or
have colors specified as 6-digit RGB hexadecimal, 

#### Create a new Nexus file

`figtree-recolor.py` writes a Nexus file on standard output. So you can run it via:

```sh
$ figtree-recolor.py --nexus figtree.nexus --colorFile colors.txt > new.nexus
```

It can also read the Nexus from standard input:

```sh
$ figtree-recolor.py --colorFile colors.txt < figtree.nexus > new.nexus
```

If your Nexus file already has colors and you want to keep them (in the
case where your color specification file doesn't indicate otherwise), you
can preserve the original colors:

```sh
$ figtree-recolor.py --nexus figtree.nexus --colorFile colors.txt --preserveOriginalColors > new.nexus
```

#### Named colors

A list of the known color names (and their hex values) can be printed via:

```sh
$ figtree-recolor.py --listColors
```
