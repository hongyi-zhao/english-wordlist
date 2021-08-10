See [here](https://lists.gnu.org/archive/html/help-gnu-emacs/2021-08/msg00138.html) for the motivation of this repository. Basically, the aim is to build an exhaustive  Enaglish word list for ispell and company-ispell, hence the name [american-english-exhaustive](https://github.com/hongyi-zhao/english-wordlist/blob/master/american-english-exhaustive) is used. The target audience is academic research and technology users who use Emacs as a way of life.

[The stardict relevant word lists](https://github.com/hongyi-zhao/english-wordlist/tree/master/stardict-wordlist) are generated by [pyglossary](https://github.com/ilius/pyglossary) using the dictionaries download from [here](http://download.huzheng.org/bigdict). The following steps is a example to create these word lists which I've described [here](https://github.com/company-mode/company-mode/issues/1146#issuecomment-886172208):

```shell
$ sudo apt-get install python3-tk tix
# pyenv python environment for this operation:
$ pyenv shell datasci
$ pip install gobject PyGObject pyglossary
$ mkdir -p ~/.stardict/dic && cd $_
# Do the same steps for other stardict dictionaries to generate the csv files.
$ curl -O http://download.huzheng.org/bigdict/stardict-Webster_s_Unabridged_3-2.4.2.tar.bz2
$ tar xvf stardict-Webster_s_Unabridged_3-2.4.2.tar.bz2
$ cd stardict-Webster_s_Unabridged_3-2.4.2
$ pyglossary Webster_s_Unabridged_3.ifo Webster_s_Unabridged_3.csv
# Extract the word list entries from the above csv files:
$ for i in *.csv; do grep -hv '^"#' $i | awk -F, '{sub(/^["]/,"",$1);sub(/["]$/,"",$1);print $1}' > ${i%.csv}.txt; done
```
The word list [american-english-insane](https://github.com/hongyi-zhao/english-wordlist/blob/master/american-english-insane) is generated by [SCOWL Custom List/Dictionary Creator](http://app.aspell.net/create) with the following option:

![image](https://user-images.githubusercontent.com/11155854/128634359-d13323a0-38ab-4adb-b06e-90b093c50531.png)

The word list [words.txt](https://github.com/hongyi-zhao/english-wordlist/blob/master/words.txt) comes [here](https://github.com/dwyl/english-words/blob/master/words.txt). Finally the word list [american-english-exhaustive](https://github.com/hongyi-zhao/english-wordlist/blob/master/american-english-exhaustive) is generated by the following command:

```shell
$ sort -u stardict-wordlist/* american-english-insane words.txt -o american-english-exhaustive
```

Set the follownig variable in Emacs initiazation file to use [american-english-exhaustive](https://github.com/hongyi-zhao/english-wordlist/blob/master/american-english-healthy):

```emacs-lisp
(setq ispell-alternate-dictionary (file-truename "/path/to/american-english-exhaustive"))
;or
;(setq company-ispell-dictionary (file-truename "/path/to/american-english-exhaustive"))
```        
