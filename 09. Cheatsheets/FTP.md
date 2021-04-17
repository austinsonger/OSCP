# Anonymous FTP
#use PASV or PORT first

425 Use PORT or PASV first.

And here’s where it gets interesting.  This Telnet session I’ve got here is like a control window.  But if I want any actual data from the server, I’m going to need to either open up one of my ports (and do some port-forwarding on my router) to receive it (PORT), or connect to another port that the FTP server can pipe data through (with PASV).

I’d rather not go through all of the trouble of port-forwarding, so I’m going to choose the latter.  I type in PASV.  The server responds with:

227 Entering Passive Mode (63,245,208,138,225,55)

So what does that big string of numbers mean?  The first 4 are the IP address I’m to connect to (63.245.208.138).
The last two tell me what PORT to connect to.
The formula to determine the port number is N1*256 + N2.  N1, in this case, is 225.  N2 is 55.  So 225*256 + 55 is 57655.

So I open another Telnet in a separate window, connect to 63.245.208.138 on port 57655, and get….

nothing.

Yep, just a blank.  I’ve made the connection, but I haven’t asked for any data, so there’s nothing for the connection to say.

However, if I type LIST again in the command window, I get

150 Here comes the directory listing.
226 Directory send OK.