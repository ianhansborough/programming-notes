#Strict Mode
_reference: <https://www.w3schools.com/js/js_strict.asp>_

Strict mode is declared by adding `"use strict";` to the beginning of a script or a function.
Declared at the beginning of a script, it has global scope (all code in the script will execute in strict mode).

##What does it do?
Strict mode prevents you from using undeclared variables and will cause the compiler to throw an error if it sees one.

##Why?
Strict mode makes it easier to write "secure" Javascript.
It changes previously accepted "bad syntax" into real errors.

#Source Map
_reference: <http://blog.teamtreehouse.com/introduction-source-maps>_

##What is it?
A source map provides a way of mapping code in a compressed file back to it's original position in a source file. 
This means that you can easily debug your application even after your assets have been optimized.

##Why?
Using source maps allow developers to maintain a straightforward debugging environment while at the same time optimiing their site for performance. 