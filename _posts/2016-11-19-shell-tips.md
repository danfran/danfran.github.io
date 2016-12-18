---
layout: post
title: "Shell Tips"
date:   2016-11-19
categories: shell
tags: sh bash awk grep tr sed cut
---
### Good readings for AWK

* O'Reilly - Effective awk Programming
* OReilly - Sed & Awk
* Sed N AWK 101 Hacks
* [http://www.vectorsite.net/tsawk.html](http://www.vectorsite.net/tsawk.html)
* [http://www.grymoire.com/Unix/Awk.html](http://www.grymoire.com/Unix/Awk.html)
* [http://techarena51.com/index.php/advance-text-processing-examples-awk/](http://techarena51.com/index.php/advance-text-processing-examples-awk/)

### Pipe values to Grep
Cut first 9 chars from a file, and the single remaining line become the term to grep; "-f" is for file, and the file "-" is for stdin.

```sh
$ cut -c9- somefile.txt | grep -f - anotherfile.txt
```

### How to count the number of occurrencies for a character (e.g.: ") line by line
```sh
$ tr -d -c '"\n' < filename.csv | awk '{ print length; }'
```

more overkill

```sh
$ sed 's/[^"]//g' filename.csv | awk '{ print length }' | uniq -c | sort
```

### How to check if the CSV has the right number of commas and expected column characters
```sh
$ awk 'BEGIN{FS=OFS=","} NF==30 && $1~/^[0-9]$/ && $2~/^[a-z]{2,4}$/ && $3~/^[YN]$/' filename.csv
```

or

```sh
$ awk 'BEGIN{FS=OFS=","} NF!=30 {print "not enough fields"; exit} !($1~/^[0-9]$/) {print "1st field invalid"; exit}' filename.csv
```

### How to find bad lines (with an odd number of double-quotes) and copy them on a separate file
```sh
$ tr -d -c '"\n' < filename.csv | awk '{ if (length == 5 || length == 3) print NR }' > lines.txt
$ while read line; do head -n $line filename.csv | tail -1 ; done <lines.txt > bad_lines.txt
```

### How to find the line in a file by number of line (e.g. 5th line):
```sh
sed '5!d' file
```

or

```sh
awk 'NR==5' file
```

### How to convert the file from an encoding (-f) to another (-t) (e.g. from UTF-8 to ISO-8859-15)
```iconv -f ISO-8859-15 -t UTF-8 file_to_convert.txt > converted_file.txt```

### How to delete a row from a file (e.g. fifth row)
```sh
sed -e '5d' filename.txt > new_filename.txt
```

or to overwrite the same file

```sh
sed -i -e '5d' filename.txt
```

### How to split a text line in 2 parts by regex e returning the matched part (e.g. the last 14 tokens in a CSV)
```sh
grep -oP "sometexthere(,[^,.]*){13}$" filename.csv
```

### How to remove lines from one file that have indexes in another file at the first column
```sh
awk 'NR==FNR{a[$1]=$1;next} !a[$1]'  indexes.txt file.txt > cleaned_file.txt
```

### Convert to lower case (for example a file)
```sh
cat file.txt | tr '[:upper:]' '[:lower:]' > file_lowercase.txt
```

### Display the nth line number of a file (e.g. 1002th line)
```sh
sed "1002q;d" file.txt
```

### How to redirect output to a location I don't have permission to write to
```sh
sudo sh -c 'head -1000000 filename.txt > filename.txt.part'
```

### How to create different output files when process a CSV based on field conditions
```sh
awk -F\, '$8 == "\"Some value\"" {
    gsub(/"| /, "", $6);

    if ($4 ~ /foo/)
    {
      print > "output1.csv"
    } else if ($4 ~ /bar/)
    {
      print > "output2.csv"
    } else
    {
      print > "output3.csv"
    }
  }' input_file.csv
```
