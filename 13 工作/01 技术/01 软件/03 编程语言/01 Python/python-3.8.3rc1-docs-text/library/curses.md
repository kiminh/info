"curses" --- 终端字符单元显示的处理
***********************************

======================================================================

"curses" 模块提供了 curses 库的接口，这是可移植高级终端处理的事实标准
。

虽然 curses 在 Unix 环境中使用最为广泛，但也有适用于 Windows，DOS 以及
其他可能的系统的版本。此扩展模块旨在匹配 ncurses 的 API，这是一个部署
在 Linux 和 Unix 的 BSD 变体上的开源 curses 库。

注解:

  每当文档提到 **字符** 时，它可以被指定为一个整数，一个单字符 Unicode
  字符串或者一个单字节的字节字符串。每当此文档提到 **字符串** 时，它可
  以被指定为一个 Unicode 字符串或者一个字节字符串。

注解:

  从 5.4 版本开始，ncurses 库使用 "nl_langinfo" 函数来决定如何解释非
  ASCII 数据。这意味着你需要在程序中调用 "locale.setlocale()" 函数，并
  使用一种系统中可用的编码方法来编码 Unicode 字符串。这个例子使用了系
  统默认的编码:

     import locale
     locale.setlocale(locale.LC_ALL, '')
     code = locale.getpreferredencoding()

  然后使用 *code* 作为 "str.encode()" 调用的编码。

参见:

  模块 "curses.ascii"
     在 ASCII 字符上工作的工具，无论你的区域设置是什么。

  模块 "curses.panel"
     A panel stack extension that adds depth to  curses windows.

  Module "curses.textpad"
     Editable text widget for curses supporting  **Emacs**-like
     bindings.

  用 Python 进行 Curses 编程
     Tutorial material on using curses with Python, by Andrew Kuchling
     and Eric Raymond.

  The Tools/demo/ directory in the Python source distribution contains
  some example programs using the curses bindings provided by this
  module.


函数
====

"curses" 模块定义了以下异常：

exception curses.error

   当 curses 库中函数返回一个错误时引发的异常。

注解:

  Whenever *x* or *y* arguments to a function or a method are
  optional, they default to the current cursor location. Whenever
  *attr* is optional, it defaults to "A_NORMAL".

"curses" 模块定义了以下函数：

curses.baudrate()

   Return the output speed of the terminal in bits per second.  On
   software terminal emulators it will have a fixed high value.
   Included for historical reasons; in former times, it was used to
   write output loops for time delays and occasionally to change
   interfaces depending on the line speed.

curses.beep()

   Emit a short attention sound.

curses.can_change_color()

   Return "True" or "False", depending on whether the programmer can
   change the colors displayed by the terminal.

curses.cbreak()

   Enter cbreak mode.  In cbreak mode (sometimes called "rare" mode)
   normal tty line buffering is turned off and characters are
   available to be read one by one. However, unlike raw mode, special
   characters (interrupt, quit, suspend, and flow control) retain
   their effects on the tty driver and calling program.  Calling first
   "raw()" then "cbreak()" leaves the terminal in cbreak mode.

curses.color_content(color_number)

   Return the intensity of the red, green, and blue (RGB) components
   in the color *color_number*, which must be between "0" and
   "COLORS".  Return a 3-tuple, containing the R,G,B values for the
   given color, which will be between "0" (no component) and "1000"
   (maximum amount of component).

curses.color_pair(color_number)

   Return the attribute value for displaying text in the specified
   color.  This attribute value can be combined with "A_STANDOUT",
   "A_REVERSE", and the other "A_*" attributes.  "pair_number()" is
   the counterpart to this function.

curses.curs_set(visibility)

   Set the cursor state.  *visibility* can be set to "0", "1", or "2",
   for invisible, normal, or very visible.  If the terminal supports
   the visibility requested, return the previous cursor state;
   otherwise raise an exception.  On many terminals, the "visible"
   mode is an underline cursor and the "very visible" mode is a block
   cursor.

curses.def_prog_mode()

   Save the current terminal mode as the "program" mode, the mode when
   the running program is using curses.  (Its counterpart is the
   "shell" mode, for when the program is not in curses.)  Subsequent
   calls to "reset_prog_mode()" will restore this mode.

curses.def_shell_mode()

   Save the current terminal mode as the "shell" mode, the mode when
   the running program is not using curses.  (Its counterpart is the
   "program" mode, when the program is using curses capabilities.)
   Subsequent calls to "reset_shell_mode()" will restore this mode.

curses.delay_output(ms)

   Insert an *ms* millisecond pause in output.

curses.doupdate()

   Update the physical screen.  The curses library keeps two data
   structures, one representing the current physical screen contents
   and a virtual screen representing the desired next state.  The
   "doupdate()" ground updates the physical screen to match the
   virtual screen.

   The virtual screen may be updated by a "noutrefresh()" call after
   write operations such as "addstr()" have been performed on a
   window.  The normal "refresh()" call is simply "noutrefresh()"
   followed by "doupdate()"; if you have to update multiple windows,
   you can speed performance and perhaps reduce screen flicker by
   issuing "noutrefresh()" calls on all windows, followed by a single
   "doupdate()".

curses.echo()

   Enter echo mode.  In echo mode, each character input is echoed to
   the screen as it is entered.

curses.endwin()

   De-initialize the library, and return terminal to normal status.

curses.erasechar()

   Return the user's current erase character as a one-byte bytes
   object.  Under Unix operating systems this is a property of the
   controlling tty of the curses program, and is not set by the curses
   library itself.

curses.filter()

   The "filter()" routine, if used, must be called before "initscr()"
   is called.  The effect is that, during those calls, "LINES" is set
   to "1"; the capabilities "clear", "cup", "cud", "cud1", "cuu1",
   "cuu", "vpa" are disabled; and the "home" string is set to the
   value of "cr". The effect is that the cursor is confined to the
   current line, and so are screen updates.  This may be used for
   enabling character-at-a-time  line editing without touching the
   rest of the screen.

curses.flash()

   Flash the screen.  That is, change it to reverse-video and then
   change it back in a short interval.  Some people prefer such as
   'visible bell' to the audible attention signal produced by
   "beep()".

curses.flushinp()

   Flush all input buffers.  This throws away any  typeahead  that
   has been typed by the user and has not yet been processed by the
   program.

curses.getmouse()

   After "getch()" returns "KEY_MOUSE" to signal a mouse event, this
   method should be call to retrieve the queued mouse event,
   represented as a 5-tuple "(id, x, y, z, bstate)". *id* is an ID
   value used to distinguish multiple devices, and *x*, *y*, *z* are
   the event's coordinates.  (*z* is currently unused.)  *bstate* is
   an integer value whose bits will be set to indicate the type of
   event, and will be the bitwise OR of one or more of the following
   constants, where *n* is the button number from 1 to 4:
   "BUTTONn_PRESSED", "BUTTONn_RELEASED", "BUTTONn_CLICKED",
   "BUTTONn_DOUBLE_CLICKED", "BUTTONn_TRIPLE_CLICKED", "BUTTON_SHIFT",
   "BUTTON_CTRL", "BUTTON_ALT".

curses.getsyx()

   Return the current coordinates of the virtual screen cursor as a
   tuple "(y, x)".  If "leaveok" is currently "True", then return
   "(-1, -1)".

curses.getwin(file)

   Read window related data stored in the file by an earlier
   "putwin()" call. The routine then creates and initializes a new
   window using that data, returning the new window object.

curses.has_colors()

   Return "True" if the terminal can display colors; otherwise, return
   "False".

curses.has_ic()

   Return "True" if the terminal has insert- and delete-character
   capabilities. This function is included for historical reasons
   only, as all modern software terminal emulators have such
   capabilities.

