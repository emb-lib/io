# Formatted input/output support library for embedded applications

Only output functionality is supported for now.

## Functions

```C
extern "C" int vsprintf(char *string, const char *format, va_list ap);
extern "C" int sprintf(char *buffer, char const *format, ...)
extern int _floatp10(double *fnum, bool *negative, int prec);
extern "C" char *itoa(int num, char *str, int radix);
extern "C" char *utoa(unsigned num, char *str, int radix);

```

There is no `printf` function, but it can be easely added at user's project level even in thread-safe implementation(note, standard `printf` is not reentrant function) and using uC UART port for output, for example:

```C++
#include <stdio.h>
#include <stdarg.h>
#include <string.h>

os_mutex mutex;
char print_buf[TX_BUF_SIZE/2];

int printf(char *format, ...)
{
    mutex_lock();
    va_list args;
    va_start(args, format);
    uint16_t size = vsprintf(print_buf, format, args);
    va_end(args);

    TxBuf.write(print_buf, size);
    
    mutex_unlock();
    
    uart_tx_enable();

    return size;
}

```
