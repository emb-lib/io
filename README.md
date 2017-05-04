# Formatted input/output support library for embedded applications

Only output functionality is supported for now. Use `-std=gnu++11` option to turn on non-standard functions support (necessary for `itoa`, `utoa`).

## Functions

```C
extern "C" int vsprintf(char *string, const char *format, va_list ap);
extern "C" int sprintf(char *buffer, char const *format, ...)
extern int _floatp10(double *fnum, bool *negative, int prec);
```

There is no `printf` function, but it can be easely added at user's project level even in thread-safe implementation(note, standard `printf` is not reentrant function) and using uC UART port for output, for example:

```C++
#include <stdio.h>
#include <stdarg.h>
#include <string.h>

const int TX_BUF_SIZE = 256;

os_mutex mutex;
char print_buf[TX_BUF_SIZE];

int printf(char *format, ...)
{
    mutex_lock();                                      // thread-safe guard
                                                       //
    va_list args;                                      //
    va_start(args, format);                            //
    uint16_t size = vsprintf(print_buf, format, args); //
    va_end(args);                                      //
                                                       //
    TxBuf.write(print_buf, size);                      // put formatted data to port buffer
                                                       //
    mutex_unlock();                                    //
                                                       //
    uart_tx_enable();                                  // start output
                                                       //
    return size;
}

```