curses.has_il()

   Return "True" if the terminal has insert- and delete-line
   capabilities, or can simulate  them  using scrolling regions. This
   function is included for historical reasons only, as all modern
   software terminal emulators have such capabilities.

curses.has_key(ch)

   Take a key value *ch*, and return "True" if the current terminal
   type recognizes a key with that value.

curses.halfdelay(tenths)

   Used for half-delay mode, which is similar to cbreak mode in that
   characters typed by the user are immediately available to the
   program. However, after blocking for *tenths* tenths of seconds,
   raise an exception if nothing has been typed.  The value of
   *tenths* must be a number between "1" and "255".  Use "nocbreak()"
   to leave half-delay mode.

curses.init_color(color_number, r, g, b)

   Change the definition of a color, taking the number of the color to
   be changed followed by three RGB values (for the amounts of red,
   green, and blue components).  The value of *color_number* must be
   between "0" and "COLORS".  Each of *r*, *g*, *b*, must be a value
   between "0" and "1000".  When "init_color()" is used, all
   occurrences of that color on the screen immediately change to the
   new definition.  This function is a no-op on most terminals; it is
   active only if "can_change_color()" returns "True".

curses.init_pair(pair_number, fg, bg)

   Change the definition of a color-pair.  It takes three arguments:
   the number of the color-pair to be changed, the foreground color
   number, and the background color number.  The value of
   *pair_number* must be between "1" and "COLOR_PAIRS - 1" (the "0"
   color pair is wired to white on black and cannot be changed).  The
   value of *fg* and *bg* arguments must be between "0" and "COLORS".
   If the color-pair was previously initialized, the screen is
   refreshed and all occurrences of that color-pair are changed to the
   new definition.

curses.initscr()

   Initialize the library. Return a window object which represents the
   whole screen.

   注解:

     If there is an error opening the terminal, the underlying curses
     library may cause the interpreter to exit.

curses.is_term_resized(nlines, ncols)

   Return "True" if "resize_term()" would modify the window structure,
   "False" otherwise.

curses.isendwin()

   Return "True" if "endwin()" has been called (that is, the  curses
   library has been deinitialized).

curses.keyname(k)

   Return the name of the key numbered *k* as a bytes object.  The
   name of a key generating printable ASCII character is the key's
   character.  The name of a control-key combination is a two-byte
   bytes object consisting of a caret ("b'^'") followed by the
   corresponding printable ASCII character.  The name of an alt-key
   combination (128--255) is a bytes object consisting of the prefix
   "b'M-'" followed by the name of the corresponding ASCII character.

curses.killchar()

   Return the user's current line kill character as a one-byte bytes
   object. Under Unix operating systems this is a property of the
   controlling tty of the curses program, and is not set by the curses
   library itself.

curses.longname()

   Return a bytes object containing the terminfo long name field
   describing the current terminal.  The maximum length of a verbose
   description is 128 characters.  It is defined only after the call
   to "initscr()".

curses.meta(flag)

   If *flag* is "True", allow 8-bit characters to be input.  If *flag*
   is "False",  allow only 7-bit chars.

curses.mouseinterval(interval)

   Set the maximum time in milliseconds that can elapse between press
   and release events in order for them to be recognized as a click,
   and return the previous interval value.  The default value is 200
   msec, or one fifth of a second.

curses.mousemask(mousemask)

   Set the mouse events to be reported, and return a tuple
   "(availmask, oldmask)".   *availmask* indicates which of the
   specified mouse events can be reported; on complete failure it
   returns "0".  *oldmask* is the previous value of the given window's
   mouse event mask.  If this function is never called, no mouse
   events are ever reported.

curses.napms(ms)

   Sleep for *ms* milliseconds.

curses.newpad(nlines, ncols)

   Create and return a pointer to a new pad data structure with the
   given number of lines and columns.  Return a pad as a window
   object.

   A pad is like a window, except that it is not restricted by the
   screen size, and is not necessarily associated with a particular
   part of the screen.  Pads can be used when a large window is
   needed, and only a part of the window will be on the screen at one
   time.  Automatic refreshes of pads (such as from scrolling or
   echoing of input) do not occur.  The "refresh()" and
   "noutrefresh()" methods of a pad require 6 arguments to specify the
   part of the pad to be displayed and the location on the screen to
   be used for the display. The arguments are *pminrow*, *pmincol*,
   *sminrow*, *smincol*, *smaxrow*, *smaxcol*; the *p* arguments refer
   to the upper left corner of the pad region to be displayed and the
   *s* arguments define a clipping box on the screen within which the
   pad region is to be displayed.

curses.newwin(nlines, ncols)
curses.newwin(nlines, ncols, begin_y, begin_x)

   Return a new window, whose left-upper corner is at  "(begin_y,
   begin_x)", and whose height/width is  *nlines*/*ncols*.

   By default, the window will extend from the  specified position to
   the lower right corner of the screen.

curses.nl()

   Enter newline mode.  This mode translates the return key into
   newline on input, and translates newline into return and line-feed
   on output. Newline mode is initially on.

curses.nocbreak()

   Leave cbreak mode.  Return to normal "cooked" mode with line
   buffering.

curses.noecho()

   Leave echo mode.  Echoing of input characters is turned off.

curses.nonl()

   Leave newline mode.  Disable translation of return into newline on
   input, and disable low-level translation of newline into
   newline/return on output (but this does not change the behavior of
   "addch('\n')", which always does the equivalent of return and line
   feed on the virtual screen).  With translation off, curses can
   sometimes speed up vertical motion a little; also, it will be able
   to detect the return key on input.

curses.noqiflush()

   When the "noqiflush()" routine is used, normal flush of input and
   output queues associated with the "INTR", "QUIT" and "SUSP"
   characters will not be done.  You may want to call "noqiflush()" in
   a signal handler if you want output to continue as though the
   interrupt had not occurred, after the handler exits.

curses.noraw()

   Leave raw mode. Return to normal "cooked" mode with line buffering.

curses.pair_content(pair_number)

   Return a tuple "(fg, bg)" containing the colors for the requested
   color pair. The value of *pair_number* must be between "1" and
   "COLOR_PAIRS - 1".

curses.pair_number(attr)

   Return the number of the color-pair set by the attribute value
   *attr*. "color_pair()" is the counterpart to this function.

curses.putp(str)

   Equivalent to "tputs(str, 1, putchar)"; emit the value of a
   specified terminfo capability for the current terminal.  Note that
   the output of "putp()" always goes to standard output.

curses.qiflush([flag])

   If *flag* is "False", the effect is the same as calling
   "noqiflush()". If *flag* is "True", or no argument is provided, the
   queues will be flushed when these control characters are read.

curses.raw()

   Enter raw mode.  In raw mode, normal line buffering and  processing
   of interrupt, quit, suspend, and flow control keys are turned off;
   characters are presented to curses input functions one by one.

curses.reset_prog_mode()

   Restore the  terminal  to "program" mode, as previously saved  by
   "def_prog_mode()".

curses.reset_shell_mode()

   Restore the  terminal  to "shell" mode, as previously saved  by
   "def_shell_mode()".

curses.resetty()

   Restore the state of the terminal modes to what it was at the last
   call to "savetty()".

curses.resize_term(nlines, ncols)

   Backend function used by "resizeterm()", performing most of the
   work; when resizing the windows, "resize_term()" blank-fills the
   areas that are extended.  The calling application should fill in
   these areas with appropriate data.  The "resize_term()" function
   attempts to resize all windows.  However, due to the calling
   convention of pads, it is not possible to resize these without
   additional interaction with the application.

