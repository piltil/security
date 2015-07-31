# security
M-138
0. compile (tested with gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1). ubuntu 14.04)
make


1. input and functions
	./testM138 -input ASTRINGOFLENGTHONEHUNDRED -decript	//this will decript	
	./testM138 -input ASTRINGOFLENGTHONEHUNDRED -encript	//this will encript	
	Note that the input string above will have charactors of 'A' to 'Z' only; lowercase characters 'a' to 'z'are ok, but will be changed to responding uppercases. 
	For demonstration purpose, here the length of input string can not be larger than 1024.

	There is also a double-checking function, which is reversing the decryption/encription, just for double check purpose.

2. test case:
./testM138 -input TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQUOTe  -decrypt

Output:
Note: all lowercase characters (if any) will be moved to upcase. 
Input string is 
		TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQUOTE 
After decrption, new string is 
		IYMBWCMOQGEIOSMKRNROPTIXLAIIKCKVBNMIKHSDRQOPQGGOISEFENMZQZKUNBQKXXDAZQWYVRQLNIWOHVWDTENNGQBPJPIJXLEU 
For double check only: after re-encrption, string (should be equal to input string) is 
		TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQUOTE 


./testM138 -input TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQ  -decrypt
Output:

Warning! Input string is of length < 100; will be appended by 'O' to 100 chars.
Note: all lowercase characters (if any) will be moved to upcase. 
Input string is 
		TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQOOOO 
After decrption, new string is 
		IYMBWCMOQGEIOSMKRNROPTIXLAIIKCKVBNMIKHSDRQOPQGGOISEFENMZQZKUNBQKXXDAZQWYVRQLNIWOHVWDTENNGQBPJPIJELWS 
For double check only: after re-encrption, string (should be equal to input string) is 
		TWOTHINGSAREINFINITETHEUNIVERSEANDHUMANSTUPIDITYCONGRATULATIONYOUHAVEDECIPHEREDANALBERTEINSTEINQOOOO
