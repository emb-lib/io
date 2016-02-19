# Input/output support library for embedded applications

Only output functionality is supported for now.

## Functions

```C
extern "C" int vsprintf(char *string, const char *format, va_list ap);
extern "C" int sprintf(char *buffer, char const *format, ...)
extern int _floatp10(double *fnum, bool *negative, int prec);
extern "C" char *itoa(int num, char *str, int radix);
extern "C" char *utoa(unsigned num, char *str, int radix);

```
