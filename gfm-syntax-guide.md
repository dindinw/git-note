The Makedown for Github Guide 
=============================


Install
=======

Install haskell

    sudo apt-get install ghc6 cabal-install
    cabal install cabal-install

Add ~/.cabal/bin to your .bashrc

~~~~
export PATH=~/.cabal/bin:$PATH
~~~~

Install pandoc with syntax highlight

~~~~
cabal update
cabal install pandoc -fhighlighting
~~~~

Fix markdown2pdf (lack dependence for texlive) 

~~~~
sudo apt-get install texlive-latex-recommended 


Headlines 
=========

H1

~~~~
This is an H1
=============
# This is also an H1  
# This is an H1 again (look petty in txt) #
~~~~

H2

~~~~
This is an headline H2
----------------------
## This is also an H2
~~~~

H3-H6

~~~~
### This is an H3
#### This is an H4
##### This is an H5
###### This is an H6
~~~~

Paragraphs
==========

This a regural paragraphs.
with hard warp the line foo bar, foo bar. foo bar, foo bar.foo bar, foo bar.foo
bar, foo bar. foo bar, foo bar.foo bar, foo bar.foo bar, foo bar.foo bar, foo 
bar.foo bar, foo bar.foo bar, foo bar.foo bar, foo bar.foo bar, foo bar.foo bar
, foo bar.foo bar, foo bar.

This is the second paragraph. This time we don't hard wrap the line. so foo bar, foo bar.foo bar. foo bar,foo bar,foo bar. foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.foo bar,foo bar.          


Blockquote
==========

> This is a blockquote with two paragraphs.
> 
> paragraph 2

Some other text

> This is another blockquote.
> 
> > This is the nested blockquote
> 
> Cool?

Bullet List
===========

* fruits
    + apples
        - macintosh
        - red delicious
    + pears
    + peaches

~~~~
* fruits
    + apples
        - macintosh
        - red delicious
    + pears
    + peaches
~~~~

Table
=====

Right     Left     Center     Default
-------   ------ ----------   -------
     12     12        12           12
    123     123       123          123
      1     1          1             1


Code with Syntax
================


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.java}
// Hello World in Java syntax.

class HelloWorld {
	static public void main( String args[] ) {
		System.out.println( "Hello World!" );
	}
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ {.ruby .numberLines}
# Hello World in Ruby syntax.
 
 puts 'Hello world'

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

