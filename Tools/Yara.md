https://www.section.io/engineering-education/getting-started-with-yara-for-malware-analysis/

https://yara.readthedocs.io/en/stable/commandline.html
# Rule Ex
```c
rule SectionSample{
  meta:
   author = “Felix Vaati”
   description = “Simple YARA rule”
  strings:
   $text_sample="readers" nocase
   $hex_sample={73 65 63 74 69 6f 6e 20 ?? 65 61 64 65 72 73}
  
  condition:
   any of them //checks whether a file has any of the above rules
}
```

# Running the Rules
```bash
┌──(dyrstiu㉿kali)-[~/Documents/Demo]
└─$ yara sample.yar test.txt
SectionSample test.txt
```

```bash
┌──(dyrstiu㉿kali)-[~/Documents/Demo]
└─$ yara sample.yar ~/. 
SectionSample /home/kali/./stegsolve.jar
SectionSample /home/kali/./phoneinfoga
```