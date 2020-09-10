# netcat with just a cat
	{ echo -e "GET / HTTP/1.0\r\nHost: www.google.com\r\n\r" >&3;\
	cat <&3 ;\
	} 3<> /dev/tcp/www.google.com/80