curses.resizeterm(nlines, ncols)

   Resize the standard and current windows to the specified
   dimensions, and adjusts other bookkeeping data used by the curses
   library that record the window dimensions (in particular the
   SIGWINCH handler).

curses.savetty()

   Save the current state of the terminal modes in a buffer, usable by
   "resetty()".

curses.setsyx(y, x)

   Set the virtual screen cursor to *y*, *x*. If *y* and *x* are both
   "-1", then "leaveok" is set "True".

curses.setupterm(term=None, fd=-1)

   Initialize the terminal.  *term* is a string giving the terminal
   name, or "None"; if omitted or "None", the value of the "TERM"
   environment variable will be used.  *fd* is the file descriptor to
   which any initialization sequences will be sent; if not supplied or
   "-1", the file descriptor for "sys.stdout" will be used.

curses.start_color()

   Must be called if the programmer wants to use colors, and before
   any other color manipulation routine is called.  It is good
   practice to call this routine right after "initscr()".

   "start_color()" initializes eight basic colors (black, red,  green,
   yellow, blue, magenta, cyan, and white), and two global variables
   in the "curses" module, "COLORS" and "COLOR_PAIRS", containing the
   maximum number of colors and color-pairs the terminal can support.
   It also restores the colors on the terminal to the values they had
   when the terminal was just turned on.

curses.termattrs()

   Return a logical OR of all video attributes supported by the
   terminal.  This information is useful when a curses program needs
   complete control over the appearance of the screen.

curses.termname()

   Return the value of the environment variable "TERM", as a bytes
   object, truncated to 14 characters.

curses.tigetflag(capname)

   Return the value of the Boolean capability corresponding to the
   terminfo capability name *capname* as an integer.  Return the value
   "-1" if *capname* is not a Boolean capability, or "0" if it is
   canceled or absent from the terminal description.

curses.tigetnum(capname)

   Return the value of the numeric capability corresponding to the
   terminfo capability name *capname* as an integer.  Return the value
   "-2" if *capname* is not a numeric capability, or "-1" if it is
   canceled or absent from the terminal description.

curses.tigetstr(capname)

   Return the value of the string capability corresponding to the
   terminfo capability name *capname* as a bytes object.  Return
   "None" if *capname* is not a terminfo "string capability", or is
   canceled or absent from the terminal description.

curses.tparm(str[, ...])

   Instantiate the bytes object *str* with the supplied parameters,
   where *str* should be a parameterized string obtained from the
   terminfo database.  E.g. "tparm(tigetstr("cup"), 5, 3)" could
   result in "b'\033[6;4H'", the exact result depending on terminal
   type.

curses.typeahead(fd)

   Specify that the file descriptor *fd* be used for typeahead
   checking.  If *fd* is "-1", then no typeahead checking is done.

   The curses library does "line-breakout optimization" by looking for
   typeahead periodically while updating the screen.  If input is
   found, and it is coming from a tty, the current update is postponed
   until refresh or doupdate is called again, allowing faster response
   to commands typed in advance. This function allows specifying a
   different file descriptor for typeahead checking.

curses.unctrl(ch)

   Return a bytes object which is a printable representation of the
   character *ch*. Control characters are represented as a caret
   followed by the character, for example as "b'^C'". Printing
   characters are left as they are.

curses.ungetch(ch)

   Push *ch* so the next "getch()" will return it.

   注解:

     Only one *ch* can be pushed before "getch()" is called.

curses.update_lines_cols()

   Update "LINES" and "COLS". Useful for detecting manual screen
   resize.

   3.5 新版功能.

curses.unget_wch(ch)

   Push *ch* so the next "get_wch()" will return it.

   注解:

     Only one *ch* can be pushed before "get_wch()" is called.

   3.3 新版功能.

curses.ungetmouse(id, x, y, z, bstate)

   Push a "KEY_MOUSE" event onto the input queue, associating the
   given state data with it.

curses.use_env(flag)

   If used, this function should be called before "initscr()" or
   newterm are called.  When *flag* is "False", the values of lines
   and columns specified in the terminfo database will be used, even
   if environment variables "LINES" and "COLUMNS" (used by default)
   are set, or if curses is running in a window (in which case default
   behavior would be to use the window size if "LINES" and "COLUMNS"
   are not set).

curses.use_default_colors()

   Allow use of default values for colors on terminals supporting this
   feature. Use this to support transparency in your application.  The
   default color is assigned to the color number "-1". After calling
   this function,  "init_pair(x, curses.COLOR_RED, -1)" initializes,
   for instance, color pair *x* to a red foreground color on the
   default background.

curses.wrapper(func, ...)

   Initialize curses and call another callable object, *func*, which
   should be the rest of your curses-using application.  If the
   application raises an exception, this function will restore the
   terminal to a sane state before re-raising the exception and
   generating a traceback.  The callable object *func* is then passed
   the main window 'stdscr' as its first argument, followed by any
   other arguments passed to "wrapper()".  Before calling *func*,
   "wrapper()" turns on cbreak mode, turns off echo, enables the
   terminal keypad, and initializes colors if the terminal has color
   support.  On exit (whether normally or by exception) it restores
   cooked mode, turns on echo, and disables the terminal keypad.


Window Objects
==============

Window objects, as returned by "initscr()" and "newwin()" above, have
the following methods and attributes:

window.addch(ch[, attr])
window.addch(y, x, ch[, attr])

   Paint character *ch* at "(y, x)" with attributes *attr*,
   overwriting any character previously painter at that location.  By
   default, the character position and attributes are the current
   settings for the window object.

   注解:

     Writing outside the window, subwindow, or pad raises a
     "curses.error". Attempting to write to the lower right corner of
     a window, subwindow, or pad will cause an exception to be raised
     after the character is printed.

window.addnstr(str, n[, attr])
window.addnstr(y, x, str, n[, attr])

   Paint at most *n* characters of the character string *str* at "(y,
   x)" with attributes *attr*, overwriting anything previously on the
   display.

window.addstr(str[, attr])
window.addstr(y, x, str[, attr])

   Paint the character string *str* at "(y, x)" with attributes
   *attr*, overwriting anything previously on the display.

   注解:

     * Writing outside the window, subwindow, or pad raises
       "curses.error". Attempting to write to the lower right corner
       of a window, subwindow, or pad will cause an exception to be
       raised after the string is printed.

     * A bug in ncurses, the backend for this Python module, can cause
       SegFaults when resizing windows. This is fixed in
       ncurses-6.1-20190511.  If you are stuck with an earlier
       ncurses, you can avoid triggering this if you do not call
       "addstr()" with a *str* that has embedded newlines.  Instead,
       call "addstr()" separately for each line.

window.attroff(attr)

   Remove attribute *attr* from the "background" set applied to all
   writes to the current window.

window.attron(attr)

   Add attribute *attr* from the "background" set applied to all
   writes to the current window.

window.attrset(attr)

   Set the "background" set of attributes to *attr*.  This set is
   initially "0" (no attributes).

window.bkgd(ch[, attr])

   Set the background property of the window to the character *ch*,
   with attributes *attr*.  The change is then applied to every
   character position in that window:

   * The attribute of every character in the window  is changed to the
     new background attribute.

   * Wherever  the  former background character appears, it is changed
     to the new background character.

