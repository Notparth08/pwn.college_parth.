# Challenge Writeup Template — Globbing & Tab Completion

# Challenge Name
type what the challenge asks

## My solve
**Flag:** `pwn.college{helloworld}`

Type in your solve and your thought process behind solving the challenge. Include as much info as possible. Use triple ticks for bash.

```bash
example triple ticks for bash
pwn.college{helloworld}
```

## What I learned
explain what you learned

## References 
Add any references or videos you used while solving the challenge.

---

## Collected flags
1) `pwn.college{QxVxJZDGYRl4NJX-Vu9FxYcGQTi.QXxIDO0wSM2gjNzEzW}`
2) `pwn.college{sDPtufyXApSV2ZWO2BAvN7TxcT-.QXyIDO0wSM2gjNzEzW}`
3) `pwn.college{IIWNfXYI60Q82l6LoUelEfNVop0.QXzIDO0wSM2gjNzEzW}`
4) `pwn.college{YwO9O5kGmT2uCNQBlbOiShzXdQu.QX0IDO0wSM2gjNzEzW}`
5) `pwn.college{8DFnbLzy0wG27bSCbzR0ULTrmna.0lM3kjNxwSM2gjNzEzW}`
6) `pwn.college{cbi9TV22xD9SETMDe--VY07lJMc.QX1IDO0wSM2gjNzEzW}`
7) `pwn.college{MUgutFp2VFanuFGm7iH5pZyXj1W.QX2IDO0wSM2gjNzEzW}`
8) `pwn.college{kLKRLqzEr9R3kfn3S7Yn5VCYwhn.0FN0EzNxwSM2gjNzEzW}`
9) `pwn.college{4FW0ahjK9cG-ltA-K2Fb7jdPcwp.0lN0EzNxwSM2gjNzEzW}`
10) `pwn.college{QWrrPyTiklVS3ESQmh39cQ_SDNd.0VN0EzNxwSM2gjNzEzW}`

---

## Module Text (globbing & tab completion)
The first glob we'll learn is `*`. When it encounters a `*` character in any argument, the shell will treat it as a "wildcard" and try to replace that argument with any files that match the pattern. It's easier to show you than explain:

```bash
hacker@dojo:~$ touch file_a
hacker@dojo:~$ touch file_b
hacker@dojo:~$ touch file_c
hacker@dojo:~$ ls
file_a  file_b  file_c
hacker@dojo:~$ echo Look: file_*
Look: file_a file_b file_c
```

The `*` matches any part of the filename except for `/` or a leading `.` character.

### Practice tasks (from the challenge)
- Change directory to `/challenge` but use globbing so the argument to `cd` is at most four characters. Then run `/challenge/run` for the flag.
- Use `?` to replace single characters when changing directory to `/challenge` (replace `c` and `l` with `?`).
- Use `[]` bracket-globs to match specific single characters (exercise: change to `/challenge/files` and run `/challenge/run` with a single argument that bracket-globs into `file_b`, `file_a`, `file_s`, and `file_h`).
- Use absolute path globbing to match `/home/hacker/file_[ab]`.
- Use multiple `*` globs in a single word (≤3 characters) to cover every word that contains the letter `p`.
- Create a single short glob (≤6 characters) that matches `challenging`, `educational`, and `pwning`.
- Use inverted bracket-glob `[!abc]` or `[^abc]` to match files that do not start with `p`, `w`, or `n`.
- Use tab-completion to read `/challenge/pwncollege` (the flag file requires tab-complete to be typed).
- Tab-complete options when multiple matches exist (press TAB twice to list options).

### Notes / Caveats
- Globs are expanded by the shell before the command runs — be careful with commands like `rm`.
- Tab completion behavior can vary by shell and configuration.
- Inverting bracket globs uses `!` or `^` as the first character inside `[]`. Using `!` elsewhere in `[]` has different meanings in some shells.

---

If you'd like, I can:
- Fill in the "My solve" section with a real example solution and exact commands (if you paste the exact outputs you got in your shell).
- Generate a second version formatted for a writeup site (clean prompts and outputs).
