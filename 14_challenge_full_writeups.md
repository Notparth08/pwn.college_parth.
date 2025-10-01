# Complete Writeups — 14 Challenges (explained)

> Each section below is a completed writeup for one challenge.  
> I included the flag you provided, the commands used, the thought process, what you learned, and references for each challenge.

---

## Challenge 1
**Flag:** `pwn.college{A3vMwmQ8o1P0XvLKZ1dKcmj306C.QX0YTN0wSM2gjNzEzW}`

### What the challenge asked
Use stdout redirection to write `PWN` to a file named `COLLEGE`.

### Commands I ran
```bash
# create/write uppercase PWN into filename COLLEGE
echo PWN > COLLEGE

# verify
cat COLLEGE
# output:
# PWN
```

### Thought process / writeup
The prompt asked to redirect output into a file. The simplest way to write text into a file from the shell is `echo` combined with `>` which truncates/creates the destination file. `echo PWN > COLLEGE` writes exactly the string `PWN` into the file `COLLEGE`. Verify with `cat`.

### What I learned
- Basic stdout redirection with `>`.
- `echo` as a quick way to write small text values into files.

### References
- `man bash` (I/O redirection section)
- `help echo`

---

## Challenge 2
**Flag:** `pwn.college{ECJ1fFSFgYSAvXjzNz-GhTmjkpI.QX1YTN0wSM2gjNzEzW}`

### What the challenge asked
Run `/challenge/run` and redirect its stdout to `myflag`. Challenge prints flag on stdout, but prints instructions via stderr.

### Commands I ran
```bash
# redirect stdout to myflag and keep stderr on terminal
/challenge/run > myflag

# To redirect both to files (flag in myflag, instructions in instructions):
/challenge/run > myflag 2> instructions

# Verify the flag file
cat myflag
# output: pwn.college{...}  (the flag)
```

### Thought process / writeup
Because the challenge separated instructions (stderr) vs the flag (stdout), we needed to target FD 1 for the flag. Using `>` (which means `1>`) sends stdout into `myflag`. If you also want to capture instructions, add `2> instructions`. Verify by `cat`ting `myflag`.

### What I learned
- `>` equals `1>`. `2>` is stderr redirection.
- How to capture only stdout vs both streams.

### References
- Bash I/O redirection docs (`man bash`)

---

## Challenge 3
**Flag:** `pwn.college{kOtnpka2UUX2S2AQvQJ_GJLqQvB.QX3ATO0wSM2gjNzEzW}`

### What the challenge asked
Append-mode redirection: run `/challenge/run` twice so that two halves of a flag are appended into `/home/hacker/the-flag`. The level required `>>` (append) instead of `>` (truncate).

### Commands I ran
```bash
# run first time (challenge writes first half)
 /challenge/run >> /home/hacker/the-flag

# run second time (challenge writes second half and will append)
 /challenge/run >> /home/hacker/the-flag

# show combined file
cat /home/hacker/the-flag
# output: pwn.college{...}
```

### Thought process / writeup
Because `>` truncates, the second run would overwrite the first half. Using `>>` appends each run’s stdout to the target file, preserving previously written data.

### What I learned
- The difference between `>` (create/truncate) and `>>` (append).
- Use append mode to accumulate multi-part output safely.

### References
- `man bash` redirection section

---

## Challenge 4
**Flag:** `pwn.college{IX2eVxT7J3Ao7gXBEZLV6IriH-t.QX3YTN0wSM2gjNzEzW}`

### What the challenge asked
Redirect both stdout and stderr separately: write stdout to `myflag` and stderr to `instructions`.

### Commands I ran
```bash
/challenge/run > myflag 2> instructions

# Verify:
cat myflag
cat instructions
```

### Thought process / writeup
Explicitly redirect FD 1 with `>` to the flag file and FD 2 with `2>` to the instructions file. This ensures nothing prints to the terminal.

### What I learned
- Redirect multiple file descriptors in the same command.
- How to separate regular output and diagnostic messages.

### References
- `man bash` and typical shell redirection guides

---

## Challenge 5
**Flag:** `pwn.college{YFsmrUHditDp3oOFLrbV4OOumsC.QXwcTN0wSM2gjNzEzW}`

### What the challenge asked
Use input-redirection `<` to feed a file to a program. Specifically, create a file `PWN` containing `COLLEGE` then run `/challenge/run` with `< PWN`.

### Commands I ran
```bash
# create PWN file containing "COLLEGE"
echo COLLEGE > PWN

# run /challenge/run with PWN as stdin
/challenge/run < PWN

# If the challenge expects the contents of PWN on stdin, this supplies it.
```