window.bkgdset(ch[, attr])

   Set the window's background.  A window's background consists of a
   character and any combination of attributes.  The attribute part of
   the background is combined (OR'ed) with all non-blank characters
   that are written into the window.  Both the character and attribute
   parts of the background are combined with the blank characters.
   The background becomes a property of the character and moves with
   the character through any scrolling and insert/delete
   line/character operations.

window.border([ls[, rs[, ts[, bs[, tl[, tr[, bl[, br]]]]]]]])

   在窗口边缘绘制边框。每个参数指定用于边界特定部分的字符;请参阅下表了
   解更多详情。

   注解:

     A "0" value for any parameter will cause the default character to
     be used for that parameter.  Keyword parameters can *not* be
     used.  The defaults are listed in this table:

   +-------------+-----------------------+-------------------------+
   | 参数        | 描述                  | 默认值                  |
   |=============|=======================|=========================|
   | *ls*        | 左侧                  | "ACS_VLINE"             |
   +-------------+-----------------------+-------------------------+
   | *rs*        | 右侧                  | "ACS_VLINE"             |
   +-------------+-----------------------+-------------------------+
   | *ts*        | 顶部                  | "ACS_HLINE"             |
   +-------------+-----------------------+-------------------------+
   | *bs*        | 底部                  | "ACS_HLINE"             |
   +-------------+-----------------------+-------------------------+
   | *tl*        | 左上角                | "ACS_ULCORNER"          |
   +-------------+-----------------------+-------------------------+
   | *tr*        | 右上角                | "ACS_URCORNER"          |
   +-------------+-----------------------+-------------------------+
   | *bl*        | 左下角                | "ACS_LLCORNER"          |
   +-------------+-----------------------+-------------------------+
   | *br*        | 右下角                | "ACS_LRCORNER"          |
   +-------------+-----------------------+-------------------------+

window.box([vertch, horch])

   Similar to "border()", but both *ls* and *rs* are *vertch* and both
   *ts* and *bs* are *horch*.  The default corner characters are
   always used by this function.

window.chgat(attr)
window.chgat(num, attr)
window.chgat(y, x, attr)
window.chgat(y, x, num, attr)

   Set the attributes of *num* characters at the current cursor
   position, or at position "(y, x)" if supplied. If *num* is not
   given or is "-1", the attribute will be set on all the characters
   to the end of the line.  This function moves cursor to position
   "(y, x)" if supplied. The changed line will be touched using the
   "touchline()" method so that the contents will be redisplayed by
   the next window refresh.

window.clear()

   Like "erase()", but also cause the whole window to be repainted
   upon next call to "refresh()".

window.clearok(flag)

   If *flag* is "True", the next call to "refresh()" will clear the
   window completely.

window.clrtobot()

   Erase from cursor to the end of the window: all lines below the
   cursor are deleted, and then the equivalent of "clrtoeol()" is
   performed.

window.clrtoeol()

   Erase from cursor to the end of the line.

window.cursyncup()

   Update the current cursor position of all the ancestors of the
   window to reflect the current cursor position of the window.

window.delch([y, x])

   Delete any character at "(y, x)".

window.deleteln()

   Delete the line under the cursor. All following lines are moved up
   by one line.

window.derwin(begin_y, begin_x)
window.derwin(nlines, ncols, begin_y, begin_x)

   An abbreviation for "derive window", "derwin()" is the same as
   calling "subwin()", except that *begin_y* and *begin_x* are
   relative to the origin of the window, rather than relative to the
   entire screen.  Return a window object for the derived window.

window.echochar(ch[, attr])

   Add character *ch* with attribute *attr*, and immediately  call
   "refresh()" on the window.

window.enclose(y, x)

   Test whether the given pair of screen-relative character-cell
   coordinates are enclosed by the given window, returning "True" or
   "False".  It is useful for determining what subset of the screen
   windows enclose the location of a mouse event.

window.encoding

   Encoding used to encode method arguments (Unicode strings and
   characters). The encoding attribute is inherited from the parent
   window when a subwindow is created, for example with
   "window.subwin()". By default, the locale encoding is used (see
   "locale.getpreferredencoding()").

   3.3 新版功能.

window.erase()

   Clear the window.

window.getbegyx()

   Return a tuple "(y, x)" of co-ordinates of upper-left corner.

window.getbkgd()

   Return the given window's current background character/attribute
   pair.

window.getch([y, x])

   Get a character. Note that the integer returned does *not* have to
   be in ASCII range: function keys, keypad keys and so on are
   represented by numbers higher than 255.  In no-delay mode, return
   "-1" if there is no input, otherwise wait until a key is pressed.

window.get_wch([y, x])

   Get a wide character. Return a character for most keys, or an
   integer for function keys, keypad keys, and other special keys. In
   no-delay mode, raise an exception if there is no input.

   3.3 新版功能.

window.getkey([y, x])

   Get a character, returning a string instead of an integer, as
   "getch()" does. Function keys, keypad keys and other special keys
   return a multibyte string containing the key name.  In no-delay
   mode, raise an exception if there is no input.

window.getmaxyx()

   Return a tuple "(y, x)" of the height and width of the window.

window.getparyx()

   Return the beginning coordinates of this window relative to its
   parent window as a tuple "(y, x)".  Return "(-1, -1)" if this
   window has no parent.

window.getstr()
window.getstr(n)
window.getstr(y, x)
window.getstr(y, x, n)

   Read a bytes object from the user, with primitive line editing
   capacity.

window.getyx()

   Return a tuple "(y, x)" of current cursor position  relative to the
   window's upper-left corner.

window.hline(ch, n)
window.hline(y, x, ch, n)

   Display a horizontal line starting at "(y, x)" with length *n*
   consisting of the character *ch*.

window.idcok(flag)

   If *flag* is "False", curses no longer considers using the hardware
   insert/delete character feature of the terminal; if *flag* is
   "True", use of character insertion and deletion is enabled.  When
   curses is first initialized, use of character insert/delete is
   enabled by default.

window.idlok(flag)

   If *flag* is "True", "curses" will try and use hardware line
   editing facilities. Otherwise, line insertion/deletion are
   disabled.

window.immedok(flag)

   If *flag* is "True", any change in the window image automatically
   causes the window to be refreshed; you no longer have to call
   "refresh()" yourself. However, it may degrade performance
   considerably, due to repeated calls to wrefresh.  This option is
   disabled by default.

window.inch([y, x])

   Return the character at the given position in the window. The
   bottom 8 bits are the character proper, and upper bits are the
   attributes.

window.insch(ch[, attr])
window.insch(y, x, ch[, attr])

   Paint character *ch* at "(y, x)" with attributes *attr*, moving the
   line from position *x* right by one character.

window.insdelln(nlines)

   Insert *nlines* lines into the specified window above the current
   line.  The *nlines* bottom lines are lost.  For negative *nlines*,
   delete *nlines* lines starting with the one under the cursor, and
   move the remaining lines up.  The bottom *nlines* lines are
   cleared.  The current cursor position remains the same.

window.insertln()

   Insert a blank line under the cursor. All following lines are moved
   down by one line.

window.insnstr(str, n[, attr])
window.insnstr(y, x, str, n[, attr])

   Insert a character string (as many characters as will fit on the
   line) before the character under the cursor, up to *n* characters.
   If *n* is zero or negative, the entire string is inserted. All
   characters to the right of the cursor are shifted right, with the
   rightmost characters on the line being lost. The cursor position
   does not change (after moving to *y*, *x*, if specified).

window.insstr(str[, attr])
window.insstr(y, x, str[, attr])

   Insert a character string (as many characters as will fit on the
   line) before the character under the cursor.  All characters to the
   right of the cursor are shifted right, with the rightmost
   characters on the line being lost.  The cursor position does not
   change (after moving to *y*, *x*, if specified).

window.instr([n])
window.instr(y, x[, n])

   Return a bytes object of characters, extracted from the window
   starting at the current cursor position, or at *y*, *x* if
   specified. Attributes are stripped from the characters.  If *n* is
   specified, "instr()" returns a string at most *n* characters long
   (exclusive of the trailing NUL).

window.is_linetouched(line)

   Return "True" if the specified line was modified since the last
   call to "refresh()"; otherwise return "False".  Raise a
   "curses.error" exception if *line* is not valid for the given
   window.

window.is_wintouched()

   Return "True" if the specified window was modified since the last
   call to "refresh()"; otherwise return "False".

window.keypad(flag)

   If *flag* is "True", escape sequences generated by some keys
   (keypad,  function keys) will be interpreted by "curses". If *flag*
   is "False", escape sequences will be left as is in the input
   stream.

window.leaveok(flag)

   If *flag* is "True", cursor is left where it is on update, instead
   of being at "cursor position."  This reduces cursor movement where
   possible. If possible the cursor will be made invisible.

   If *flag* is "False", cursor will always be at "cursor position"
   after an update.

window.move(new_y, new_x)

   Move cursor to "(new_y, new_x)".

window.mvderwin(y, x)

   Move the window inside its parent window.  The screen-relative
   parameters of the window are not changed.  This routine is used to
   display different parts of the parent window at the same physical
   position on the screen.

window.mvwin(new_y, new_x)

   Move the window so its upper-left corner is at "(new_y, new_x)".

window.nodelay(flag)

   If *flag* is "True", "getch()" will be non-blocking.

window.notimeout(flag)

   If *flag* is "True", escape sequences will not be timed out.

   If *flag* is "False", after a few milliseconds, an escape sequence
   will not be interpreted, and will be left in the input stream as
   is.

window.noutrefresh()

   Mark for refresh but wait.  This function updates the data
   structure representing the desired state of the window, but does
   not force an update of the physical screen.  To accomplish that,
   call  "doupdate()".

window.overlay(destwin[, sminrow, smincol, dminrow, dmincol, dmaxrow, dmaxcol])

   Overlay the window on top of *destwin*. The windows need not be the
   same size, only the overlapping region is copied. This copy is non-
   destructive, which means that the current background character does
   not overwrite the old contents of *destwin*.

   To get fine-grained control over the copied region, the second form
   of "overlay()" can be used. *sminrow* and *smincol* are the upper-
   left coordinates of the source window, and the other variables mark
   a rectangle in the destination window.

window.overwrite(destwin[, sminrow, smincol, dminrow, dmincol, dmaxrow, dmaxcol])

   Overwrite the window on top of *destwin*. The windows need not be
   the same size, in which case only the overlapping region is copied.
   This copy is destructive, which means that the current background
   character overwrites the old contents of *destwin*.

   To get fine-grained control over the copied region, the second form
   of "overwrite()" can be used. *sminrow* and *smincol* are the
   upper-left coordinates of the source window, the other variables
   mark a rectangle in the destination window.

window.putwin(file)

   Write all data associated with the window into the provided file
   object.  This information can be later retrieved using the
   "getwin()" function.

window.redrawln(beg, num)

   Indicate that the *num* screen lines, starting at line *beg*, are
   corrupted and should be completely redrawn on the next "refresh()"
   call.

window.redrawwin()

   Touch the entire window, causing it to be completely redrawn on the
   next "refresh()" call.

window.refresh([pminrow, pmincol, sminrow, smincol, smaxrow, smaxcol])

   Update the display immediately (sync actual screen with previous
   drawing/deleting methods).

   The 6 optional arguments can only be specified when the window is a
   pad created with "newpad()".  The additional parameters are needed
   to indicate what part of the pad and screen are involved. *pminrow*
   and *pmincol* specify the upper left-hand corner of the rectangle
   to be displayed in the pad.  *sminrow*, *smincol*, *smaxrow*, and
   *smaxcol* specify the edges of the rectangle to be displayed on the
   screen.  The lower right-hand corner of the rectangle to be
   displayed in the pad is calculated from the screen coordinates,
   since the rectangles must be the same size.  Both rectangles must
   be entirely contained within their respective structures.  Negative
   values of *pminrow*, *pmincol*, *sminrow*, or *smincol* are treated
   as if they were zero.

window.resize(nlines, ncols)

   Reallocate storage for a curses window to adjust its dimensions to
   the specified values.  If either dimension is larger than the
   current values, the window's data is filled with blanks that have
   the current background rendition (as set by "bkgdset()") merged
   into them.

window.scroll([lines=1])

   Scroll the screen or scrolling region upward by *lines* lines.

window.scrollok(flag)

   Control what happens when the cursor of a window is moved off the
   edge of the window or scrolling region, either as a result of a
   newline action on the bottom line, or typing the last character of
   the last line.  If *flag* is "False", the cursor is left on the
   bottom line.  If *flag* is "True", the window is scrolled up one
   line.  Note that in order to get the physical scrolling effect on
   the terminal, it is also necessary to call "idlok()".

window.setscrreg(top, bottom)

   Set the scrolling region from line *top* to line *bottom*. All
   scrolling actions will take place in this region.

window.standend()

   Turn off the standout attribute.  On some terminals this has the
   side effect of turning off all attributes.

window.standout()

   Turn on attribute *A_STANDOUT*.

window.subpad(begin_y, begin_x)
window.subpad(nlines, ncols, begin_y, begin_x)

   Return a sub-window, whose upper-left corner is at "(begin_y,
   begin_x)", and whose width/height is *ncols*/*nlines*.

