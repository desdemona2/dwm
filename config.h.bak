/* See LICENSE file for copyright and license details. */

#include <X11/XF86keysym.h>

/* appearance */
static const unsigned int borderpx  = 1;        /* border pixel of windows */
static const unsigned int gappx     = 8;        /* gaps between windows */
static const unsigned int snap      = 32;       /* snap pixel */
static const int showbar            = 1;        /* 0 means no bar */
static const int topbar             = 1;        /* 0 means bottom bar */
static const char *fonts[]          = { "SourceCodePro:size=14","Font Awesome 5 Pro:pixelsize=16" };
static const char dmenufont[]       = "SourceCodePro:size=14";
static const char col_gray1[]       = "#222222";
static const char col_gray2[]       = "#444444";
static const char col_gray3[]       = "#bbbbbb";
static const char col_gray4[]       = "#eeeeee";
static const char col_cyan[]        = "#005577";
static const char *colors[][3]      = {
	/*               fg         bg         border   */
	[SchemeNorm] = { col_gray3, col_gray1, col_gray2 },
	[SchemeSel]  = { col_gray4, col_cyan,  col_cyan  },
};

/* tagging */
static const char *tags[] = { "1", "2", "3", "4", "5", "6", "7", "8", "9" };

static const Rule *rules[] = {
	/* class      instance    title       tags mask     isfloating   monitor  */
/*	{ "Gimp",     NULL,       NULL,       0,            1,           -1 },
	{ "Firefox",  NULL,       NULL,       1 << 8,       0,           -1 },	  */
	{ "Appname",  NULL,	      NULL,	      2,	        0,		     -1 },
};

/* layout(s) */
static const float mfact     = 0.55; /* factor of master area size [0.05..0.95] */
static const int nmaster     = 1;    /* number of clients in master area */
static const int resizehints = 1;    /* 1 means respect size hints in tiled resizals */

static const Layout layouts[] = {
	/* symbol     arrange function */
	{ "[]=",      tile },    /* first entry is default */
	{ "><>",      NULL },    /* no layout function means floating behavior */
	{ "[M]",      monocle },
	{ "|M|",      centeredmaster },
	{ ">M>",      centeredfloatingmaster },
};