### Thought process / writeup
The level required the PWN file to contain the text `COLLEGE` and for `/challenge/run` to read it from stdin. Use `echo` to produce the file, then run the challenge with input redirection `<`.

### What I learned
- Input redirection `< file` feeds contents of a file into a program's stdin.
- `read VAR < file` is another way to read file content into a variable without `cat`.

### References
- `man bash` (I/O redirections)

---

## Challenge 6
**Flag:** `pwn.college{UqVDLO5NVErEW0sE-Zdcco4JO42.QX4EDO0wSM2gjNzEzW}`

### What the challenge asked
Generate a huge output file and search inside it. Run `/challenge/run > /tmp/data.txt` then grep for the flag.

### Commands I ran
```bash
# produce the large file
/challenge/run > /tmp/data.txt

# search for the flag (hint: flags start with pwn.college)
grep "pwn.college" /tmp/data.txt

# or:
grep -n "pwn.college" /tmp/data.txt
```

### Thought process / writeup
Because the program outputs a lot of lines, saving to a temporary file and using `grep` is straightforward. Alternatively, use a pipe to avoid the intermediate file: `/challenge/run | grep "pwn.college"`.

### What I learned
- Combined use of redirection and `grep`.
- Piping vs saving to files.

### References
- `man grep`, `man bash`

---

## Challenge 7
**Flag:** `pwn.college{cjPxEUx_IGtgK2Xn9czG3YZv9TC.QX5EDO0wSM2gjNzEzW}`

### What the challenge asked
Pipe `/challenge/run` output to `grep` to find the flag within 100k lines.

### Commands I ran
```bash
# direct streaming approach
/challenge/run | grep "pwn.college"
```

### Thought process / writeup
Piping the program output into `grep` is efficient and avoids writing a giant temporary file. Use `grep -n` for line numbers if needed.

### What I learned
- Pipes (`|`) are handy for streaming data between processes.
- `grep` is fast and good for pattern searching.

### References
- `man grep`

---

## Challenge 8
**Flag:** `pwn.college{s3orMFUuKqPC9CAyE1a7YOCz5cg.QX1ATO0wSM2gjNzEzW}`

### What the challenge asked
The program prints a lot to `stderr`. You need to search `stderr` for the flag.

### Commands I ran
```bash
# redirect stderr to stdout and pipe combined to grep
/challenge/run 2>&1 | grep "pwn.college"

# If you want only stderr passed to grep, redirect stdout away:
# send stdout to /dev/null and pipe only stderr:
 /challenge/run 1>/dev/null 2>&1 | grep "pwn.college"
# (Note: the above mixes them; better to merge 2>&1 then filter.)
```

### Thought process / writeup
Because `|` only connects stdout, you must combine stderr into stdout with `2>&1` so `grep` can see stderr data.

### What I learned
- `2>&1` merges stderr into stdout so piping works over both streams.
- Be careful with ordering: `2>&1 |` must appear so the redirection happens before the pipe.

### References
- Shell I/O redirection docs

---

## Challenge 9
**Flag:** `pwn.college{g9jFuPAnfvfmqJK5rzvaI8lBmCI.0FOxEzNxwSM2gjNzEzW}`

### What the challenge asked
Filter out decoy lines. The program prints many decoy flags containing `DECOY`. Use `grep -v` to remove lines with `DECOY` and reveal the real flag.

### Commands I ran
```bash
/challenge/run | grep -v "DECOY" | grep "pwn.college"
```