window.subwin(begin_y, begin_x)
window.subwin(nlines, ncols, begin_y, begin_x)

   Return a sub-window, whose upper-left corner is at "(begin_y,
   begin_x)", and whose width/height is *ncols*/*nlines*.

   By default, the sub-window will extend from the specified position
   to the lower right corner of the window.

window.syncdown()

   Touch each location in the window that has been touched in any of
   its ancestor windows.  This routine is called by "refresh()", so it
   should almost never be necessary to call it manually.

window.syncok(flag)

   If *flag* is "True", then "syncup()" is called automatically
   whenever there is a change in the window.

window.syncup()

   Touch all locations in ancestors of the window that have been
   changed in  the window.

window.timeout(delay)

   Set blocking or non-blocking read behavior for the window.  If
   *delay* is negative, blocking read is used (which will wait
   indefinitely for input).  If *delay* is zero, then non-blocking
   read is used, and "getch()" will return "-1" if no input is
   waiting.  If *delay* is positive, then "getch()" will block for
   *delay* milliseconds, and return "-1" if there is still no input at
   the end of that time.

window.touchline(start, count[, changed])

   Pretend *count* lines have been changed, starting with line
   *start*.  If *changed* is supplied, it specifies whether the
   affected lines are marked as having been changed (*changed*"=True")
   or unchanged (*changed*"=False").

window.touchwin()

   Pretend the whole window has been changed, for purposes of drawing
   optimizations.

window.untouchwin()

   Mark all lines in  the  window  as unchanged since the last call to
   "refresh()".

window.vline(ch, n)
window.vline(y, x, ch, n)

   Display a vertical line starting at "(y, x)" with length *n*
   consisting of the character *ch*.


常量
====

The "curses" module defines the following data members:

curses.ERR

   Some curses routines  that  return  an integer, such as "getch()",
   return "ERR" upon failure.

curses.OK

   Some curses routines  that  return  an integer, such as  "napms()",
   return "OK" upon success.

curses.version

   A bytes object representing the current version of the module.
   Also available as "__version__".

curses.ncurses_version

   A named tuple containing the three components of the ncurses
   library version: *major*, *minor*, and *patch*.  All values are
   integers.  The components can also be accessed by name,  so
   "curses.ncurses_version[0]" is equivalent to
   "curses.ncurses_version.major" and so on.

   Availability: if the ncurses library is used.

   3.8 新版功能.

Some constants are available to specify character cell attributes. The
exact constants available are system dependent.

