
# picoCTF-2022
Hi there, and Welcome to my picoCTF-2022 progress and writeups



<details>
  <summary><h2> 1. basic-file-exploit ✔️ </h2></summary>
  <h3>Description:</h3>
	The program provided allows you to write to a file and read what you wrote from it. Try playing around with it and see if you can break it! <br/>
	<s>Connect to the program with netcat:</s><br/> 
	<s>$ nc saturn.picoctf.net 49700</s><br/>
	<i>Use <a href=https://github.com/LogicBypass/CTF-s/raw/main/picoCTF-2022/1.basic-file-exploit/1.basic-file-exploit.out><code>1.basic-file-exploit.out</code></a> instead</i><br/>
	The program's source code with the flag redacted can be downloaded <a href=https://github.com/LogicBypass/CTF-s/blob/main/picoCTF-2022/1.basic-file-exploit/1.basic-file-exploit.c>here.</a> <br/>
	<br/>
  
  > Hint: Try passing in things the program doesn't expect. Like a string instead of a number.
  ----
  <h3>Reconnaissance:</h3>
	Download the files: <code>wget link-to-file</code><br/> 
	Make it exacutable:<code>chmod +x *</code><br/>
	Open the program's source code file and analyze the flag retrieving function:<br/>
	<code>  if ((entry_number = strtol(entry, NULL, 10)) == 0) {puts(flag); fseek(stdin, 0, SEEK_END); exit(0);}</code><br/>
	Now we need to manipulate the input to get the function result == 0 </br>
	
	
</details>



 Checkmarks:
✔️
❌


