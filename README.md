haskell-js-html-builder
=================

A monad and monoid instance for Haste.DOM elements. It can be ported to other haskell-js compilers

Haste is a compiler that generates Javascript code from Haskell.

https://github.com/valderman/haste-compiler

The Haste.DOM module define a thin layer over the JavaScript DOM. That makes the creation and manipulation of DOM elements  as painful as in JavaScript.

This package makes the creation of DOM elements easy with a syntax  similar to other haskell HTML generators, using monoids and monads, such is the case of the package blaze-html.

This is an example. `withElem`  is a Haste.DOM call that give the DOM object whose id is "idelem", that has been created "by hand" in Main.hs. The builder takes this element and add content to it:

      main= do
       withElem "idelem" $   build $ do
       div $ do
         div $ do
               p "hello"
               p ! atr "style" "color:red" $   "world"

       return ()

       

The equivalent monoid expression can also be used, by concatenating elements with the operator <>

How to run
----------

install the ghc compiler

install Haste:

    >cabal install haste-compiler

clone haskell-js-html-builder
  
    >git clone http://github.com/agocorona/haskell-js-html-builder.git
    
compile

    >hastec Main.hs
    
browse the Main.html file. In windows simply execute it in the command line:

    >Main.html

Execute it in the same directory where Main.js is, since it references it assuming that it is in the current folder


Status
---------

Standard tags and attributes are not defined except div, p and b. Define your owns. For example:

div cont=  nelem "div" `child`  cont

p cont = nelem "p" `child` cont

b cont = nelem "b" `child` cont

onclick= atr "onclick"
 

How it works
------------


The basic element is a "builder" that has a "hole" parameter and a IO action about what element will be created. The hole will receive the parent (Elem) of the element/s that will be created by the builder. Upon created, an elem  is added to the parent and return itself as parent of the next elements. To append two elements, both are added to the parent.

The Monad instance is there in order to use the do notation, that add a new level of syntax, in the style of the package blaze-html. This monad invokes the same appending mechanism.
