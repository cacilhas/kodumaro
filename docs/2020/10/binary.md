![MoonScript](//cacilhas.cc/img/glider.png)

I’ve seen people not understanding the difference between binary and text.

In this context, binary and text are ways the data is represented.

### Binary protocol

The binary protocol uses bytes to represent data as directly as possible.

Let’s take the number 98 765. Converting from decimal to binary one gets:

1.1000.0001.1100.1101₂

C has a type named `int` that represents integers as 4-byte data. Regrouping the data above in 8-bit chunks (bytes), and completing the 4 bytes with zeros:

00000000.00000001.10000001.11001101₂

Counter-intuitively, the simplier way to represent binary data isn’t binary, but hexadecimal. Each 4 bits turns directly into a hexadecimal digit, so every byte is represented as 2 hexadecimal digits. So we got:

00.01.81.CD₁₆

The mathematical representation of numbers is **big-endian**, which means, the more representative digit comes first. Network uses big-endian too.

Most of the operating systems are **little-endian**, which means, the inverted order from big-endian:

CD.81.01.00₁₆

Considering we’re building a binary protocol. It needs to supply the data type in order to identify which kind of data we’re talking about.

For simplicity (something like [SDXF](http://www.pinpi.com/en/SDXF_2.htm)), let’s use 1-byte type tag. For informing integer, let’s use the number 2:

02.00.01.81.CD₁₆

### Text protocol

Text data are fully represented as strings. So the number 98 765 is represented as the characters needed to write the decimal number:

39.38.37.36.35₁₆

Now it needs a way to tell the system where the string ends. C uses the null character as stop one:

39.38.37.36.35.00₁₆

Pascal starts the string with 2 bytes telling the string length:

05.00.39.38.37.36.35₁₆

But, in text protocols, we cannot use non-text bytes (like 05₁₆), so we need to represent it as text too, using a separator (CR, for example) to distinguish the length from the data:

35.0C.39.38.37.36.35₁₆

Let’s put it all in perspective:

Locally

Network

Binary

CD.81.01.00₁₆

02.00.01.81.CD₁₆

Text

39.38.37.36.35.00₁₆

35.0C.39.38.37.36.35₁₆

### Strings

And what about strings? Over a binary protocol, strings are easily represented, one just needs a type flag, the string length, and the string itself.

Let’s take `Kodumaro` (4B.6F.64.75.6D.61.72.6F₁₆):

01.4B.6F.64.75.6D.61.72.6F.00₁₆

Or, in a network transmission:

01.08.00.4B.6F.64.75.6D.61.72.6F₁₆

Over a text protocol, it usually uses a terminator character on both ends. The most common is the quotation mark (`"`, 22₁₆):

31.30.0C.22.4B.6F.64.75.6D.61.72.6F.22₁₆

Locally

Network

Binary

4B.6F.64.75.6D.61.72.6F.00₁₆

01.08.00.4B.6F.64.75.6D.61.72.6F₁₆

Text

22.4B.6F.64.75.6D.61.72.6F.22.00₁₆

31.30.0C.22.4B.6F.64.75.6D.61.72.6F.22₁₆

### Afterword

I hope you can see how text representations are heavier than binary ones. Things get worse when considering the protocol headers.

Everytime it’s possible, prefer binary representations over text.

* * *

Original post (in Portuguese): [Diferença entre dado binário e dado textual](https://kodumaro.cacilhas.cc/2017/06/binario-texto.html).

Also in [DEV.to](https://dev.to/cacilhas/binary-vs-text-2blo).