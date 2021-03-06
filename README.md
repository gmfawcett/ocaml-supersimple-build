This project provides `obuild`, a script (currently in Python) that
handles the basics of building a small OCaml project. When a Makefile
or Ocamlbuild is too much, `obuild` might just be the ticket.

## Usage

    obuild myprogram.ml        --> build native executable 'myprogram'
    obuild -b myprogram.ml     --> build bytecode executable 'myprogram'
    obuild -c myprogram.ml     --> just compile to myprogram.cmx
    obuild -b -c myprogram.ml  --> just compile to myprogram.cmo
    obuild -f myprogram.ml     --> force compilation even if current

The `.ml` extension may be omitted.

Obuild will use `ocamldep` to discover and compile other source files
in the current directory which are dependencies of the main program.

A simple convention is used to specify dependencies on third-party
packages, and to specify syntax extensions. Dependencies and
extensions must be discoverable via `ocamlfind`.

## Example

Here is a small program, `test.ml` that uses the `xstrp4`
string-interpolation syntax, as well as the `curl` package:

    (* LANG xstrp4 *)
    (* PKG curl *)
    		     
    module C = Curl

    let fetch url = 
      let c = C.init () in
        C.set_url c url;
        C.perform c

    let url = "http://www.example.net/" in
      fetch url;
      print_endline (interpolate "I just fetched the URL $url.")

Ensure you have the `xstrp4` and `curl` packages installed (I used
[godi][] to get them), then compile this program with `obuild test.ml`
and you'll have an executable named `test` ready for action.

[GODI]: http://godi.camlcity.org/godi/index.html

### Directives: LANG and PKG
The special comments starting with `LANG` and `PKG` specify camlp4
syntax extensions and package dependencies, respectively. Either can
be omitted; they can be presented in either order; and they may appear
on the same line if you prefer. They do not have to be the first line
in the source file. (Note, the ordering of package and syntax names
*within* these directives may be relevant to your application.)

## Current weaknesses

- You need Python 2.x to run the script. Version 2.5 or higher should
  be fine.

- Several other weaknesses I haven't thought of yet.
