# ><>

This is Harpyon's original [fish.py](https://gist.github.com/anonymous/6392418) implementation, extended with debugging capabilities. Inspiration comes from [fishlanguage.com](http://fishlanguage.com) and the debugging functionality offered by the official  [Hexagony](https://github.com/m-ender/hexagony) interpreter.

I really like **><>**, but lacking a proper way of debugging makes programming in it pretty hard. The online interpreter provides a good start, but still requires to sit trough almost all of the animation, manually pausing at the desired point. Furthermore, it is not guaranteed to be up all the time.

In general, functionality of the original *fish.py* interpreter shouldn't have changed. Rather, two things were added as explained in the next two sections.

### Debug view

A debug view prints the program with the last executed instruction in red. Furthermore, it displays

* the current stack
* the current output
* the register
* the direction of the instruction pointer

whenever they are applicable. There are two main ways to trigger debug views.

#### Debug mode

When running the interpreter with the `--debug` flag, a debug view is printed after each instruction is executed. In conjunction with `--tick <seconds>`, this roughly mimicks the online interpreter.

#### Breakpoints

A breakpoint can be put on a cell by a backtick \`. Each time this cell it executed, a debug view is printed. Output to stdout is resumed normally.

### Comment section

When writing a **><>** program, I like to keep some notes with me such as a pseudocode outline of what it should do, examples of stack manipulation sequences or an explanation of code for PPCG answers. Generally, they can be included in the **><>** program without interrupting its functionality, like in the following example from [this answer](http://codegolf.stackexchange.com/a/111300/63647).

    :1%:?vr1(?v1n;
         >n;n0<
    
    :                 Duplicate i (need it if i%1 != 0)
     1                Push 1
      %               Pop i and 1, push i%1
       :              Duplicate top of stack because
                      we need one for the if
        ?v            If i%1 != 0 ------------------------,
          r           Reverse stack so that i is TOS      |
           1(?v       If i > 0 (not < 1)                  |
               1n;      Print 1 and Exit                  |
                      Else                                |
            n0<         Print 0 and --,                   |
         >n           Print n <-------|-------------------'
           ;          Exit <----------'

This is a perfectly valid **><>** program because the instruction pointer will never get to the comment section. When debugging, however, it is not nice to have the explanation printed each time. Or sometimes we will need vertical wrapping around. To this end, I allowed for a comment section at the bottom of a **><>** program.

It is started by double backticks \`\` as the first to characters on a new line. As soon as these are encountered, the last line is removed and the interpreter stops looking for code. The program

    :1%:?vr1(?v1n;
         >n;n0<
    
    ``
    Here can be anything.

is interpreted as

    :1%:?vr1(?v1n;
         >n;n0<

