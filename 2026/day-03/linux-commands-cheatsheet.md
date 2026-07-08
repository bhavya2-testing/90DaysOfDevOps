Linux Command Cheat Sheet
#######################File & Directory Management#########################
Command	Usage
pwd - Show the current working directory.
ls -la - List all files with detailed information.
cd <dir> - 	Change to another directory.
mkdir <dir>	- Create a new directory.
rmdir <dir>	- Remove an empty directory.
rm -r <dir>	- Delete a directory and its contents.
cp <src> <dest>	- Copy files or directories.
mv <src> <dest>	- Move or rename a file/directory.
find <path> -name "<file>" - Search for files by name.

##########################File Viewing & Editing############################
Command	Usage
cat <file>	- Display the contents of a file.
less <file>	 - View large files one page at a time.
head -n 10 <file>	- Show the first 10 lines of a file.
tail -f <file> - Follow a log file in real time.
nano <file>	- Edit a file using the Nano editor.

###########################Permissions & Ownership##########################
Command	Usage
chmod 755 <file>	Change file permissions.
chown user:group <file>	Change file owner and group.

###############################Process Management############################
Command	Usage
ps -ef	List all running processes.
top	Monitor system processes interactively.
kill <PID>	Terminate a process by PID.

###############################Disk & System Information#####################
Command	Usage
df -h	- Show disk usage in human-readable format.
du -sh <dir>	- Show total size of a directory.
free -h	- Display memory usage.
uname -a -	Show Linux kernel and system information.

###############################Search & Text Processing########################
Command	Usage
grep "text" <file>	- Search for matching text in a file.
wc -l <file>	- Count the number of lines in a file.

#################################Networking###################################
Command	Usage
ping google.com	 - Check network connectivity to a host.
ip addr	- Display network interface IP addresses.
curl https://example.com - 	Fetch data from a URL or test an API.
