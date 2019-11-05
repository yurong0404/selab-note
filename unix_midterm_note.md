## UNIX 筆記
> getopt<br>
```C
#include <unistd.h>
int main(int argc, char *argv[])
{
  int opt;
  while ((opt = getopt(argc, argv, "nt:")) != -1) {
    switch (opt) {
    case 'n':
       blah;
       break;
    case 't':
       blah;
       break;
    default:
       blah;
    }
  }
}
```
> getopt_long
```C
#include <getopt.h>
void setfilter(int argc, char *argv[], int *tcp, int *udp)
{
	char opt;
	const char *optstring = "tu";
	struct option opts[] = 
	{
		{"tcp", 0, NULL, 't'},
		{"udp", 0, NULL, 'u'},
		{ NULL, 0, NULL, 0 }
	};
	while ((opt = getopt_long(argc, argv, optstring, opts, NULL)) != -1)
	{
		switch(opt)
		{
			case 't':
				*udp = 1;
				break;
			case 'u':
				*tcp = 1;
				break;
			default:
				break;
		}
	}	
}
```
