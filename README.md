
	 ██▓     ▄▄▄       ██▒   █▓ ▄▄▄       ██▓     ▄▄▄       ███▄ ▄███▓ ██▓███
	▓██▒    ▒████▄    ▓██░   █▒▒████▄    ▓██▒    ▒████▄    ▓██▒▀█▀ ██▒▓██░  ██▒
	▒██░    ▒██  ▀█▄   ▓██  █▒░▒██  ▀█▄  ▒██░    ▒██  ▀█▄  ▓██    ▓██░▓██░ ██▓▒
	▒██░    ░██▄▄▄▄██   ▒██ █░░░██▄▄▄▄██ ▒██░    ░██▄▄▄▄██ ▒██    ▒██ ▒██▄█▓▒ ▒
	░██████▒ ▓█   ▓██▒   ▒▀█░   ▓█   ▓██▒░██████▒ ▓█   ▓██▒▒██▒   ░██▒▒██▒ ░  ░
	░ ▒░▓  ░ ▒▒   ▓▒█░   ░ ▐░   ▒▒   ▓▒█░░ ▒░▓  ░ ▒▒   ▓▒█░░ ▒░   ░  ░▒▓▒░ ░  ░
	░ ░ ▒  ░  ▒   ▒▒ ░   ░ ░░    ▒   ▒▒ ░░ ░ ▒  ░  ▒   ▒▒ ░░  ░      ░░▒ ░
	  ░ ░     ░   ▒        ░░    ░   ▒     ░ ░     ░   ▒   ░      ░   ░░
		░  ░      ░  ░      ░        ░  ░    ░  ░      ░  ░       ░
						   ░

LavaPasswordFactory is a simple little script for both generating password lists for online and offline attacks as well as cleaning existing password lists based on given password policies. The script can be invoked in one of two modes of operation: generation mode and cleaning mode.


**[Generation Mode]**


In generation mode the script will first create a password list based on the words passed to the script. The length of the resulting password list is based on a number of factors, but the largest factor is whether or not the --is-offline command line argument is present at invocation. When --is-offline is NOT present, the resulting password list is intended to be used in online attacks, such as brute-forcing a login for a Web application or network service. When --is-offline IS present, the resulting password list is intended to be used to crack hashes in offline fashion. The main difference between these two is that an offline attack does not require going through a third-party to validate passwords, and as such the password list for offline attacks can be much larger. It should be noted that the difference in scale between password lists generated for online versus offline attacks can be order of magnitude, or less than a million versus billions. It follows that the amount of time and storage space required to generate an offline list is much larger than an online list.

When generating password lists, the number of words passed to the script should typically only be two to four words long. Passing longer lists of words is technically supported, but for accuracy and effectiveness is not recommended. The words that are passed to the script should be directly related to the target to attack, meaning that a password list for a Web application that exists at foo.com and sells shoes could be generated using the list of "foo shoes sales".

The user has the option of supplying additional parameters at invocation time that map to common password policy restrictions. For instance, passing "--min-length 8" will set a requirement that all returned passwords must be at least eight characters long and passing "--illegal-chars !@#" will ensure that no returned passwords contain the characters !, @, or #. In online mode the password list is generated and stored in memory in its entirety and then cleaned before being written to disk. In offline mode, as each password is generated it is validated against the input set of rules and written to disk before the next password is generated.


**[Cleaning Mode]**


In cleaning mode the script will read passwords from an input file, validate those passwords against the input set of rules, and then write all of the valid passwords into the specified output file. This is handy for use with tools such as CeWL (http://digi.ninja/projects/cewl.php) where an effective list may be created, but the resulting list contains many phrases which do not abide by the password policy of whatever the target may be.


**[Tool Help]**


**Generic help:**

usage: LavaPasswordFactory.py [-h]
							  [--log-level <DEBUG|INFO|WARNING|ERROR|CRITICAL>]
							  {generate,clean} ...

LavaPasswordFactory - for all your password list

positional arguments:
  {generate,clean}      sub-command help
	generate            generate help
	clean               clean help

optional arguments:
  -h, --help            show this help message and exit
  --log-level <DEBUG|INFO|WARNING|ERROR|CRITICAL>
						The log message level to receive when running the
						script. Validvalues are DEBUG, INFO, WARNING, ERROR,
						CRITICAL.

**Generation mode help:**

usage: LavaPasswordFactory.py generate [-h] [--special-chars <chars>]
									   [--illegal-chars <chars>]
									   [--max-special <num>]
									   [--min-special <num>]
									   [--max-caps <num>] [--min-caps <num>]
									   [--is-offline] [--min-length <len>]
									   [--max-length <len>] --words
									   <word_list> --output-file <file_path>

optional arguments:
  -h, --help            show this help message and exit
  --special-chars <chars>
						The characters to be considered as 'special
						characters' for thepurpose of filtering out ineligible
						passwords.
  --illegal-chars <chars>
						Characters that are not allowed in the generated
						passwords.
  --max-special <num>   The maximum number of special characters per generated
						password.
  --min-special <num>   The minimum number of special characters per generated
						password.
  --max-caps <num>      The maximum number of capital letters per generated
						password.
  --min-caps <num>      The minimum number of capital letters per generated
						password.
  --is-offline          Whether or not to configure the LavaPasswordFactory to
						produce a passwordlist suited for offline attacks
						(significantly larger than for onlineattacks). Flying
						this flag enables the offline mode.
  --min-length <len>    The minimum length for generated passwords.
  --max-length <len>    The maximum length for generated passwords.
  --words <word_list>   The list of words used as the seeds for the password
						generation process,seperated by spaces. For instance,
						"hello goodbye" would result in ['hello', 'goodbye']
						as the input words
  --output-file <file_path>
						Path to the file that the resulting passwords should
						be written to.		

**Cleaning mode help:**

usage: LavaPasswordFactory.py clean [-h] [--special-chars <chars>]
									[--illegal-chars <chars>]
									[--max-special <num>]
									[--min-special <num>] [--max-caps <num>]
									[--min-caps <num>] [--min-length <len>]
									[--max-length <len>] --input-file
									<file_path> --output-file <file_path>

optional arguments:
  -h, --help            show this help message and exit
  --special-chars <chars>
						The characters to be considered as 'special
						characters' for thepurpose of filtering out ineligible
						passwords.
  --illegal-chars <chars>
						Characters that are not allowed in the generated
						passwords.
  --max-special <num>   The maximum number of special characters per generated
						password.
  --min-special <num>   The minimum number of special characters per generated
						password.
  --max-caps <num>      The maximum number of capital letters per generated
						password.
  --min-caps <num>      The minimum number of capital letters per generated
						password.
  --min-length <len>    The minimum length for generated passwords.
  --max-length <len>    The maximum length for generated passwords.
  --input-file <file_path>
						Path to the file that the existing password list to
						clean resides in.
  --output-file <file_path>
						Path to the file that the resulting passwords should
						be written to.


**[Future Directions and Features]**


- Add a mode that 1337-speaks all words within an input file