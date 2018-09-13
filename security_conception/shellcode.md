## shellcode {#shellcode}

[http://skypher.com/wiki/index.php?title=ASCII_Art](http://skypher.com/wiki/index.php?title=ASCII_Art)

This shellcode was created by manually modifying the original ASCII art in a debugger. It took me a good couple of hours to get working GetPC code and a decoder injected into the first eight lines of the file. I then wrote a tool that encoded and inserted my shellcode into the remaining ASCII Art. The decoder only decodes uppercase characters A-P, which makes it easy to insert the encoded shellcode data without the end result looking much different from the original ASCII Art. The shellcode will only work when it is run in writable &amp; executable memory.TX ``` .èÿÿÿÿñë.

This shellcode was created using ALPHA2 to encode a (second) decoder that scans the data following it for uppercase characters A-P and decodes those as other ALPHA2 decoders would. This allows me to encoded shellcode data into the ASCII Art without the end result looking much different from the original ASCII Art. The shellcode will only work when it is run in writable &amp; executable memory and if ECX points to the base address of the shellcode.

This shellcode was created using Ars Ex Machina Coda. The shellcode will only work when it is run in writable &amp; executable memory and if ECX points to the base address of the shellcode.