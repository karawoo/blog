---
layout:     post
title:      Reconfiguring keyboard layouts in Ubuntu 14.04
date:       2014-11-12 13:03
summary:    How to customize keyboard layouts in Ubuntu and get a Russian phonetic keyboard that makes more sense.
comments:   True
published:  True
tags:       ['linux', 'keyboard configuration', 'ubuntu']
categories: ['linux', 'keyboard configuration', 'ubuntu']
---

In my work on the
[Lake Baikal Dimensions of Biodiversity](http://baikaldimensions.wordpress.com)
project I frequently translate texts and correspondence between Russian and
English. I never learned to use a standard Russian keyboard so I rely on a
phonetic keyboard. However, some of the default letter placements on Ubuntu make
no sense to me (Ж mapped to V? Seriously?) and, more importantly, they're very
different from the OS X phonetic Russian keyboard I'm already accustomed to. I
spent some time today reconfiguring the keyboard layout to my liking; here's a
quick guide if you would like to do the same.

First, open up a terminal and navigate to the location where the keyboard
configurations are stored (see below). Then create a backup of the Russian
keyboard file in case anything goes wrong or you decide you want things back the
way they were.

{% highlight bash %}
cd /usr/share/X11/xkb/symbols
sudo cp ru ru_backup
{% endhighlight %}

Next, open up the `ru` file in your editor of choice (for me, it's emacs).

{% highlight bash %}
sudo emacs ru
{% endhighlight %}

At this point you'll see a file that looks like this:

{% highlight bash %}
// based on
// russian standard keyboard
// AEN <aen@logic.ru>
// 2001/12/23 by Leon Kanter <leon@blackcatlinux.com>
// 2005/12/09 Valery Inozemtsev <shrek@altlinux.ru>

// Windows layout
default  partial alphanumeric_keys
xkb_symbols "winkeys" {
    include "ru(common)"

    name[Group1]= "Russian";

    key <AE03> {        [               3,      numerosign      ]       };
    key <AE04> {        [               4,       semicolon      ]       };
    key <AE05> {        [               5,         percent      ]       };
    key <AE06> {        [               6,           colon      ]       };
    key <AE07> {        [               7,        question      ]       };
    key <AE08> {        [               8,        asterisk      ]       };

    key <AB10> {        [          period,           comma      ]       };
    key <BKSL> {        [       backslash,           slash      ]       };
};

partial hidden alphanumeric_keys
xkb_symbols "common" {

    key	<AE01> {	[		1,	    exclam 	]	};
    key	<AE02> {	[		2,        quotedbl	]	};
    key	<AE03> {	[		3,      numbersign	]	};
    key	<AE04> {	[		4,        asterisk	]	};

{% endhighlight %}

(but way longer). To edit the phonetic keyboard, you'll want to scroll down to
the phonetic layout, indicated by `xkb_symbols "phonetic"`.

Now comes the long, tedious part where you change all the offending letter
placements. I did this by comparing all of the keys to a picture of the OS X
phonetic Russian keyboard and editing the key mappings accordingly. But you can
take advantage of my labors and just copy/paste my settings into your file:

{% highlight bash %}
partial alphanumeric_keys
xkb_symbols "phonetic" {

    name[Group1]= "Russian (phonetic)";

    key	<AE01> {	[		1,	    exclam 	]	};
    key <AE02> {        [               2,              at      ]       };
    key <AE03> {        [               3,      numbersign      ]       };
    key <AE04> {        [               4,          dollar      ]       };
    key <AE05> {        [               5,         percent      ]       };
    key <AE06> {        [               6,     asciicircum      ]       };
    key <AE07> {        [               7,       ampersand      ]       };
    key	<AE08> {	[		8,        asterisk	]	};
    key	<AE09> {	[		9,	 parenleft	]	};
    key	<AE10> {	[		0,	parenright	]	};
    key <AE11> {	[Cyrillic_softsign,Cyrillic_SOFTSIGN	]	};

    key	<AB09> {	[	   period,	   greater	]	};
    key	<AB10> {	[	    slash,	  question	]	};
    key	<AB08> {	[	    comma,	      less	]	};
    key	<AC10> {	[	semicolon,	     colon	]	};
    key	<AC11> {	[      apostrophe,	  quotedbl	]	};
    key	<LSGT> {	[	      bar,	 brokenbar	]	};

    key	<TLDE> {	[  Cyrillic_shcha,  Cyrillic_SHCHA	]	};
    key	<LatQ> {	[     Cyrillic_ya,     Cyrillic_YA	]	};
    key	<LatZ> {	[     Cyrillic_ze,     Cyrillic_ZE	]	};
    key	<LatS> {	[     Cyrillic_es,     Cyrillic_ES	]	};
    key	<LatA> {	[      Cyrillic_a,	Cyrillic_A	]	};
    key	<LatW> {	[    Cyrillic_sha,    Cyrillic_SHA	]	};
    key	<LatC> {	[    Cyrillic_tse,    Cyrillic_TSE	]	};
    key	<LatX> {	[     Cyrillic_ha,     Cyrillic_HA	]	};
    key	<LatD> {	[     Cyrillic_de,     Cyrillic_DE	]	};
    key	<LatE> {	[     Cyrillic_ie,     Cyrillic_IE	]	};
    key	<LatV> {	[     Cyrillic_ve,     Cyrillic_VE	]	};
    key	<LatF> {	[     Cyrillic_ef,     Cyrillic_EF	]	};
    key	<LatT> {	[     Cyrillic_te,     Cyrillic_TE	]	};
    key	<LatR> {	[     Cyrillic_er,     Cyrillic_ER	]	};
    key	<LatN> {	[     Cyrillic_en,     Cyrillic_EN	]	};
    key	<LatB> {	[     Cyrillic_be,     Cyrillic_BE	]	};
    key	<LatH> {	[    Cyrillic_che,    Cyrillic_CHE	]	};
    key	<LatG> {	[    Cyrillic_ghe,    Cyrillic_GHE	]	};
    key	<LatY> {	[   Cyrillic_yeru,   Cyrillic_YERU	]	};
    key	<LatM> {	[     Cyrillic_em,     Cyrillic_EM	]	};
    key	<LatJ> {	[ Cyrillic_shorti, Cyrillic_SHORTI	]	};
    key	<LatU> {	[      Cyrillic_u,	Cyrillic_U	]	};
    key	<LatK> {	[     Cyrillic_ka,     Cyrillic_KA	]	};
    key	<LatI> {	[      Cyrillic_i,	Cyrillic_I	]	};
    key	<LatO> {	[      Cyrillic_o,	Cyrillic_O	]	};
    key	<LatL> {	[     Cyrillic_el,     Cyrillic_EL	]	};
    key	<LatP> {	[     Cyrillic_pe,     Cyrillic_PE	]	};

    key	<AD11> {	[     Cyrillic_yu,     Cyrillic_YU	]	};
    key	<AE12> {	[Cyrillic_hardsign,Cyrillic_HARDSIGN	]	};
    key	<AD12> {	[    Cyrillic_zhe,    Cyrillic_ZHE	]	};
    key	<BKSL> {	[      Cyrillic_e,	Cyrillic_E	]	};
    };
{% endhighlight %}

Note: I didn't bother mapping Ё to anything because I never use it.

Save the file and exit. At this point I restarted my computer, expecting the new
settings to take effect automatically as the [helpful](http://www.dotkam.com/2007/06/25/custom-keyboard-layout-in-ubuntu-or-just-linux-2/) [tutorials](http://stanford.edu/~wbraynen/linux/) I read suggested. However, on Ubuntu 14.04 [one more step](http://askubuntu.com/a/439198/342035) is needed; open up a terminal and type:

{% highlight bash %}
sudo dpkg-reconfigure xkb-data
{% endhighlight %}

And you're all set!
