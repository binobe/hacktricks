# Fcrackzip



```text
  fcrackzip searches each zipfile given for encrypted files and tries to guess the password.
       All files must be encrypted with the same  password,  the  more  files  you  provide,  the
       better.

   OPTIONS
       -h, --help
              Prints the version number and (hopefully) some helpful insights.

       -v, --verbose
              Each -v makes the program more verbose.

       -b, --brute-force
              Select  brute  force  mode. This tries all possible combinations of the letters you
              specify.

       -D, --dictionary
              Select dictionary mode. In this mode, fcrackzip will read passwords  from  a  file,
              which  must contain one password per line and should be alphabetically sorted (e.g.
              using sort(1)).

       -c, --charset characterset-specification
              Select the characters to use in brute-force cracking. Must be one of

                a   include all lowercase characters [a-z]
                A   include all uppercase characters [A-Z]
                1   include the digits [0-9]
                !   include [!:$%&/()=?{[]}+*~#]
                :   the following characters upto the end of the spe-
                    cification string are included in the character set.
                    This way you can include any character except binary
                    null (at least under unix).

              For example, a1:$% selects lowercase characters, digits and the dollar and  percent
              signs.

       -p, --init-password string
              Set  initial  (starting)  password  for brute-force searching to string, or use the
              file with the name string to supply passwords for dictionary searching.

       -l, --length min[-max]
              Use an initial password of length min, and check all passwords  upto  passwords  of
              length max (including). You can omit the max parameter.

       -u, --use-unzip
              Try  to  decompress the first file by calling unzip with the guessed password. This
              weeds out false positives when not enough files have been given.

       -m, --method name
              Use method number "name" instead of the default cracking method. The switch  --help
              will  print  a  list of available methods. Use --benchmark to see which method does
              perform best on your machine. The name can also be the number of the method to use.

       -2, --modulo r/m
              Calculate only r/m of the password. Not yet supported.

       -B, --benchmark
              Make a small benchmark, the output is nearly meaningless.

       -V, --validate
              Make some basic checks wether the cracker works.
```



EXAMPLE":

 fcrackzip -D -p ../../tools/rockyou.txt backup.zip 