/* key definitions */
#define MODKEY Mod4Mask
#define TAGKEYS(KEY,TAG) \
	{ MODKEY,                       KEY,      view,           {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask,           KEY,      toggleview,     {.ui = 1 << TAG} }, \
	{ MODKEY|ShiftMask,             KEY,      tag,            {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask|ShiftMask, KEY,      toggletag,      {.ui = 1 << TAG} },

/* helper for spawning shell commands in the pre dwm-5.0 fashion */
#define SHCMD(cmd) { .v = (const char*[]){ "/bin/sh", "-c", cmd, NULL } }

/* commands */
static char dmenumon[2] = "0"; /* component of dmenucmd, manipulated in spawn() */
static const char *dmenucmd[] = { "dmenu_run", "-m", dmenumon, "-fn", dmenufont, "-nb", col_gray1, "-nf", col_gray3, "-sb", col_cyan, "-sf", col_gray4, NULL };

/* *********************** APPLICATION COMMANDS **************************** */

static const char *termcmd[]  =  { "alacritty", NULL };
static const char *brave[]    =  { "brave", NULL  };
static const char *chrome[]   =  { "google-chrome-stable", NULL };
static const char *discord[]  =  { "discord", NULL };
static const char *thunar[]   =  { "thunar", NULL };
static const char *st[]       =  { "st", NULL };
static const char *telegram[] =  { "telegram-desktop", NULL };
static const char *code[]     =  { "code", NULL };

/* commands spawned when clicking statusbar, the mouse button pressed is exported as BUTTON */
static const StatusCmd statuscmds[] = {
	{ "$HOME/scripts/status/Click/network_click.sh", 	1 },
	{ "$HOME/scripts/status/Click/cpu_click.sh", 		2 },
	{ "notify-send 3rd", 								3 },
	{ "pavucontrol &", 									4 },
	{ "$HOME/scripts/status/Click/time_click.sh", 		5 },
	{ "$HOME/scripts/status/Click/battery_click.sh", 	6 },
	{ "$HOME/scripts/status/Click/update_click.sh", 	7 },
	{ "brave &", 										8 },
	{ "telegram-desktop", 								9 },
	{ "i3lock -i $HOME/.i3wall.png", 					10 },
};
static const char *statuscmd[] = { "/bin/sh", "-c", NULL, NULL };

static Key keys[] = {

	/* ************************* Application keys  ************************* */

	/* modifier             key        function        argument */
	{ MODKEY,				XK_Return, spawn,      {.v = termcmd } },	  // Alacritty
	{ MODKEY|ShiftMask,     XK_p,      spawn,      {.v = dmenucmd } },
	{ MODKEY|ShiftMask,     XK_Return, spawn,      {.v = st } },
	{ MODKEY|ShiftMask,		XK_b,	   spawn,	   {.v = brave } },  	 // Brave
	{ MODKEY|ShiftMask,		XK_c,	   spawn,	   {.v = chrome  } }, 	 // Chrome
	{ MODKEY|ShiftMask,		XK_d,	   spawn,	   {.v = discord  } },	 // Discord
	{ MODKEY|ShiftMask, 	XK_f,	   spawn,	   {.v = thunar  } },	 // Thunar
	{ MODKEY|ShiftMask,		XK_t,	   spawn,	   {.v = telegram  } },	 // Telegram
	{ MODKEY|ShiftMask,		XK_v,	   spawn,	   {.v = code  } },	     // VS Code

  	/* **************************** Layout keys ******************************** */	
	
	{ MODKEY,                       XK_b,      togglebar,      {0} },
	{ MODKEY,                       XK_j,      focusstack,     {.i = +1 } },
	{ MODKEY,                       XK_k,      focusstack,     {.i = -1 } },
	{ MODKEY,                       XK_i,      incnmaster,     {.i = +1 } },
	{ MODKEY,                       XK_d,      incnmaster,     {.i = -1 } },
	{ MODKEY,                       XK_h,      setmfact,       {.f = -0.05} },
	{ MODKEY,                       XK_l,      setmfact,       {.f = +0.05} },
	{ MODKEY|ShiftMask,             XK_Return, zoom,           {0} },
	{ MODKEY,                       XK_Tab,    view,           {0} },	
	{ MODKEY,	                    XK_c,      killclient,     {0} },	// kill process
	
	
	{ MODKEY,                       XK_t,      setlayout,      {.v = &layouts[0]} },
	{ MODKEY,                       XK_f,      setlayout,      {.v = &layouts[1]} },
	{ MODKEY,                       XK_m,      setlayout,      {.v = &layouts[2]} },
	{ MODKEY,                       XK_u,      setlayout,      {.v = &layouts[3]} },
	{ MODKEY,                       XK_o,      setlayout,      {.v = &layouts[4]} },
	{ MODKEY,                       XK_space,  setlayout,      {0} },
	
	
	{ MODKEY|ShiftMask,             XK_space,  togglefloating, {0} },
	{ MODKEY,                       XK_0,      view,           {.ui = ~0 } },
	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~0 } },
	{ MODKEY,                       XK_comma,  focusmon,       {.i = -1 } },
	{ MODKEY,                       XK_period, focusmon,       {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_comma,  tagmon,         {.i = -1 } },
	{ MODKEY|ShiftMask,             XK_period, tagmon,         {.i = +1 } },
	{ MODKEY,                       XK_minus,  setgaps,        {.i = -1 } },
	{ MODKEY,                       XK_equal,  setgaps,        {.i = +1 } },
	{ MODKEY|ShiftMask,             XK_equal,  setgaps,        {.i = 0  } },
	
	/* ***************************** HOTKEYS ******************************** */

	{ 0, 			XF86XK_AudioMute, 	  spawn,	SHCMD("amixer -q set Master toggle; kill -46 $(pidof dwmblocks)" ) },
	{ 0,			XF86XK_AudioLowerVolume,  spawn, 	SHCMD("amixer -q set Master 5%-; kill -46 $(pidof dwmblocks)" ) },
	{ 0,			XF86XK_AudioRaiseVolume,  spawn,	SHCMD("amixer -q set Master 5%+; kill -46 $(pidof dwmblocks)" ) },
	{ 0,			XF86XK_MonBrightnessDown, spawn,	SHCMD("brightnessctl set 2%-; kill -54 $(pidof dwmblocks)" ) },
	{ 0,			XF86XK_MonBrightnessUp,   spawn,	SHCMD("brightnessctl set 2%+; kill -54 $(pidof dwmblocks)" ) },
	{ 0,			XF86XK_TouchpadToggle,	  spawn,	SHCMD("~/scripts/touchpad_toggle.sh") },


				     /*  Logitech */
	{ 0,			XF86XK_HomePage,	  spawn,	SHCMD("alacritty -e lf ~") },
	{ 0,			XF86XK_Tools,		  spawn,	SHCMD("pavucontrol &") },

	/* ***************************** TAG KEYS ******************************* */
	
	TAGKEYS(                        XK_1,                      0)
	TAGKEYS(                        XK_2,                      1)
	TAGKEYS(                        XK_3,                      2)
	TAGKEYS(                        XK_4,                      3)
	TAGKEYS(                        XK_5,                      4)
	TAGKEYS(                        XK_6,                      5)
	TAGKEYS(                        XK_7,                      6)
	TAGKEYS(                        XK_8,                      7)
	TAGKEYS(                        XK_9,                      8)
	{ MODKEY|ShiftMask,             XK_q,      quit,           {0} },		// kill dwm
	/* *********************************************************************** */
};


/* button definitions */
/* click can be ClkTagBar, ClkLtSymbol, ClkStatusText, ClkWinTitle, ClkClientWin, or ClkRootWin */
static Button buttons[] = {
	/* click                event mask      button          function        argument */
	{ ClkLtSymbol,          0,              Button1,        setlayout,      {0} },
	{ ClkLtSymbol,          0,              Button3,        setlayout,      {.v = &layouts[2]} },
	{ ClkWinTitle,          0,              Button2,        zoom,           {0} },
	{ ClkStatusText,        0,              Button1,        spawn,          {.v = statuscmd } },
	{ ClkStatusText,        0,              Button2,        spawn,          {.v = statuscmd } },
	{ ClkStatusText,        0,              Button3,        spawn,          {.v = statuscmd } },
	{ ClkClientWin,         MODKEY,         Button1,        movemouse,      {0} },
	{ ClkClientWin,         MODKEY,         Button2,        togglefloating, {0} },
	{ ClkClientWin,         MODKEY,         Button3,        resizemouse,    {0} },
	{ ClkTagBar,            0,              Button1,        view,           {0} },
	{ ClkTagBar,            0,              Button3,        toggleview,     {0} },
	{ ClkTagBar,            MODKEY,         Button1,        tag,            {0} },
	{ ClkTagBar,            MODKEY,         Button3,        toggletag,      {0} },
};