+--------------------+---------------------------------+
| 属性               | 含义                            |
|====================|=================================|
| "A_ALTCHARSET"     | 备用字符集模式                  |
+--------------------+---------------------------------+
| "A_BLINK"          | 闪烁模式                        |
+--------------------+---------------------------------+
| "A_BOLD"           | 粗体模式                        |
+--------------------+---------------------------------+
| "A_DIM"            | 暗淡模式                        |
+--------------------+---------------------------------+
| "A_INVIS"          | 不可见或空白模式                |
+--------------------+---------------------------------+
| "A_ITALIC"         | 斜体模式                        |
+--------------------+---------------------------------+
| "A_NORMAL"         | 正常属性                        |
+--------------------+---------------------------------+
| "A_PROTECT"        | 保护模式                        |
+--------------------+---------------------------------+
| "A_REVERSE"        | 反转背景色和前景色              |
+--------------------+---------------------------------+
| "A_STANDOUT"       | 突出模式                        |
+--------------------+---------------------------------+
| "A_UNDERLINE"      | 下划线模式                      |
+--------------------+---------------------------------+
| "A_HORIZONTAL"     | 水平突出显示                    |
+--------------------+---------------------------------+
| "A_LEFT"           | 左高亮                          |
+--------------------+---------------------------------+
| "A_LOW"            | 底部高亮                        |
+--------------------+---------------------------------+
| "A_RIGHT"          | 右高亮                          |
+--------------------+---------------------------------+
| "A_TOP"            | 顶部高亮                        |
+--------------------+---------------------------------+
| "A_VERTICAL"       | 垂直突出显示                    |
+--------------------+---------------------------------+
| "A_CHARTEXT"       | 用于提取字符的位掩码            |
+--------------------+---------------------------------+

3.7 新版功能: "A_ITALIC" was added.

有几个常量可用于提取某些方法返回的相应属性。

+--------------------+---------------------------------+
| 位掩码             | 含义                            |
|====================|=================================|
| "A_ATTRIBUTES"     | 用于提取属性的位掩码            |
+--------------------+---------------------------------+
| "A_CHARTEXT"       | 用于提取字符的位掩码            |
+--------------------+---------------------------------+
| "A_COLOR"          | 用于提取颜色对字段信息的位掩码  |
+--------------------+---------------------------------+

键由名称以 "KEY_" 开头的整数常量引用。确切的可用键取决于系统。

+---------------------+----------------------------------------------+
| 关键常数            | 键                                           |
|=====================|==============================================|
| "KEY_MIN"           | 最小键值                                     |
+---------------------+----------------------------------------------+
| "KEY_BREAK"         | 中断键（不可靠）                             |
+---------------------+----------------------------------------------+
| "KEY_DOWN"          | 向下箭头                                     |
+---------------------+----------------------------------------------+
| "KEY_UP"            | 向上箭头                                     |
+---------------------+----------------------------------------------+
| "KEY_LEFT"          | 向左箭头                                     |
+---------------------+----------------------------------------------+
| "KEY_RIGHT"         | 向右箭头                                     |
+---------------------+----------------------------------------------+
| "KEY_HOME"          | Home key (upward+left arrow)                 |
+---------------------+----------------------------------------------+
| "KEY_BACKSPACE"     | 退格（不可靠）                               |
+---------------------+----------------------------------------------+
| "KEY_F0"            | Function keys.  Up to 64 function keys are   |
|                     | supported.                                   |
+---------------------+----------------------------------------------+
| "KEY_Fn"            | Value of function key *n*                    |
+---------------------+----------------------------------------------+
| "KEY_DL"            | 删除行                                       |
+---------------------+----------------------------------------------+
| "KEY_IL"            | 插入行                                       |
+---------------------+----------------------------------------------+
| "KEY_DC"            | Delete character                             |
+---------------------+----------------------------------------------+
| "KEY_IC"            | Insert char or enter insert mode             |
+---------------------+----------------------------------------------+
| "KEY_EIC"           | Exit insert char mode                        |
+---------------------+----------------------------------------------+
| "KEY_CLEAR"         | Clear screen                                 |
+---------------------+----------------------------------------------+
| "KEY_EOS"           | Clear to end of screen                       |
+---------------------+----------------------------------------------+
| "KEY_EOL"           | Clear to end of line                         |
+---------------------+----------------------------------------------+
| "KEY_SF"            | Scroll 1 line forward                        |
+---------------------+----------------------------------------------+
| "KEY_SR"            | Scroll 1 line backward (reverse)             |
+---------------------+----------------------------------------------+
| "KEY_NPAGE"         | 下一页                                       |
+---------------------+----------------------------------------------+
| "KEY_PPAGE"         | 上一页                                       |
+---------------------+----------------------------------------------+
| "KEY_STAB"          | Set tab                                      |
+---------------------+----------------------------------------------+
| "KEY_CTAB"          | Clear tab                                    |
+---------------------+----------------------------------------------+
| "KEY_CATAB"         | Clear all tabs                               |
+---------------------+----------------------------------------------+
| "KEY_ENTER"         | Enter or send (unreliable)                   |
+---------------------+----------------------------------------------+
| "KEY_SRESET"        | Soft (partial) reset (unreliable)            |
+---------------------+----------------------------------------------+
| "KEY_RESET"         | Reset or hard reset (unreliable)             |
+---------------------+----------------------------------------------+
| "KEY_PRINT"         | 打印                                         |
+---------------------+----------------------------------------------+
| "KEY_LL"            | Home down or bottom (lower left)             |
+---------------------+----------------------------------------------+
| "KEY_A1"            | 键盘的左上角                                 |
+---------------------+----------------------------------------------+
| "KEY_A3"            | 键盘的右上角                                 |
+---------------------+----------------------------------------------+
| "KEY_B2"            | 键盘的中心                                   |
+---------------------+----------------------------------------------+
| "KEY_C1"            | 键盘左下方                                   |
+---------------------+----------------------------------------------+
| "KEY_C3"            | 键盘右下方                                   |
+---------------------+----------------------------------------------+
| "KEY_BTAB"          | Back tab                                     |
+---------------------+----------------------------------------------+
| "KEY_BEG"           | Beg (beginning)                              |
+---------------------+----------------------------------------------+
| "KEY_CANCEL"        | 取消                                         |
+---------------------+----------------------------------------------+
| "KEY_CLOSE"         | 关闭                                         |
+---------------------+----------------------------------------------+
| "KEY_COMMAND"       | Cmd (命令行)                                 |
+---------------------+----------------------------------------------+
| "KEY_COPY"          | 复制                                         |
+---------------------+----------------------------------------------+
| "KEY_CREATE"        | 创建                                         |
+---------------------+----------------------------------------------+
| "KEY_END"           | End                                          |
+---------------------+----------------------------------------------+
| "KEY_EXIT"          | 退出                                         |
+---------------------+----------------------------------------------+
| "KEY_FIND"          | 查找                                         |
+---------------------+----------------------------------------------+
| "KEY_HELP"          | 帮助                                         |
+---------------------+----------------------------------------------+
| "KEY_MARK"          | 标记                                         |
+---------------------+----------------------------------------------+
| "KEY_MESSAGE"       | 消息                                         |
+---------------------+----------------------------------------------+
| "KEY_MOVE"          | 移动                                         |
+---------------------+----------------------------------------------+
| "KEY_NEXT"          | 下一个                                       |
+---------------------+----------------------------------------------+
| "KEY_OPEN"          | 打开                                         |
+---------------------+----------------------------------------------+
| "KEY_OPTIONS"       | 选项                                         |
+---------------------+----------------------------------------------+
| "KEY_PREVIOUS"      | Prev (previous)                              |
+---------------------+----------------------------------------------+
| "KEY_REDO"          | 重做                                         |
+---------------------+----------------------------------------------+
| "KEY_REFERENCE"     | Ref (reference)                              |
+---------------------+----------------------------------------------+
| "KEY_REFRESH"       | 刷新                                         |
+---------------------+----------------------------------------------+
| "KEY_REPLACE"       | 替换                                         |
+---------------------+----------------------------------------------+
| "KEY_RESTART"       | 重启                                         |
+---------------------+----------------------------------------------+
| "KEY_RESUME"        | 恢复                                         |
+---------------------+----------------------------------------------+
| "KEY_SAVE"          | 保存                                         |
+---------------------+----------------------------------------------+
| "KEY_SBEG"          | Shifted Beg (beginning)                      |
+---------------------+----------------------------------------------+
| "KEY_SCANCEL"       | Shifted Cancel                               |
+---------------------+----------------------------------------------+
| "KEY_SCOMMAND"      | Shifted Command                              |
+---------------------+----------------------------------------------+
| "KEY_SCOPY"         | Shifted Copy                                 |
+---------------------+----------------------------------------------+
| "KEY_SCREATE"       | Shifted Create                               |
+---------------------+----------------------------------------------+
| "KEY_SDC"           | Shifted Delete char                          |
+---------------------+----------------------------------------------+
| "KEY_SDL"           | Shifted Delete line                          |
+---------------------+----------------------------------------------+
| "KEY_SELECT"        | Select                                       |
+---------------------+----------------------------------------------+
| "KEY_SEND"          | Shifted End                                  |
+---------------------+----------------------------------------------+
| "KEY_SEOL"          | Shifted Clear line                           |
+---------------------+----------------------------------------------+
| "KEY_SEXIT"         | Shifted Exit                                 |
+---------------------+----------------------------------------------+
| "KEY_SFIND"         | Shifted Find                                 |
+---------------------+----------------------------------------------+
| "KEY_SHELP"         | Shifted Help                                 |
+---------------------+----------------------------------------------+
| "KEY_SHOME"         | Shifted Home                                 |
+---------------------+----------------------------------------------+
| "KEY_SIC"           | Shifted Input                                |
+---------------------+----------------------------------------------+
| "KEY_SLEFT"         | Shifted Left arrow                           |
+---------------------+----------------------------------------------+
| "KEY_SMESSAGE"      | Shifted Message                              |
+---------------------+----------------------------------------------+
| "KEY_SMOVE"         | Shifted Move                                 |
+---------------------+----------------------------------------------+
| "KEY_SNEXT"         | Shifted Next                                 |
+---------------------+----------------------------------------------+
| "KEY_SOPTIONS"      | Shifted Options                              |
+---------------------+----------------------------------------------+
| "KEY_SPREVIOUS"     | Shifted Prev                                 |
+---------------------+----------------------------------------------+
| "KEY_SPRINT"        | Shifted Print                                |
+---------------------+----------------------------------------------+
| "KEY_SREDO"         | Shifted Redo                                 |
+---------------------+----------------------------------------------+
| "KEY_SREPLACE"      | Shifted Replace                              |
+---------------------+----------------------------------------------+
| "KEY_SRIGHT"        | Shifted Right arrow                          |
+---------------------+----------------------------------------------+
| "KEY_SRSUME"        | Shifted Resume                               |
+---------------------+----------------------------------------------+
| "KEY_SSAVE"         | Shifted Save                                 |
+---------------------+----------------------------------------------+
| "KEY_SSUSPEND"      | Shifted Suspend                              |
+---------------------+----------------------------------------------+
| "KEY_SUNDO"         | Shifted Undo                                 |
+---------------------+----------------------------------------------+
| "KEY_SUSPEND"       | Suspend                                      |
+---------------------+----------------------------------------------+
| "KEY_UNDO"          | 撤销操作                                     |
+---------------------+----------------------------------------------+
| "KEY_MOUSE"         | Mouse event has occurred                     |
+---------------------+----------------------------------------------+
| "KEY_RESIZE"        | Terminal resize event                        |
+---------------------+----------------------------------------------+
| "KEY_MAX"           | Maximum key value                            |
+---------------------+----------------------------------------------+

