
# picoCTF-2022
### Hi there, and Welcome to my picoCTF-2022 progress and writeups

**Total Chalennges : 65**
✔️Solved: 51
❌Unsolved : 14 

**Team rank: 677 from 7794 (Top 8%)**

![image](https://user-images.githubusercontent.com/62214984/163713181-4492b90e-03a6-476b-a968-f44da82bb5ed.png)

## The provided here flags are not genuine PicoCTF flags

<details>
  <summary><h2> 1. basic-file-exploit ✔️ </h2></summary>
  <h3>Description:</h3>
	The program provided allows you to write to a file and read what you wrote from it. Try playing around with it and see if you can break it! <br/>
	<s>Connect to the program with netcat:</s><br/> 
	<s>$ nc saturn.picoctf.net 49000</s><br/>
	"<i>Use <a href=https://github.com/LogicBypass/CTF-s/raw/main/picoCTF-2022/1.basic-file-exploit/1.basic-file-exploit.out><code>1.basic-file-exploit.out</code></a> instead</i>"<br/>
	The program's source code with the flag redacted can be downloaded <a href=https://github.com/LogicBypass/CTF-s/blob/main/picoCTF-2022/1.basic-file-exploit/1.basic-file-exploit.c>here.</a> <br/>
	<br/>
  
  > Hint: Try passing in things the program doesn't expect. Like a string instead of a number.
  ----
  <h3>Reconnaissance:</h3>
	Download the files: <code>wget link-to-file</code><br/> 
	Make it exacutable:<code>chmod +x *</code><br/>
	Open the program's source code file and analyze the flag retrieving function:<br/>
<pre>
if ((entry_number = strtol(entry, NULL, 10)) == 0) {
	puts(flag);
	fseek(stdin, 0, SEEK_END);
	exit(0);
}
</pre>
	<br/>
	Now we need to manipulate the input to get the function result == 0 </br>
  <hr>
  <h3>Exploitation:</h3>
	At the first run, let's try to use it as intended</br>
<pre>
└─$ ./1.basic-file-exploit.out
Hi, welcome to my echo chamber!
Type '1' to enter a phrase into our database
Type '2' to echo a phrase in our database
Type '3' to exit the program
1
Please enter your data:
Logic Bypass
Please enter the length of your data:
12
Your entry number is: 1
Write successful, would you like to do anything else?
2
Please enter the entry number of your data:
1
Logic Bypass
Read successful, would you like to do anything else?
Timed out waiting for user input. Press Ctrl-C to disconnect
Goodbye!
</pre>
<br/>
	Now let's try to insert "0" into different fields:<br/>
	First try:
<pre>
└─$ ./1.basic-file-exploit.out
Hi, welcome to my echo chamber!
Type '1' to enter a phrase into our database
Type '2' to echo a phrase in our database
Type '3' to exit the program
1
Please enter your data:
0
Please enter the length of your data:
0
Please put in a valid length		#Not allowed to insert 0
Please enter the length of your data:
1
Your entry number is: 1
Write successful, would you like to do anything else?
2                                                      #Read our entry
Please enter the entry number of your data:
1
0
Read successful, would you like to do anything else?

Timed out waiting for user input. Press Ctrl-C to disconnect
Goodbye!
</pre>
<br/>
	Nothing interesting</br>
	Second try, the valid input but try to read the "0" entry number
<pre>
└─$ ./1.basic-file-exploit.out
Hi, welcome to my echo chamber!
Type '1' to enter a phrase into our database
Type '2' to echo a phrase in our database
Type '3' to exit the program
1
Please enter your data:
Logic Bypass
Please enter the length of your data:
12
Your entry number is: 1
Write successful, would you like to do anything else?
2                                                      #Switch to Read entry
Please enter the entry number of your data:
0
picoCTF{#########################}
</pre>
Hooray! We have a flag!
<br/>
We also can get the flag by "Trying passing strings instead of number" as mentioned in "Hint"<br/>
This works because of "strtol() function error" that says: In case no conversion was performed (no digits seen, and 0 returned)
<pre>
└─$ ./1.basic-file-exploit.out
Hi, welcome to my echo chamber!
Type '1' to enter a phrase into our database
Type '2' to echo a phrase in our database
Type '3' to exit the program
1
Please enter your data:
Logic Bypass
Please enter the length of your data:
12
Your entry number is: 1
Write successful, would you like to do anything else?
2                                                      #Switch to Read entry
Please enter the entry number of your data:
A
picoCTF{#########################}
</pre>
Hooray! We have a flag!
</details>

---

<details>
  <summary><h2> 2. basic-mod1 ✔️ </h2></summary>
  <h3>Description:</h3>
	We found this weird message being passed around on the servers, we think we have a working decryption scheme.<br/>
	<br/>
	Message:<br/> 
	<i> 202 137 390 235 114 369 198 110 350 396 390 383 225 258 38 291 75 324 401 142 288 397 </i><br/>
	<br/>
	Take each number mod 37 and map it to the following character set:<br/>
	0-25 is the alphabet (uppercase)<br/>
	26-35 are the decimal digits<br/>
	36 is an underscore<br/>
	Wrap your decrypted message in the picoCTF flag format<br/>
	<br/>
  
  > Hint1: Do you know what mod 37 means?
	
  > Hint2: mod 37 means modulo 37. It gives the remainder of a number after being divided by 37.

  ----
  <h3>Reconnaissance and Exploitation:</h3>
	Go to <a href="https://www.dcode.fr/cipher-identifier">Chipher Identifier</a> to find a Modulo Cipher decoder.<br/>
	Open <a href="https://www.dcode.fr/modulo-cipher">Modulo Cipher</a> from results, change modulo nr to 37 and insert the Message.<br/>
	Take the output and using <a href="https://www.dcode.fr/substitution-cipher">Substitution Cipher</a> and map it to the character set from the description:<br/>
	<pre>
	0-25 is the alphabet (uppercase).
	26-35 are the decimal digits.
	36 is an underscore.</pre>
	Wrap your decrypted message in the picoCTF flag format<br/>

	picoCTF{R****_N_*0***_*E***3**}
Hooray! We have a flag!
</details>

---

 Checkmarks:
✔️
❌


