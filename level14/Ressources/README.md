## level14

This level is like, a desert. There's nothing to see here, no files, no binaries, nothing.

Here's a (non-exhaustive) list of things I tried :

- Checking owned files
- Looking for binaries everywhere
- Looking running processes
- Looking for cronjobs with `crontab -l`
- Checking env variables

But then, I understood. Maybe we should attack the `getflag` binary itself.

Looking at the binary's strings there are some interesting things :

```
You are root are you that dumb ? # <--- this is printed if we are root :^)
I`fA>_88eEd:=`85h0D8HE>,D
7`4Ci4=^d=J,?>i;6,7d416,7
<>B16\AD<C6,G_<1>^7ci>l4B
B8b:6,3fj7:,;bh>D@>8i:6@D
?4d@:,C>8C60G>8:h:Gb4?l,A
G8H.6,=4k5J0<cd/D@>>B:>:4
H8B8h_20B4J43><8>\ED<;j@3
78H:J4<4<9i_I4k0J^5>B1j`9
bci`mC{)jxkn<"uD~6%g7FK`7
Dc6m~;}f8Cj#xFkel;#&ycfbK
74H9D^3ed7k05445J0E4e;Da4
70hCi,E44Df[A4B/J@3f<=:`D
8_Dw"4#?+3i]q&;p6 gtw88EC
boe]!ai0FB@.:|L6l@A?>qJ}I
g <t61:|4_|!@IF.-62FH&G~DCK/Ekrvvdwz?v|
Nope there is no token here for you sorry. Try again :)
```

15 lines of random characters, and then a message saying there is no token here. Doesn't that look like a little bit suspicious?

Well, that's because each cryptic lines are actually each flags, but ciphered.

### Reversing

So it seems like the `ft_des` function is still here (from the previous level), and each flag is getting passed to this function.

And before all the if / else if (or switch?) there is a call to `getuid` :^)

Looking at `/etc/passwd` the `flag14` user's uid is 3014, so we could do the same thing as before, but meh, let's try to reverse the `ft_des` function for more fun.


```c
#include <string.h>
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#define byte unsigned char
#define uint unsigned int

char *ft_des(char *s)
{
	char cVar1;
	char *pcVar2;
	uint uVar3;
	char *pcVar4;
	byte bVar5;
	uint local_20;
	int local_1c;
	int local_18;
	int local_14;

	bVar5 = 0;
	pcVar2 = strdup(s);
	local_1c = 0;
	local_20 = 0;
	do
	{
		uVar3 = 0xffffffff;
		pcVar4 = pcVar2;
		do
		{
			if (uVar3 == 0)
				break;
			uVar3 = uVar3 - 1;
			cVar1 = *pcVar4;
			pcVar4 = pcVar4 + (uint)bVar5 * -2 + 1;
		} while (cVar1 != '\0');
		if (~uVar3 - 1 <= local_20)
		{
			return pcVar2;
		}
		if (local_1c == 6)
		{
			local_1c = 0;
		}
		if ((local_20 & 1) == 0)
		{
			if ((local_20 & 1) == 0)
			{
				for (local_14 = 0; local_14 < "0123456"[local_1c]; local_14 = local_14 + 1)
				{
					pcVar2[local_20] = pcVar2[local_20] + -1;
					if (pcVar2[local_20] == '\x1f')
					{
						pcVar2[local_20] = '~';
					}
				}
			}
		}
		else
		{
			for (local_18 = 0; local_18 < "0123456"[local_1c]; local_18 = local_18 + 1)
			{
				pcVar2[local_20] = pcVar2[local_20] + '\x01';
				if (pcVar2[local_20] == '\x7f')
				{
					pcVar2[local_20] = ' ';
				}
			}
		}
		local_20 = local_20 + 1;
		local_1c = local_1c + 1;
	} while (true);
}

int main(void)
{
	puts(ft_des("g <t61:|4_|!@IF.-62FH&G~DCK/Ekrvvdwz?v|"));
}
```

This recoded function gives us the flag : `7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ`
