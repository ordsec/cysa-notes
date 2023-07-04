### Password attacks

- How can you break a password?
	- Read it off of a post-it in the victim's office
	- Online brute force: going through every possible combination of numbers and letters within a specific length
		- This likely won't work because of how much time it'll require
		- It's also very noisy and you'll get locked out or otherwise limited in some fashion
	- Hack the server, steal the passwords
		- Not likely to work either - passwords are usually not stored in cleartext, but they're stored as hashes instead
	- Hack the server, steal hashed passwords, crack the hashes
		- Performed offline, where no one can stop you
		- Still relies on low-entropy passwords
		- But more likely to succeed!
	- Social engineering
		- As long as you know where to poke and prod, your victim will give it up
		- Humans are the biggest vulnerability of any system, no matter how secure
		- The most likely to succeed

### Brute force

- Trying all sorts of combinations, online, likely impractical
- Tools:
	- Medusa: works with a bunch of different services
	- Hydra: supports tons of protocols and services, supports login/password lists, etc.
	- John the Ripper: brute force utility for password cracking
	- **Hashcat** (formerly **oclHashcat**): also for password cracking, has charsets for brute force and a pretty intelligent approach to the brute force process, can leverage GPU power to perform more operations per second
- Approaches:
	- Charsets (just combinations of characters)
	- Dictionaries (predefined lists of passwords)
	- Hybrid (some type of combination of the two above methods)
- Can take massive, unrealistic amounts of time, particularly since the response to each password attempt isn't instantaneous, so you're gonna have a delay before every subsequent attempt
- Easily detected (same user, same IP address - lots of ways), then you're banned or locked out

### Hashing

- Passwords must be stored as one-way hashes created by a good algorithm
- Hashes are irreversible - can't deduce the original value from the hash
- Hashes are deterministic - one output will always produce the same result when using the same algorithm
- Collisions occur when two different inputs produce the same output (MD5 got done dirty by collisions)
	- How likely is it that we can find two such inputs? Quite likely!
	- We have a lot of potential input 
	- It doesn't matter what those two inputs are - they produce the same output, and that's what gets stored to stand in for the password
	- This is known as a **birthday attack**, which is based on the birthday paradox
		- You only need 23 people in the room for a 50% chance that two of them will have the same birthday
		- It's no specific birthday - just any birthday as long as they both share it
		- 70% chance for 30 people!
		- **99.9%** chance for 70 people!
	- The more inputs you try, the better the chances that you'll find a collision 
	- A system will technically accept any password as long as it creates the correct hash
	- This deprecated a lot of older hashing algorithms because they are inherently vulnerable to these attacks
- To crack a hash, you must find the data that generated it - either by comparing a bunch of pre-compiled hashes from a rainbow table or by calculating your own based on a certain list (more operations).

### Rainbow tables

- A great solution to reduce the time spent on cracking a hash
- A massive file with pre-computed hashes 
- Search, don't compute
- Different rainbow tables for different hashing algos
- Someone has to create them
- Download them, access them online, create your own
	- Available for purchase as well
- Can contain any passwords: brute-force-like lists with all possible combinations, 1,000,000 most common passwords, etc.
- RTGen can be used to generate rainbow tables
- RCrack for cracking using a rainbow table
- RT lookup is possible: [CrackStation](https://crackstation.net/)
- Mitigation:
	- Users: 
		- Create longer passwords with good entropy (small/capital, numbers, characters, 16 symbols at minimum)
		- Don't use words, especially those that mean something to you (can be figured out from OSINT or social engineering)
	- Developers:
		- Don't just hash a password once
		- Do multiple iterations, use key stretching
		- Use the **salting** technique
			- On the server side, before hashing a password, add a string of random characters to it
			- Hash the resulting string of password + salt
			- Store the salt on the server - not even the user knows what it is!
				- And even if the attacker finds the salt, it still gets added - once again - to the password upon login, producing a completely different hash 
			- You'd need a rainbow table for every single password + salt combination, which is not possible

### Horizontal brute force

- **Password spraying**
	- Going over a whole bunch of usernames using 1-3 different passwords
		- How many users in a company with 3000 employees use "password123"?
		- It might easily be more than zero
		- And now you're in
- **Credential stuffing**
	- You've cracked one password and you know you have a valid user/pass combination for one website/service
	- Maybe they've used the same combination for every website/service? People reuse passwords all the time, unfortunately

---

### Exam

Understand hashing! Very important concept. Be quite familiar with password cracking techniques, including hash cracking, talk about different attacks and how they can be mitigated. 