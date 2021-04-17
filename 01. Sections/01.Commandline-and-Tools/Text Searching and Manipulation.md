
```
grep                                          Search for a string
grep -i                                       Search for a string but ignore the case
grep -r                                       Search recursively for a string in a directory
grep                                          Search for a string in a zip file
grep -v                                       Exclude a string from a file
grep -v '(FOO|BAR)'                           Exclude multiple strings from a file
```

```bash
cut -d ' ' -f2                                Cut with a delimiter of a space, then print the second field
cut -d ' ' -f2-5                              Cut with a delimiter of a space, then print the second through the fifth fields
cut -d ' ' -f2-                               Cut with a delimiter of a space, then print the second field to the end
```


```bash
awk '{print $2}'                              Print the second column
awk '{print $2,$5}'                           Print the second and fifth columns
awk -F "https://" '{print $2}'                Use a string as a delimiter, then print the second column
```


```bash
sed 's/^/FOO/g'                               Add FOO to the beginning of each line
sed 's/$/FOO/g'                               Add FOO to the end of each line
```


```bash
sed '/[[:blank:]]/d'                                          Find lines that contain a single word
grep 'FOO'                                                    Find FOO
sed '/FOO/I,+12 d'                                            Find FOO, and delete that and the next 12 lines
sed 's/FOO.*$//g'                                             Find FOO, and delete to the end of file
sed 's/FOO/\n&/g'                                             Find FOO, and insert a new line and FOO
grep 'FOO\|BAR'                                               Find FOO, and lines that contain BAR
sed '1N;N;/\(.*\n\)\{2\}.*FOO/P;$d;D'                         Find FOO, and print the second line before that
awk -v n=-2 'NR==n+1 && !NF{next} /FOO/ {n=NR}1'              Find FOO, if the next line is blank, delete it
awk -v n=-2 'NR==n+1 && !NF{next} /^[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}/ {n=NR}1'
                                                              Find lines that start with IP, if the next line is blank, delete it
awk -v n=-2 'NR==n+1 && NF{print hold} /FOO/ {n=NR;hold=$0;next}1'
                                                              Find FOO, if the next line is blank, delete line containing FOO
awk '/FOO/ { FOO = 1; next }  FOO == 1 && /^$/ { FOO = 0; next }  { FOO = 0 }  { print }'
                                                              Find FOO, if the next line is blank, delete both lines
awk -v n=-2 'NR==n+1 {if($0 ~ /BAR/) { next; } else { print hold } } /FOO/ {n=NR;hold=$0;next}1'
                                                              Find FOO, if the next line contains BAR, delete both lines
grep -E 'FOO|BAR'                                             Find lines that contain either string 1 or 2
grep '[0-9]'                                                  Find lines that contain a number
grep '[0-9]$'                                                 Find lines that end with a number
grep 'FOO$'                                                   Find lines that end with FOO
sed 's/FOO$//'                                                Find lines that end with FOO, and delete FOO
```

```
grep '^[0-9]'                                                 Find lines that start with a number
grep '^FOO'                                                   Find lines that start with FOO
sed '/^FOO/{n;d;}'                                            Find lines that start with FOO, and delete the following line
printf '%s\n' 'g/^FOO/-1d' w | ed -s                          Find lines that start with FOO, and delete the previous line
sed '/^FOO/i\ '                                               Find lines that start with FOO, and insert a line before
sed '/^FOO/a\ '                                               Find lines that start with FOO, and insert a line after
sed -e :a -e '$!N;s/\nFOO/ /;ta' -e 'P;D'                     Find lines that start with FOO, insert a space and append to the previous line
```



```bash
sed -n '/FOO/,$p'                                             Print from FOO to the end of the file
sed s/FOO.*//                                                 Print from FOO to the end of the line
sed -n '5,/^$/p'                                              Print from the 5th line to the first blank line
awk 'NR == 1 || NR % 3 == 0'                                  Print the first and every third line
awk 'NR % 3 == 0'                                             Print every third line
sed -n '/FOO/,/BAR/p'                                         Print lines between FOO and BAR
sed -e '/./{H;$!d;}' -e 'x;/FOO/!d;'                          Print paragraphs that contains FOO
awk '{print $2 " " $1}'                                       Print the second column, insert a space, then the first column
```

```bash
sed 's/[A-Z]\{2\},//g'                                        Remove any 2 capital letters followed by a comma
sed '/^$/d'                                                   Remove blank lines
sed 's/[0-9]\{2\}\/[0-9]\{2\}\/[0-9]\{2\}//g'                 Remove dates (mm/dd/yy)
sed 's/^....//'                                               Remove first 4 characters from each line
sed '1,5d'                                                    Remove first 5 lines
sed 's/^[ \t]*//; s/[ \t]*$//'                                Remove leading and trailing whitespace from each line
sed 's/^[ \t]*//'                                             Remove leading whitespace from each line
sed '/FOO/,/BAR/d'                                            Remove lines between FOO and BAR
awk '/FOO/{f=1} (!f || f>2){print} (f && /BAR/){f++}'         Remove lines from FOO and the second BAR
awk '$2 !~ /[a-z]/'                                           Remove lines that contain [a-z] in the second column
sed '/[[:blank:]]/!d'                                         Remove lines that contain a single word
awk 'NF != 2'                                                 Remove lines that contain two words
awk '!(/FOO/ && /BAR/)'                                       Remove lines that contain FOO and BAR
printf '%s\n' 'g/FOO/d\' '-d' w | ed -s                       Remove lines that contain FOO and the previous line
sed '/@.*@/d'                                                 Remove lines that contain two @ symbols
sed '/[0-9]$/'                                                Remove lines that contain a number
sed '/[0-9]$/d'                                               Remove lines that end with a number
sed '/FOO$/d'                                                 Remove lines that end with FOO
sed '/^[0-9]/d'                                               Remove lines that start with a number
sed '/^FOO/d'                                                 Remove lines that start with FOO
grep -Ev '^\b([0-9]{1,3}\.){3}[0-9]{1,3}\b'                   Remove lines that start with an IP
sed '/^.,/d'                                                  Remove lines where the second letter is a comma
cat tmp | tr -d "\n" | tr -d " "                              Remove new lines and spaces. Ex copying a hash from Rubeus for use in Mimikatz
sed 's/[ \t]*$//'                                             Remove trailing whitespace from each line
```

```bash
sed 's/ \{2,\}/,/g'                                           Replace 2 or more spaces with a comma
sed 's/\.\.\.//g'                                             Replace 3 periods with nothing
sed 's/FOO/BAR/g'                                             Replace FOO with BAR
find . -type f -exec sed -i 's/FOO/BAR/g' {} +                Replace FOO with BAR recursively in the current directory
sed '/TEST/!s/FOO/BAR/g'                                      Replace FOO with BAR, except on lines that contain TEST
sed '/TEST/s/FOO/BAR/g'                                       Replace FOO with BAR, on lines that contain TEST
sed 's/FOO//g'                                                Replace FOO with nothing
sed -n 'H; ${x; s/\n/|/g; p}'                                 Replace 'new line' with a pipe '|'
sed 's/\([^,]*,\)\{7\}[^,]*,/&\n/g'                           Replace the 8th comma with a new line
```



```bash
sed 's/[^ ]\+/\L\u&/g'                                        Title case
```