在VT100及其软件仿真（例如X终端仿真器）上，通常至少有四个功能键（
"KEY_F1", "KEY_F2", "KEY_F3", "KEY_F4" ）可用，并且箭头键以明显的方式
映射到 "KEY_UP", "KEY_DOWN", "KEY_LEFT" 和 "KEY_RIGHT" 。如果您的机器
有一个PC键盘，可以安全地使用箭头键和十二个功能键（旧的PC键盘可能只有十
个功能键）;此外，以下键盘映射是标准的：

+--------------------+-------------+
| 键帽               | 常数        |
|====================|=============|
| "Insert"           | KEY_IC      |
+--------------------+-------------+
| "Delete"           | KEY_DC      |
+--------------------+-------------+
| "Home"             | KEY_HOME    |
+--------------------+-------------+
| "End"              | KEY_END     |
+--------------------+-------------+
| "Page Up"          | KEY_PPAGE   |
+--------------------+-------------+
| "Page Down"        | KEY_NPAGE   |
+--------------------+-------------+

The following table lists characters from the alternate character set.
These are inherited from the VT100 terminal, and will generally be
available on software emulations such as X terminals.  When there is
no graphic available, curses falls back on a crude printable ASCII
approximation.

注解:

  只有在调用 "initscr()" 之后才能使用它们

+--------------------+--------------------------------------------+
| ACS代码            | 含义                                       |
|====================|============================================|
| "ACS_BBSS"         | 右上角的别名                               |
+--------------------+--------------------------------------------+
| "ACS_BLOCK"        | 实心方块                                   |
+--------------------+--------------------------------------------+
| "ACS_BOARD"        | 正方形                                     |
+--------------------+--------------------------------------------+
| "ACS_BSBS"         | 水平线的别名                               |
+--------------------+--------------------------------------------+
| "ACS_BSSB"         | 左上角的别名                               |
+--------------------+--------------------------------------------+
| "ACS_BSSS"         | 顶部 T 型的别名                            |
+--------------------+--------------------------------------------+
| "ACS_BTEE"         | 底部 T 型                                  |
+--------------------+--------------------------------------------+
| "ACS_BULLET"       | 正方形                                     |
+--------------------+--------------------------------------------+
| "ACS_CKBOARD"      | 棋盘（点刻）                               |
+--------------------+--------------------------------------------+
| "ACS_DARROW"       | 向下箭头                                   |
+--------------------+--------------------------------------------+
| "ACS_DEGREE"       | 等级符                                     |
+--------------------+--------------------------------------------+
| "ACS_DIAMOND"      | 菱形                                       |
+--------------------+--------------------------------------------+
| "ACS_GEQUAL"       | 大于或等于                                 |
+--------------------+--------------------------------------------+
| "ACS_HLINE"        | 水平线                                     |
+--------------------+--------------------------------------------+
| "ACS_LANTERN"      | 灯形符号                                   |
+--------------------+--------------------------------------------+
| "ACS_LARROW"       | 向左箭头                                   |
+--------------------+--------------------------------------------+
| "ACS_LEQUAL"       | 小于或等于                                 |
+--------------------+--------------------------------------------+
| "ACS_LLCORNER"     | 左下角                                     |
+--------------------+--------------------------------------------+
| "ACS_LRCORNER"     | 右下角                                     |
+--------------------+--------------------------------------------+
| "ACS_LTEE"         | 左侧 T 型                                  |
+--------------------+--------------------------------------------+
| "ACS_NEQUAL"       | 不等号                                     |
+--------------------+--------------------------------------------+
| "ACS_PI"           | 字母π                                      |
+--------------------+--------------------------------------------+
| "ACS_PLMINUS"      | 正负号                                     |
+--------------------+--------------------------------------------+
| "ACS_PLUS"         | 加号                                       |
+--------------------+--------------------------------------------+
| "ACS_RARROW"       | 向右箭头                                   |
+--------------------+--------------------------------------------+
| "ACS_RTEE"         | 右侧 T 型                                  |
+--------------------+--------------------------------------------+
| "ACS_S1"           | 扫描线 1                                   |
+--------------------+--------------------------------------------+
| "ACS_S3"           | 扫描线3                                    |
+--------------------+--------------------------------------------+
| "ACS_S7"           | 扫描线7                                    |
+--------------------+--------------------------------------------+
| "ACS_S9"           | 扫描线 9                                   |
+--------------------+--------------------------------------------+
| "ACS_SBBS"         | 右下角的别名                               |
+--------------------+--------------------------------------------+
| "ACS_SBSB"         | 垂直线的别名                               |
+--------------------+--------------------------------------------+
| "ACS_SBSS"         | 右侧 T 型的别名                            |
+--------------------+--------------------------------------------+
| "ACS_SSBB"         | 左下角的别名                               |
+--------------------+--------------------------------------------+
| "ACS_SSBS"         | 底部 T 型的别名                            |
+--------------------+--------------------------------------------+
| "ACS_SSSB"         | 左侧 T 型的别名                            |
+--------------------+--------------------------------------------+
| "ACS_SSSS"         | alternate name for crossover or big plus   |
+--------------------+--------------------------------------------+
| "ACS_STERLING"     | 英镑                                       |
+--------------------+--------------------------------------------+
| "ACS_TTEE"         | 顶部 T 型                                  |
+--------------------+--------------------------------------------+
| "ACS_UARROW"       | 向上箭头                                   |
+--------------------+--------------------------------------------+
| "ACS_ULCORNER"     | 左上角                                     |
+--------------------+--------------------------------------------+
| "ACS_URCORNER"     | 右上角                                     |
+--------------------+--------------------------------------------+
| "ACS_VLINE"        | 垂线                                       |
+--------------------+--------------------------------------------+