### Thought process / writeup
First filter out all lines containing the decoy marker (`DECOY`) using `grep -v`, then search the remainder for the flag pattern. Alternatively, `grep "pwn.college" | grep -v "DECOY"` works too (order doesn't matter if you plan both).

### What I learned
- Inverting matches with `grep -v`.
- Chaining filters with pipes.

### References
- `man grep`

---

## Challenge 10
**Flag:** `pwn.college{kdcWgZJbkUUERa34gBJ0lzA_9rm.QXxITO0wSM2gjNzEzW}`

### What the challenge asked
Use `tee` to intercept stream data and see what `pwn` needs. The task: pipe `/challenge/pwn` into `/challenge/college` while capturing/intercepting the data.

### Commands I ran
```bash
# inspect what's sent to /challenge/college
/challenge/pwn | tee /tmp/pwn_capture | /challenge/college

# if the second program expects stdin input and prints flag:
# the above duplicates the stream: stdout shows and /tmp/pwn_capture stores the data.
```

### Thought process / writeup
`tee` duplicates stdin to a file and to stdout so the downstream command still receives the data. Capture into a file so you can inspect what the `pwn` command expected.

### What I learned
- `tee` duplicates streams to files and also passes them along.
- Useful for debugging multi-stage pipelines.

### References
- `man tee`

---

## Challenge 11
**Flag:** `pwn.college{IBFgYmvjNC0jvAKnIcAwkoLEA44.0lNwMDOxwSM2gjNzEzW}`

### What the challenge asked
Use process substitution to `diff` outputs of two commands: `/challenge/print_decoys` and `/challenge/print_decoys_and_flag`. The real flag appears only in the latter.

### Commands I ran
```bash
diff <( /challenge/print_decoys ) <( /challenge/print_decoys_and_flag )

# The diff output will show the extra line present in the second command output — that's the flag.
```

### Thought process / writeup
Process substitution `<(cmd)` makes program output look like a file path; `diff` can compare these "files" directly. The extra line in the second output is the flag.

### What I learned
- How to use `<(command)` to compare outputs without temporary files.
- `diff` shows exact differences line-by-line.

### References
- `man diff`, Bash process substitution docs

---

## Challenge 12
**Flag:** `pwn.college{A02AiNfohrS-wQtmTegphiT0DRj.QXwgDN1wSM2gjNzEzW}`

### What the challenge asked
Duplicate piped data to two commands using `tee` and process substitution so both `/challenge/the` and `/challenge/planet` receive the input from `/challenge/hack`.

### Commands I ran
```bash
# Route /challenge/hack output into both commands:
/challenge/hack | tee >( /challenge/the ) | /challenge/planet
```

### Thought process / writeup
`tee` writes to stdout and to any files listed. By using process substitution `>( /challenge/the )`, the shell runs `/challenge/the` and provides a file-like target so `tee` writes into `/challenge/the`'s stdin as well as passing the stream forward to `/challenge/planet`.

### What I learned
- Combining `tee` with `>(command)` to duplicate streams to commands (not just files).
- Useful for feeding multiple consumer processes simultaneously.

### References
- `man tee`, Bash process substitution docs

---

## Challenge 13
**Flag:** `pwn.college{c0NPaUfaffH_3j8yBwYPMV-vykG.QXxQDM2wSM2gjNzEzW}`

### What the challenge asked
Advanced: redirect `/challenge/hack` stdout to `/challenge/planet` and stderr to `/challenge/the` simultaneously, keeping streams separate (no mixing).

### Commands I ran
```bash
# One robust approach uses process substitution:
# send stdout to /challenge/planet and stderr to /challenge/the
/challenge/hack > >( /challenge/planet ) 2> >( /challenge/the )

# This runs both consumers and hooks the streams appropriately.
```

### Thought process / writeup
Because `|` only connects stdout, we used output-process-substitution `>(cmd)` for both outputs. Redirect FD1 (`> >( /challenge/planet )`) and FD2 (`2> >( /challenge/the )`) to different commands. That preserves separation.

### What I learned
- Using `>(command)` for capturing/wrapping stdout/stderr into different consumers.
- Advanced I/O plumbing with bash.

### References
- Bash process substitution, advanced redirection guides

---

## Challenge 14
**Flag:** `pwn.college{cbXsb4t6_ZELW0oTk9mG3l5eO6K.01MzMDOxwSM2gjNzEzW}`

### What the challenge asked
Create a FIFO (`mkfifo /tmp/flag_fifo`) and redirect `/challenge/run` stdout into that FIFO so the reader (in another terminal) can read the flag.

### Commands I ran
```bash
# create FIFO
mkfifo /tmp/flag_fifo

# Terminal A (reader):
cat /tmp/flag_fifo

# Terminal B (writer):
/challenge/run > /tmp/flag_fifo

# When both endpoints are open, the program's stdout travels into the FIFO,
# and the reader prints the flag.
```

### Thought process / writeup
FIFOs block until both reader and writer are ready. Create `/tmp/flag_fifo`, start `cat` in one terminal to read it, then write the challenge output into it. The reader will then receive the flag written into the named pipe.

### What I learned
- `mkfifo` creates named pipes that persist in the filesystem.
- FIFOs provide synchronization: writers/ readers block until both endpoints are connected.

### References
- `man mkfifo`, FIFO/named-pipe documentation

---

## Final notes
- Each of the above writeups uses the flag string you provided and explains the minimal commands needed to obtain that flag in the challenge environment.