下表列出了预定义的颜色：

+---------------------+------------------------------+
| 常数                | 颜色                         |
|=====================|==============================|
| "COLOR_BLACK"       | 黑色                         |
+---------------------+------------------------------+
| "COLOR_BLUE"        | 蓝色                         |
+---------------------+------------------------------+
| "COLOR_CYAN"        | 青色（浅绿蓝色）             |
+---------------------+------------------------------+
| "COLOR_GREEN"       | 绿色                         |
+---------------------+------------------------------+
| "COLOR_MAGENTA"     | 洋红色（紫红色）             |
+---------------------+------------------------------+
| "COLOR_RED"         | 红色                         |
+---------------------+------------------------------+
| "COLOR_WHITE"       | 白色                         |
+---------------------+------------------------------+
| "COLOR_YELLOW"      | 黄色                         |
+---------------------+------------------------------+


"curses.textpad" --- Text input widget for curses programs
**********************************************************

The "curses.textpad" module provides a "Textbox" class that handles
elementary text editing in a curses window, supporting a set of
keybindings resembling those of Emacs (thus, also of Netscape
Navigator, BBedit 6.x, FrameMaker, and many other programs).  The
module also provides a rectangle-drawing function useful for framing
text boxes or for other purposes.

The module "curses.textpad" defines the following function:

curses.textpad.rectangle(win, uly, ulx, lry, lrx)

   Draw a rectangle.  The first argument must be a window object; the
   remaining arguments are coordinates relative to that window.  The
   second and third arguments are the y and x coordinates of the upper
   left hand corner of the rectangle to be drawn; the fourth and fifth
   arguments are the y and x coordinates of the lower right hand
   corner. The rectangle will be drawn using VT100/IBM PC forms
   characters on terminals that make this possible (including xterm
   and most other software terminal emulators).  Otherwise it will be
   drawn with ASCII  dashes, vertical bars, and plus signs.


文本框对象
==========

You can instantiate a "Textbox" object as follows:

class curses.textpad.Textbox(win)

   Return a textbox widget object.  The *win* argument should be a
   curses window object in which the textbox is to be contained. The
   edit cursor of the textbox is initially located at the upper left
   hand corner of the containing window, with coordinates "(0, 0)".
   The instance's "stripspaces" flag is initially on.

   "Textbox" objects have the following methods:

   edit([validator])

      This is the entry point you will normally use.  It accepts
      editing keystrokes until one of the termination keystrokes is
      entered.  If *validator* is supplied, it must be a function.  It
      will be called for each keystroke entered with the keystroke as
      a parameter; command dispatch is done on the result. This method
      returns the window contents as a string; whether blanks in the
      window are included is affected by the "stripspaces" attribute.

   do_command(ch)

      处理单个按键命令。以下是支持的特殊按键：

      +--------------------+---------------------------------------------+
      | 按键               | 动作                                        |
      |====================|=============================================|
      | "Control-A"        | 转到窗口的左边缘。                          |
      +--------------------+---------------------------------------------+
      | "Control-B"        | 光标向左，如果可能，包含前一行。            |
      +--------------------+---------------------------------------------+
      | "Control-D"        | 删除光标下的字符。                          |
      +--------------------+---------------------------------------------+
      | "Control-E"        | Go to right edge (stripspaces off) or end   |
      |                    | of line (stripspaces on).                   |
      +--------------------+---------------------------------------------+
      | "Control-F"        | 向右移动光标，适当时换行到下一行。          |
      +--------------------+---------------------------------------------+
      | "Control-G"        | 终止，返回窗口内容。                        |
      +--------------------+---------------------------------------------+
      | "Control-H"        | 向后删除字符。                              |
      +--------------------+---------------------------------------------+
      | "Control-J"        | 如果窗口是1行则终止，否则插入换行符。       |
      +--------------------+---------------------------------------------+
      | "Control-K"        | 如果行为空，则删除它，否则清除到行尾。      |
      +--------------------+---------------------------------------------+
      | "Control-L"        | 刷新屏幕。                                  |
      +--------------------+---------------------------------------------+
      | "Control-N"        | 光标向下;向下移动一行。                     |
      +--------------------+---------------------------------------------+
      | "Control-O"        | 在光标位置插入一个空行。                    |
      +--------------------+---------------------------------------------+
      | "Control-P"        | 光标向上;向上移动一行。                     |
      +--------------------+---------------------------------------------+

      如果光标位于无法移动的边缘，则移动操作不执行任何操作。在可能的情
      况下，支持以下同义词：

      +--------------------------+--------------------+
      | 常数                     | 按键               |
      |==========================|====================|
      | "KEY_LEFT"               | "Control-B"        |
      +--------------------------+--------------------+
      | "KEY_RIGHT"              | "Control-F"        |
      +--------------------------+--------------------+
      | "KEY_UP"                 | "Control-P"        |
      +--------------------------+--------------------+
      | "KEY_DOWN"               | "Control-N"        |
      +--------------------------+--------------------+
      | "KEY_BACKSPACE"          | "Control-h"        |
      +--------------------------+--------------------+

      All other keystrokes are treated as a command to insert the
      given character and move right (with line wrapping).

   gather()

      Return the window contents as a string; whether blanks in the
      window are included is affected by the "stripspaces" member.

   stripspaces

      This attribute is a flag which controls the interpretation of
      blanks in the window.  When it is on, trailing blanks on each
      line are ignored; any cursor motion that would land the cursor
      on a trailing blank goes to the end of that line instead, and
      trailing blanks are stripped when the window contents are
      gathered.
